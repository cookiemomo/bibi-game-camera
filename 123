<!doctype html>
<html lang="zh-Hant">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1, user-scalable=no" />
  <title>鼻鼻大冒險｜Week 1-2 混合完整版</title>
  
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils/camera_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/control_utils/control_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/drawing_utils/drawing_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/face_mesh/face_mesh.js" crossorigin="anonymous"></script>

  <style>
    /* ===== 核心變數 ===== */
    :root{
      --bg1:#2b1b14; --bg2:#7a4b2b;
      --panel:#f4e2c8; --panel2:#f0d6b0;
      --ink:#1a1412; --muted:#5b4b42;
      --accent:#ffb23f; --accent2:#ff7a59;
      --good:#1b7f4b; --bad:#b45309;
      --round:18px;
      --cute-pink: #ffb7c5;
      --tag-green: #8bc34a;
      --skin-base: #ffccbc; 
      --skin-shadow: #eab8a8;
      --line-brown: #4e342e;
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family:system-ui,-apple-system,"Microsoft JhengHei",sans-serif;
      color:var(--ink); min-height:100vh;
      background: radial-gradient(circle at 50% 10%, rgba(255,255,255,0.1), transparent), linear-gradient(155deg, var(--bg1), var(--bg2));
      overflow-x: hidden; padding-bottom: 40px;
      user-select: none; -webkit-user-select: none;
    }
    .wrap{max-width:480px;margin:0 auto;padding:16px;}
    
    /* 頂部 & 地圖 */
    .topbar{display:flex;justify-content:space-between;align-items:center;color:#fff;margin-bottom:20px;padding:0 10px;}
    .brand{font-weight:900;font-size:20px;}
    .btnTiny{background:rgba(0,0,0,.2);color:#fff;border:1px solid rgba(255,255,255,.2);padding:6px 12px;border-radius:20px;cursor:pointer;font-size:12px;}

    .map-container { display: flex; flex-direction: column; align-items: center; gap: 40px; padding: 40px 0; position: relative; }
    .path-line { position: absolute; top: 60px; bottom: 60px; width: 8px; background: rgba(255,255,255,0.2); z-index: 0; border-radius: 4px; }
    .level-node { width: 90px; height: 90px; border-radius: 50%; background: #fff; border-bottom: 6px solid #e0e0e0; display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative; z-index: 2; cursor: pointer; transition: transform .1s; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
    .level-node:active { transform: translateY(4px); border-bottom-width: 2px; }
    .level-node.locked { filter: grayscale(1); opacity: 0.7; cursor: not-allowed; }
    .level-node.active { background: var(--accent); border-color: #e59400; animation: pulse 2s infinite; }
    .level-node.done { background: var(--good); border-color: #145e35; color: #fff; }
    .level-icon { font-size: 36px; margin-bottom: 4px; }
    .level-title { position: absolute; bottom: -30px; width: 140px; text-align: center; font-weight: 900; color: #fff; font-size: 14px; background: rgba(0,0,0,0.2); padding: 4px; border-radius: 10px; }
    .trophy-btn { width: 70px; height: 70px; border-radius: 50%; background: #fff; display: flex; align-items: center; justify-content: center; font-size: 30px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); border: 4px solid var(--accent2); cursor: pointer; position: relative; z-index: 2; margin-top: 20px; }
    @keyframes pulse { 0%{box-shadow:0 0 0 0 rgba(255,178,63,0.7)} 70%{box-shadow:0 0 0 15px rgba(255,178,63,0)} 100%{box-shadow:0 0 0 0 rgba(255,178,63,0)} }

    /* 遊戲視窗 */
    .game-view { position: fixed; inset: 0; background: linear-gradient(180deg, var(--panel), var(--panel2)); z-index: 100; overflow-y: auto; display: none; padding: 20px; flex-direction: column; }
    .game-view.show { display: flex; animation: slideUp .3s; }
    @keyframes slideUp { from{transform:translateY(100%)} to{transform:translateY(0)} }
    
    .nav-header { display: flex; align-items: center; margin-bottom: 10px; flex-shrink: 0; }
    .btn-back { background: none; border: none; font-size: 24px; cursor: pointer; margin-right: 15px; }
    .view-title { font-weight: 900; font-size: 18px; color: var(--ink); }

    /* 角色區 */
    .charStage{ padding:14px; margin-bottom: 15px; background: radial-gradient(circle at 50% 30%, rgba(255,255,255,0.8), rgba(255,255,255,0.2)); display: flex; flex-direction: column; position: relative; border-radius: 18px; min-height: 380px; flex-shrink: 0; transition: min-height 0.3s; }
    .stats{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px;}
    .statBox{background:rgba(255,255,255,.6);border-radius:12px;padding:8px;}
    .statName{font-size:11px;color:var(--muted);font-weight:900}
    .statVal{font-size:15px;font-weight:900;}
    .bar{height:6px;background:rgba(0,0,0,.1);border-radius:4px;margin-top:4px;overflow:hidden}
    .bar>div{height:100%;background:var(--accent2);transition:width .3s}

    .bibi-container { margin: auto; width: 160px; height: 160px; position: relative; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.3s; }
    .bibi-emoji { font-size: 100px; filter: drop-shadow(0 8px 16px rgba(0,0,0,0.15)); transition: transform .1s; }
    .bibi-emoji:active { transform: scale(0.95); }
    .bibiMood { position: absolute; bottom: 0; right: 0; font-size: 32px; background: #fff; border-radius: 50%; width: 45px; height: 45px; display: flex; align-items: center; justify-content: center; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }

    /* 衛生紙 & 暖身遮罩 */
    .tissue-pile {
        position: absolute; inset: 0; background: rgba(50,50,50,0.95); z-index: 50;
        display: flex; flex-direction: column; align-items: center; justify-content: center;
        border-radius: 18px; cursor: pointer; backdrop-filter: blur(4px); text-align: center;
    }
    .tp-emoji { font-size: 80px; animation: shake 2s infinite; }
    .tp-text { color: #fff; font-weight: 900; margin-top: 10px; font-size: 18px; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
    @keyframes shake { 0%,100%{transform:rotate(0deg)} 25%{transform:rotate(-5deg)} 75%{transform:rotate(5deg)} }

    .bubble { margin-top: auto; background: #fff; border: 2px solid var(--ink); border-radius: 18px; padding: 16px; position: relative; min-height: 130px; display: flex; flex-direction: column; justify-content: space-between; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
    .bubble::after { content: ""; position: absolute; top: -10px; left: 50%; transform: translateX(-50%); border-width: 0 10px 10px; border-style: solid; border-color: transparent transparent var(--ink); }
    .bubbleText { font-size: 15px; line-height: 1.5; font-weight: 500; white-space: pre-line; color: var(--ink); }
    .btnNext { align-self: flex-end; background: var(--ink); color: #fff; border: none; padding: 6px 14px; border-radius: 20px; font-size: 12px; cursor: pointer; margin-top: 8px; animation: bounce 1s infinite; }

    /* Week 1 樣式 */
    .tinder-container { position: relative; width: 100%; height: 280px; margin: 10px auto; display: flex; align-items: center; justify-content: center; }
    .tinder-card { position: absolute; width: 240px; height: 280px; background: #fff; border-radius: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.1); display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 20px; text-align: center; border: 1px solid rgba(0,0,0,0.05); transition: transform .4s, opacity .4s; z-index: 2; }
    .tinder-card.swipe-left { transform: translateX(-150%) rotate(-20deg); opacity: 0; }
    .tinder-card.swipe-right { transform: translateX(150%) rotate(20deg); opacity: 0; }
    .tc-icon { font-size: 50px; margin-bottom: 15px; }
    .tc-title { font-size: 18px; font-weight: 900; margin-bottom: 10px; }
    .tc-desc { font-size: 14px; color: var(--muted); line-height: 1.6; }
    .swipe-actions { display: flex; justify-content: center; gap: 30px; margin-top: 10px; }
    .btn-round { width: 50px; height: 50px; border-radius: 50%; border: 0; font-size: 20px; display: flex; align-items: center; justify-content: center; cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.1); background: #fff; }
    .btn-no { color: var(--bad); border: 2px solid #eee; } .btn-yes { color: var(--good); border: 2px solid #eee; }

    .action-grid { display: grid; grid-template-columns: 1fr; gap: 10px; max-height: 380px; overflow-y: auto; padding: 4px; }
    .action-card { background: #fff; border-radius: 12px; border: 2px solid transparent; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.05); padding: 15px; text-align: left; display: flex; align-items: center; }
    .action-card.selected { border-color: var(--accent); background-color: #fff8e1; }
    .action-emoji { font-size: 36px; margin-right: 15px; width: 40px; text-align: center; }
    .action-text { font-size: 14px; font-weight: 900; color: var(--ink); margin-bottom: 4px; }
    .action-desc { font-size: 12px; color: #666; font-weight: normal; }

    .compare-container { display: flex; gap: 10px; margin-bottom: 15px; margin-top: 10px; }
    .compare-card { flex: 1; background: #fff; border-radius: 12px; padding: 15px; text-align: center; cursor: pointer; border: 2px solid transparent; box-shadow: 0 2px 8px rgba(0,0,0,0.05); transition: transform 0.2s; }
    .compare-card.selected { border-color: var(--accent); background-color: #fff8e1; transform: scale(1.05); }
    .cc-title { font-weight: 900; margin-bottom: 8px; font-size: 16px; }
    .cc-list { font-size: 13px; color: #666; line-height: 1.6; }

    .room-container { position: relative; width: 100%; height: 300px; border-radius: 16px; margin-top: 10px; border: 4px solid #fff; background-image: url('https://images.unsplash.com/photo-1540518614846-7eded433c457?q=80&w=800&auto=format&fit=crop'); background-size: cover; background-position: center; box-shadow: inset 0 0 40px rgba(0,0,0,.2); overflow: hidden; }
    .prop { position: absolute; cursor: pointer; transition: transform 0.2s, filter 0.2s; z-index: 10; display: flex; flex-direction: column; align-items: center; }
    .prop:hover { transform: scale(1.1); } .prop.found { filter: grayscale(1) opacity(0.5); transform: scale(0.9); }
    .ac-unit { top: 20px; left: 30px; width: 80px; height: 35px; background: rgba(255,255,255,0.95); border-radius: 6px; border: 1px solid #ddd; display: flex; align-items: flex-end; justify-content: center; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
    .ac-vent { width: 80%; height: 4px; background: #333; margin-bottom: 6px; border-radius: 2px; }
    .dust-monster { bottom: 30%; left: 20%; width: 40px; height: 40px; background: rgba(85,85,85,0.8); border-radius: 50%; animation: float 3s infinite ease-in-out; display: flex; align-items: center; justify-content: center; color: white; font-size: 20px; }
    .bibi-room { bottom: 20px; right: 40px; font-size: 60px; text-shadow: 0 0 10px rgba(255,255,255,0.8); }

    /* === Week 2 靜態圖解模式 === */
    .acu-stage-static { 
      width: 260px; height: 260px; background: #fffcf8; border-radius: 35px;
      position: relative; margin: 10px auto; overflow: visible;
      border: 5px solid #f0d6b0; box-shadow: 0 8px 20px rgba(255, 183, 197, 0.3);
    }
    .svg-layer { width: 100%; height: 100%; position: absolute; inset: 0; pointer-events: none; }
    
    /* 臉部配色 */
    .face-fill { fill: var(--skin-base); stroke: var(--line-brown); stroke-width: 2.5; }
    .face-line { fill: none; stroke: var(--line-brown); stroke-width: 2.5; stroke-linecap: round; }
    .cute-blush { fill: var(--cute-pink); opacity: 0.6; }
    /* 手部配色 */
    .hand-fill { fill: var(--skin-base); stroke: var(--line-brown); stroke-width: 3; }
    .hand-shadow { fill: var(--skin-shadow); }
    .fingernail { fill: #ffe0bd; stroke: var(--line-brown); stroke-width: 2; }

    /* 靜態穴位紅點 */
    .static-point {
        position: absolute; width: 24px; height: 24px;
        background: rgba(255, 0, 0, 0.6); border: 2px solid white; border-radius: 50%;
        animation: pulsePoint 2s infinite; z-index: 10;
    }
    @keyframes pulsePoint { 0%{transform:translate(-50%,-50%) scale(1)} 50%{transform:translate(-50%,-50%) scale(1.3)} 100%{transform:translate(-50%,-50%) scale(1)} }

    /* 精準座標 (V47) */
    .sp-yingxiang { top: 62%; left: 37%; } .sp-yingxiang-r { top: 62%; left: 63%; }
    .sp-bitong { top: 52%; left: 41%; } .sp-bitong-r { top: 52%; left: 59%; }
    .sp-zanzhu { top: 38%; left: 35%; } .sp-zanzhu-r { top: 38%; left: 65%; }
    .sp-shangxing { top: 15%; left: 50%; }
    .sp-hegu { top: 53%; left: 42%; }

    /* === Week 2 AR 模式 === */
    .acu-stage-ar {
        position: fixed; inset: 0; background: #000; z-index: 900; display: none;
        flex-direction: column; align-items: center; justify-content: center;
    }
    .ar-video { position: absolute; width: 100%; height: 100%; object-fit: cover; transform: scaleX(-1); display: none; } /* 隱藏 raw video */
    .ar-canvas { position: absolute; width: 100%; height: 100%; object-fit: contain; transform: scaleX(-1); }
    
    .ar-ui { position: absolute; bottom: 40px; z-index: 910; width: 100%; text-align: center; pointer-events: none; }
    .ar-msg { background: rgba(0,0,0,0.6); color: #fff; padding: 10px 20px; border-radius: 20px; display: inline-block; font-weight: bold; border: 1px solid #ffb23f; }
    .btn-ar-close { 
        position: absolute; top: 20px; right: 20px; z-index: 920; 
        background: rgba(255,255,255,0.3); color: white; border: none; 
        width: 40px; height: 40px; border-radius: 50%; font-size: 20px; cursor: pointer;
    }
    .ar-timer { position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%); font-size: 80px; font-weight: 900; color: white; display: none; z-index: 920; text-shadow: 0 0 10px red; }

    .leader-list { width: 100%; display: flex; flex-direction: column; gap: 8px; }
    .leader-item { display: flex; align-items: center; justify-content: space-between; padding: 12px; border-radius: 16px; background: #fff; box-shadow: 0 2px 6px rgba(0,0,0,0.05); }
    .leader-item.me { border: 2px solid var(--accent); background: #fffdf5; }
    .rank-num { font-weight: 900; color: var(--muted); width: 30px; text-align: center; }
    .user-info { flex: 1; display: flex; align-items: center; gap: 10px; }
    .user-avatar { width: 40px; height: 40px; border-radius: 50%; background: #eee; display: flex; align-items: center; justify-content: center; font-size: 20px; }
    .user-name { font-weight: 900; font-size: 14px; }
    .user-exp { font-weight: bold; color: var(--good); }
    .league-badge { text-align: center; margin-bottom: 15px; }

    .btn { background: var(--ink); color: #fff; width: 100%; padding: 14px; border-radius: 14px; border: 0; font-weight: 900; cursor: pointer; margin-top: 10px; transition: .2s; }
    .btn:active { transform: scale(0.98); }
    .btn-choice { background: #fff; width: 100%; padding: 12px; border-radius: 12px; border: 1px solid rgba(0,0,0,.1); margin-top: 8px; text-align: left; cursor: pointer; }
    
    .hidden { display: none !important; }
    .nick-input-area { display: flex; gap: 8px; margin-top: 10px; }
    .nick-input { flex: 1; padding: 8px 12px; border: 2px solid var(--accent); border-radius: 12px; font-size: 14px; outline: none; }
    .btn-confirm { background: var(--good); color: #fff; border: none; padding: 8px 14px; border-radius: 12px; font-weight: bold; cursor: pointer; }
    
    .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,.6); z-index: 999; display: flex; align-items: center; justify-content: center; opacity: 0; pointer-events: none; transition: .3s; }
    .modal-overlay.show { opacity: 1; pointer-events: auto; }
    .modal-box { background: #fff; width: 90%; max-width: 380px; padding: 24px; border-radius: 24px; text-align: center; }
  </style>
</head>
<body>

<div class="wrap">
  <div class="topbar">
    <div class="brand">👃 鼻鼻大冒險</div>
    <div class="row">
      <button class="btnTiny" onclick="setNick()">修改暱稱</button>
      <button class="btnTiny" onclick="resetAll()">重置</button>
    </div>
  </div>

  <div class="map-container" id="mapView">
    <div class="path-line"></div>
    <div class="level-node active" id="node-w1" onclick="openLevel('w1')">
      <div class="level-icon">🌿</div>
      <div class="level-title">Week 1: 防線啟動</div>
    </div>
    <div class="level-node locked" id="node-w2" onclick="openLevel('w2')">
      <div class="level-icon">💆</div>
      <div class="level-title">Week 2: 舒緩實作</div>
    </div>
    <div class="level-node locked" onclick="alert('Week 3 敬請期待！✨')">
      <div class="level-icon">✨</div>
      <div class="level-title">Week 3: 待續</div>
    </div>
    <div class="trophy-btn" onclick="openLevel('leader')">🏆</div>
  </div>

  <div class="game-view" id="gameView">
    <div class="nav-header">
      <button class="btn-back" onclick="closeLevel()">⬅</button>
      <div class="view-title" id="viewTitle">關卡</div>
    </div>

    <div class="charStage" id="charArea">
      <div class="stats">
        <div class="statBox"><div class="statName">HP</div><div class="statVal" id="hpVal">60</div><div class="bar"><div id="hpBar" style="width:60%"></div></div></div>
        <div class="statBox"><div class="statName">LV</div><div class="statVal" id="lvVal">1</div><div class="bar"><div id="lvBar" style="width:0%"></div></div></div>
      </div>
      
      <div class="bibi-container" onclick="pokeBibi()">
         <div class="bibi-emoji" id="mainBibi">🤧</div>
         <div class="bibiMood" id="mood">👋</div>
      </div>

      <div class="tissue-pile hidden" id="tissuePile" onclick="clearTissue()">
         <div class="tp-emoji">🧻</div>
         <div class="tp-text">阿呀...救救我！<br>(點擊撥開)</div>
      </div>

      <div class="bubble">
        <div>
           <div style="font-size:12px;color:var(--bg2);font-weight:900;margin-bottom:4px">🗨️ 鼻鼻</div>
           <div class="bubbleText" id="bubbleText">......</div>
           <div id="nickInputArea" class="nick-input-area hidden">
              <input type="text" id="nickInput" class="nick-input" placeholder="輸入你的暱稱...">
              <button class="btn-confirm" onclick="confirmNick()">確認</button>
           </div>
        </div>
        <button class="btnNext hidden" id="btnNext" onclick="nextStory()">下一步 ▶</button>
      </div>
    </div>

    <div id="content-w1" class="hidden">
        <div id="w1-check" class="hidden">
           <div style="text-align:center;color:#666;margin-bottom:10px">你最近有這些狀況嗎？(O/X)</div>
           <div style="text-align:center;font-size:12px;color:#aaa;margin-bottom:5px;font-weight:bold;">( ❌ 沒有 ) ——— ( ⭕ 有 )</div>
           <div class="tinder-container" id="card-stage"></div>
           <div class="swipe-actions">
             <button class="btn-round btn-no" onclick="swipe('left')">❌</button>
             <button class="btn-round btn-yes" onclick="swipe('right')">⭕</button>
           </div>
        </div>
        <div id="w1-compare" class="hidden">
           <div style="font-weight:900;margin-bottom:10px;text-align:center">60秒判斷：哪一個是過敏的症狀呢？</div>
           <div class="compare-container">
             <div class="compare-card" onclick="selectCompare(this, 'A')">
                <div class="cc-title" style="color:var(--good)">卡片 A</div>
                <div class="cc-list">清鼻水<br>眼睛癢<br>沒有發燒</div>
             </div>
             <div class="compare-card" onclick="selectCompare(this, 'B')">
                <div class="cc-title" style="color:var(--bad)">卡片 B</div>
                <div class="cc-list">發燒<br>喉嚨痛<br>鼻涕變濃</div>
             </div>
           </div>
           <button class="btn" id="btn-compare" onclick="confirmCompare()">請選擇一張卡片</button>
        </div>
        <div id="w1-shield" class="hidden" style="text-align:center;padding:10px;">
           <div style="font-size:80px;margin-bottom:10px">🛡️</div>
           <div style="font-weight:900;color:var(--muted);font-size:18px;margin-bottom:10px">防護罩理論</div>
           <p style="color:#666;line-height:1.6;text-align:left">
             簡單說，就像身體的<b>『防護罩』</b>變弱了。<br><br>
             當防護罩守不住，冷風、灰塵一來，鼻子就會過度反應，發出警報！
           </p>
           <button class="btn" onclick="nextW1Step('type')">那我的防護罩哪裡破了？</button>
        </div>
        <div id="w1-type" class="hidden">
           <div style="font-weight:900;margin-bottom:10px;color:var(--muted)">你的體質防線 (可複選)</div>
           <div class="action-grid">
             <div class="action-card" id="type-A" onclick="toggleType('type-A', 'A')">
               <div class="action-emoji">🌬️</div><div><div class="action-text">風吹就發作 (肺氣弱)</div><div class="action-desc">一吹冷風，鼻子就癢</div></div>
             </div>
             <div class="action-card" id="type-B" onclick="toggleType('type-B', 'B')">
               <div class="action-emoji">🥘</div><div><div class="action-text">鼻塞＋腸胃 (脾氣弱)</div><div class="action-desc">鼻塞重、脹氣、便便軟</div></div>
             </div>
             <div class="action-card" id="type-C" onclick="toggleType('type-C', 'C')">
               <div class="action-emoji">❄️</div><div><div class="action-text">反覆很久型 (腎氣弱)</div><div class="action-desc">過敏多年、怕冷、手腳冷</div></div>
             </div>
           </div>
           <button class="btn" onclick="confirmType()">選好了，看對策！</button>
        </div>
        <div id="w1-room" class="hidden">
           <div style="text-align:center; margin-bottom:15px;">
             <div style="font-size:20px; margin-bottom:5px;">🧐</div>
             <div style="font-weight:900; color:var(--ink); font-size:16px;">環境大搜查！</div>
             <div style="font-size:13px; color:#666; line-height:1.5;">
               知道了體質還不夠，<b>環境</b>也是關鍵！<br>請點擊畫面，找出 3 個讓鼻子過敏的壞東西！
             </div>
           </div>
           <div class="room-container">
             <div class="ac-unit prop" id="prop-ac" onclick="findClue(1,this)" title="冷氣"><div class="ac-vent"></div></div>
             <div class="dust-monster prop" id="prop-dust" onclick="findClue(2,this)" title="灰塵">🌫️</div>
             <div class="bibi-room prop" id="prop-bibi" onclick="findClue(3,this)" title="鼻鼻">☯️</div>
           </div>
        </div>
        <div class="hidden" id="w1-done" style="text-align:center;padding:30px">
          <div style="font-size:50px;margin-bottom:10px">✨</div>
          <div style="font-weight:900;color:var(--good)">第一章完成！</div>
          <p style="font-size:13px;color:#888;line-height:1.6">原來鼻子是防護罩破了。<br>下週我們來學習怎麼修補它！</p>
          <button class="btn" onclick="closeLevel()">解鎖 Week 2 🔓</button>
        </div>
    </div>

    <div id="content-w2" class="hidden">
        
        <div id="w2-static-mode" style="text-align:center;">
           <div style="font-weight:900;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center;">
             <span>✋ 按摩圖解</span>
             <span id="acu-counter" style="font-size:12px;color:var(--muted)">1/4</span>
           </div>
           
           <div class="acu-stage-static" id="staticStage">
             <svg id="svg-face" class="svg-layer" viewBox="0 0 200 200">
                <circle cx="100" cy="100" r="92" class="face-fill"/>
                <circle cx="55" cy="120" r="18" class="cute-blush"/>
                <circle cx="145" cy="120" r="18" class="cute-blush"/>
                <path d="M50,70 Q65,60 80,70 M120,70 Q135,60 150,70" class="face-line"/>
                <path d="M55,90 Q65,80 75,90 M125,90 Q135,80 145,90" stroke="#4e342e" stroke-width="3" fill="none" stroke-linecap="round"/>
                <path d="M92,60 Q92,90 85,110" class="face-line" opacity="0.6"/> 
                <path d="M108,60 Q108,90 115,110" class="face-line" opacity="0.6"/>
                <path d="M85,120 Q100,135 115,120" class="face-line"/> 
                <path d="M75,115 Q65,120 78,130" class="face-line"/> 
                <path d="M125,115 Q135,120 122,130" class="face-line"/> 
                <path d="M80,150 Q100,165 120,150" class="face-line"/>
             </svg>

             <svg id="svg-hand" class="svg-layer hidden" viewBox="0 0 200 200">
                <path d="M10,140 L50,135" class="hand-fill" stroke="none"/>
                <path d="M10,180 L60,170" class="hand-fill" stroke="none"/>
                <path d="M10,140 Q40,135 70,110 Q100,90 140,95 L180,110 Q195,115 190,130 Q185,145 160,140 L120,130 Q100,130 90,140 Q70,160 50,165 Q30,170 10,180" class="hand-fill"/>
                <path d="M60,165 Q80,150 110,155 Q130,160 125,180 Q120,195 100,190 Q70,180 60,165" class="hand-fill"/>
                <path d="M70,120 Q90,140 110,135" fill="none" stroke="var(--skin-shadow)" stroke-width="3"/> 
                <path d="M85,160 Q100,170 115,165" fill="none" stroke="var(--skin-shadow)" stroke-width="3"/>
                <path d="M10,140 L50,135" fill="none" stroke="var(--line-brown)" stroke-width="3"/>
                <path d="M10,180 L60,170" fill="none" stroke="var(--line-brown)" stroke-width="3"/>
                <path d="M175,112 Q188,115 185,130 Q172,128 175,112" class="fingernail"/>
             </svg>
             
             <div class="static-point sp-yingxiang"></div><div class="static-point sp-yingxiang-r"></div>
             <div class="static-point sp-bitong"></div><div class="static-point sp-bitong-r"></div>
             <div class="static-point sp-zanzhu"></div><div class="static-point sp-zanzhu-r"></div>
             <div class="static-point sp-shangxing"></div>
             <div class="static-point sp-hegu"></div>
           </div>

           <div id="acu-name" style="font-weight:900;font-size:24px;margin-bottom:5px;color:var(--ink)"></div>
           <div id="acu-desc" style="font-size:14px;color:#666;margin-bottom:10px;min-height:50px;white-space:pre-line;line-height:1.5;"></div>
           
           <button class="btn" style="background:var(--accent2)" id="btn-ar-start" onclick="openAR()">📸 啟動 AR 魔鏡 (試試看！)</button>
           <button class="btn" onclick="nextAcuStatic()">下一個</button>
        </div>

        <div id="w2-ar-mode" class="acu-stage-ar">
            <button class="btn-ar-close" onclick="closeAR()">✕</button>
            <video class="input_video" playsinline></video>
            <canvas class="ar-canvas" id="output_canvas"></canvas>
            
            <div class="ar-ui">
               <div class="ar-msg" id="ar-status">請將臉對準中央...</div>
            </div>
            <div class="ar-timer" id="arTimer">10</div>
        </div>

        <div id="w2-summary" class="hidden" style="padding:10px;">
           <div style="text-align:center;font-size:50px;margin-bottom:10px">📜</div>
           <div style="text-align:center;font-weight:900;font-size:18px;margin-bottom:15px">鼻鼻的私房筆記</div>
           <div style="background:#fff;padding:15px;border-radius:12px;font-size:13px;color:#555;line-height:1.8;box-shadow:0 2px 8px rgba(0,0,0,0.05);">
             <b>🌞 日常保養：</b>早起或睡前按，氣血更順。<br>
             <b>🤧 過敏感冒：</b>多按「迎香、鼻通、上星」。<br>
             <b>👀 讀書疲勞：</b>多按「攢竹、合谷」明目。<br>
             <br>
             <div style="text-align:center;color:var(--good);font-weight:bold;">恭喜學會了防護罩修補術！</div>
           </div>
           <button class="btn" onclick="nextW2Step('action')">我記住了！</button>
        </div>

        <div id="w2-action" class="hidden">
           <div style="font-weight:900;margin-bottom:10px;color:var(--muted)">除了按摩，你願意嘗試哪些小改變？</div>
           <div style="font-size:12px;color:#aaa;margin-bottom:5px">(可多選)</div>
           <div class="action-grid" id="action-select-grid"></div>
           <button class="btn" onclick="submitActions()">我決定做這些改變！</button>
        </div>

        <div class="hidden" id="w2-quiz-area" style="margin-top:20px">
             <div style="font-weight:900;margin-bottom:10px;color:var(--bg2)">最後，鼻鼻想問問你...</div>
             <div id="w2q1">
               <div style="margin-bottom:10px;font-size:15px;line-height:1.4;">「你以前會不會覺得——<br>鼻子過敏就是體質不好，只能忍？」</div>
               <button class="btn-choice" onclick="answerQ('q1','一直忍耐','w2q1','w2q2')">好像一直都是這樣想</button>
               <button class="btn-choice" onclick="answerQ('q1','習慣了','w2q1','w2q2')">沒特別想過，但好像也習慣了</button>
               <button class="btn-choice" onclick="answerQ('q1','想過改善','w2q1','w2q2')">我其實有想過它是不是可以改善</button>
             </div>
             <div id="w2q2" class="hidden">
               <div style="margin-bottom:10px;font-size:15px;line-height:1.4;">「如果鼻子過敏不是天生注定，<br>而是跟環境、體質有關，<br>你會怎麼想？」</div>
               <button class="btn-choice" onclick="answerQ('q2','試試看','w2q2','w2q3')">那好像可以試試看調整</button>
               <button class="btn-choice" onclick="answerQ('q2','有可能','w2q2','w2q3')">聽起來有可能</button>
               <button class="btn-choice" onclick="answerQ('q2','不相信','w2q2','w2q3')">還不太相信</button>
             </div>
             <div id="w2q3" class="hidden">
               <div style="margin-bottom:10px;font-size:15px;line-height:1.4;">「如果有方法可以讓鼻子慢慢穩定，<br>你比較接近哪種想法？」</div>
               <button class="btn-choice" onclick="answerQ('q3','想看效果','w2q3','DONE')">想看看是不是真的有差</button>
               <button class="btn-choice" onclick="answerQ('q3','別太麻煩','w2q3','DONE')">只要不要太麻煩就好</button>
               <button class="btn-choice" onclick="answerQ('q3','先觀察','w2q3','DONE')">先觀察看看</button>
             </div>
        </div>

        <div class="hidden" id="w2-done" style="text-align:center;padding:30px">
          <div style="font-size:50px;margin-bottom:10px">🎉</div>
          <div style="font-weight:900;color:var(--good)">Week 2 完成！</div>
          <p style="font-size:13px;color:#888;line-height:1.6">你已經學會怎麼照顧鼻子了！<br>記得常常練習喔。</p>
          <button class="btn" onclick="closeLevel()">回到地圖</button>
        </div>
    </div>

    <div id="content-leader" class="hidden">
       <div class="league-badge">
         <div style="font-size:40px" id="badgeIcon">🥉</div>
         <div style="font-weight:900;color:var(--accent)" id="badgeName">銅牌新秀</div>
       </div>
       <div id="leaderList" class="leader-list"></div>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal" onclick="closeModal()">
  <div class="modal-box" onclick="event.stopPropagation()">
    <div style="font-size:40px;margin-bottom:10px" id="m-icon"></div>
    <div style="font-weight:900;font-size:18px;margin-bottom:10px" id="m-title"></div>
    <div style="font-size:14px;color:#666;line-height:1.5" id="m-desc"></div>
    <button class="btn" style="margin-top:15px" onclick="closeModal()">知道了</button>
  </div>
</div>

<script>
const GOOGLE_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzWT_39e3ev68nMQhniLyBIGgkD-gqqBUGiWJ_6uAuAR7ILTwRKbPDTrXy3rcR_EMQbNw/exec"; 
const DB_KEY = "bibi_final_v56_hybrid"; 

const W1_CARDS = [
  { i:"🤧", t:"早上一直打噴嚏", d:"衛生紙包餛飩？" },
  { i:"🥶", t:"冷氣房特別不舒服", d:"一進去就鼻塞？" },
  { i:"😤", t:"鼻塞到很煩", d:"呼吸都不順暢" },
  { i:"😷", t:"常被說感冒？", d:"其實你沒感冒" }
];

const ACTION_OPTIONS = [
  { t:"少吃冰的", img:"🧊" }, { t:"定期洗床單", img:"🛏️" }, { t:"規律服藥", img:"💊" }, { t:"出門戴口罩", img:"😷" },
  { t:"少放絨毛娃娃", img:"🧸" }, { t:"作息固定", img:"⏰" }, { t:"保持運動", img:"🏃" }, { t:"起床穿外套", img:"🧥" }
];

let s = {}; try { s = JSON.parse(localStorage.getItem(DB_KEY)||"{}"); } catch(e) { s = {}; }
if(!s.hp) s = { hp:60, lv:1, exp:0, w1d:false, w2d:false, nick:"" };

function save(){ localStorage.setItem(DB_KEY, JSON.stringify(s)); updateMap(); }

let curLevel = "";
let storyIdx = 0;
let cardIdx = 0;
let cluesFound = 0;
let w1Data = { symptoms:[], type:[], actions:[] };
let w2Answers = { q1:"", q2:"", q3:"" };
let selectedCompareType = null; 

// 按摩參數
let acuStep = 0;
const ACU_STEPS = [
  { n:"1. 迎香 & 鼻通", d:"📍位置：鼻翼旁凹陷處 (迎香) & 法令紋上端 (鼻通)\n✨功效：強力通鼻，緩解鼻塞流鼻水", css:"sp-yingxiang,sp-yingxiang-r,sp-bitong,sp-bitong-r", type:"face", ar:[203,423,196,419] },
  { n:"2. 攢竹", d:"📍位置：眉頭內側邊緣凹陷處\n✨功效：明目舒壓，緩解頭痛眼痠", css:"sp-zanzhu,sp-zanzhu-r", type:"face", ar:[55,285] },
  { n:"3. 上星", d:"📍位置：髮際線正中央，往上一拇指寬\n✨功效：清腦通竅，舒緩鼻炎不適", css:"sp-shangxing", type:"face", ar:[10] },
  { n:"4. 合谷", d:"📍位置：手背虎口，肌肉隆起最高點\n✨功效：止痛要穴，增強免疫力", css:"sp-hegu", type:"hand", ar:[] } // 手部不支援AR
];

// AR 變數
let camera = null;
let faceMesh = null;
let isARActive = false;
let isPressing = false;
let pressStartTime = 0;
let touchX = 0, touchY = 0;
let isTouching = false;

function updateMap(){
  const n1 = document.getElementById("node-w1");
  const n2 = document.getElementById("node-w2");
  if(s.w1d) { n1.classList.add("done"); n1.classList.remove("active"); n2.classList.remove("locked"); n2.classList.add("active"); }
  if(s.w2d) { n2.classList.add("done"); n2.classList.remove("active"); }
}

function openLevel(lvl){
  if(lvl==='w2' && !s.w1d) return alert("🔒 請先完成 Week 1");
  curLevel = lvl;
  document.getElementById("mapView").classList.add("hidden");
  document.getElementById("gameView").classList.add("show");
  document.getElementById("content-w1").classList.add("hidden");
  document.getElementById("content-w2").classList.add("hidden");
  document.getElementById("content-leader").classList.add("hidden");
  document.getElementById("charArea").classList.remove("hidden");

  if(lvl==='w1'){
    document.getElementById("viewTitle").innerText = "Week 1: 防線啟動";
    document.getElementById("content-w1").classList.remove("hidden");
    if(s.w1d) { document.getElementById("w1-done").classList.remove("hidden"); document.getElementById("tissuePile").classList.add("hidden"); return; }
    document.getElementById("tissuePile").classList.remove("hidden");
    ["w1-check", "w1-compare", "w1-shield", "w1-type", "w1-room"].forEach(id => document.getElementById(id).classList.add("hidden"));
  } 
  else if(lvl==='w2'){
    document.getElementById("viewTitle").innerText = "Week 2: 舒緩實作";
    document.getElementById("content-w2").classList.remove("hidden");
    if(s.w2d) { document.getElementById("w2-done").classList.remove("hidden"); return; }
    if(!s.w2s) playStory('w2-intro');
    else { updateAcuStatic(); }
  }
  else if(lvl==='leader'){
    document.getElementById("viewTitle").innerText = "🏆 排行榜";
    document.getElementById("content-leader").classList.remove("hidden");
    document.getElementById("charArea").classList.add("hidden");
    renderLeader();
  }
  renderStats();
}

function closeLevel(){
  document.getElementById("gameView").classList.remove("show");
  document.getElementById("mapView").classList.remove("hidden");
  updateMap();
}

function clearTissue(){
  document.getElementById("tissuePile").classList.add("hidden");
  if(!s.nick) playStory('w1-intro'); else playStory('w1-symptom');
}

function playStory(type){
  storyIdx = 0; curLevel = type;
  document.getElementById("btnNext").classList.remove("hidden");
  renderStoryFrame();
}

function renderStoryFrame(){
  let list = [];
  const n = s.nick || "小主人";
  if(curLevel === 'w1-intro') list = [{ t: "阿呀... 救命啊！\n謝謝你救了我！💨", m: "😄" }, { t: "我是鼻鼻，很高興認識你！\n請問救命恩人怎麼稱呼？", m: "👋", action: "askNick" }];
  else if (curLevel === 'w1-symptom') list = [{ t: n+"，其實這是我最近的慘況...\n最近我常常一連串地打噴嚏，鼻子裡面也很癢。", m: "🤧" }, { t: "有時候鼻子會悶悶的，呼吸不太順，早上起床時特別明顯。", m: "😐" }, { t: "我發現只要遇到灰塵、花粉，或冷氣開很強的時候，這些不舒服好像就會更明顯。", m: "🌡️" }, { t: "你可以陪我玩個小遊戲，確認一下我們什麼時候比較容易不舒服嗎？", m: "🤔" }];
  else if (curLevel === 'w1-mid') list = [{ t: "原來不只你，很多人都有這樣的狀況。\n等等... 這麼多症狀...", m: "🤔" }, { t: "我們來確認一下，這到底是感冒還是過敏？", m: "🧐" }];
  else if (curLevel === 'w2-intro') list = [{ t: "小主人，上週我們找到了防護罩破洞的原因。\n這週我們要來學「修補」的方法！", m: "✨" }, { t: "我會教你幾個神奇的按鈕（穴道），按了會很舒服喔！", m: "💆" }];

  const item = list[storyIdx];
  if(!item) return nextStory(); 
  if(item.action === 'askNick') { document.getElementById("bubbleText").innerText = item.t; document.getElementById("nickInputArea").classList.remove("hidden"); document.getElementById("btnNext").classList.add("hidden"); return; }
  document.getElementById("bubbleText").innerText = item.t;
  document.getElementById("mood").innerText = item.m;
}

function nextStory(){
  const listMap = { 'w1-intro':2, 'w1-symptom':4, 'w1-mid':2, 'w2-intro':2 };
  storyIdx++;
  if(storyIdx >= listMap[curLevel]) {
    document.getElementById("btnNext").classList.add("hidden");
    if(curLevel === 'w1-intro') playStory('w1-symptom');
    if(curLevel === 'w1-symptom') { document.getElementById("w1-check").classList.remove("hidden"); renderCard(); } 
    if(curLevel === 'w1-mid') nextW1Step('compare');
    if(curLevel === 'w2-intro') { s.w2s=true; save(); updateAcuStatic(); }
  } else { renderStoryFrame(); }
}

function confirmNick(){
  const val = document.getElementById("nickInput").value.trim();
  if(!val) return alert("請輸入暱稱");
  s.nick=val; save(); document.getElementById("nickInputArea").classList.add("hidden"); playStory('w1-symptom');
}

// === W1 Logic ===
function nextW1Step(step) {
  ["w1-check", "w1-compare", "w1-shield", "w1-type", "w1-room"].forEach(id => document.getElementById(id).classList.add("hidden"));
  if(step === 'compare') document.getElementById("w1-compare").classList.remove("hidden");
  else if(step === 'shield') document.getElementById("w1-shield").classList.remove("hidden");
  else if(step === 'type') { document.getElementById("w1-type").classList.remove("hidden"); document.getElementById("bubbleText").innerText = "每個人防護罩破洞的地方不太一樣。\n下面這三種，哪幾種跟你比較像？(可複選)"; }
  else if(step === 'room') { document.getElementById("w1-room").classList.remove("hidden"); document.getElementById("bubbleText").innerText = "環境大搜查！\n請幫我找出房間裡有 3 個讓鼻子不舒服的兇手！"; }
}

function renderCard(){
  const box = document.getElementById("card-stage"); box.innerHTML = "";
  if(cardIdx >= W1_CARDS.length) { document.getElementById("w1-check").classList.add("hidden"); playStory('w1-mid'); return; }
  const c = W1_CARDS[cardIdx];
  const div = document.createElement("div"); div.className = "tinder-card"; div.id = "active-card";
  div.innerHTML = `<div class="tc-icon">${c.i}</div><div class="tc-title">${c.t}</div><div class="tc-desc">${c.d}</div>`;
  box.appendChild(div);
}

function swipe(dir){
  const card = document.getElementById("active-card"); if(!card) return;
  card.classList.add(dir==='left'?'swipe-left':'swipe-right');
  if(dir==='right') w1Data.symptoms.push(W1_CARDS[cardIdx].t);
  setTimeout(()=>{ cardIdx++; renderCard(); }, 400);
}

function selectCompare(el, type){ document.querySelectorAll(".compare-card").forEach(c=>c.classList.remove("selected")); el.classList.add("selected"); selectedCompareType = type; document.getElementById('btn-compare').innerText = "我是卡片 " + type; }
function confirmCompare(){
  if(!selectedCompareType) return alert("請選擇一張卡片");
  if(selectedCompareType === 'B') { alert("卡片 B 有發燒喔！過敏通常不會發燒。"); return; }
  alert("沒錯！卡片 A 比較像過敏喔！"); document.getElementById("bubbleText").innerText = "既然不是感冒病毒在作怪，那為什麼鼻子會失控呢？"; nextW1Step('shield');
}

function toggleType(id, type){
  const el = document.getElementById(id);
  if(w1Data.type.includes(type)) { w1Data.type = w1Data.type.filter(t => t !== type); el.classList.remove("selected"); }
  else { if(w1Data.type.length >= 2) return alert("最多選 2 個喔！"); w1Data.type.push(type); el.classList.add("selected"); }
}
function confirmType(){ if(w1Data.type.length===0) return alert("請至少選一種"); let adv=""; if(w1Data.type.includes('A')) adv+="🌬️ 風吹型：保暖＋迎香穴\n"; if(w1Data.type.includes('B')) adv+="🥘 腸胃型：少吃冰＋足三里\n"; if(w1Data.type.includes('C')) adv+="❄️ 怕冷型：規律睡＋保暖\n"; alert("收到！本週加成祕技：\n\n"+adv); nextW1Step('room'); }

const W1_CLUES = { 1:{t:"環境",i:"❄️",d:"冷氣或溫差",r:"一進冷氣房就不舒服"}, 2:{t:"過敏原",i:"🌫️",d:"灰塵花粉",r:"碰到灰塵就想打噴嚏"}, 3:{t:"中醫觀點",i:"🧘",d:"體質差異",r:"因為體質不同，所以症狀也不同。\n原來不是每個人都一樣！"} };
function findClue(id, el){
  if(el.classList.contains("found")) return;
  const c = W1_CLUES[id];
  document.getElementById("m-icon").innerText = c.i; document.getElementById("m-title").innerText = c.t; document.getElementById("m-desc").innerText = c.r;
  document.getElementById("modal").classList.add("show");
  el.classList.add("found"); cluesFound++;
  if(cluesFound >= 3) { setTimeout(finishW1, 1500); }
}

function finishW1(){
  s.w1d = true; s.exp += 50; save();
  sendData({ w1_done: "Yes", w1_symptoms: w1Data.symptoms.join(", "), w1_type: w1Data.type.join("+"), exp: s.exp });
  document.getElementById("w1-room").classList.add("hidden");
  document.getElementById("w1-done").classList.remove("hidden");
  document.getElementById("bubbleText").innerText = "Week 1 完成！\n原來是防護罩破了，下週來修補它！";
}

// === W2 靜態圖解模式 ===
function nextAcuStatic(){
  acuStep++;
  if(acuStep >= ACU_STEPS.length) { 
     document.getElementById("w2-static-mode").classList.add("hidden");
     document.getElementById("w2-summary").classList.remove("hidden");
     document.getElementById("bubbleText").innerText = "做得好！這些祕訣要記起來喔！";
     return;
  }
  updateAcuStatic();
}

function updateAcuStatic(){
  const s = ACU_STEPS[acuStep];
  document.getElementById("acu-name").innerText = s.n;
  document.getElementById("acu-desc").innerText = s.d;
  document.getElementById("acu-counter").innerText = (acuStep+1) + "/" + ACU_STEPS.length;
  
  // 切換圖示 (SVG)
  const faceSVG = document.getElementById("svg-face");
  const handSVG = document.getElementById("svg-hand");
  const btnAR = document.getElementById("btn-ar-start");
  
  if(s.type === 'hand') {
     faceSVG.classList.add("hidden");
     handSVG.classList.remove("hidden");
     btnAR.style.display = 'none'; // 手部無 AR
  } else {
     faceSVG.classList.remove("hidden");
     handSVG.classList.add("hidden");
     btnAR.style.display = 'block'; // 臉部有 AR
  }

  // 顯示對應紅點
  document.querySelectorAll(".static-point").forEach(el => el.style.display = 'none');
  s.css.split(",").forEach(c => {
      const el = document.querySelector("."+c);
      if(el) el.style.display = 'block';
  });
}

// === W2 AR 模式 (結合 V53) ===
async function openAR() {
    document.getElementById("w2-ar-mode").style.display = "flex";
    document.getElementById("ar-status").innerText = "啟動相機中...";
    
    // 初始化 MediaPipe (只做一次)
    if(!faceMesh) {
        faceMesh = new FaceMesh({locateFile: (file) => `https://cdn.jsdelivr.net/npm/@mediapipe/face_mesh/${file}`});
        faceMesh.setOptions({maxNumFaces: 1, refineLandmarks: true, minDetectionConfidence: 0.5, minTrackingConfidence: 0.5});
        faceMesh.onResults(onARResults);
    }
    
    const videoElement = document.getElementsByClassName('input_video')[0];
    if(!camera) {
        camera = new Camera(videoElement, {
            onFrame: async () => { await faceMesh.send({image: videoElement}); },
            width: 1280, height: 720
        });
    }
    
    try {
        await camera.start();
        isARActive = true;
    } catch(err) {
        alert("無法啟動相機 (請確認 HTTPS 或權限)");
        closeAR();
    }
}

function closeAR() {
    document.getElementById("w2-ar-mode").style.display = "none";
    // 不用真的關閉相機，隱藏就好，避免下次開啟要重讀
    isARActive = false; 
}

// AR 觸控邏輯
const arCanvas = document.getElementById('output_canvas');
const arCtx = arCanvas.getContext('2d');

arCanvas.addEventListener('touchstart', (e) => { isTouching = true; updateTouchPos(e.touches[0]); });
arCanvas.addEventListener('touchmove', (e) => { if(isTouching) updateTouchPos(e.touches[0]); });
arCanvas.addEventListener('touchend', () => { isTouching = false; isPressing = false; document.getElementById('arTimer').style.display='none'; });

function updateTouchPos(e) {
    const rect = arCanvas.getBoundingClientRect();
    let rawX = e.clientX - rect.left;
    touchX = arCanvas.width - (rawX * (arCanvas.width / rect.width)); // 鏡像修正
    touchY = (e.clientY - rect.top) * (arCanvas.height / rect.height);
}

function onARResults(results) {
    if(!isARActive) return;
    
    arCanvas.width = results.image.width;
    arCanvas.height = results.image.height;
    arCtx.save();
    arCtx.clearRect(0, 0, arCanvas.width, arCanvas.height);
    arCtx.drawImage(results.image, 0, 0, arCanvas.width, arCanvas.height);

    if (results.multiFaceLandmarks && results.multiFaceLandmarks.length > 0) {
        document.getElementById("ar-status").innerText = "按住紅點 10 秒！";
        const landmarks = results.multiFaceLandmarks[0];
        const s = ACU_STEPS[acuStep];
        let hit = false;

        // 畫觸控點 (黃圈)
        if(isTouching) {
            arCtx.beginPath();
            arCtx.arc(touchX, touchY, 30, 0, 2 * Math.PI);
            arCtx.fillStyle = "rgba(255,255,0,0.3)"; arCtx.fill();
            arCtx.strokeStyle = "yellow"; arCtx.lineWidth = 3; arCtx.stroke();
        }

        for (const id of s.ar) {
            const pt = landmarks[id];
            const x = pt.x * arCanvas.width;
            const y = pt.y * arCanvas.height;
            
            // 判定觸控
            if(isTouching && Math.sqrt(Math.pow(x-touchX, 2) + Math.pow(y-touchY, 2)) < 80) hit = true;

            // 畫紅點
            arCtx.beginPath();
            arCtx.arc(x, y, 10, 0, 2 * Math.PI);
            arCtx.fillStyle = "red"; arCtx.fill();
            
            // 畫進度
            if(hit && isPressing) {
                let progress = Math.min((Date.now() - pressStartTime)/10000, 1);
                arCtx.beginPath();
                arCtx.arc(x, y, 30, -0.5*Math.PI, (2*Math.PI*progress)-0.5*Math.PI);
                arCtx.strokeStyle = "#00ff00"; arCtx.lineWidth = 6; arCtx.stroke();
            }
        }

        if(hit) {
            if(!isPressing) { isPressing = true; pressStartTime = Date.now(); }
            let sec = Math.ceil(10 - (Date.now() - pressStartTime)/1000);
            if(sec <= 0) {
                closeAR();
                alert("太棒了！完成按摩！");
                nextAcuStatic(); // 跳下一關
            } else {
                const t = document.getElementById('arTimer');
                t.innerText = sec; t.style.display = 'block';
            }
        } else {
            isPressing = false; document.getElementById('arTimer').style.display='none';
        }
    }
    arCtx.restore();
}

// === W2 後半段 ===
function nextW2Step(step){
  document.getElementById("w2-summary").classList.add("hidden");
  if(step === 'action') {
     document.getElementById("w2-action").classList.remove("hidden");
     document.getElementById("bubbleText").innerText = "學會按摩了，再來挑幾個願意做的小改變吧！";
     renderActionGrid();
  }
}

function renderActionGrid(){
  document.getElementById("action-select-grid").innerHTML = ACTION_OPTIONS.map((a, i) => `
    <div class="action-card" id="act-${i}" onclick="toggleAction(${i})">
      <div class="action-emoji">${a.img}</div><div class="action-text">${a.t}</div>
    </div>`).join("");
}

function toggleAction(i) {
  const el = document.getElementById(`act-${i}`);
  const actionName = ACTION_OPTIONS[i].t;
  if(w1Data.actions.includes(actionName)) {
     w1Data.actions = w1Data.actions.filter(x => x !== actionName);
     el.classList.remove("selected");
  } else {
     w1Data.actions.push(actionName);
     el.classList.add("selected");
  }
}

function submitActions(){
  if(w1Data.actions.length === 0) return alert("請至少選一個喔！");
  alert("太棒了！我們一起努力！");
  document.getElementById("w2-action").classList.add("hidden");
  document.getElementById("w2-quiz-area").classList.remove("hidden");
}

function answerQ(qid, ans, hideId, showId){
  w2Answers[qid] = ans;
  if(showId === 'DONE') { finishW2(); } else { document.getElementById(hideId).classList.add("hidden"); document.getElementById(showId).classList.remove("hidden"); }
}

function finishW2(){
  if(s.w2d) return;
  s.w2d = true; s.exp += 50; save();
  const actionStr = w1Data.actions.join(", ");
  sendData({ w2_done: "Yes", q1_ans: w2Answers.q1, q2_ans: w2Answers.q2, q3_ans: w2Answers.q3, w1_symptoms: "行動承諾: " + actionStr, exp: s.exp });
  document.getElementById("w2-quiz-area").classList.add("hidden");
  document.getElementById("w2-done").classList.remove("hidden");
}

function sendData(payload) {
  const finalData = Object.assign({}, { nick: s.nick, exp: s.exp }, payload);
  fetch(GOOGLE_SCRIPT_URL, { method: 'POST', mode: 'no-cors', body: JSON.stringify(finalData), headers: {'Content-Type': 'application/json'} });
}

function renderStats(){ document.getElementById("hpVal").innerText = s.hp; document.getElementById("hpBar").style.width = s.hp+"%"; document.getElementById("lvVal").innerText = s.lv; document.getElementById("lvBar").style.width = ((s.exp%50)*2)+"%"; }

function renderLeader(){
  const myExp = s.exp;
  let rankIcon = "🥉", rankName = "銅牌新秀", rankColor = "#cd7f32";
  if(myExp >= 50) { rankIcon="🥈"; rankName="銀牌高手"; rankColor="#7f8c8d"; }
  if(myExp >= 100) { rankIcon="💎"; rankName="鑽石大師"; rankColor="#3498db"; }
  document.getElementById("badgeIcon").innerText = rankIcon;
  document.getElementById("badgeName").innerText = rankName;
  document.getElementById("badgeName").style.color = rankColor;
  
  const list = [{n:"小傑",e:myExp+15,a:"👦"},{n:"阿豪",e:myExp+5,a:"😎"},{n:"Yumi",e:Math.max(0,myExp-10),a:"👧"},{n:s.nick||"我",e:myExp,a:"😊",me:true}].sort((a,b)=>b.e-a.e);
  document.getElementById("leaderList").innerHTML = list.map((u,i)=>`<div class="leader-item ${u.me?'me':''}"><div class="rank-num">${i+1}</div><div class="user-info"><div class="user-avatar">${u.a}</div><div class="user-name">${u.n}</div></div><div class="user-exp">${u.e} EXP</div></div>`).join("");
}

function closeModal(){ document.getElementById("modal").classList.remove("show"); }
function pokeBibi(){ if(curStory) return; document.getElementById("bubbleText").innerText = "今天也要加油喔！"; }
function setNick(){ const n=prompt("修改暱稱", s.nick); if(n){s.nick=n;save();} }
function resetAll(){ localStorage.removeItem(DB_KEY); location.reload(); }

renderStats();
updateMap();
</script>
</body>
</html>
