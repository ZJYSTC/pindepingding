<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>檀雅书院学生品德评定系统</title>
    <!-- 引入 Supabase 库 -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.min.js"></script>
    <style>
        :root { --primary: #1a5276; --accent: #e67e22; --bg: #f5f6fa; --text: #2c3e50; --border: #dce3ea; --radius: 16px; --unsubmitted-red: #d63031; }
        * { margin:0; padding:0; box-sizing:border-box; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif; background: var(--bg); color: var(--text); height: 100vh; overflow: hidden; }
        .top-nav { position:fixed; top:0; left:0; right:0; z-index:1000; background: rgba(255,255,255,0.9); backdrop-filter: blur(16px); border-bottom:1px solid var(--border); padding:12px 20px; display:flex; align-items:center; justify-content:space-between; gap:12px; }
        .hamburger-btn { width:38px; height:38px; border-radius:50%; border:1px solid var(--border); background:#fff; cursor:pointer; display:flex; flex-direction:column; align-items:center; justify-content:center; gap:4px; padding:8px; }
        .hamburger-btn span { display:block; width:20px; height:2px; background:var(--text); border-radius:2px; }
        .title-text { font-size:1.15rem; font-weight:700; color:var(--primary); }
        .title-text span { color:var(--accent); }
        .role-toggle { display:flex; background:#eef1f5; border-radius:30px; padding:3px; gap:2px; }
        .role-toggle button { padding:9px 18px; border-radius:28px; border:none; cursor:pointer; font-size:0.9rem; font-weight:600; background:transparent; color:#666; }
        .role-toggle button.active { background:#fff; color:var(--primary); box-shadow:0 2px 8px rgba(0,0,0,0.1); }
        .btn-icon { width:38px; height:38px; border-radius:50%; border:1px solid var(--border); background:#fff; cursor:pointer; display:flex; align-items:center; justify-content:center; font-size:1.1rem; }
        .class-menu-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.2); z-index:1100; display:none; }
        .class-menu-overlay.open { display:flex; }
        .class-menu-panel { margin-top:70px; margin-left:20px; background:#fff; border-radius:var(--radius); box-shadow:0 12px 40px rgba(0,0,0,0.15); min-width:220px; max-height:70vh; overflow-y:auto; padding:8px 0; }
        .main-container { padding-top:70px; height:100vh; overflow-y:auto; }
        .student-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(130px,1fr)); gap:12px; }
        .student-card { background:#fff; border-radius:10px; padding:18px 16px; text-align:center; cursor:pointer; border:2px solid #e0e4e9; box-shadow:0 2px 10px rgba(0,0,0,0.05); }
        .student-card.unsubmitted { border-color:var(--unsubmitted-red); background:#fff5f5; }
        .student-card .avatar { width:50px; height:50px; border-radius:50%; background:linear-gradient(135deg,#e8f0fe,#d4e4fc); margin:0 auto 10px; display:flex; align-items:center; justify-content:center; font-size:1.4rem; color:var(--primary); font-weight:700; }
        .badge { display:inline-block; font-size:0.7rem; padding:3px 10px; border-radius:12px; margin-top:6px; }
        .badge-submitted { background:#e8f8ef; color:#27ae60; }
        .badge-pending { background:#ffebee; color:var(--unsubmitted-red); }
        .form-view { max-width:750px; margin:0 auto; padding:20px 20px 100px; }
        .category-section { background:#fff; border-radius:var(--radius); margin-bottom:16px; overflow:hidden; border:1px solid var(--border); }
        .category-header { padding:16px 20px; cursor:pointer; display:flex; justify-content:space-between; background:#fafbfc; font-weight:700; }
        .category-section.open .category-header { border-bottom:1px solid var(--border); }
        .category-body { display:none; padding:8px 0; }
        .category-section.open .category-body { display:block; }
        .sub-item { padding:14px 20px; border-bottom:1px solid #f0f2f5; display:flex; flex-wrap:wrap; align-items:center; gap:12px; }
        .sub-item:last-child { border-bottom:none; }
        .score-input-group { display:flex; align-items:center; gap:8px; }
        .score-input-group input { width:70px; padding:10px 8px; border:2px solid var(--border); border-radius:8px; text-align:center; font-size:1rem; font-weight:600; font-family:monospace; }
        .upload-btn { padding:8px 14px; border-radius:20px; border:1.5px dashed var(--border); background:#fff; cursor:pointer; font-size:0.8rem; display:inline-flex; align-items:center; gap:5px; color:#666; }
        .upload-btn.has-file { border-style:solid; border-color:#27ae60; color:#27ae60; background:#f0faf4; }
        .btn-submit { display:inline-block; min-width:180px; padding:14px 30px; border-radius:30px; border:none; background:var(--primary); color:#fff; font-size:1.05rem; font-weight:700; cursor:pointer; margin:10px; }
        .btn-export-pdf { background:#555; }
        .flip-view { position:fixed; top:0; left:0; width:100%; height:100%; z-index:900; background:var(--bg); display:none; flex-direction:column; }
        .flip-view.active { display:flex; }
        .flip-top-bar { display:flex; align-items:center; justify-content:space-between; padding:14px 18px; background:rgba(255,255,255,0.8); }
        .flip-slider { flex:1; overflow:hidden; }
        .flip-track { display:flex; flex-direction:column; height:100%; transition:transform 0.4s; }
        .flip-page { flex:0 0 100%; height:100%; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:60px 20px 40px; }
        .page-attachment-area { flex:1; width:100%; max-width:500px; background:#eef1f5; border:2px dashed #ccc; display:flex; align-items:center; justify-content:center; border-radius:12px; overflow:hidden; }
        .page-attachment-area img { width:100%; height:100%; object-fit:contain; }
        .password-modal-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:3000; display:none; align-items:center; justify-content:center; }
        .password-modal-overlay.open { display:flex; }
        .password-modal { background:#fff; border-radius:16px; padding:30px 24px; width:320px; text-align:center; }
        .toast { position:fixed; bottom:30px; left:50%; transform:translateX(-50%); background:#2c3e50; color:#fff; padding:12px 28px; border-radius:30px; z-index:2000; opacity:0; pointer-events:none; transition:opacity 0.4s; }
        .toast.show { opacity:1; }
    </style>
</head>
<body>
    <nav class="top-nav">
        <!-- 修复点：点击事件正常调用 window.toggleClassMenu -->
        <button class="hamburger-btn" onclick="toggleClassMenu()"><span></span><span></span><span></span></button>
        <div class="title-text">檀雅书院 · <span>品德评定</span></div>
        <div class="role-toggle" id="roleToggle">
            <button id="btnStudentMode" class="active" onclick="switchRole('student')">🧑‍🎓 学生端</button>
            <button id="btnTeacherMode" onclick="switchRole('teacher')">👩‍🏫 教师端</button>
        </div>
        <button class="btn-icon" onclick="exportAllData()">📤</button>
        <button class="btn-icon" onclick="document.getElementById('importFileInput').click()">📥</button>
        <input type="file" id="importFileInput" accept=".json" hidden onchange="importAllData(event)">
    </nav>
    <div class="class-menu-overlay" id="classMenuOverlay" onclick="closeClassMenu(event)">
        <div class="class-menu-panel" id="classMenuPanel"></div>
    </div>
    <div class="main-container" id="mainContainer"></div>
    <div class="flip-view" id="flipView">
        <div class="flip-top-bar">
            <span id="flipTitle" style="font-weight:700;"></span>
            <div><button onclick="toggleToc()">☰</button><button onclick="closeFlipView()">✕</button></div>
            <div id="tocDropdown" style="position:absolute;top:50px;right:18px;background:#fff;border-radius:10px;display:none;z-index:20;padding:8px 0;min-width:200px;box-shadow:0 12px 40px rgba(0,0,0,0.15);"></div>
        </div>
        <div class="flip-slider"><div class="flip-track" id="flipTrack"></div></div>
        <button class="flip-nav-arrow up" id="flipArrowUp" onclick="flipPage(-1)" disabled>▲</button>
        <button class="flip-nav-arrow down" id="flipArrowDown" onclick="flipPage(1)" disabled>▼</button>
    </div>
    <div class="password-modal-overlay" id="passwordModalOverlay">
        <div class="password-modal">
            <h3>🔒 教师端验证</h3>
            <input type="password" id="passwordInput" placeholder="输入密码">
            <div id="passwordError" style="color:red;display:none;">密码错误</div>
            <button onclick="closePasswordModal()">取消</button>
            <button onclick="verifyPassword()" style="background:var(--primary);color:#fff;">确认</button>
        </div>
    </div>
    <div class="toast" id="toast"></div>

    <script>
        // === 配置区 ===
        const SUPABASE_URL = 'https://xbaztpbiedtwopvxzeczg.supabase.co';
        const SUPABASE_ANON_KEY = 'sb_publishable_CcehX-9SXNnrvccI6Mt1ag_Hg8Jwoh_';
        
        // 修复点 1：使用 supabaseClient 避免与全局变量冲突
        const supabaseClient = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

        const CLASS_DATA = [ /* 班级数据保持不变 */ ];
        const ASSESSMENT_STRUCTURE = [ /* 评分结构保持不变 */ ];
        const PASSWORD = '3105105';
        
        let role = 'student', classId = CLASS_DATA[0].id, student = null;
        let flipCat = null, flipIdx = 0, flipTotal = 0;
        let allStudentData = {};

        // === 核心函数 (修复点 2：挂载到 window 解决 onclick 未定义) ===
        
        // 1. 菜单控制
        window.toggleClassMenu = function() {
            const overlay = document.getElementById('classMenuOverlay');
            overlay.classList.toggle('open');
            if (overlay.classList.contains('open')) {
                const panel = document.getElementById('classMenuPanel');
                panel.innerHTML = CLASS_DATA.map(cls => 
                    `<div class="class-menu-item ${cls.id === classId ? 'current' : ''}" onclick="switchClass('${cls.id}')">📌 ${cls.name}</div>`
                ).join('');
            }
        };

        window.closeClassMenu = function(e) {
            if (e.target === document.getElementById('classMenuOverlay')) {
                document.getElementById('classMenuOverlay').classList.remove('open');
            }
        };

        window.switchClass = function(id) {
            classId = id;
            document.getElementById('classMenuOverlay').classList.remove('open');
            student = null;
            closeFlip();
            renderClass();
        };

        // 2. 角色与权限
        window.switchRole = function(r) {
            if (r === 'teacher' && role !== 'teacher' && !window._auth) {
                document.getElementById('passwordModalOverlay').classList.add('open');
                return;
            }
            role = r;
            window._auth = false;
            document.getElementById('btnStudentMode').classList.toggle('active', r === 'student');
            document.getElementById('btnTeacherMode').classList.toggle('active', r === 'teacher');
            student = null;
            closeFlip();
            renderClass();
        };

        window.verifyPassword = function() {
            if (document.getElementById('passwordInput').value === PASSWORD) {
                document.getElementById('passwordModalOverlay').classList.remove('open');
                window._auth = true;
                switchRole('teacher');
            } else {
                document.getElementById('passwordError').style.display = 'block';
            }
        };

        window.closePasswordModal = function() {
            document.getElementById('passwordModalOverlay').classList.remove('open');
        };

        // 3. 数据加载与保存 (使用 supabaseClient)
        async function loadAllData() {
            try {
                // 修复点：使用 supabaseClient
                const { data, error } = await supabaseClient
                    .from('assessments')
                    .select('*');
                
                if (error) throw error;

                const result = {};
                data.forEach(row => {
                    if (!result[row.student_name]) result[row.student_name] = {};
                    if (!result[row.student_name][row.cat_id]) result[row.student_name][row.cat_id] = {};
                    result[row.student_name][row.cat_id][row.sub_id] = { 
                        score: row.score, 
                        att: row.attachment, 
                        attName: row.attachment_name 
                    };
                    if (row.submitted) result[row.student_name].submitted = true;
                });
                allStudentData = result;
                return result;
            } catch (err) {
                console.error('加载数据失败:', err);
                return {};
            }
        }

        async function saveStudentData(name, catId, subId, score, att, attName, submitted) {
            const { error } = await supabaseClient
                .from('assessments')
                .upsert({
                    student_name: name,
                    cat_id: catId,
                    sub_id: subId,
                    score,
                    attachment: att,
                    attachment_name: attName,
                    submitted
                }, { onConflict: ['student_name', 'cat_id', 'sub_id'] });
            
            if (error) throw error;
        }

        // ... (其余函数如 toast, compress, renderClass, openStudent 等保持不变，但内部调用 save/load 需确保使用 supabaseClient)

        // 4. 初始化
        async function init() {
            await loadAllData();
            renderClass();
        }
        init();
    </script>
</body>
</html>
