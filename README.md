<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Density.AI - Global Monitor V10</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/remixicon@3.5.0/fonts/remixicon.css" rel="stylesheet">
    
    <style>
        /* =========================================
           Theme: Premium Dark (Density Style)
           ========================================= */
        :root {
            --bg-body: #0b0d11;
            --bg-panel: #15181e;
            --bg-card: #1e222b;
            --primary: #00f0ff;
            --primary-dim: rgba(0, 240, 255, 0.1);
            --text-main: #ffffff;
            --text-sub: #94a3b8;
            --border: #2d3748;
            --success: #00ff9d;
        }

        body, html, #container { width: 100%; height: 100%; margin: 0; padding: 0; font-family: 'Inter', -apple-system, "Microsoft YaHei", sans-serif; background: var(--bg-body); overflow: hidden; color: var(--text-main); }

        /* 顶部导航 */
        .navbar {
            position: absolute; top: 0; width: 100%; height: 60px;
            background: rgba(21, 24, 30, 0.95); backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--border);
            display: flex; align-items: center; justify-content: space-between; padding: 0 24px; z-index: 500; box-sizing: border-box;
        }
        .nav-brand { font-size: 18px; font-weight: 700; letter-spacing: -0.5px; display: flex; align-items: center; gap: 8px; }
        .nav-brand i { color: var(--primary); font-size: 22px; }
        
        .nav-controls { display: flex; gap: 12px; align-items: center; }
        
        /* 语言切换器 */
        .lang-switch {
            background: var(--bg-card); border: 1px solid var(--border); border-radius: 4px;
            display: flex; overflow: hidden; cursor: pointer;
        }
        .lang-opt { padding: 4px 10px; font-size: 12px; font-weight: bold; color: var(--text-sub); transition: 0.2s; }
        .lang-opt.active { background: var(--primary); color: #000; }

        .btn-action {
            padding: 8px 16px; border-radius: 4px; border: 1px solid var(--border);
            background: var(--bg-panel); color: var(--text-main); cursor: pointer;
            font-size: 13px; font-weight: 500; transition: 0.2s; display: flex; align-items: center; gap: 6px;
        }
        .btn-action:hover { border-color: var(--primary); color: var(--primary); }
        .btn-action.active { background: var(--primary); color: #000; border-color: var(--primary); }

        /* 左侧面板 */
        .panel {
            position: absolute; top: 80px; left: 24px; width: 360px;
            background: var(--bg-panel); border: 1px solid var(--border); border-radius: 8px;
            display: flex; flex-direction: column; z-index: 400;
            box-shadow: 0 20px 40px rgba(0,0,0,0.6);
            max-height: calc(100vh - 100px); overflow-y: auto;
        }
        .panel-head { padding: 20px; border-bottom: 1px solid var(--border); }
        .panel-title { font-size: 16px; font-weight: 600; margin-bottom: 4px; }
        .panel-sub { font-size: 12px; color: var(--text-sub); display: flex; align-items: center; gap: 6px; }

        /* 硬件状态 */
        .iot-box {
            background: var(--primary-dim); border: 1px dashed var(--primary);
            margin: 20px; padding: 12px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center;
        }
        .iot-label { font-size: 11px; color: var(--primary); font-weight: bold; letter-spacing: 0.5px; }
        .status-badge { display: flex; align-items: center; gap: 6px; font-size: 11px; color: var(--text-sub); }
        .led { width: 6px; height: 6px; background: #555; border-radius: 50%; }
        .led.on { background: var(--success); box-shadow: 0 0 8px var(--success); animation: pulse 2s infinite; }

        /* 数据卡片 */
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; padding: 0 20px 20px; }
        .card { background: var(--bg-card); padding: 12px; border-radius: 6px; position: relative; }
        .card-lbl { font-size: 10px; color: var(--text-sub); text-transform: uppercase; margin-bottom: 6px; }
        .card-val { font-size: 22px; font-weight: 700; color: #fff; }
        .tag { position: absolute; top: 8px; right: 8px; font-size: 9px; padding: 2px 4px; border-radius: 2px; }
        .tag-hw { color: var(--primary); border: 1px solid rgba(0,240,255,0.2); }
        .tag-api { color: #ccc; background: rgba(255,255,255,0.05); }

        /* Chatbot */
        .chatbot {
            position: absolute; bottom: 24px; right: 24px; width: 320px;
            background: var(--bg-panel); border: 1px solid var(--border); border-radius: 8px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5); z-index: 500; display: flex; flex-direction: column; overflow: hidden;
        }
        .chat-head { padding: 12px 16px; background: var(--bg-card); font-size: 13px; font-weight: 600; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between; }
        .chat-body { height: 220px; padding: 15px; overflow-y: auto; background: #0f1115; }
        .msg { margin-bottom: 10px; padding: 8px 12px; border-radius: 6px; font-size: 12px; line-height: 1.4; max-width: 85%; }
        .msg.bot { background: var(--bg-card); color: #fff; border-left: 2px solid var(--primary); }
        .msg.user { background: var(--primary); color: #000; align-self: flex-end; margin-left: auto; }
        .chat-inp-area { padding: 10px; border-top: 1px solid var(--border); display: flex; gap: 8px; }
        .chat-inp { flex: 1; background: var(--bg-body); border: 1px solid var(--border); color: #fff; padding: 8px; border-radius: 4px; font-size: 12px; outline: none; }

        /* Unlocked Report Modal */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85); backdrop-filter: blur(8px);
            z-index: 2000; display: none; align-items: center; justify-content: center;
        }
        .report-paper {
            width: 800px; height: 85vh; background: #fff; color: #1a1a1a;
            border-radius: 8px; overflow-y: auto; box-shadow: 0 0 50px rgba(0,0,0,0.5);
            animation: slideUp 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
            display: flex; flex-direction: column;
        }
        .report-header {
            padding: 30px 40px; background: #f8f9fa; border-bottom: 1px solid #e9ecef;
            display: flex; justify-content: space-between; align-items: center;
        }
        .report-body { padding: 40px; flex: 1; }
        .report-section { margin-bottom: 40px; }
        .report-h3 { font-size: 16px; font-weight: 700; color: #000; margin-bottom: 15px; border-left: 4px solid var(--primary); padding-left: 10px; text-transform: uppercase; }
        .report-text { font-size: 14px; line-height: 1.7; color: #4a5568; margin-bottom: 20px; text-align: justify; }
        
        .score-card { display: flex; gap: 20px; background: #f1f5f9; padding: 20px; border-radius: 8px; }
        .score-item { flex: 1; text-align: center; }
        .score-val { font-size: 28px; font-weight: 800; color: var(--primary); display: block; filter: brightness(0.8); }
        .score-lbl { font-size: 11px; font-weight: bold; color: #64748b; text-transform: uppercase; }

        @keyframes pulse { 0% {opacity: 1;} 50% {opacity: 0.4;} 100% {opacity: 1;} }
        @keyframes slideUp { from {transform: translateY(40px); opacity:0;} to {transform: translateY(0); opacity:1;} }
    </style>

    <script type="text/javascript">
        window._AMapSecurityConfig = { securityJsCode: 'c26bae17eef0083d09159f44206407ff' }
    </script>
    <script src="https://webapi.amap.com/maps?v=2.0&key=d8c07a05572ccedd4066c7543b7aaea8&plugin=AMap.MouseTool,AMap.PlaceSearch,AMap.Geocoder,AMap.Weather"></script>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js"></script>
</head>
<body>

    <!-- 导航栏 -->
    <nav class="navbar">
        <div class="nav-brand">
            <i class="ri-radar-line"></i>
            DENSITY<span style="color:var(--primary)">.AI</span>
        </div>
        
        <div class="nav-controls">
            <!-- 语言切换 -->
            <div class="lang-switch">
                <div class="lang-opt active" id="lang-en" onclick="setLang('en')">EN</div>
                <div class="lang-opt" id="lang-zh" onclick="setLang('zh')">中</div>
            </div>
            
            <button class="btn-action" id="btn-select" onclick="toggleSelection()">
                <i class="ri-add-line"></i> <span data-i18n="new_site">New Site</span>
            </button>
            <button class="btn-action" onclick="generateReport()">
                <i class="ri-file-chart-line"></i> <span data-i18n="report">Analysis Report</span>
            </button>
        </div>
    </nav>

    <!-- 地图 -->
    <div id="container"></div>

    <!-- 左侧面板 -->
    <div class="panel">
        <div class="panel-head">
            <div class="panel-title" id="site-name" data-i18n="no_site">No Site Selected</div>
            <div class="panel-sub" id="site-addr"><i class="ri-map-pin-line"></i> <span data-i18n="select_hint">Select a location on map</span></div>
        </div>

        <!-- 硬件模拟 -->
        <div class="iot-box">
            <div>
                <div class="iot-label" data-i18n="hw_interface">HARDWARE INTERFACE</div>
                <div style="font-size:10px; color:#666;">ID: <span id="sensor-id">--</span></div>
            </div>
            <div class="status-badge">
                <span id="iot-status" data-i18n="disconnected">Disconnected</span>
                <div class="led" id="iot-led"></div>
            </div>
        </div>

        <div class="grid-2">
            <div class="card">
                <div class="card-lbl" data-i18n="occupancy">Occupancy</div>
                <div class="card-val" id="val-occ">--</div>
                <div class="tag tag-hw">IOT</div>
            </div>
            <div class="card">
                <div class="card-lbl" data-i18n="dwell">Dwell Time</div>
                <div class="card-val" id="val-dwell">--</div>
                <div class="tag tag-hw">IOT</div>
            </div>
            <div class="card">
                <div class="card-lbl" data-i18n="weather">Weather</div>
                <div class="card-val" id="val-temp">--</div>
                <div class="tag tag-api">API</div>
            </div>
            <div class="card">
                <div class="card-lbl" data-i18n="poi_count">Nearby POIs</div>
                <div class="card-val" id="val-poi">--</div>
                <div class="tag tag-api">API</div>
            </div>
        </div>
        
        <div style="padding:0 20px 20px; height:180px;" id="chart-radar"></div>
    </div>

    <!-- AI Chatbot -->
    <div class="chatbot">
        <div class="chat-head">
            <span><i class="ri-robot-line"></i> <span data-i18n="ai_assistant">AI Assistant</span></span>
        </div>
        <div class="chat-body" id="chat-box">
            <div class="msg bot" data-i18n="ai_welcome">System ready. Select a site to begin analysis.</div>
        </div>
        <div class="chat-inp-area">
            <input type="text" class="chat-inp" id="chat-input" placeholder="..." onkeypress="handleChat(event)">
            <i class="ri-send-plane-fill" style="cursor:pointer; color:var(--primary); font-size:18px;" onclick="sendChat()"></i>
        </div>
    </div>

    <!-- 全解锁报告弹窗 (Unlocked Report) -->
    <div class="modal-overlay" id="report-modal">
        <div class="report-paper">
            <div class="report-header">
                <div>
                    <h2 style="margin:0; font-size:24px;" data-i18n="report_title">Site Feasibility Analysis</h2>
                    <div style="font-size:12px; color:#666; margin-top:5px;">Generated by Density.AI Engine | <span id="report-date"></span></div>
                </div>
                <i class="ri-close-circle-fill" style="font-size:32px; color:#ccc; cursor:pointer;" onclick="closeReport()"></i>
            </div>
            
            <!-- Loading State -->
            <div id="report-loading" style="display:flex; flex-direction:column; align-items:center; justify-content:center; height:100%;">
                <i class="ri-loader-4-line" style="font-size:40px; color:var(--primary); animation:spin 1s linear infinite;"></i>
                <p style="margin-top:20px; color:#666;" data-i18n="generating">Generating Insights...</p>
            </div>

            <!-- Report Content -->
            <div class="report-body" id="report-content" style="display:none;">
                
                <!-- 1. Scores -->
                <div class="score-card">
                    <div class="score-item">
                        <span class="score-val">A+</span>
                        <span class="score-lbl" data-i18n="score_commercial">Commercial Value</span>
                    </div>
                    <div class="score-item">
                        <span class="score-val" style="color:#10b981;">92</span>
                        <span class="score-lbl" data-i18n="score_traffic">Traffic Score</span>
                    </div>
                    <div class="score-item">
                        <span class="score-val" style="color:#f59e0b;">High</span>
                        <span class="score-lbl" data-i18n="score_growth">Growth Potential</span>
                    </div>
                </div>

                <!-- 2. Summary -->
                <div class="report-section" style="margin-top:30px;">
                    <div class="report-h3" data-i18n="rep_h_summary">Executive Summary</div>
                    <p class="report-text" id="rep-summary-text">
                        The selected site demonstrates exceptional potential for mixed-use development. Our real-time IoT simulation indicates a steady foot traffic flow exceeding 120 persons/hour during off-peak times. Combined with the high density of surrounding retail POIs (API verified), this location is prime for retail or F&B expansion.
                    </p>
                </div>

                <!-- 3. Traffic Chart -->
                <div class="report-section">
                    <div class="report-h3" data-i18n="rep_h_traffic">Projected Traffic Analysis (Next 7 Days)</div>
                    <div id="chart-report-traffic" style="width:100%; height:250px;"></div>
                </div>

                <!-- 4. Demographics -->
                <div class="report-section">
                    <div class="report-h3" data-i18n="rep_h_demographics">Visitor Demographics</div>
                    <div id="chart-report-demo" style="width:100%; height:200px;"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ==========================================
        // 1. Language & Dictionary
        // ==========================================
        var currentLang = 'en';
        
        const dict = {
            en: {
                new_site: "New Site",
                report: "Analysis Report",
                no_site: "No Site Selected",
                select_hint: "Select a location on map",
                hw_interface: "HARDWARE INTERFACE",
                disconnected: "Disconnected",
                occupancy: "Occupancy",
                dwell: "Dwell Time",
                weather: "Weather",
                poi_count: "Nearby POIs",
                ai_assistant: "AI Assistant",
                ai_welcome: "System ready. Select a site to begin analysis.",
                report_title: "Site Feasibility Analysis",
                generating: "Analyzing Data & Generating Report...",
                score_commercial: "Commercial Value",
                score_traffic: "Traffic Score",
                score_growth: "Growth Potential",
                rep_h_summary: "1. Executive Summary",
                rep_h_traffic: "2. Projected Traffic Trend",
                rep_h_demographics: "3. Visitor Composition",
                ai_connected: "Sensor connected. Real-time data stream active.",
                ai_analyzing: "Analyzing site context...",
                summary_content: "The selected site demonstrates exceptional potential. Real-time IoT simulation indicates a steady foot traffic flow. Combined with high density of surrounding retail POIs, this location is prime for expansion."
            },
            zh: {
                new_site: "新建选址",
                report: "生成分析报告",
                no_site: "未选择地块",
                select_hint: "请在地图上点击选择位置",
                hw_interface: "硬件数据接口",
                disconnected: "未连接",
                occupancy: "实时人流",
                dwell: "驻留时长(分)",
                weather: "实时天气",
                poi_count: "周边配套数",
                ai_assistant: "AI 智能助手",
                ai_welcome: "系统就绪。请在地图选点以开始分析。",
                report_title: "地块商业价值深度分析报告",
                generating: "正在分析全域数据并生成报告...",
                score_commercial: "商业价值评级",
                score_traffic: "客流活力指数",
                score_growth: "未来增长潜力",
                rep_h_summary: "1. 核心综述 (Executive Summary)",
                rep_h_traffic: "2. 未来7天客流预测",
                rep_h_demographics: "3. 到访客群画像",
                ai_connected: "传感器已连接。实时数据流传输中...",
                ai_analyzing: "正在分析地块周边环境...",
                summary_content: "所选地块显示出极佳的开发潜力。IoT 实时监测数据显示，即使在非高峰时段，人流量仍保持在 120人/小时以上。结合周边高密度的商业配套（经API核实），该位置非常适合零售或餐饮业态拓展。"
            }
        };

        function setLang(lang) {
            currentLang = lang;
            // Update Toggle UI
            document.querySelectorAll('.lang-opt').forEach(el => el.classList.remove('active'));
            document.getElementById('lang-' + lang).classList.add('active');
            
            // Update Text Elements
            document.querySelectorAll('[data-i18n]').forEach(el => {
                const key = el.getAttribute('data-i18n');
                if(dict[lang][key]) el.innerText = dict[lang][key];
            });
        }

        // ==========================================
        // 2. Map & Logic
        // ==========================================
        var map = new AMap.Map('container', {
            zoom: 16,
            center: [106.5235, 29.6318],
            pitch: 50,
            viewMode: '3D',
            mapStyle: 'amap://styles/grey',
        });

        var geocoder = new AMap.Geocoder();
        var weather = new AMap.Weather();
        var placeSearch = new AMap.PlaceSearch({ pageSize: 50 });
        
        var isSelecting = false;
        var currentPolygon = null;
        var sensorTimer = null;

        // Radar Chart Init
        var radarChart = echarts.init(document.getElementById('chart-radar'));
        var radarOption = {
            radar: {
                indicator: [{name:'Retail'},{name:'Food'},{name:'Transit'},{name:'Office'},{name:'Res'}],
                splitArea: { areaStyle: { color: ['#1a1d24'] } },
                axisLine: { lineStyle: { color: '#333' } },
                splitLine: { lineStyle: { color: '#333' } }
            },
            series: [{
                type: 'radar',
                data: [{ value: [0,0,0,0,0], name: 'Context' }],
                itemStyle: { color: '#00f0ff' },
                areaStyle: { color: 'rgba(0, 240, 255, 0.2)' }
            }]
        };
        radarChart.setOption(radarOption);

        function toggleSelection() {
            isSelecting = !isSelecting;
            var btn = document.getElementById('btn-select');
            if(isSelecting) {
                btn.classList.add('active');
                map.setDefaultCursor("crosshair");
                map.on('click', handleMapClick);
            } else {
                btn.classList.remove('active');
                map.setDefaultCursor("default");
                map.off('click', handleMapClick);
            }
        }

        function handleMapClick(e) {
            var lnglat = [e.lnglat.getLng(), e.lnglat.getLat()];
            
            // Draw
            if(currentPolygon) map.remove(currentPolygon);
            var offset = 0.0015;
            var path = [[lnglat[0]-offset, lnglat[1]+offset], [lnglat[0]+offset, lnglat[1]+offset], [lnglat[0]+offset, lnglat[1]-offset], [lnglat[0]-offset, lnglat[1]-offset]];
            currentPolygon = new AMap.Polygon({ path: path, strokeColor: "#00f0ff", fillColor: "#00f0ff", fillOpacity: 0.1 });
            map.add(currentPolygon);
            map.panTo(lnglat);

            // Fetch Data
            fetchRealData(lnglat);
            connectIoT();
            
            toggleSelection();
            addBotMsg(dict[currentLang].ai_analyzing);
        }

        function fetchRealData(lnglat) {
            geocoder.getAddress(lnglat, function(status, result) {
                if(status === 'complete') {
                    document.getElementById('site-name').innerText = currentLang==='en'?"Selected Site":"已选定地块";
                    document.getElementById('site-addr').innerText = result.regeocode.formattedAddress;
                    
                    var district = result.regeocode.addressComponent.district;
                    weather.getLive(district, function(err, data) {
                        if(!err) document.getElementById('val-temp').innerText = data.temperature + "° " + data.weather;
                    });
                }
            });

            placeSearch.searchNearBy('', lnglat, 500, function(status, result) {
                if(status === 'complete') {
                    var c = result.poiList.count;
                    document.getElementById('val-poi').innerText = c;
                    // Mock Radar
                    radarChart.setOption({ series: [{ data: [{ value: [c*0.8, c*0.5, c*0.2, c*0.3, c*0.6] }] }] });
                }
            });
        }

        function connectIoT() {
            document.getElementById('iot-status').innerText = "Connecting...";
            document.getElementById('iot-status').style.color = "#f59e0b";
            
            setTimeout(() => {
                document.getElementById('iot-status').innerText = "Online";
                document.getElementById('iot-status').style.color = "#00ff9d";
                document.getElementById('iot-led').classList.add('on');
                document.getElementById('sensor-id').innerText = "DEN-" + Math.floor(Math.random()*9000+1000);
                
                if(sensorTimer) clearInterval(sensorTimer);
                sensorTimer = setInterval(() => {
                    document.getElementById('val-occ').innerText = Math.floor(Math.random()*50 + 100);
                    document.getElementById('val-dwell').innerText = (Math.random()*10 + 5).toFixed(1);
                }, 2000);

                addBotMsg(dict[currentLang].ai_connected);
            }, 1500);
        }

        // ==========================================
        // 3. Unlocked Report Logic
        // ==========================================
        function generateReport() {
            // Check if site is selected (simplified check)
            if(document.getElementById('sensor-id').innerText === '--') {
                alert(currentLang==='en' ? "Please select a site first." : "请先在地图上选择一个地块。");
                return;
            }

            var modal = document.getElementById('report-modal');
            var loading = document.getElementById('report-loading');
            var content = document.getElementById('report-content');
            
            modal.style.display = 'flex';
            loading.style.display = 'flex';
            content.style.display = 'none';
            document.getElementById('report-date').innerText = new Date().toLocaleDateString();

            // Simulate AI Generation
            setTimeout(() => {
                loading.style.display = 'none';
                content.style.display = 'block';
                
                // Update Summary Text
                document.getElementById('rep-summary-text').innerText = dict[currentLang].summary_content;

                // Render Charts inside Report
                renderReportCharts();
            }, 2000);
        }

        function renderReportCharts() {
            var chartT = echarts.init(document.getElementById('chart-report-traffic'));
            chartT.setOption({
                tooltip: { trigger: 'axis' },
                grid: { top: 20, bottom: 30, left: 40, right: 20 },
                xAxis: { type: 'category', data: ['Mon','Tue','Wed','Thu','Fri','Sat','Sun'], axisLine:{lineStyle:{color:'#ccc'}} },
                yAxis: { type: 'value', axisLine:{lineStyle:{color:'#ccc'}}, splitLine:{lineStyle:{color:'#eee'}} },
                series: [{ 
                    data: [150, 230, 224, 218, 135, 147, 260], type: 'line', smooth: true, 
                    areaStyle: { color: 'rgba(0, 240, 255, 0.2)' }, itemStyle: { color: '#00f0ff' } 
                }]
            });
            chartT.resize();

            var chartD = echarts.init(document.getElementById('chart-report-demo'));
            chartD.setOption({
                tooltip: { trigger: 'item' },
                series: [{
                    type: 'pie', radius: ['40%', '70%'],
                    data: [
                        { value: 1048, name: 'Gen Z (18-25)' },
                        { value: 735, name: 'Millennials (26-40)' },
                        { value: 580, name: 'Gen X (41-55)' },
                        { value: 484, name: 'Boomers (55+)' }
                    ],
                    itemStyle: { borderRadius: 5, borderColor: '#fff', borderWidth: 2 }
                }]
            });
            chartD.resize();
        }

        function closeReport() {
            document.getElementById('report-modal').style.display = 'none';
        }

        // ==========================================
        // 4. Chatbot
        // ==========================================
        function handleChat(e) { if(e.key === 'Enter') sendChat(); }
        function sendChat() {
            var inp = document.getElementById('chat-input');
            var val = inp.value.trim();
            if(!val) return;
            addMsg(val, 'user');
            inp.value = '';
            
            setTimeout(() => {
                var rep = currentLang==='en' ? "Data updated. Analysis shows positive trend." : "数据已更新。分析显示该区域呈正向增长趋势。";
                addBotMsg(rep);
            }, 800);
        }
        function addMsg(txt, type) {
            var box = document.getElementById('chat-box');
            box.innerHTML += `<div class="msg ${type}">${txt}</div>`;
            box.scrollTop = box.scrollHeight;
        }
        function addBotMsg(txt) { addMsg(txt, 'bot'); }

    </script>
</body>
</html>
