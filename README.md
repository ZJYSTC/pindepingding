<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
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
            flex-shrink: 0;
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
            white-space: nowrap;
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
            flex-shrink: 0;
        }
        .role-toggle button {
            padding: 9px 18px;
            border-radius: 28px;
            border: none;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: 600;
            background: transparent;
            color: var(--text-light);
            white-space: nowrap;
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
            transition: 0.3s;
            flex-shrink: 0;
        }
        .btn-icon:hover {
            background: #f0f3f7;
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
            align-items: flex-start;
            justify-content: flex-start;
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
            border: 1px solid var(--border);
        }
        .class-menu-item {
            padding: 12px 20px;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: 500;
            transition: 0.3s;
            border-left: 3px solid transparent;
            color: var(--text);
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .class-menu-item:hover {
            background: #f5f7fa;
            border-left-color: #2980b9;
            color: var(--primary);
        }
        .class-menu-item.current {
            background: #eef4fb;
            border-left-color: var(--primary);
            font-weight: 700;
            color: var(--primary);
        }
        .main-container {
            padding-top: 70px;
            height: 100vh;
            overflow-y: auto;
            overflow-x: hidden;
        }
        .class-view {
            max-width: 700px;
            margin: 0 auto;
            padding: 20px;
        }
        .class-view h2 {
            font-size: 1.5rem;
            margin-bottom: 8px;
            color: var(--primary);
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
            transition: 0.3s;
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
            font-weight: 500;
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
            padding: 20px;
            padding-bottom: 100px;
        }
        .category-section {
            background: #fff;
            border-radius: var(--radius);
            margin-bottom: 16px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid var(--border);
            overflow: hidden;
        }
        .category-header {
            padding: 16px 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #fafbfc;
            font-weight: 700;
            font-size: 1rem;
            color: var(--primary);
            border-bottom: 1px solid transparent;
        }
        .category-section.open .category-header {
            border-bottom-color: var(--border);
            background: #f8f9fb;
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
        .item-info {
            flex: 1;
            min-width: 180px;
        }
        .score-input-group {
            display: flex;
            align-items: center;
            gap: 8px;
            flex-shrink: 0;
        }
        .score-input-group input {
            width: 70px;
            padding: 10px 8px;
            border: 2px solid var(--border);
            border-radius: 8px;
            text-align: center;
            font-size: 1rem;
            font-weight: 600;
            transition: 0.3s;
            background: #fff;
            color: var(--text);
            font-family: monospace;
        }
        .upload-btn {
            padding: 8px 14px;
            border-radius: 20px;
            border: 1.5px dashed var(--border);
            background: #fff;
            cursor: pointer;
            font-size: 0.8rem;
            transition: 0.3s;
            white-space: nowrap;
            display: flex;
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
            transition: 0.3s;
            margin: 10px;
            box-shadow: 0 6px 20px rgba(26, 82, 118, 0.3);
        }
        .btn-export-pdf {
            background: #555;
        }
        .btn-export-pdf:hover {
            background: #333;
        }
        .teacher-category-view {
            max-width: 700px;
            margin: 0 auto;
            padding: 20px;
        }
        .category-select-card {
            background: #fff;
            border-radius: 10px;
            padding: 18px 20px;
            margin-bottom: 12px;
            cursor: pointer;
            border: 2px solid transparent;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        .category-select-card:hover {
            border-color: #2980b9;
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
            backdrop-filter: blur(12px);
        }
        .flip-slider {
            flex: 1;
            overflow: hidden;
            position: relative;
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
        .page-score-display {
            margin-top: 20px;
            text-align: center;
            font-weight: 700;
            font-size: 1.2rem;
            color: var(--primary);
        }
        .flip-nav-arrow {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid #ccc;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.3rem;
            z-index: 5;
            transition: 0.3s;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        .flip-nav-arrow:disabled {
            opacity: 0.3;
            cursor: not-allowed;
        }
        .flip-nav-arrow.up {
            left: 20px;
        }
        .flip-nav-arrow.down {
            right: 20px;
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
            font-weight: 500;
        }
        .toast.show {
            opacity: 1;
        }
        .toast.success {
            background: #27ae60;
        }
        @media (max-width: 600px) {
            .sub-item {
                flex-direction: column;
                align-items: stretch;
            }
            .role-toggle button {
                padding: 7px 13px;
                font-size: 0.78rem;
            }
        }
    </style>
</head>
<body>
    <nav class="top-nav">
        <button class="hamburger-btn" onclick="toggleClassMenu()" title="班级目录">
            <span></span><span></span><span></span>
        </button>
        <div class="title-text">檀雅书院 · <span>品德评定</span></div>
        <div class="role-toggle" id="roleToggle">
            <button id="btnStudentMode" class="active" onclick="switchRole('student')">🧑‍🎓 学生端</button>
            <button id="btnTeacherMode" onclick="switchRole('teacher')">👩‍🏫 教师端</button>
        </div>
        <button class="btn-icon" onclick="exportAllData()" title="导出全部">📤</button>
        <button class="btn-icon" onclick="document.getElementById('importFileInput').click()" title="导入">📥</button>
        <input type="file" id="importFileInput" accept=".json" style="display:none" onchange="importAllData(event)">
    </nav>

    <div class="class-menu-overlay" id="classMenuOverlay" onclick="closeClassMenu(event)">
        <div class="class-menu-panel" id="classMenuPanel" onclick="event.stopPropagation()"></div>
    </div>

    <div class="main-container" id="mainContainer"></div>

    <div class="flip-view" id="flipView">
        <div class="flip-top-bar">
            <span id="flipTitle" style="font-weight:700;"></span>
            <div style="display:flex;gap:8px;">
                <button onclick="toggleToc()">☰</button>
                <button onclick="closeFlipView()">✕</button>
            </div>
            <div id="tocDropdown" style="position:absolute; top:50px; right:18px; background:#fff; box-shadow:0 12px 40px rgba(0,0,0,0.15); border-radius:10px; display:none; z-index:20; padding:8px 0; min-width:200px;"></div>
        </div>
        <div class="flip-slider">
            <div class="flip-track" id="flipTrack"></div>
        </div>
        <button class="flip-nav-arrow up" id="flipArrowUp" onclick="flipPage(-1)" disabled>▲</button>
        <button class="flip-nav-arrow down" id="flipArrowDown" onclick="flipPage(1)" disabled>▼</button>
    </div>

    <div class="password-modal-overlay" id="passwordModalOverlay">
        <div class="password-modal">
            <h3>🔒 教师端验证</h3>
            <input type="password" id="passwordInput" placeholder="请输入密码" onkeypress="if(event.key==='Enter')verifyPassword()">
            <div id="passwordError" style="color:red;display:none;">密码错误</div>
            <div style="margin-top:16px;">
                <button onclick="closePasswordModal()">取消</button>
                <button onclick="verifyPassword()" style="background:var(--primary);color:#fff;margin-left:10px;">确认</button>
            </div>
        </div>
    </div>

    <div class="toast" id="toast"></div>

    <script>
        (function() {
            // ==================== 数据 ====================
            const CLASS_DATA = [
                { id:'A1-1', name:'A1栋1班', students:['赵玉芳','郎雯','唐佳佳','钱宇晨','代畅远','黎儒香','程菲','谭亚青','何广晴','胡悦','李慧源','赖玉婷','林岑','寇正清','林小甜','蒲云欣','李韵韵','陈关珊','黄林婷','韦佳','陈彦西','甘晓雪','吴靖怡','张愉琴','唐秋彤','王思琪','唐林洁','苏心雨','唐诗雅','孙荣璟'] },
                { id:'A1-2', name:'A1栋2班', students:['谈梓妍','王乐','谢柔音','熊以琳','席晓春','徐迦南','薛惠文','梁耀文','李赛文','李一鸣','王克英','顿成慧','李梦蕾','黄淑君','管欣蓉','邱湘','武婧','张霄佳','杨红','苏云峰','邓国芳','赵益涵','李德巧','丛嘉乐','陈艳艳','卓恒忆'] },
                { id:'A1-3', name:'A1栋3班', students:['黄梦盈','黄梦珂','江美曦','李好','柳小英','李文蕊','林位卿','刘含笑','王慧莹','孙娇','李奕凝','牟善胤','吴亮颖','王江宁','张梦薇','叶朝璇','张晖','祝海丽','范为琴','孙言语','王雪田','陈颖彤'] },
                { id:'A1-4', name:'A1栋4班', students:['王艺','蔡玟静','李晗','李春燕','李佳佳','李茹萍','胥梦洋','张睿','刘琦','魏延琴','李嘉玟','白永霞','杨梅','李小平','苏婷','刘秀龙'] },
                { id:'A1-5', name:'A1栋5班', students:['张婷婷','杨璨华','邓文迪','青雨虹','庞宇蕾','巫春梅','朱焕丽','慈小艳','林雨晴','李漪琳','李晗','张琳碧','田梦','张汉英','吴江兰','杨玉靖','李玥琪','马艺艳'] },
                { id:'A1-6', name:'A1栋6班', students:['冯佳茹','符秋暖','梁苑叶','梁昕','许宇昕','谷淑娴','刘晓辰','李路瑶','李彦咏雪','李焮焮','李亚茹','庞倩妹','王琴雅','王二欣','常小雨','谢欢','杨欢','梁毕毕','张佳慧','余姿林','赵梦真','王亚妮','陈卫','陈怡','方如玉','刘冰蕾','农艳婷','唐海玲','唐统叶','吴增莉'] },
                { id:'A1-7', name:'A1栋7班', students:['马思雅','范紫真','张培兰','农可懿','程昶','康晓玲','杨琳'] },
                { id:'youth', name:'青年奋进班', students:['陈阳','杜昭怡','冯文硕','吉如昔','李嘉嘉','梁涛','谢汶瑾','张佳蕴','赵健馨','杨静雯','陈绵秀','王艳','李诗桐','庄家秀','周娴红','张婧妍','林佳棋','洪莹晶','林茵茵','林康姿','王盈英','刘馨','王钰','张涵','林上米','周冠妃','王静','杨琦','王小清','周思睿','粟婷','王希月','张子倩','王雅欣','蔡子轩','林声旭','李政琦','李炎','林佳莹','梁正妍','李鑫乐','李子涵','王杰','马鑫彤','翟釨嫣'] }
            ];
            const ASSESSMENT_STRUCTURE = [
                {
                    id: 'cat1', title: '思想政治表现', total: 25, items: [
                        { id: 'sub1_1', name: '政治理论学习', max: 7, desc: '学习强国、青年大学习等，无故不参加扣2分', def: 0 },
                        { id: 'sub1_2', name: '参与组织活动', max: 6, desc: '团日等活动，无故不参加扣2分', def: 0 },
                        { id: 'sub1_3', name: '明辨大是大非', max: 6, desc: '抵制不良思想，违反扣3分', def: 0 },
                        { id: 'sub1_4', name: '自我教育管理', max: 6, desc: '学生干部履职加2-6分', def: 0 }
                    ]
                },
                {
                    id: 'cat2', title: '道德品质表现', total: 25, items: [
                        { id: 'sub2_1', name: '心怀国之大者', max: 5, desc: '服务国家战略加2分/次', def: 0 },
                        { id: 'sub2_2', name: '遵守社会公德', max: 5, desc: '文明上网等，违反扣2分', def: 0 },
                        { id: 'sub2_3', name: '遵守学术道德', max: 5, desc: '学术不端扣2分', def: 0 },
                        { id: 'sub2_4', name: '涵育个人品德', max: 5, desc: '尊敬师长等，违反扣2分', def: 0 },
                        { id: 'sub2_5', name: '其他不良行为', max: 5, desc: '弄虚作假扣2-5分', def: 0 }
                    ]
                },
                {
                    id: 'cat3', title: '遵纪守法表现', total: 25, items: [
                        { id: 'sub3_1', name: '遵守法律法规', max: 10, desc: '受处分扣分', def: 0 },
                        { id: 'sub3_2', name: '服从管理', max: 10, desc: '不服从扣1分/次', def: 0 },
                        { id: 'sub3_3', name: '课堂行为', max: 5, desc: '违纪扣分', def: 0 }
                    ]
                },
                {
                    id: 'cat4', title: '劳动服务表现', total: 25, items: [
                        { id: 'sub4_1', name: '社区劳动', max: 8, desc: '宿舍卫生等，可负分', def: 0, neg: true },
                        { id: 'sub4_2', name: '志愿服务', max: 9, desc: '志愿活动加2分/次', def: 0 },
                        { id: 'sub4_3', name: '科技文体活动', max: 8, desc: '参加活动加2-5分', def: 0 }
                    ]
                }
            ];

            const PASSWORD = '3105105';
            const STORAGE_KEY = 'tanya_final_v1';

            let currentRole = 'student';
            let currentClassId = CLASS_DATA[0].id;
            let currentStudent = null;
            let flipCatId = null;
            let flipIdx = 0;
            let flipTotal = 0;

            // ==================== 存储操作 ====================
            function loadAllData() { try { return JSON.parse(localStorage.getItem(STORAGE_KEY)) || {}; } catch (e) { return {}; } }
            function saveAllData(d) { localStorage.setItem(STORAGE_KEY, JSON.stringify(d)); }

            function ensureRecord(name) {
                const all = loadAllData();
                if (!all[name]) {
                    const rec = { submitted: false };
                    ASSESSMENT_STRUCTURE.forEach(c => {
                        rec[c.id] = {};
                        c.items.forEach(s => {
                            rec[c.id][s.id] = { score: s.def, max: s.max, att: null, attName: null };
                        });
                    });
                    all[name] = rec;
                    saveAllData(all);
                }
                return all[name];
            }

            function getStudentData(name) { return loadAllData()[name] || null; }

            function isSubmitted(name) {
                const d = getStudentData(name);
                return d?.submitted === true;
            }

            function updateScore(name, catId, subId, score) {
                const all = loadAllData();
                if (!all[name]) ensureRecord(name);
                all[name][catId][subId].score = score;
                saveAllData(all);
            }

            function updateAttachment(name, catId, subId, base64, fname) {
                const all = loadAllData();
                if (!all[name]) ensureRecord(name);
                all[name][catId][subId].att = base64;
                all[name][catId][subId].attName = fname;
                saveAllData(all);
            }

            function removeAttachment(name, catId, subId) {
                const all = loadAllData();
                if (!all[name]) return;
                all[name][catId][subId].att = null;
                all[name][catId][subId].attName = null;
                saveAllData(all);
            }

            function submitStudent(name) {
                ensureRecord(name);
                const all = loadAllData();
                all[name].submitted = true;
                saveAllData(all);
            }

            // ==================== 工具函数 ====================
            function toast(msg, isSuccess = true) {
                const t = document.getElementById('toast');
                t.textContent = msg;
                t.className = 'toast ' + (isSuccess ? 'success' : '');
                t.classList.add('show');
                clearTimeout(t._t);
                t._t = setTimeout(() => t.classList.remove('show'), 2000);
            }

            function compressImage(file) {
                return new Promise((resolve) => {
                    if (file.size > 3 * 1024 * 1024) {
                        const reader = new FileReader();
                        reader.onload = e => {
                            const img = new Image();
                            img.onload = () => {
                                const canvas = document.createElement('canvas');
                                let w = img.width, h = img.height;
                                const maxDim = 1200;
                                if (w > maxDim || h > maxDim) {
                                    if (w > h) { h *= maxDim / w; w = maxDim; } else { w *= maxDim / h; h = maxDim; }
                                }
                                canvas.width = w;
                                canvas.height = h;
                                canvas.getContext('2d').drawImage(img, 0, 0, w, h);
                                resolve(canvas.toDataURL('image/jpeg', 0.7));
                            };
                            img.src = e.target.result;
                        };
                        reader.readAsDataURL(file);
                    } else {
                        const reader = new FileReader();
                        reader.onload = e => resolve(e.target.result);
                        reader.readAsDataURL(file);
                    }
                });
            }

            // ==================== 班级菜单 ====================
            function toggleClassMenu() {
                const overlay = document.getElementById('classMenuOverlay');
                overlay.classList.toggle('open');
                if (overlay.classList.contains('open')) {
                    const panel = document.getElementById('classMenuPanel');
                    panel.innerHTML = CLASS_DATA.map(c => `
                        <div class="class-menu-item ${c.id === currentClassId ? 'current' : ''}" onclick="switchClass('${c.id}')">
                            📌 ${c.name}
                        </div>
                    `).join('');
                }
            }

            function closeClassMenu(event) {
                if (event.target === document.getElementById('classMenuOverlay')) {
                    document.getElementById('classMenuOverlay').classList.remove('open');
                }
            }

            function switchClass(classId) {
                currentClassId = classId;
                document.getElementById('classMenuOverlay').classList.remove('open');
                currentStudent = null;
                closeFlipView();
                renderClassView();
            }

            function getCurrentClass() {
                return CLASS_DATA.find(c => c.id === currentClassId) || CLASS_DATA[0];
            }

            // ==================== 角色与密码 ====================
            function switchRole(role) {
                if (role === 'teacher' && currentRole !== 'teacher' && !window._auth) {
                    document.getElementById('passwordModalOverlay').classList.add('open');
                    document.getElementById('passwordInput').value = '';
                    document.getElementById('passwordError').style.display = 'none';
                    return;
                }
                if (role === 'teacher' && window._auth) {
                    window._auth = false;
                }
                currentRole = role;
                document.getElementById('btnStudentMode').classList.toggle('active', role === 'student');
                document.getElementById('btnTeacherMode').classList.toggle('active', role === 'teacher');
                currentStudent = null;
                closeFlipView();
                renderClassView();
            }

            function verifyPassword() {
                if (document.getElementById('passwordInput').value === PASSWORD) {
                    document.getElementById('passwordModalOverlay').classList.remove('open');
                    window._auth = true;
                    switchRole('teacher');
                } else {
                    document.getElementById('passwordError').style.display = 'block';
                }
            }

            function closePasswordModal() {
                document.getElementById('passwordModalOverlay').classList.remove('open');
            }

            // ==================== 页面渲染 ====================
            function renderClassView() {
                const cls = getCurrentClass();
                const all = loadAllData();
                document.getElementById('mainContainer').innerHTML = `
                    <div class="class-view">
                        <h2>${currentRole === 'student' ? '📋 请选择你的名字' : '📋 ' + cls.name + ' 学生名单'}</h2>
                        <p style="margin-bottom:20px;color:#666;">${currentRole === 'student' ? '点击名字进入评定' : '点击名字查看详情'} · ${cls.name} · ${cls.students.length}人</p>
                        <div class="student-grid">
                            ${cls.students.map(name => {
                                const submitted = all[name]?.submitted === true;
                                return `
                                    <div class="student-card ${submitted ? '' : 'unsubmitted'}" onclick="openStudent('${name}')">
                                        <div class="avatar">${name.charAt(0)}</div>
                                        <div style="font-weight:600;">${name}</div>
                                        <span class="badge ${submitted ? 'badge-submitted' : 'badge-pending'}">${submitted ? '已提交' : '未提交'}</span>
                                    </div>
                                `;
                            }).join('')}
                        </div>
                    </div>
                `;
            }

            function openStudent(name) {
                currentStudent = name;
                if (currentRole === 'student') {
                    renderStudentForm(name);
                } else {
                    const data = getStudentData(name);
                    if (!data || !data.submitted) {
                        document.getElementById('mainContainer').innerHTML = `
                            <div class="teacher-category-view">
                                <button onclick="goBackToClass()">← 返回</button>
                                <h2>${name}</h2>
                                <p style="color:red;padding:40px;text-align:center;">该生尚未提交评定数据</p>
                            </div>`;
                    } else {
                        renderTeacherCategoryView(name);
                    }
                }
            }

            // ==================== 学生表单 ====================
            function renderStudentForm(name) {
                const rec = getStudentData(name) || {};
                document.getElementById('mainContainer').innerHTML = `
                    <div class="form-view">
                        <button onclick="goBackToClass()">← 返回班级</button>
                        <h2>📝 ${name} 的评定</h2>
                        <p style="color:#666;margin-bottom:20px;">班级：${getCurrentClass().name}</p>
                        ${ASSESSMENT_STRUCTURE.map(cat => `
                            <div class="category-section" id="form-cat-${cat.id}">
                                <div class="category-header" onclick="toggleCat('${cat.id}')">
                                    <span>📌 ${cat.title}（满分${cat.total}分）</span>
                                    <span>▼</span>
                                </div>
                                <div class="category-body">
                                    ${cat.items.map(sub => {
                                        const val = rec[cat.id]?.[sub.id]?.score ?? sub.def;
                                        const att = rec[cat.id]?.[sub.id]?.att;
                                        const allowNeg = sub.neg === true;
                                        return `
                                            <div class="sub-item">
                                                <div class="item-info">
                                                    <b>${sub.name}</b><br><small>${sub.desc}</small>
                                                </div>
                                                <div class="score-input-group">
                                                    <input type="number" id="score-${sub.id}" value="${val}" 
                                                        ${allowNeg ? '' : 'min="0"'} max="${sub.max}" step="0.5"
                                                        onchange="onScoreChange('${name}','${cat.id}','${sub.id}', this.value, ${sub.max}, ${allowNeg})">
                                                    <span>/ ${sub.max}分 ${allowNeg ? '(可负)' : ''}</span>
                                                </div>
                                                <button class="upload-btn ${att ? 'has-file' : ''}" onclick="triggerUpload('${sub.id}')">
                                                    ${att ? '📎 已上传' : '📎 上传附件'}
                                                    ${att ? `<span onclick="event.stopPropagation();removeAtt('${name}','${cat.id}','${sub.id}')"> ×</span>` : ''}
                                                </button>
                                                <input type="file" id="file-${sub.id}" accept="image/*" hidden onchange="handleFileUpload('${name}','${cat.id}','${sub.id}', this)">
                                                ${att ? `<img src="${att}" style="width:40px;height:40px;border-radius:6px;">` : ''}
                                            </div>
                                        `;
                                    }).join('')}
                                </div>
                            </div>
                        `).join('')}
                        <div style="display:flex; justify-content:center; gap:16px; margin-top:24px;">
                            <button class="btn-submit" onclick="submitForm('${name}')">✅ 保存并提交</button>
                            <button class="btn-submit btn-export-pdf" onclick="exportStudentPDF('${name}')">📄 导出PDF</button>
                        </div>
                    </div>
                `;
                setTimeout(() => {
                    const firstCat = ASSESSMENT_STRUCTURE[0];
                    if (firstCat) document.getElementById('form-cat-' + firstCat.id)?.classList.add('open');
                }, 100);
            }

            function toggleCat(catId) {
                document.getElementById('form-cat-' + catId)?.classList.toggle('open');
            }

            function onScoreChange(name, catId, subId, rawValue, maxScore, allowNegative) {
                let val = parseFloat(rawValue);
                if (isNaN(val)) val = 0;
                if (!allowNegative && val < 0) val = 0;
                if (val > maxScore) val = maxScore;
                updateScore(name, catId, subId, val);
            }

            function triggerUpload(subId) {
                document.getElementById('file-' + subId).click();
            }

            async function handleFileUpload(name, catId, subId, fileInput) {
                const file = fileInput.files[0];
                if (!file) return;
                if (!file.type.startsWith('image/')) { toast('请上传图片文件', false); return; }
                const base64 = await compressImage(file);
                updateAttachment(name, catId, subId, base64, file.name);
                toast('附件已上传');
                renderStudentForm(name);
            }

            function removeAtt(name, catId, subId) {
                if (confirm('确定删除附件吗？')) {
                    removeAttachment(name, catId, subId);
                    toast('附件已删除');
                    renderStudentForm(name);
                }
            }

            function submitForm(name) {
                submitStudent(name);
                toast('评定数据已提交！');
                setTimeout(() => goBackToClass(), 1000);
            }

            function goBackToClass() {
                currentStudent = null;
                closeFlipView();
                renderClassView();
            }

            // ==================== 教师端查看 ====================
            function renderTeacherCategoryView(name) {
                const data = getStudentData(name);
                document.getElementById('mainContainer').innerHTML = `
                    <div class="teacher-category-view">
                        <button onclick="goBackToClass()">← 返回班级</button>
                        <h2>📊 ${name}</h2>
                        <p style="color:#666;">班级：${getCurrentClass().name}</p>
                        ${ASSESSMENT_STRUCTURE.map(cat => {
                            let sum = 0;
                            cat.items.forEach(s => sum += (data[cat.id][s.id]?.score || 0));
                            return `
                                <div class="category-select-card" onclick="openFlipView('${name}','${cat.id}')">
                                    <div><b>${cat.title}</b><br><small>${sum}/${cat.total} 分 · ${cat.items.length}个评分项</small></div>
                                    <span>→</span>
                                </div>
                            `;
                        }).join('')}
                    </div>
                `;
            }

            // ==================== 翻页视图 ====================
            function openFlipView(name, catId) {
                const data = getStudentData(name);
                if (!data || !data.submitted) return;
                const cat = ASSESSMENT_STRUCTURE.find(c => c.id === catId);
                if (!cat) return;
                flipCatId = catId;
                flipIdx = 0;
                flipTotal = cat.items.length;
                document.getElementById('flipTitle').textContent = cat.title;
                document.getElementById('flipView').classList.add('active');
                const track = document.getElementById('flipTrack');
                track.innerHTML = cat.items.map((sub, i) => {
                    const d = data[catId][sub.id];
                    const hasAtt = !!d?.att;
                    return `
                        <div class="flip-page">
                            <div class="page-attachment-area">
                                ${hasAtt ? `<img src="${d.att}" alt="附件">` : '<div style="color:#999;">暂无附件</div>'}
                            </div>
                            <div class="page-score-display">${sub.name} ： <b>${d?.score ?? 0}</b> / ${sub.max} 分</div>
                            <div style="margin-top:4px;color:#666;font-size:0.8rem;">${sub.desc}</div>
                            <div style="margin-top:10px;color:#999;">${i+1}/${flipTotal}</div>
                        </div>
                    `;
                }).join('');
                updateFlipPosition(0);
                document.getElementById('flipArrowUp').disabled = true;
                document.getElementById('flipArrowDown').disabled = flipTotal <= 1;
                updateToc(cat);
                document.addEventListener('keydown', handleFlipKeydown);
            }

            function updateFlipPosition(duration = 0) {
                const track = document.getElementById('flipTrack');
                track.style.transition = duration ? `transform ${duration}ms` : 'none';
                track.style.transform = `translateY(-${flipIdx * 100}%)`;
                document.getElementById('flipArrowUp').disabled = flipIdx <= 0;
                document.getElementById('flipArrowDown').disabled = flipIdx >= flipTotal - 1;
            }

            function flipPage(delta) {
                const newIdx = flipIdx + delta;
                if (newIdx < 0 || newIdx >= flipTotal) return;
                flipIdx = newIdx;
                updateFlipPosition(300);
                updateToc();
            }

            function updateToc(cat) {
                const dropdown = document.getElementById('tocDropdown');
                if (!cat) {
                    const currentCat = ASSESSMENT_STRUCTURE.find(c => c.id === flipCatId);
                    if (!currentCat) return;
                    cat = currentCat;
                }
                dropdown.innerHTML = cat.items.map((s, i) => `
                    <div class="toc-item" style="padding:11px 18px;cursor:pointer;${i===flipIdx?'background:#eef4fb;':''}"
                        onclick="jumpFlipPage(${i})">${i+1}. ${s.name}</div>
                `).join('');
            }

            function jumpFlipPage(idx) {
                flipIdx = idx;
                updateFlipPosition(300);
                document.getElementById('tocDropdown').style.display = 'none';
            }

            function toggleToc() {
                const d = document.getElementById('tocDropdown');
                d.style.display = d.style.display === 'block' ? 'none' : 'block';
                if (d.style.display === 'block') {
                    const currentCat = ASSESSMENT_STRUCTURE.find(c => c.id === flipCatId);
                    if (currentCat) updateToc(currentCat);
                }
            }

            function closeFlipView() {
                document.getElementById('flipView').classList.remove('active');
                document.removeEventListener('keydown', handleFlipKeydown);
                document.getElementById('tocDropdown').style.display = 'none';
            }

            function handleFlipKeydown(e) {
                if (!document.getElementById('flipView').classList.contains('active')) return;
                if (e.key === 'ArrowDown' || e.key === 'PageDown') { e.preventDefault(); flipPage(1); }
                if (e.key === 'ArrowUp' || e.key === 'PageUp') { e.preventDefault(); flipPage(-1); }
                if (e.key === 'Escape') closeFlipView();
            }

            // ==================== PDF导出（浏览器打印，保证内容可见） ====================
            function exportStudentPDF(name) {
                const data = getStudentData(name);
                if (!data || !data.submitted) {
                    toast('请先保存评定数据再导出', false);
                    return;
                }

                // 构建完整的报告HTML
                let html = `<html><head><meta charset="UTF-8"><title>${name}品德评定</title>
                <style>
                    body { font-family: 'SimSun', '宋体', 'Microsoft YaHei', sans-serif; padding: 20px; color: #000; background: #fff; }
                    h1 { text-align: center; color: #1a5276; }
                    h2 { background: #eef; padding: 6px 12px; margin: 20px 0 10px; }
                    .item { margin: 10px 0; padding-left: 12px; border-left: 3px solid #333; }
                    .item h3 { margin: 0 0 4px; }
                    .item p { margin: 0; font-size: 0.9rem; color: #555; }
                    img { max-width: 100%; max-height: 400px; margin-top: 8px; border: 1px solid #ddd; padding: 4px; }
                    @media print { body { -webkit-print-color-adjust: exact; print-color-adjust: exact; } }
                </style></head><body>
                <h1>${name} 品德评定报告</h1>
                <p style="text-align:center; color:#666;">檀雅书院 · ${getCurrentClass().name}</p>
                <hr>`;

                ASSESSMENT_STRUCTURE.forEach(cat => {
                    html += `<h2>${cat.title}（满分 ${cat.total} 分）</h2>`;
                    cat.items.forEach(sub => {
                        const d = data[cat.id][sub.id];
                        const score = d?.score ?? 0;
                        html += `<div class="item">
                            <h3>${sub.name} —— 得分：${score} / ${sub.max} 分</h3>
                            <p>${sub.desc}</p>`;
                        if (d?.att) {
                            html += `<div style="text-align:center;"><img src="${d.att}" alt="附件图片"/></div>`;
                        }
                        html += `</div>`;
                    });
                });

                html += `</body></html>`;

                // 打开新窗口并打印
                const printWindow = window.open('', '_blank', 'width=800,height=600');
                if (!printWindow) {
                    toast('请允许弹出窗口以导出PDF', false);
                    return;
                }
                printWindow.document.write(html);
                printWindow.document.close();
                printWindow.focus();
                // 延迟打印，确保图片加载
                setTimeout(() => {
                    printWindow.print();
                    toast('请在打印对话框中选择“另存为PDF”');
                }, 500);
            }

            // ==================== 全局导出导入 ====================
            function exportAllData() {
                const blob = new Blob([JSON.stringify({ data: loadAllData() }, null, 2)], { type: 'application/json' });
                const a = document.createElement('a');
                a.href = URL.createObjectURL(blob);
                a.download = '檀雅书院品德评定_全部数据.json';
                a.click();
                toast('全部数据已导出');
            }

            function importAllData(event) {
                const file = event.target.files[0];
                if (!file) return;
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const json = JSON.parse(e.target.result);
                        if (json.data) {
                            saveAllData(json.data);
                            toast('数据导入成功');
                            renderClassView();
                        }
                    } catch (ex) {
                        toast('导入失败，文件格式错误', false);
                    }
                };
                reader.readAsText(file);
                event.target.value = '';
            }

            // ==================== 暴露全局函数 ====================
            window.toggleClassMenu = toggleClassMenu;
            window.closeClassMenu = closeClassMenu;
            window.switchClass = switchClass;
            window.switchRole = switchRole;
            window.verifyPassword = verifyPassword;
            window.closePasswordModal = closePasswordModal;
            window.openStudent = openStudent;
            window.toggleCat = toggleCat;
            window.onScoreChange = onScoreChange;
            window.triggerUpload = triggerUpload;
            window.handleFileUpload = handleFileUpload;
            window.removeAtt = removeAtt;
            window.submitForm = submitForm;
            window.goBackToClass = goBackToClass;
            window.openFlipView = openFlipView;
            window.flipPage = flipPage;
            window.jumpFlipPage = jumpFlipPage;
            window.toggleToc = toggleToc;
            window.closeFlipView = closeFlipView;
            window.exportStudentPDF = exportStudentPDF;
            window.exportAllData = exportAllData;
            window.importAllData = importAllData;

            // ==================== 初始化 ====================
            function init() {
                // 预初始化所有学生记录（避免首次加载空值）
                CLASS_DATA.forEach(cls => cls.students.forEach(name => ensureRecord(name)));
                renderClassView();
                console.log('檀雅书院品德评定系统已启动');
            }
            init();
        })();
    </script>
</body>
</html>
