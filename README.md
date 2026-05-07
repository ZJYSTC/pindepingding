<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>檀雅书院学生品德评定系统</title>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.min.js">
    </script>
    <style>
        :root {
            --primary: #1a5276;
            --accent: #e67e22;
            --bg: #f5f6fa;
            --text: #2c3e50;
            --border: #dce3ea;
            --radius: 16px;
            --unsubmitted-red: #d63031;
        }
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
            background: var(--bg);
            color: var(--text);
            height: 100vh;
            overflow: hidden;
        }
        .top-nav {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(16px);
            border-bottom: 1px solid var(--border);
            padding: 12px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }
        .hamburger-btn {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            border: 1px solid var(--border);
            background: #fff;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 4px;
            padding: 8px;
        }
        .hamburger-btn span {
            display: block;
            width: 20px;
            height: 2px;
            background: var(--text);
            border-radius: 2px;
        }
        .title-text {
            font-size: 1.15rem;
            font-weight: 700;
            color: var(--primary);
        }
        .title-text span {
            color: var(--accent);
        }
        .role-toggle {
            display: flex;
            background: #eef1f5;
            border-radius: 30px;
            padding: 3px;
            gap: 2px;
        }
        .role-toggle button {
            padding: 9px 18px;
            border-radius: 28px;
            border: none;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: 600;
            background: transparent;
            color: #666;
        }
        .role-toggle button.active {
            background: #fff;
            color: var(--primary);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }
        .btn-icon {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            border: 1px solid var(--border);
            background: #fff;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.1rem;
        }
        .class-menu-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.2);
            z-index: 1100;
            display: none;
        }
        .class-menu-overlay.open {
            display: flex;
        }
        .class-menu-panel {
            margin-top: 70px;
            margin-left: 20px;
            background: #fff;
            border-radius: var(--radius);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
            min-width: 220px;
            max-height: 70vh;
            overflow-y: auto;
            padding: 8px 0;
        }
        .main-container {
            padding-top: 70px;
            height: 100vh;
            overflow-y: auto;
        }
        .student-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
            gap: 12px;
        }
        .student-card {
            background: #fff;
            border-radius: 10px;
            padding: 18px 16px;
            text-align: center;
            cursor: pointer;
            border: 2px solid #e0e4e9;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }
        .student-card.unsubmitted {
            border-color: var(--unsubmitted-red);
            background: #fff5f5;
        }
        .student-card .avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(135deg, #e8f0fe, #d4e4fc);
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.4rem;
            color: var(--primary);
            font-weight: 700;
        }
        .badge {
            display: inline-block;
            font-size: 0.7rem;
            padding: 3px 10px;
            border-radius: 12px;
            margin-top: 6px;
        }
        .badge-submitted {
            background: #e8f8ef;
            color: #27ae60;
        }
        .badge-pending {
            background: #ffebee;
            color: var(--unsubmitted-red);
        }
        .form-view {
            max-width: 750px;
            margin: 0 auto;
            padding: 20px 20px 100px;
        }
        .category-section {
            background: #fff;
            border-radius: var(--radius);
            margin-bottom: 16px;
            overflow: hidden;
            border: 1px solid var(--border);
        }
        .category-header {
            padding: 16px 20px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            background: #fafbfc;
            font-weight: 700;
        }
        .category-section.open .category-header {
            border-bottom: 1px solid var(--border);
        }
        .category-body {
            display: none;
            padding: 8px 0;
        }
        .category-section.open .category-body {
            display: block;
        }
        .sub-item {
            padding: 14px 20px;
            border-bottom: 1px solid #f0f2f5;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 12px;
        }
        .sub-item:last-child {
            border-bottom: none;
        }
        .score-input-group {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .score-input-group input {
            width: 70px;
            padding: 10px 8px;
            border: 2px solid var(--border);
            border-radius: 8px;
            text-align: center;
            font-size: 1rem;
            font-weight: 600;
            font-family: monospace;
        }
        .upload-btn {
            padding: 8px 14px;
            border-radius: 20px;
            border: 1.5px dashed var(--border);
            background: #fff;
            cursor: pointer;
            font-size: 0.8rem;
            display: inline-flex;
            align-items: center;
            gap: 5px;
            color: #666;
        }
        .upload-btn.has-file {
            border-style: solid;
            border-color: #27ae60;
            color: #27ae60;
            background: #f0faf4;
        }
        .btn-submit {
            display: inline-block;
            min-width: 180px;
            padding: 14px 30px;
            border-radius: 30px;
            border: none;
            background: var(--primary);
            color: #fff;
            font-size: 1.05rem;
            font-weight: 700;
            cursor: pointer;
            margin: 10px;
        }
        .btn-export-pdf {
            background: #555;
        }
        .flip-view {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 900;
            background: var(--bg);
            display: none;
            flex-direction: column;
        }
        .flip-view.active {
            display: flex;
        }
        .flip-top-bar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 14px 18px;
            background: rgba(255, 255, 255, 0.8);
        }
        .flip-slider {
            flex: 1;
            overflow: hidden;
        }
        .flip-track {
            display: flex;
            flex-direction: column;
            height: 100%;
            transition: transform 0.4s;
        }
        .flip-page {
            flex: 0 0 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 60px 20px 40px;
        }
        .page-attachment-area {
            flex: 1;
            width: 100%;
            max-width: 500px;
            background: #eef1f5;
            border: 2px dashed #ccc;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 12px;
            overflow: hidden;
        }
        .page-attachment-area img {
            width: 100%;
            height: 100%;
            object-fit: contain;
        }
        .password-modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 3000;
            display: none;
            align-items: center;
            justify-content: center;
        }
        .password-modal-overlay.open {
            display: flex;
        }
        .password-modal {
            background: #fff;
            border-radius: 16px;
            padding: 30px 24px;
            width: 320px;
            text-align: center;
        }
        .toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: #2c3e50;
            color: #fff;
            padding: 12px 28px;
            border-radius: 30px;
            z-index: 2000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s;
        }
        .toast.show {
            opacity: 1;
        }
    </style>
</head>
<body>
    <nav class="top-nav">
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
        // ==================== Supabase 配置 ====================
        const SUPABASE_URL = 'https://xbaztpbiedtwopvxzeczg.supabase.co';
        const SUPABASE_ANON_KEY = 'sb_publishable_CcehX-9SXNnrvccI6Mt1ag_Hg8Jwoh_';
        const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

        const CLASS_DATA = [
            { id: 'A1-1', name: 'A1栋1班', students: ['赵玉芳', '郎雯', '唐佳佳', '钱宇晨', '代畅远', '黎儒香', '程菲', '谭亚青', '何广晴', '胡悦', '李慧源', '赖玉婷', '林岑', '寇正清', '林小甜', '蒲云欣', '李韵韵', '陈关珊', '黄林婷', '韦佳', '陈彦西', '甘晓雪', '吴靖怡', '张愉琴', '唐秋彤', '王思琪', '唐林洁', '苏心雨', '唐诗雅', '孙荣璟'] },
            { id: 'A1-2', name: 'A1栋2班', students: ['谈梓妍', '王乐', '谢柔音', '熊以琳', '席晓春', '徐迦南', '薛惠文', '梁耀文', '李赛文', '李一鸣', '王克英', '顿成慧', '李梦蕾', '黄淑君', '管欣蓉', '邱湘', '武婧', '张霄佳', '杨红', '苏云峰', '邓国芳', '赵益涵', '李德巧', '丛嘉乐', '陈艳艳', '卓恒忆'] },
            { id: 'A1-3', name: 'A1栋3班', students: ['黄梦盈', '黄梦珂', '江美曦', '李好', '柳小英', '李文蕊', '林位卿', '刘含笑', '王慧莹', '孙娇', '李奕凝', '牟善胤', '吴亮颖', '王江宁', '张梦薇', '叶朝璇', '张晖', '祝海丽', '范为琴', '孙言语', '王雪田', '陈颖彤'] },
            { id: 'A1-4', name: 'A1栋4班', students: ['王艺', '蔡玟静', '李晗', '李春燕', '李佳佳', '李茹萍', '胥梦洋', '张睿', '刘琦', '魏延琴', '李嘉玟', '白永霞', '杨梅', '李小平', '苏婷', '刘秀龙'] },
            { id: 'A1-5', name: 'A1栋5班', students: ['张婷婷', '杨璨华', '邓文迪', '青雨虹', '庞宇蕾', '巫春梅', '朱焕丽', '慈小艳', '林雨晴', '李漪琳', '李晗', '张琳碧', '田梦', '张汉英', '吴江兰', '杨玉靖', '李玥琪', '马艺艳'] },
            { id: 'A1-6', name: 'A1栋6班', students: ['冯佳茹', '符秋暖', '梁苑叶', '梁昕', '许宇昕', '谷淑娴', '刘晓辰', '李路瑶', '李彦咏雪', '李焮焮', '李亚茹', '庞倩妹', '王琴雅', '王二欣', '常小雨', '谢欢', '杨欢', '梁毕毕', '张佳慧', '余姿林', '赵梦真', '王亚妮', '陈卫', '陈怡', '方如玉', '刘冰蕾', '农艳婷', '唐海玲', '唐统叶', '吴增莉'] },
            { id: 'A1-7', name: 'A1栋7班', students: ['马思雅', '范紫真', '张培兰', '农可懿', '程昶', '康晓玲', '杨琳'] },
            { id: 'youth', name: '青年奋进班', students: ['陈阳', '杜昭怡', '冯文硕', '吉如昔', '李嘉嘉', '梁涛', '谢汶瑾', '张佳蕴', '赵健馨', '杨静雯', '陈绵秀', '王艳', '李诗桐', '庄家秀', '周娴红', '张婧妍', '林佳棋', '洪莹晶', '林茵茵', '林康姿', '王盈英', '刘馨', '王钰', '张涵', '林上米', '周冠妃', '王静', '杨琦', '王小清', '周思睿', '粟婷', '王希月', '张子倩', '王雅欣', '蔡子轩', '林声旭', '李政琦', '李炎', '林佳莹', '梁正妍', '李鑫乐', '李子涵', '王杰', '马鑫彤', '翟釨嫣'] }
        ];
        const ASSESSMENT_STRUCTURE = [
            { id: 'cat1', title: '思想政治表现', total: 25, items: [
                { id: 'sub1_1', name: '政治理论学习', max: 7, desc: '学习强国、青年大学习等，无故不参加扣2分', def: 0 },
                { id: 'sub1_2', name: '参与组织活动', max: 6, desc: '团日等活动，无故不参加扣2分', def: 0 },
                { id: 'sub1_3', name: '明辨大是大非', max: 6, desc: '抵制不良思想，违反扣3分', def: 0 },
                { id: 'sub1_4', name: '自我教育管理', max: 6, desc: '学生干部履职加2-6分', def: 0 }
            ]},
            { id: 'cat2', title: '道德品质表现', total: 25, items: [
                { id: 'sub2_1', name: '心怀国之大者', max: 5, desc: '服务国家战略加2分/次', def: 0 },
                { id: 'sub2_2', name: '遵守社会公德', max: 5, desc: '文明上网等，违反扣2分', def: 0 },
                { id: 'sub2_3', name: '遵守学术道德', max: 5, desc: '学术不端扣2分', def: 0 },
                { id: 'sub2_4', name: '涵育个人品德', max: 5, desc: '尊敬师长等，违反扣2分', def: 0 },
                { id: 'sub2_5', name: '其他不良行为', max: 5, desc: '弄虚作假扣2-5分', def: 0 }
            ]},
            { id: 'cat3', title: '遵纪守法表现', total: 25, items: [
                { id: 'sub3_1', name: '遵守法律法规', max: 10, desc: '受处分扣分', def: 0 },
                { id: 'sub3_2', name: '服从管理', max: 10, desc: '不服从扣1分/次', def: 0 },
                { id: 'sub3_3', name: '课堂行为', max: 5, desc: '违纪扣分', def: 0 }
            ]},
            { id: 'cat4', title: '劳动服务表现', total: 25, items: [
                { id: 'sub4_1', name: '社区劳动', max: 8, desc: '宿舍卫生等，可负分', def: 0, neg: true },
                { id: 'sub4_2', name: '志愿服务', max: 9, desc: '志愿活动加2分/次', def: 0 },
                { id: 'sub4_3', name: '科技文体活动', max: 8, desc: '参加活动加2-5分', def: 0 }
            ]}
        ];
        const PASSWORD = '3105105';
        let role = 'student', classId = CLASS_DATA[0].id, student = null;
        let flipCat = null, flipIdx = 0, flipTotal = 0;
        let allStudentData = {};

        async function loadAllData() {
            const { data, error } = await supabase.from('assessments').select('*');
            if (error) { console.error(error); return {}; }
            const result = {};
            data.forEach(row => {
                if (!result[row.student_name]) result[row.student_name] = {};
                if (!result[row.student_name][row.cat_id]) result[row.student_name][row.cat_id] = {};
                result[row.student_name][row.cat_id][row.sub_id] = {
                    score: row.score, att: row.attachment, attName: row.attachment_name
                };
                if (row.submitted) result[row.student_name].submitted = true;
            });
            allStudentData = result;
            return result;
        }

        async function saveStudentData(name, catId, subId, score, att, attName, submitted) {
            const { error } = await supabase.from('assessments').upsert({
                student_name: name, cat_id: catId, sub_id: subId,
                score, attachment: att, attachment_name: attName, submitted
            }, { onConflict: 'student_name, cat_id, sub_id' });
            if (error) throw error;
        }

        async function updateSubmissionStatus(name, submitted) {
            await supabase.from('assessments').update({ submitted }).eq('student_name', name);
        }

        function toast(msg, ok = true) {
            const t = document.getElementById('toast');
            t.textContent = msg; t.className = 'toast ' + (ok ? 'success' : '');
            t.classList.add('show'); clearTimeout(t._t); t._t = setTimeout(() => t.classList.remove('show'), 2000);
        }

        async function compress(file) {
            if (file.size > 3 * 1024 * 1024) {
                return new Promise(resolve => {
                    const reader = new FileReader();
                    reader.onload = e => {
                        const img = new Image();
                        img.onload = () => {
                            const c = document.createElement('canvas');
                            let w = img.width, h = img.height;
                            if (w > 1200 || h > 1200) { if (w > h) { h *= 1200 / w; w = 1200; } else { w *= 1200 / h; h = 1200; } }
                            c.width = w; c.height = h;
                            c.getContext('2d').drawImage(img, 0, 0, w, h);
                            resolve(c.toDataURL('image/jpeg', 0.7));
                        };
                        img.src = e.target.result;
                    };
                    reader.readAsDataURL(file);
                });
            } else {
                return new Promise(resolve => {
                    const reader = new FileReader();
                    reader.onload = e => resolve(e.target.result);
                    reader.readAsDataURL(file);
                });
            }
        }

        // 将核心函数绑定到 window 对象
        window.toggleClassMenu = () => {
            const o = document.getElementById('classMenuOverlay');
            o.classList.toggle('open');
            if (o.classList.contains('open')) {
                document.getElementById('classMenuPanel').innerHTML = CLASS_DATA.map(c => `
                    <div class="class-menu-item ${c.id === classId ? 'current' : ''}" onclick="switchClass('${c.id}')">📌 ${c.name}</div>
                `).join('');
            }
        };
        window.closeClassMenu = e => { if (e.target === document.getElementById('classMenuOverlay')) document.getElementById('classMenuOverlay').classList.remove('open'); };
        window.switchClass = id => { classId = id; document.getElementById('classMenuOverlay').classList.remove('open'); student = null; closeFlip(); renderClass(); };
        window.switchRole = r => {
            if (r === 'teacher' && role !== 'teacher' && !window._auth) {
                document.getElementById('passwordModalOverlay').classList.add('open');
                return;
            }
            role = r; window._auth = false;
            document.getElementById('btnStudentMode').classList.toggle('active', r === 'student');
            document.getElementById('btnTeacherMode').classList.toggle('active', r === 'teacher');
            student = null; closeFlip(); renderClass();
        };
        window.verifyPassword = () => {
            if (document.getElementById('passwordInput').value === PASSWORD) {
                document.getElementById('passwordModalOverlay').classList.remove('open');
                window._auth = true; switchRole('teacher');
            } else document.getElementById('passwordError').style.display = 'block';
        };
        window.closePasswordModal = () => document.getElementById('passwordModalOverlay').classList.remove('open');

        async function renderClass() {
            await loadAllData();
            const cls = CLASS_DATA.find(c => c.id === classId);
            document.getElementById('mainContainer').innerHTML = `
            <div style="max-width:700px;margin:0 auto;padding:20px;">
                <h2>${role === 'student' ? '📋 选择名字' : '📋 ' + cls.name}</h2>
                <p style="margin-bottom:20px;">${cls.name} · ${cls.students.length}人</p>
                <div class="student-grid">
                    ${cls.students.map(n => {
                        const sub = allStudentData[n]?.submitted;
                        return `<div class="student-card ${sub ? '' : 'unsubmitted'}" onclick="openStudent('${n}')">
                            <div class="avatar">${n[0]}</div><div style="font-weight:600;">${n}</div>
                            <span class="badge ${sub ? 'badge-submitted' : 'badge-pending'}">${sub ? '已提交' : '未提交'}</span>
                        </div>`;
                    }).join('')}
                </div>
            </div>`;
        }

        window.openStudent = name => {
            student = name;
            if (role === 'student') renderForm(name);
            else {
                const d = allStudentData[name];
                if (!d || !d.submitted) document.getElementById('mainContainer').innerHTML = `<div style="max-width:700px;margin:40px auto;"><button onclick="goBack()">← 返回</button><h2>${name}</h2><p style="color:red;">该生尚未提交</p></div>`;
                else renderTeacher(name);
            }
        };

        function renderForm(name) {
            const rec = allStudentData[name] || {};
            document.getElementById('mainContainer').innerHTML = `
            <div class="form-view">
                <button onclick="goBack()">← 返回</button>
                <h2>📝 ${name}</h2>
                <p>班级：${CLASS_DATA.find(c => c.id === classId).name}</p>
                ${ASSESSMENT_STRUCTURE.map(c => `
                    <div class="category-section" id="form-cat-${c.id}">
                        <div class="category-header" onclick="toggleCat('${c.id}')">${c.title}（满分${c.total}）</div>
                        <div class="category-body">
                            ${c.items.map(s => {
                                const val = rec[c.id]?.[s.id]?.score ?? s.def;
                                const att = rec[c.id]?.[s.id]?.att;
                                const neg = s.neg === true;
                                return `<div class="sub-item">
                                    <div><b>${s.name}</b><br><small>${s.desc}</small></div>
                                    <div class="score-input-group">
                                        <input type="number" id="score-${s.id}" value="${val}" ${neg ? '' : 'min="0"'} max="${s.max}" step="0.5"
                                            onchange="onScore('${name}','${c.id}','${s.id}',this.value,${s.max},${neg})">
                                        <span>/ ${s.max}分 ${neg ? '(可负)' : ''}</span>
                                    </div>
                                    <button class="upload-btn ${att ? 'has-file' : ''}" onclick="triggerUpload('${s.id}')">
                                        ${att ? '📎已上传' : '📎上传附件'}
                                        ${att ? `<span onclick="event.stopPropagation(); delAtt('${name}','${c.id}','${s.id}')"> ×</span>` : ''}
                                    </button>
                                    <input type="file" id="file-${s.id}" accept="image/*" style="position:absolute;opacity:0;width:0;height:0;" onchange="handleUpload('${name}','${c.id}','${s.id}',this)">
                                    ${att ? `<img src="${att}" style="width:40px;height:40px;border-radius:6px;cursor:pointer;" onclick="event.stopPropagation(); delAtt('${name}','${c.id}','${s.id}')">` : ''}
                                </div>`;
                            }).join('')}
                        </div>
                    </div>
                `).join('')}
                <div style="text-align:center;margin-top:20px;">
                    <button class="btn-submit" onclick="submitForm('${name}')">✅ 保存并提交</button>
                    <button class="btn-submit btn-export-pdf" onclick="exportPDF('${name}')">📄 导出PDF</button>
                </div>
            </div>`;
            setTimeout(() => document.getElementById('form-cat-' + ASSESSMENT_STRUCTURE[0].id)?.classList.add('open'), 100);
        }

        window.toggleCat = id => document.getElementById('form-cat-' + id)?.classList.toggle('open');
        window.onScore = (n, cid, sid, v, max, neg) => {
            let val = parseFloat(v); if (isNaN(val)) val = 0;
            if (!neg && val < 0) val = 0; if (val > max) val = max;
            if (!allStudentData[n]) allStudentData[n] = {};
            if (!allStudentData[n][cid]) allStudentData[n][cid] = {};
            if (!allStudentData[n][cid][sid]) allStudentData[n][cid][sid] = {};
            allStudentData[n][cid][sid].score = val;
        };
        window.triggerUpload = (subId) => {
            const input = document.getElementById('file-' + subId);
            if (input) input.click();
        };
        window.handleUpload = async (n, cid, sid, input) => {
            const f = input.files[0]; if (!f) return;
            if (!f.type.startsWith('image/')) { toast('请上传图片'); return; }
            const b64 = await compress(f);
            if (!allStudentData[n]) allStudentData[n] = {};
            if (!allStudentData[n][cid]) allStudentData[n][cid] = {};
            if (!allStudentData[n][cid][sid]) allStudentData[n][cid][sid] = {};
            allStudentData[n][cid][sid].att = b64;
            allStudentData[n][cid][sid].attName = f.name;
            toast('附件已上传');
            renderForm(n);
        };
        window.delAtt = (n, cid, sid) => {
            if (confirm('删除附件？')) {
                if (allStudentData[n] && allStudentData[n][cid] && allStudentData[n][cid][sid]) {
                    allStudentData[n][cid][sid].att = null;
                    allStudentData[n][cid][sid].attName = null;
                }
                renderForm(n);
            }
        };

        async function submitForm(name) {
            try {
                const rec = allStudentData[name] || {};
                for (let cat of ASSESSMENT_STRUCTURE) {
                    for (let sub of cat.items) {
                        const detail = rec[cat.id]?.[sub.id] || {};
                        await saveStudentData(name, cat.id, sub.id, detail.score ?? sub.def, detail.att || null, detail.attName || null, true);
                    }
                }
                await updateSubmissionStatus(name, true);
                allStudentData[name].submitted = true;
                toast('提交成功！');
                setTimeout(() => goBack(), 1000);
            } catch (e) {
                toast('提交失败，请检查网络', false);
            }
        }

        window.goBack = () => { student = null; closeFlip(); renderClass(); };

        function renderTeacher(name) {
            const data = allStudentData[name];
            document.getElementById('mainContainer').innerHTML = `
            <div style="max-width:700px;margin:20px auto;">
                <button onclick="goBack()">← 返回</button>
                <h2>📊 ${name}</h2>
                ${ASSESSMENT_STRUCTURE.map(c => {
                    let sum = 0; c.items.forEach(s => sum += (data[c.id]?.[s.id]?.score || 0));
                    return `<div class="category-select-card" onclick="openFlip('${name}','${c.id}')" style="background:#fff;padding:18px;margin-bottom:12px;border-radius:10px;cursor:pointer;box-shadow:0 2px 8px rgba(0,0,0,0.05);">
                        <div><b>${c.title}</b><br><small>${sum}/${c.total}分 · ${c.items.length}项</small></div><span>→</span>
                    </div>`;
                }).join('')}
            </div>`;
        }

        window.openFlip = (name, catId) => {
            const data = allStudentData[name];
            if (!data?.submitted) return;
            const cat = ASSESSMENT_STRUCTURE.find(c => c.id === catId);
            flipCat = catId; flipIdx = 0; flipTotal = cat.items.length;
            document.getElementById('flipTitle').textContent = cat.title;
            document.getElementById('flipView').classList.add('active');
            document.getElementById('flipTrack').innerHTML = cat.items.map((s, i) => {
                const d = data[catId]?.[s.id];
                return `<div class="flip-page">
                    <div class="page-attachment-area">${d?.att ? `<img src="${d.att}">` : '<div>暂无附件</div>'}</div>
                    <div style="margin-top:20px;font-weight:700;">${s.name} ： ${d?.score ?? 0}/${s.max}分</div>
                    <div style="margin-top:10px;color:#999;">${i + 1}/${flipTotal}</div>
                </div>`;
            }).join('');
            updateFlip(0);
            document.getElementById('flipArrowUp').disabled = true;
            document.getElementById('flipArrowDown').disabled = flipTotal <= 1;
            document.addEventListener('keydown', keyFlip);
        };

        function updateFlip(dur = 0) {
            const t = document.getElementById('flipTrack');
            t.style.transition = dur ? `transform ${dur}ms` : 'none';
            t.style.transform = `translateY(-${flipIdx * 100}%)`;
            document.getElementById('flipArrowUp').disabled = flipIdx <= 0;
            document.getElementById('flipArrowDown').disabled = flipIdx >= flipTotal - 1;
        }
        window.flipPage = d => { const n = flipIdx + d; if (n < 0 || n >= flipTotal) return; flipIdx = n; updateFlip(300); };
        function keyFlip(e) {
            if (!document.getElementById('flipView').classList.contains('active')) return;
            if (e.key === 'ArrowDown' || e.key === 'PageDown') { e.preventDefault(); flipPage(1); }
            if (e.key === 'ArrowUp' || e.key === 'PageUp') { e.preventDefault(); flipPage(-1); }
            if (e.key === 'Escape') closeFlipView();
        }
        window.closeFlipView = () => { document.getElementById('flipView').classList.remove('active'); document.removeEventListener('keydown', keyFlip); };
        window.toggleToc = () => {
            const d = document.getElementById('tocDropdown');
            d.style.display = d.style.display === 'block' ? 'none' : 'block';
            if (d.style.display === 'block') {
                const cat = ASSESSMENT_STRUCTURE.find(c => c.id === flipCat);
                if (cat) d.innerHTML = cat.items.map((s, i) => `<div style="padding:11px 18px;cursor:pointer;${i === flipIdx ? 'background:#eef4fb' : ''}" onclick="jumpFlip(${i})">${i + 1}. ${s.name}</div>`).join('');
            }
        };
        window.jumpFlip = i => { flipIdx = i; updateFlip(300); document.getElementById('tocDropdown').style.display = 'none'; };
        function closeFlip() { document.getElementById('flipView').classList.remove('active'); }

        window.exportPDF = name => {
            const data = allStudentData[name];
            if (!data?.submitted) { toast('请先提交'); return; }
            let html = `<html><head><meta charset="UTF-8"><style>body{font-family:SimSun,sans-serif;padding:20px;}h1{text-align:center;}h2{background:#eef;padding:6px;}.item{margin:10px 0;border-left:3px solid #333;padding-left:12px;}img{max-width:100%;max-height:400px;}</style></head><body><h1>${name} 品德评定报告</h1><hr>`;
            ASSESSMENT_STRUCTURE.forEach(c => {
                html += `<h2>${c.title}（满分${c.total}分）</h2>`;
                c.items.forEach(s => {
                    const d = data[c.id]?.[s.id];
                    html += `<div class="item"><h3>${s.name} —— ${d?.score ?? 0}/${s.max}分</h3><p>${s.desc}</p>`;
                    if (d?.att) html += `<img src="${d.att}">`;
                    html += `</div>`;
                });
            });
            html += `</body></html>`;
            const w = window.open('', '_blank', 'width=800,height=600');
            w.document.write(html); w.document.close();
            setTimeout(() => { w.print(); toast('请选择“另存为PDF”'); }, 800);
        };

        window.exportAllData = async () => {
            await loadAllData();
            const blob = new Blob([JSON.stringify(allStudentData)], { type: 'application/json' });
            const a = document.createElement('a'); a.href = URL.createObjectURL(blob);
            a.download = '檀雅书院品德评定.json'; a.click(); toast('已导出');
        };
        window.importAllData = async e => {
            const f = e.target.files[0]; if (!f) return;
            const reader = new FileReader();
            reader.onload = async ev => {
                try {
                    const json = JSON.parse(ev.target.result);
                    for (let name in json) {
                        const rec = json[name];
                        if (rec.submitted !== undefined) {
                            for (let cat of ASSESSMENT_STRUCTURE) {
                                for (let sub of cat.items) {
                                    const detail = rec[cat.id]?.[sub.id] || {};
                                    await saveStudentData(name, cat.id, sub.id, detail.score ?? sub.def, detail.att || null, detail.attName || null, rec.submitted);
                                }
                            }
                        }
                    }
                    await loadAllData();
                    renderClass();
                    toast('导入成功');
                } catch (ex) { toast('文件错误'); }
            };
            reader.readAsText(f);
        };

        async function init() {
            await loadAllData();
            renderClass();
        }
        init();
    </script>
</body>
</html>
