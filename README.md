<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>⚡ 廣島・山陽旅遊 | Cyber Travel HUD</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">

  <!-- APP 桌面專屬圖示 (科技酷黑風) -->
  <link rel="apple-touch-icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180'%3E%3Crect width='180' height='180' fill='%230F172A'/%3E%3Ccircle cx='90' cy='90' r='60' fill='none' stroke='%2338BDF8' stroke-width='8'/%3E%3Cpath d='M60 90 L120 90 M90 60 L90 120' stroke='%234ADE80' stroke-width='8' stroke-linecap='round'/%3E%3C/svg%3E" />
  <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 180 180'%3E%3Crect width='180' height='180' rx='36' fill='%230F172A'/%3E%3Ccircle cx='90' cy='90' r='60' fill='none' stroke='%2338BDF8' stroke-width='8'/%3E%3Cpath d='M60 90 L120 90 M90 60 L90 120' stroke='%234ADE80' stroke-width='8' stroke-linecap='round'/%3E%3C/svg%3E" />

  <style>
    :root {
      --bg-dark: #090D16;
      --card-bg: #131D31;
      --card-border: #1E293B;
      --text-main: #F8FAFC;
      --text-sub: #94A3B8;
      --neon-blue: #38BDF8;
      --neon-green: #4ADE80;
      --neon-purple: #C084FC;
      --neon-pink: #F43F5E;
      --neon-yellow: #FACC15;
      --accent-glow: rgba(56, 189, 248, 0.15);
    }

    * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    html { scroll-behavior: smooth; }

    body {
      margin: 0; padding: 0;
      background-color: var(--bg-dark);
      background-image: 
        radial-gradient(circle at 50% 0%, rgba(56, 189, 248, 0.08) 0%, transparent 50%),
        radial-gradient(circle at 100% 100%, rgba(192, 132, 252, 0.05) 0%, transparent 50%);
      color: var(--text-main);
      font-family: 'Inter', 'Noto Sans TC', -apple-system, BlinkMacSystemFont, sans-serif;
      line-height: 1.6; padding-top: 76px; padding-bottom: 50px;
    }

    .app-container { max-width: 480px; margin: 0 auto; padding: 0 16px; }

    /* 頂部毛玻璃導覽列 */
    .top-nav {
      position: fixed; top: 0; left: 0; right: 0; z-index: 100;
      background: rgba(9, 13, 22, 0.85);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--card-border);
      padding: 10px 12px;
    }
    .top-nav-inner {
      max-width: 480px; margin: 0 auto; display: flex; align-items: center;
      gap: 8px; overflow-x: auto; scrollbar-width: none; white-space: nowrap;
      padding-bottom: 2px;
    }
    .top-nav-inner::-webkit-scrollbar { display: none; }

    .nav-pill {
      flex-shrink: 0; text-decoration: none; color: var(--text-sub); font-size: 13.5px; font-weight: 600;
      padding: 6px 14px; border-radius: 999px; border: 1px solid var(--card-border); background: var(--card-bg);
      cursor: pointer; display: inline-flex; align-items: center; gap: 5px; transition: all 0.2s ease;
    }
    .nav-pill:hover { color: var(--text-main); border-color: var(--neon-blue); }
    .nav-pill.active {
      color: var(--bg-dark); background: var(--neon-blue); border-color: var(--neon-blue);
      box-shadow: 0 0 12px rgba(56, 189, 248, 0.4); font-weight: 700;
    }

    /* 科技感卡片 */
    .ehon-card {
      background: var(--card-bg); border: 1px solid var(--card-border);
      border-radius: 20px; box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
      padding: 18px; margin-bottom: 16px; position: relative; overflow: hidden;
    }
    .ehon-card::before {
      content: ""; position: absolute; top: 0; left: 0; right: 0; height: 1px;
      background: linear-gradient(90deg, transparent, rgba(56, 189, 248, 0.3), transparent);
    }

    .ehon-badge {
      display: inline-flex; align-items: center; gap: 5px; padding: 4px 12px;
      border: 1px solid rgba(56, 189, 248, 0.3); border-radius: 999px; font-size: 12px; font-weight: 600;
      background: rgba(56, 189, 248, 0.1); color: var(--neon-blue);
    }

    .hero-card { padding: 0; overflow: hidden; background: var(--card-bg); }
    .hero-svg-wrap { width: 100%; background: #0A1124; border-bottom: 1px solid var(--card-border); display: block; line-height: 0; }
    .hero-info { padding: 18px 20px 20px; }
    .hero-title { margin: 0 0 6px; font-size: 24px; font-weight: 800; letter-spacing: -0.5px; color: #F8FAFC; }
    .hero-sub { margin: 0; font-size: 14px; color: var(--text-sub); }

    .countdown-row { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; gap: 8px; }
    .countdown-pill {
      background: rgba(250, 204, 21, 0.1); border: 1px solid rgba(250, 204, 21, 0.3);
      color: var(--neon-yellow); border-radius: 999px; padding: 6px 14px; font-size: 13px; font-weight: 700;
    }
    .sync-status { font-size: 11.5px; font-weight: 600; padding: 5px 12px; border-radius: 999px; border: 1px solid rgba(74, 222, 128, 0.3); background: rgba(74, 222, 128, 0.1); color: var(--neon-green); white-space: nowrap; }

    .section-header { display: flex; align-items: center; justify-content: space-between; margin: 12px 0 14px; }
    .section-title { margin: 0; font-size: 20px; font-weight: 800; letter-spacing: -0.5px; }

    .tab-section { display: none; }
    .tab-section.active-section { display: block; }

    .flight-route { display: flex; align-items: center; justify-content: space-between; margin: 14px 0 12px; }
    .airport-code { font-size: 26px; font-weight: 900; line-height: 1.1; letter-spacing: 1px; color: var(--neon-blue); }
    .airport-name { font-size: 12px; color: var(--text-sub); margin-top: 2px; }
    .flight-time { font-size: 15px; font-weight: 700; margin-top: 4px; }
    .plane-circle {
      width: 44px; height: 44px; border-radius: 50%; background: rgba(56, 189, 248, 0.1);
      border: 1px solid rgba(56, 189, 248, 0.3); display: flex; align-items: center; justify-content: center; font-size: 18px;
    }
    .flight-footer { border-top: 1px dashed var(--card-border); padding-top: 10px; font-size: 13px; color: var(--text-sub); display: flex; justify-content: space-between; }
    .flight-footer strong { color: var(--text-main); }

    .day-tabs { display: flex; gap: 8px; overflow-x: auto; padding: 4px 2px 12px; margin-bottom: 12px; scrollbar-width: none; }
    .day-tabs::-webkit-scrollbar { display: none; }
    .day-tab {
      flex-shrink: 0; background: var(--card-bg); color: var(--text-sub); border: 1px solid var(--card-border);
      border-radius: 14px; padding: 8px 16px; font-family: inherit; cursor: pointer; text-align: center; transition: all 0.2s;
    }
    .day-tab:hover { border-color: var(--neon-blue); color: var(--text-main); }
    .day-tab.active { background: var(--neon-blue); color: var(--bg-dark); border-color: var(--neon-blue); font-weight: 800; box-shadow: 0 0 12px rgba(56, 189, 248, 0.3); }
    .day-tab-num { display: block; font-size: 15px; font-weight: 800; }
    .day-tab-date { display: block; font-size: 11.5px; opacity: 0.85; }

    .timeline-wrap { position: relative; padding-left: 24px; }
    .timeline-wrap::before { content: ""; position: absolute; left: 8px; top: 18px; bottom: 18px; width: 2px; background: var(--card-border); }
    .timeline-item { position: relative; margin-bottom: 16px; }
    .timeline-dot {
      position: absolute; left: -28px; top: 20px; width: 14px; height: 14px; border-radius: 50%;
      border: 2px solid var(--neon-blue); background: var(--bg-dark); z-index: 2; box-shadow: 0 0 8px var(--neon-blue);
    }
    .timeline-item:nth-child(2n) .timeline-dot { border-color: var(--neon-purple); box-shadow: 0 0 8px var(--neon-purple); }
    .timeline-item:nth-child(3n) .timeline-dot { border-color: var(--neon-green); box-shadow: 0 0 8px var(--neon-green); }

    .spot-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 8px; }
    .spot-time { font-size: 13.5px; font-weight: 700; color: var(--neon-blue); margin-bottom: 2px; font-family: monospace; }
    .spot-title { font-size: 17px; font-weight: 700; margin: 0 0 6px; color: var(--text-main); }
    .must-go-badge {
      flex-shrink: 0; background: rgba(244, 63, 94, 0.15); border: 1px solid rgba(244, 63, 94, 0.4);
      color: var(--neon-pink); border-radius: 999px; padding: 2px 10px; font-size: 11.5px; font-weight: 700;
    }

    .spot-note { font-size: 13.5px; margin: 0 0 12px; color: var(--text-sub); }
    .spot-tags { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; }

    .transport-tag { display: inline-flex; align-items: center; padding: 3px 10px; border-radius: 999px; font-size: 12px; font-weight: 600; border: 1px solid var(--card-border); }
    .t-bus { background: rgba(244, 63, 94, 0.1); color: #FDA4AF; border-color: rgba(244, 63, 94, 0.3); }
    .t-train { background: rgba(74, 222, 128, 0.1); color: #86EFAC; border-color: rgba(74, 222, 128, 0.3); }
    .t-walk { background: rgba(56, 189, 248, 0.1); color: #7DD3FC; border-color: rgba(56, 189, 248, 0.3); }
    .t-ship { background: rgba(250, 204, 21, 0.1); color: #FDE047; border-color: rgba(250, 204, 21, 0.3); }
    .t-subway { background: rgba(192, 132, 252, 0.1); color: #D8B4FE; border-color: rgba(192, 132, 252, 0.3); }
    .t-shinkansen { background: rgba(45, 212, 191, 0.1); color: #99F6E4; border-color: rgba(45, 212, 191, 0.3); }

    .nav-map-btn {
      display: inline-flex; align-items: center; gap: 4px; padding: 4px 12px;
      background: rgba(56, 189, 248, 0.1); color: var(--neon-blue); text-decoration: none;
      border: 1px solid rgba(56, 189, 248, 0.3); border-radius: 10px; font-size: 12px; font-weight: 600;
      transition: all 0.2s;
    }
    .nav-map-btn:hover { background: var(--neon-blue); color: var(--bg-dark); }

    .card-actions { display: flex; gap: 6px; margin-left: auto; }
    .action-btn {
      background: rgba(255, 255, 255, 0.05); border: 1px solid var(--card-border); color: var(--text-sub);
      border-radius: 8px; padding: 3px 8px; font-family: inherit; font-size: 12px; font-weight: 600; cursor: pointer; transition: all 0.2s;
    }
    .action-btn:hover { color: var(--text-main); border-color: var(--text-sub); }
    .action-btn.del { background: rgba(244, 63, 94, 0.1); color: #FDA4AF; border-color: rgba(244, 63, 94, 0.3); }

    /* 子行程收闔區塊 */
    .sub-items-box {
      margin-top: 12px; padding-top: 10px; border-top: 1px dashed var(--card-border);
      display: flex; flex-direction: column; gap: 8px;
    }
    .sub-item-row {
      background: rgba(9, 13, 22, 0.5); border: 1px solid var(--card-border); border-radius: 10px;
      padding: 8px 10px; font-size: 12.5px; display: flex; justify-content: space-between; align-items: flex-start; gap: 8px; color: var(--text-sub);
    }
    .sub-item-tag {
      display: inline-block; background: rgba(56, 189, 248, 0.15); border: 1px solid rgba(56, 189, 248, 0.3);
      color: var(--neon-blue); border-radius: 999px; padding: 1px 7px; font-size: 10.5px; font-weight: 700; margin-right: 4px;
    }

    .tip-banner {
      background: rgba(74, 222, 128, 0.08); border: 1px solid rgba(74, 222, 128, 0.25);
      border-radius: 16px; padding: 12px 14px; font-size: 13.5px; font-weight: 500; color: #86EFAC;
      margin: 14px 0 20px; display: flex; align-items: center; gap: 10px;
    }

    .hotel-table { width: 100%; border-collapse: collapse; margin: 10px 0 14px; font-size: 13.5px; }
    .hotel-table td { padding: 4px 0; vertical-align: top; }
    .hotel-table td:first-child { width: 75px; color: var(--text-sub); }
    .hotel-table td:last-child { color: var(--text-main); font-weight: 500; }

    .btn-coral {
      display: inline-flex; align-items: center; justify-content: center; gap: 6px;
      background: var(--neon-blue); color: var(--bg-dark); text-decoration: none;
      border: none; border-radius: 12px; padding: 8px 16px;
      font-size: 13.5px; font-weight: 700; cursor: pointer; font-family: inherit;
      box-shadow: 0 0 15px rgba(56, 189, 248, 0.3); transition: all 0.2s;
    }
    .btn-coral:hover { opacity: 0.9; transform: translateY(-1px); }

    .weather-row {
      display: flex; align-items: center; justify-content: space-between;
      padding: 10px 4px; border-bottom: 1px solid var(--card-border); font-size: 14px;
    }
    .weather-row:last-child { border-bottom: none; }
    .weather-temp { font-weight: 700; font-size: 14.5px; color: var(--text-main); }
    .weather-rain {
      display: inline-flex; align-items: center; gap: 3px;
      background: rgba(56, 189, 248, 0.1); border: 1px solid rgba(56, 189, 248, 0.3);
      color: var(--neon-blue); border-radius: 999px; padding: 2px 8px; font-size: 11.5px; font-weight: 600;
    }

    .food-card {
      background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 16px;
      padding: 14px; margin-bottom: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }
    .food-title { font-size: 16px; font-weight: 700; margin: 0 0 4px; display: flex; align-items: center; justify-content: space-between; gap: 6px; color: var(--text-main); }
    .food-meta { font-size: 12px; color: var(--text-sub); margin-bottom: 8px; }
    .food-actions { display: flex; flex-wrap: wrap; gap: 8px; }
    .tabelog-btn {
      display: inline-flex; align-items: center; gap: 4px; padding: 4px 10px;
      background: rgba(250, 204, 21, 0.15); color: var(--neon-yellow); text-decoration: none;
      border: 1px solid rgba(250, 204, 21, 0.4); border-radius: 10px; font-size: 12px; font-weight: 600;
    }

    .rate-box { background: rgba(9, 13, 22, 0.4); border: 1px solid var(--card-border); border-radius: 16px; padding: 14px; margin-bottom: 14px; }
    .rate-row { display: flex; align-items: center; gap: 8px; margin-top: 8px; }
    .rate-input {
      flex: 1; border: 1px solid var(--card-border); border-radius: 10px; padding: 8px 12px;
      font-family: inherit; font-size: 14.5px; font-weight: 700; background: var(--bg-dark); color: var(--text-main); width: 100%;
    }
    .rate-input:focus { border-color: var(--neon-blue); outline: none; }

    .expense-summary-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
    .sum-box { border: 1px solid var(--card-border); border-radius: 14px; padding: 12px; text-align: center; background: rgba(9, 13, 22, 0.5); }
    .sum-label { font-size: 11.5px; font-weight: 600; color: var(--text-sub); }
    .sum-val { font-size: 19px; font-weight: 800; margin-top: 4px; color: var(--neon-blue); }

    .pill-group { display: flex; flex-wrap: wrap; gap: 6px; margin: 6px 0 12px; }
    .choice-pill {
      border: 1px solid var(--card-border); border-radius: 999px; padding: 5px 12px;
      font-family: inherit; font-size: 12.5px; font-weight: 600; background: var(--card-bg); color: var(--text-sub); cursor: pointer; transition: all 0.2s;
    }
    .choice-pill:hover { border-color: var(--text-sub); color: var(--text-main); }
    .choice-pill.active-mizu { background: rgba(56, 189, 248, 0.2); color: var(--neon-blue); border-color: var(--neon-blue); }
    .choice-pill.active-yama { background: rgba(250, 204, 21, 0.2); color: var(--neon-yellow); border-color: var(--neon-yellow); }
    .choice-pill.active-coral { background: rgba(244, 63, 94, 0.2); color: #FDA4AF; border-color: #FDA4AF; }
    .choice-pill.active-waka { background: rgba(74, 222, 128, 0.2); color: var(--neon-green); border-color: var(--neon-green); }

    .expense-item {
      display: flex; align-items: center; justify-content: space-between;
      padding: 10px 0; border-bottom: 1px solid var(--card-border); gap: 8px; font-size: 13.5px;
    }
    .expense-item:last-child { border-bottom: none; }
    .exp-cat-badge {
      display: inline-block; font-size: 11px; font-weight: 700; padding: 2px 8px;
      border: 1px solid rgba(250, 204, 21, 0.3); border-radius: 999px; background: rgba(250, 204, 21, 0.1); color: var(--neon-yellow); margin-right: 6px;
    }
    .settle-card { background: rgba(250, 204, 21, 0.06); border: 1px dashed rgba(250, 204, 21, 0.3); border-radius: 12px; padding: 12px; font-size: 13px; margin-top: 10px; color: #FEF08A; }

    .progress-text { font-size: 13.5px; font-weight: 600; margin-bottom: 6px; display: flex; justify-content: space-between; color: var(--text-sub); }
    .progress-track { width: 100%; height: 10px; background: var(--bg-dark); border: 1px solid var(--card-border); border-radius: 999px; overflow: hidden; margin-bottom: 14px; }
    .progress-fill { height: 100%; background: var(--neon-green); width: 0%; transition: width 0.3s ease; box-shadow: 0 0 10px rgba(74, 222, 128, 0.5); }

    .check-list { list-style: none; margin: 0; padding: 0; }
    .check-item { display: flex; align-items: center; justify-content: space-between; padding: 10px 4px; border-bottom: 1px solid var(--card-border); gap: 10px; }
    .check-label { display: flex; align-items: center; gap: 12px; cursor: pointer; flex: 1; font-size: 14px; color: var(--text-main); }
    .check-box {
      width: 22px; height: 22px; border: 1px solid var(--card-border); border-radius: 6px;
      background: var(--bg-dark); display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 12px; color: var(--bg-dark);
    }
    .check-item.done .check-box { background: var(--neon-green); border-color: var(--neon-green); font-weight: 900; }
    .check-item.done .check-text { text-decoration: line-through; opacity: 0.45; }

    .add-row { display: flex; gap: 8px; margin-top: 12px; }
    .add-input {
      flex: 1; border: 1px solid var(--card-border); border-radius: 12px; padding: 8px 12px;
      font-family: inherit; font-size: 13.5px; background: var(--bg-dark); color: var(--text-main);
    }
    .add-input:focus { border-color: var(--neon-blue); outline: none; }
    .add-btn {
      background: rgba(56, 189, 248, 0.15); border: 1px solid rgba(56, 189, 248, 0.4);
      color: var(--neon-blue); border-radius: 12px; padding: 8px 16px; font-family: inherit;
      font-size: 13.5px; font-weight: 700; cursor: pointer; transition: all 0.2s;
    }
    .add-btn:hover { background: var(--neon-blue); color: var(--bg-dark); }

    .luggage-tabs { display: flex; gap: 6px; margin-bottom: 12px; flex-wrap: wrap; }
    .lug-tab {
      border: 1px solid var(--card-border); background: var(--card-bg); color: var(--text-sub);
      border-radius: 999px; padding: 5px 12px; font-family: inherit; font-size: 12.5px; font-weight: 600; cursor: pointer;
    }
    .lug-tab.active { background: rgba(56, 189, 248, 0.2); color: var(--neon-blue); border-color: var(--neon-blue); }

    .sos-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 14px; }
    .sos-card {
      background: rgba(244, 63, 94, 0.08); border: 1px solid rgba(244, 63, 94, 0.3);
      border-radius: 20px; padding: 16px 12px; text-align: center; box-shadow: 0 4px 16px rgba(0,0,0,0.2);
    }
    .sos-num { font-size: 30px; font-weight: 900; line-height: 1.1; color: #FDA4AF; font-family: monospace; }
    .sos-desc { font-size: 13px; margin: 4px 0 10px; color: var(--text-sub); }
    .call-pill {
      display: inline-flex; align-items: center; justify-content: center; gap: 5px;
      background: rgba(244, 63, 94, 0.2); color: #FFF; text-decoration: none;
      border: 1px solid rgba(244, 63, 94, 0.4); border-radius: 10px; padding: 5px 14px; font-size: 13px; font-weight: 700;
    }

    .nearby-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; margin-top: 10px; }
    .nearby-btn {
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      background: rgba(9, 13, 22, 0.5); border: 1px solid var(--card-border); border-radius: 12px;
      padding: 10px 4px; text-decoration: none; color: var(--text-sub); font-size: 11px; font-weight: 600;
      transition: all 0.2s;
    }
    .nearby-btn:hover { color: var(--text-main); border-color: var(--neon-blue); background: rgba(56, 189, 248, 0.05); }
    .nearby-icon { font-size: 18px; margin-bottom: 4px; }

    .phrase-card { display: flex; align-items: center; justify-content: space-between; gap: 10px; }
    .phrase-jp { font-size: 18px; font-weight: 700; margin: 0 0 2px; color: var(--text-main); }
    .phrase-furi { font-size: 12px; color: var(--text-sub); margin: 0 0 4px; }
    .phrase-zh { font-size: 14px; font-weight: 700; margin: 0; color: var(--neon-green); }
    .zoom-btn {
      flex-shrink: 0; background: rgba(56, 189, 248, 0.1); border: 1px solid rgba(56, 189, 248, 0.3);
      color: var(--neon-blue); border-radius: 10px; padding: 6px 12px; font-family: inherit; font-size: 12px; font-weight: 600; cursor: pointer;
    }

    .modal-backdrop {
      position: fixed; inset: 0; background: rgba(9, 13, 22, 0.85); backdrop-filter: blur(8px);
      z-index: 200; display: none; align-items: center; justify-content: center; padding: 16px; overflow-y: auto;
    }
    .modal-backdrop.open { display: flex; }
    .modal-box {
      background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 24px;
      padding: 20px 18px; width: 100%; max-width: 420px; box-shadow: 0 16px 40px rgba(0,0,0,0.6); max-height: 90vh; overflow-y: auto;
    }
    .modal-input {
      width: 100%; border: 1px solid var(--card-border); border-radius: 10px; padding: 8px 12px;
      font-family: inherit; font-size: 13.5px; margin-bottom: 12px; background: var(--bg-dark); color: var(--text-main);
    }
    .modal-input:focus { border-color: var(--neon-blue); outline: none; }
  </style>
</head>
<body>

  <nav class="top-nav" aria-label="主導覽列">
    <div class="top-nav-inner">
      <button type="button" class="nav-pill tab-btn" data-target="sec-flight-box">✈️ 航班</button>
      <button type="button" class="nav-pill tab-btn active" data-target="sec-schedule-box">📅 行程</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-weather-box">🌤️ 天氣</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-food-box">🍜 美食</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-expense-box">💰 記帳</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-hotel-box">🏠 住宿</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-pre-box">✅ 任務</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-luggage-box">🧳 行李</button>
      <button type="button" class="nav-pill tab-btn" data-target="sec-helper-box">🆘 小幫手</button>
    </div>
  </nav>

  <main class="app-container">
    <div class="countdown-row">
      <div class="countdown-pill" id="countdown-badge">⏳ 計算倒數中...</div>
      <span class="sync-status" id="cloud-status">☁️ 雲端同步就緒</span>
    </div>

    <!-- 1. 航班區塊 -->
    <div id="sec-flight-box" class="tab-section">
      <section class="ehon-card hero-card" aria-label="旅程封面">
        <div class="hero-svg-wrap">
          <svg viewBox="0 0 440 200" width="100%" height="auto" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="科技感飛行線條圖">
            <rect width="440" height="200" fill="#0A1124"/>
            <path d="M0 150 Q110 100 220 150 T440 150" fill="none" stroke="#38BDF8" stroke-width="2" opacity="0.4"/>
            <path d="M0 120 Q140 170 280 110 T440 130" fill="none" stroke="#C084FC" stroke-width="1.5" opacity="0.3"/>
            <circle cx="220" cy="80" r="30" fill="rgba(56,189,248,0.1)" stroke="#38BDF8" stroke-width="1.5"/>
            <path d="M210 80 L230 80 M220 70 L220 90" stroke="#4ADE80" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
        <div class="hero-info">
          <h1 class="hero-title">⚡ HIROSHIMA HUD</h1>
          <p class="hero-sub">廣島・宮島・大阪・天橋立・奈良 ｜ 3/19 – 3/24 ・ 6 天</p>
        </div>
      </section>

      <div class="section-header">
        <h2 class="section-title">✈️ 飛行動態 HUD</h2>
        <span class="ehon-badge">中華航空</span>
      </div>
      <article class="ehon-card">
        <span class="ehon-badge">去程 ・ 3/19（五）</span>
        <div class="flight-route">
          <div><div class="airport-code">TPE</div><div class="airport-name">台北桃園</div><div class="flight-time">07:10 起飛</div></div>
          <div class="plane-circle">✈️</div>
          <div style="text-align: right;"><div class="airport-code">HIJ</div><div class="airport-name">廣島機場</div><div class="flight-time">10:25 抵達</div></div>
        </div>
        <div class="flight-footer"><span>航班：<strong>CI112</strong></span><span>航程約 2h 15m</span></div>
      </article>

      <article class="ehon-card">
        <span class="ehon-badge" style="color:var(--neon-pink); border-color:rgba(244,63,94,0.3); background:rgba(244,63,94,0.1);">回程 ・ 3/24（三）</span>
        <div class="flight-route">
          <div><div class="airport-code">KIX</div><div class="airport-name">大阪關西</div><div class="flight-time">19:00 起飛</div></div>
          <div class="plane-circle">✈️</div>
          <div style="text-align: right;"><div class="airport-code">TPE</div><div class="airport-name">台北桃園</div><div class="flight-time">21:25 抵達</div></div>
        </div>
        <div class="flight-footer"><span>航班：<strong>CI173</strong></span><span>航程約 3h 25m</span></div>
      </article>
    </div>

    <!-- 2. 行程區塊 -->
    <div id="sec-schedule-box" class="tab-section active-section">
      <div class="section-header">
        <h2 class="section-title">📅 時間軸任務</h2>
        <div style="display:flex; gap:6px; align-items:center;">
          <button type="button" class="btn-coral" id="refresh-cloud-btn" style="font-size:11.5px; padding:4px 10px; background:rgba(56,189,248,0.2); color:var(--neon-blue);">🔄 同步雲端</button>
          <span class="ehon-badge" id="day-area-badge" style="white-space: nowrap;">廣島・宮島</span>
        </div>
      </div>

      <div class="day-tabs" id="day-tabs"></div>
      <div class="timeline-wrap" id="timeline-container"></div>

      <div style="margin-top: 14px; text-align: center;">
        <button type="button" class="btn-coral" id="add-spot-btn" style="width:100%;">＋ 新增行程節點（同步好友）</button>
      </div>

      <div class="tip-banner" style="margin-top: 16px;">
        <span>💡</span>
        <span>建議抵達當天於機場或車站啟用數位交通 IC 卡（ICOCA/Suica），對應地鐵與JR快速通關。</span>
      </div>
    </div>

    <!-- 3. 天氣預報區塊 -->
    <div id="sec-weather-box" class="tab-section">
      <div class="section-header">
        <h2 class="section-title">🌤️ 氣象情報</h2>
        <span class="ehon-badge" id="weather-city-label">廣島・宮島</span>
      </div>

      <div class="pill-group" style="margin-bottom: 12px;">
        <button type="button" class="choice-pill active-mizu weather-loc-btn" data-lat="34.3853" data-lon="132.4553" data-name="廣島・宮島">⛩️ 廣島・宮島</button>
        <button type="button" class="choice-pill weather-loc-btn" data-lat="34.6937" data-lon="135.5023" data-name="大阪・京都・奈良">🏯 大阪・京都</button>
        <button type="button" class="choice-pill weather-loc-btn" id="weather-gps-btn" data-name="我的目前位置">📍 目前定位</button>
      </div>

      <article class="ehon-card">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
          <span class="ehon-badge">📅 這週預報 (Day 1-7)</span>
          <span style="font-size:11.5px; color:var(--text-sub);">溫差 / 降雨率</span>
        </div>
        <div id="weather-this-week"><p style="text-align:center; color:var(--text-sub);">⏳ 正在同步氣象衛星...</p></div>
      </article>

      <article class="ehon-card" style="margin-bottom:0;">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
          <span class="ehon-badge" style="color:var(--neon-green); border-color:rgba(74,222,128,0.3); background:rgba(74,222,128,0.1);">🗓️ 下週預報 (Day 8-14)</span>
          <span style="font-size:11.5px; color:var(--text-sub);">溫差 / 降雨率</span>
        </div>
        <div id="weather-next-week"><p style="text-align:center; color:var(--text-sub);">⏳ 載入中...</p></div>
      </article>
    </div>

    <!-- 4. 美食區塊 -->
    <div id="sec-food-box" class="tab-section">
      <div class="section-header">
        <h2 class="section-title">🍜 周邊美食雷達</h2>
        <span class="ehon-badge" style="color:var(--neon-yellow); border-color:rgba(250,204,21,0.3); background:rgba(250,204,21,0.1);">Tabelog 連結</span>
      </div>

      <article class="ehon-card" style="background: rgba(19, 29, 49, 0.8);">
        <div style="font-size:13.5px; font-weight:600; margin-bottom:8px;">📍 自動掃描周邊 1公里 內的高分餐廳與小吃</div>
        <div style="display:flex; flex-wrap:wrap; gap:8px;">
          <button type="button" class="btn-coral" id="locate-food-btn" style="flex:1;">📡 啟動 GPS 掃描附近美食</button>
        </div>
        <div style="display:flex; flex-wrap:wrap; gap:6px; margin-top:10px;">
          <button type="button" class="choice-pill preset-food-btn" data-lat="34.3955" data-lon="132.4536" data-area="廣島站・本通">廣島站・本通</button>
          <button type="button" class="choice-pill preset-food-btn" data-lat="34.2961" data-lon="132.3198" data-area="宮島">宮島表參道</button>
          <button type="button" class="choice-pill preset-food-btn" data-lat="34.6687" data-lon="135.5013" data-area="道頓堀・心齋橋">道頓堀・心齋橋</button>
        </div>
        <div id="tabelog-area-banner" style="margin-top:10px; padding-top:10px; border-top:1px dashed var(--card-border); display:flex; justify-content:space-between; align-items:center; gap:8px;">
          <span id="food-status-text" style="font-size:12px; color:var(--text-sub);">點擊按鈕立即取得當前座標美食清單</span>
          <a id="tabelog-top-ranking-link" href="https://tabelog.com/rstLst/?SrtT=rt&Srt=D" target="_blank" rel="noopener" class="tabelog-btn" style="flex-shrink:0;">🏆 評分榜 ↗</a>
        </div>
      </article>

      <article class="ehon-card">
        <div style="font-size:14px; font-weight:700; margin-bottom:4px;">📍 附近設施快速定位</div>
        <div style="font-size:11.5px; color:var(--text-sub);">一鍵開啟 Google Maps 尋找周邊據點</div>
        <div class="nearby-grid">
          <a href="https://www.google.com/maps/search/?api=1&query=ラーメン" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🍜</span>拉麵</a>
          <a href="https://www.google.com/maps/search/?api=1&query=レストラン" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🍴</span>餐廳</a>
          <a href="https://www.google.com/maps/search/?api=1&query=カフェ" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">☕</span>咖啡廳</a>
          <a href="https://www.google.com/maps/search/?api=1&query=スイーツ" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🍰</span>甜點</a>
          <a href="https://www.google.com/maps/search/?api=1&query=コンビニ" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🏪</span>便利店</a>
          <a href="https://www.google.com/maps/search/?api=1&query=ドラッグストア" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">💊</span>藥妝</a>
          <a href="https://www.google.com/maps/search/?api=1&query=コインロッカー" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🧳</span>置物櫃</a>
          <a href="https://www.google.com/maps/search/?api=1&query=トイレ" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🚻</span>洗手間</a>
          <a href="https://www.google.com/maps/search/?api=1&query=バス停" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🚌</span>巴士站</a>
          <a href="https://www.google.com/maps/search/?api=1&query=駅" target="_blank" rel="noopener" class="nearby-btn"><span class="nearby-icon">🚉</span>車站</a>
        </div>
      </article>

      <div id="food-list-container"></div>
    </div>

    <!-- 5. 記帳區塊 -->
    <div id="sec-expense-box" class="tab-section">
      <div class="section-header">
        <h2 class="section-title">💰 智能記帳與匯率</h2>
        <span class="ehon-badge" style="color:var(--neon-green); border-color:rgba(74,222,128,0.3); background:rgba(74,222,128,0.1);">同步試算表</span>
      </div>

      <section class="ehon-card" style="margin-bottom:0;">
        <div class="rate-box">
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <span style="font-size:13px; font-weight:700;">💱 匯率基準：1 JPY =</span>
            <button type="button" class="action-btn" id="fetch-rate-btn">🔄 更新即時匯率</button>
          </div>
          <div class="rate-row">
            <input type="number" step="0.0001" id="global-rate-input" class="rate-input" value="0.2150" />
            <span style="font-size:13px; font-weight:700; color:var(--text-sub); white-space:nowrap;">TWD</span>
          </div>
          <div id="rate-updated-text" style="font-size:11px; color:var(--text-sub); margin-top:4px;">點擊更新或手動輸入</div>

          <div style="margin-top:10px; padding-top:10px; border-top:1px dashed var(--card-border); display:flex; align-items:center; gap:8px;">
            <span style="font-size:12.5px; font-weight:700;">🧮 試算：</span>
            <input type="text" id="quick-calc-jpy" class="rate-input" placeholder="輸入日圓 (如 1500)" style="font-size:13px; padding:4px 8px;" />
            <span id="quick-calc-twd" style="font-size:13.5px; font-weight:800; color:var(--neon-blue); white-space:nowrap;">≈ NT$ 0</span>
          </div>
        </div>

        <div class="expense-summary-grid">
          <div class="sum-box">
            <div class="sum-label">🇯🇵 總花費 (JPY)</div>
            <div class="sum-val" id="total-jpy-display">¥ 0</div>
          </div>
          <div class="sum-box">
            <div class="sum-label">🇹🇼 折合台幣 (TWD)</div>
            <div class="sum-val" id="total-twd-display" style="color:var(--neon-green);">NT$ 0</div>
          </div>
        </div>

        <div style="margin-bottom:12px;">
          <div style="font-size:12.5px; font-weight:700; margin-bottom:4px; color:var(--text-sub);">👥 旅伴成員名單：</div>
          <div style="display:flex; flex-wrap:wrap; gap:6px; align-items:center;" id="members-list-wrap"></div>
          <div class="add-row" style="margin-top:6px;">
            <input type="text" id="new-member-input" class="add-input" placeholder="輸入名字" style="padding:5px 10px; font-size:12.5px;" />
            <button type="button" class="add-btn" id="add-member-btn" style="padding:5px 12px; font-size:12.5px;">＋ 旅伴</button>
          </div>
        </div>

        <div style="text-align:center; margin: 14px 0;">
          <button type="button" class="btn-coral" id="open-expense-modal-btn" style="width:100%; font-size:14.5px; padding:10px;">👛 新增消費紀錄</button>
        </div>

        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
          <span style="font-size:13.5px; font-weight:700;">📋 帳目清單</span>
          <button type="button" class="action-btn" id="toggle-exp-list-btn">收合 / 展開</button>
        </div>

        <div id="expense-list-container"></div>
        <div class="settle-card" id="settlement-box" style="display:none;"></div>
      </section>
    </div>

    <!-- 6. 住宿區塊 -->
    <div id="sec-hotel-box" class="tab-section">
      <div class="section-header">
        <h2 class="section-title">🏠 住宿據點</h2>
        <span class="ehon-badge">共 5 晚</span>
      </div>
      <article class="ehon-card">
        <span class="ehon-badge">3/19 – 3/20 ・ 1 晚</span>
        <h3 style="margin: 10px 0 4px; font-size: 18px; color:var(--text-main);">東橫INN廣島站前新幹線口1號</h3>
        <table class="hotel-table">
          <tr><td>入住時間</td><td>15:00 後</td></tr>
          <tr><td>退房時間</td><td>10:00 前</td></tr>
        </table>
        <a class="btn-coral" href="https://www.google.com/maps/search/?api=1&query=東橫INN廣島站前新幹線口1號" target="_blank" rel="noopener">📍 Google 導航 ↗</a>
      </article>

      <article class="ehon-card">
        <span class="ehon-badge">3/20 – 3/24 ・ 4 晚</span>
        <h3 style="margin: 10px 0 4px; font-size: 18px; color:var(--text-main);">住1道頓堀2號店</h3>
        <table class="hotel-table">
          <tr><td>入住時間</td><td>15:00 後</td></tr>
          <tr><td>退房時間</td><td>10:00 前</td></tr>
        </table>
        <a class="btn-coral" href="https://www.google.com/maps/search/?api=1&query=住1道頓堀2號店+大阪市" target="_blank" rel="noopener">📍 Google 導航 ↗</a>
      </article>
    </div>

    <!-- 7. 出發前任務 -->
    <div id="sec-pre-box" class="tab-section">
      <div class="section-header"><h2 class="section-title">✅ 出發前任務檢查</h2></div>
      <section class="ehon-card" style="margin-bottom:0;">
        <div class="progress-text"><span>完成進度</span><span id="pre-progress-label">已完成 0 / 0 項</span></div>
        <div class="progress-track"><div class="progress-fill" id="pre-progress-bar"></div></div>
        <ul class="check-list" id="pre-list"></ul>
        <div class="add-row">
          <input type="text" class="add-input" id="pre-input" placeholder="新增任務項目" />
          <button type="button" class="add-btn" id="pre-add-btn">新增</button>
        </div>
      </section>
    </div>

    <!-- 8. 行李打包清單 -->
    <div id="sec-luggage-box" class="tab-section">
      <div class="section-header"><h2 class="section-title">🧳 行李打包清單</h2></div>
      <section class="ehon-card" style="margin-bottom:0;">
        <div class="progress-text"><span>打包進度</span><span id="lug-progress-label">已完成 0 / 0 項</span></div>
        <div class="progress-track"><div class="progress-fill" id="lug-progress-bar" style="background: var(--neon-blue);"></div></div>
        <div class="luggage-tabs" id="luggage-tabs">
          <button type="button" class="lug-tab active" data-cat="all">全部</button>
          <button type="button" class="lug-tab" data-cat="隨身重要">隨身重要</button>
          <button type="button" class="lug-tab" data-cat="衣物穿搭">衣物穿搭</button>
          <button type="button" class="lug-tab" data-cat="3C電器">3C電器</button>
          <button type="button" class="lug-tab" data-cat="日常藥品">日常藥品</button>
        </div>
        <ul class="check-list" id="lug-list"></ul>
        <div class="add-row">
          <input type="text" class="add-input" id="lug-input" placeholder="新增行李項目" />
          <button type="button" class="add-btn" id="lug-add-btn">新增</button>
        </div>
      </section>
    </div>

    <!-- 9. 旅遊小幫手 -->
    <div id="sec-helper-box" class="tab-section">
      <div class="section-header">
        <h2 class="section-title">🆘 緊急與翻譯</h2>
        <span class="ehon-badge">離線支援</span>
      </div>
      <div class="sos-grid">
        <div class="sos-card"><div class="sos-num">110</div><div class="sos-desc">日本警察局</div><a href="tel:110" class="call-pill">📞 撥打</a></div>
        <div class="sos-card"><div class="sos-num">119</div><div class="sos-desc">救護・火警</div><a href="tel:119" class="call-pill">📞 撥打</a></div>
      </div>

      <div id="phrases-container"></div>
    </div>
  </main>

  <!-- 行程編輯彈窗 -->
  <div class="modal-backdrop" id="spot-modal">
    <div class="modal-box">
      <h3 id="modal-title" style="margin-top:0; color:var(--text-main);">新增行程節點</h3>
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">時間：</label>
      <input type="text" id="m-time" class="modal-input" placeholder="13:10" />
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">標題：</label>
      <input type="text" id="m-title" class="modal-input" placeholder="嚴島神社" />
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">備註：</label>
      <input type="text" id="m-note" class="modal-input" placeholder="欣賞海上鳥居" />
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">交通說明：</label>
      <input type="text" id="m-transport" class="modal-input" placeholder="步行 ・ 10分鐘" />
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">交通標籤類型：</label>
      <select id="m-ttype" class="modal-input">
        <option value="t-walk">步行 (藍)</option>
        <option value="t-train">電車 / JR (綠)</option>
        <option value="t-subway">地鐵 (紫)</option>
        <option value="t-shinkansen">新幹線 (青綠)</option>
        <option value="t-bus">巴士 (紅)</option>
        <option value="t-ship">船隻 (黃)</option>
      </select>
      <div style="margin: 8px 0;">
        <label style="font-size:13px; font-weight:600; color:var(--text-main); cursor:pointer;">
          <input type="checkbox" id="m-must" /> 標記為「★ 必去亮點」
        </label>
      </div>
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">Google 導航關鍵字：</label>
      <input type="text" id="m-query" class="modal-input" placeholder="嚴島神社" />
      <div style="display:flex; gap:10px; margin-top:14px;">
        <button type="button" class="btn-coral" id="m-save" style="flex:1;">儲存並同步</button>
        <button type="button" class="action-btn" id="m-cancel" style="flex:1; padding:10px; background:transparent;">取消</button>
      </div>
    </div>
  </div>

  <!-- 記帳彈窗 -->
  <div class="modal-backdrop" id="expense-modal">
    <div class="modal-box">
      <h3 style="margin-top:0; text-align:center; color:var(--text-main);">👛 新增記帳</h3>
      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">金額（支援算式，如 1200*3）：</label>
      <input type="text" id="exp-amount" class="modal-input" placeholder="例如：1200" style="font-size:16px; font-weight:700;" />
      <div id="exp-calc-preview" style="font-size:12px; font-weight:700; color:var(--neon-blue); margin:-6px 0 12px;">換算：≈ NT$ 0</div>

      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">幣別：</label>
      <div class="pill-group" id="exp-currency-group">
        <button type="button" class="choice-pill active-mizu" data-cur="JPY">日圓 (JPY)</button>
        <button type="button" class="choice-pill" data-cur="TWD">台幣 (TWD)</button>
      </div>

      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">類別：</label>
      <div class="pill-group" id="exp-cat-group">
        <button type="button" class="choice-pill active-yama" data-cat="餐飲">🍜 餐飲</button>
        <button type="button" class="choice-pill" data-cat="交通">🚃 交通</button>
        <button type="button" class="choice-pill" data-cat="門票">🎟️ 門票</button>
        <button type="button" class="choice-pill" data-cat="購物">🛍️ 購物</button>
        <button type="button" class="choice-pill" data-cat="住宿">🏠 住宿</button>
        <button type="button" class="choice-pill" data-cat="其他">✨ 其他</button>
      </div>

      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">付款人：</label>
      <div class="pill-group" id="exp-payer-group"></div>

      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">分攤對象：</label>
      <div class="pill-group" id="exp-split-group"></div>

      <label style="font-size:12.5px; font-weight:700; color:var(--text-sub);">備註：</label>
      <input type="text" id="exp-note" class="modal-input" placeholder="例如：宮島星鰻飯" />

      <div style="display:flex; gap:10px; margin-top:14px;">
        <button type="button" class="btn-coral" id="exp-save-btn" style="flex:1;">儲存並同步</button>
        <button type="button" class="action-btn" id="exp-cancel-btn" style="flex:1; padding:10px; background:transparent;">取消</button>
      </div>
    </div>
  </div>

  <div class="modal-backdrop" id="phrase-modal">
    <div class="modal-box" style="text-align:center;">
      <p id="modal-jp" style="font-size:28px; font-weight:800; margin:0 0 10px; color:var(--text-main);"></p>
      <p id="modal-furi" style="font-size:14px; color:var(--text-sub); margin:0 0 14px;"></p>
      <div id="modal-zh" style="font-size:16px; font-weight:700; background:rgba(74,222,128,0.15); color:var(--neon-green); display:inline-block; padding:6px 16px; border-radius:999px; border:1px solid rgba(74,222,128,0.3); margin-bottom:20px;"></div>
      <div><button type="button" class="btn-coral" id="close-phrase-btn">關閉字卡</button></div>
    </div>
  </div>

  <script>
    const GOOGLE_SHEET_API_URL = "https://script.google.com/macros/s/AKfycbyI4clVAxGhFyR4ar-IzHcJ4UaQsalZIM98AwkSdQE1VPfyGMMABoD39Jk42n91mjELTQ/exec";

    (function updateCountdown() {
      const now = new Date();
      const target = new Date(now.getFullYear(), 2, 19, 7, 10, 0);
      if (now > target) target.setFullYear(target.getFullYear() + 1);
      const diff = Math.ceil((target - now) / (1000 * 60 * 60 * 24));
      document.getElementById("countdown-badge").textContent = diff > 0 ? ("⏳ 距離出發 " + diff + " 天") : "🎉 旅程正式啟動！";
    })();

    const DEFAULT_TRIP_DAYS = [
      {
        day: "Day 1", date: "3/19 (五)", area: "廣島・宮島",
        items: [
          { time: "10:30", title: "抵達廣島機場 ✈️", note: "搭乘利木津巴士前往廣島市區", transport: "巴士 ・ 約 45 分鐘", tType: "t-bus", must: false, query: "廣島機場", subItems: [] },
          { time: "11:30", title: "搭 JR 山陽本線 往宮島口", note: "先至廣島站寄放行李", transport: "電車 ・ 約 27 分鐘", tType: "t-train", must: false, query: "宮島口站", subItems: [] },
          { time: "12:00", title: "宮島口午餐", note: "品嚐在地廣島燒或星鰻飯", transport: "步行 ・ 約 30 分鐘", tType: "t-walk", must: false, query: "宮島口棧橋", subItems: [] },
          { time: "13:10", title: "嚴島神社 ⛩️", note: "欣賞絕美海上大鳥居與表參道散步", transport: "步行 ・ 約 10 分鐘", tType: "t-walk", must: true, query: "嚴島神社", subItems: [] },
          { time: "18:00", title: "廣島寶可夢中心", note: "廣島限定紅色暴鯉龍周邊巡禮", transport: "電車 ・ 約 50 分鐘", tType: "t-train", must: true, query: "Pokemon Center Hiroshima", subItems: [] }
        ]
      },
      {
        day: "Day 2", date: "3/20 (六)", area: "廣島 → 大阪",
        items: [
          { time: "09:00", title: "原爆圓頂館", note: "世界文化遺產歷史地標", transport: "電車 ・ 約 20 分鐘", tType: "t-train", must: true, query: "原爆圓頂館", subItems: [] },
          { time: "10:00", title: "廣島和平紀念公園", note: "沿河岸參觀和平資料館", transport: "步行 ・ 約 10 分鐘", tType: "t-walk", must: false, query: "廣島和平紀念公園", subItems: [] },
          { time: "16:00", title: "心齋橋 Check in", note: "入住大阪住1道頓堀2號店", transport: "電車 ・ 約 30 分鐘", tType: "t-train", must: false, query: "住1道頓堀2號店", subItems: [] },
          { time: "22:00", title: "道頓堀夜生活", note: "體驗霓虹繁華街區", transport: "步行", tType: "t-walk", must: false, query: "道頓堀", subItems: [] }
        ]
      },
      {
        day: "Day 3", date: "3/21 (日)", area: "大阪市區巡禮",
        items: [
          { time: "09:00", title: "露天神社 ⛩️", note: "梅田著名的戀愛結緣聖地", transport: "電車", tType: "t-train", must: false, query: "露天神社 大阪", subItems: [] },
          { time: "10:00", title: "大鳥大社", note: "祈求勝運與旅途平安", transport: "電車", tType: "t-train", must: false, query: "大鳥大社", subItems: [] },
          { time: "13:00", title: "通天閣", note: "新世界通天閣展望", transport: "電車", tType: "t-train", must: false, query: "通天閣", subItems: [] },
          { time: "14:00", title: "四天王寺", note: "古寺巡禮與市集挖寶", transport: "電車", tType: "t-train", must: false, query: "四天王寺", subItems: [] },
          { time: "17:00", title: "大阪寶可夢中心", note: "大丸梅田店旗艦店", transport: "電車", tType: "t-train", must: false, query: "Pokemon Center Osaka", subItems: [] }
        ]
      },
      {
        day: "Day 4", date: "3/22 (一)", area: "海之京都",
        items: [
          { time: "09:00", title: "天橋立", note: "日本三景之一，俯瞰飛龍觀", transport: "電車 / 巴士", tType: "t-bus", must: false, query: "天橋立", subItems: [] },
          { time: "11:00", title: "伊根舟屋", note: "漫步傳統木造舟屋群", transport: "巴士", tType: "t-bus", must: false, query: "伊根舟屋", subItems: [] },
          { time: "13:00", title: "伊根灣遊船", note: "海上近距離欣賞舟屋餵海鷗", transport: "遊覽船", tType: "t-ship", must: false, query: "伊根灣賞景船", subItems: [] }
        ]
      },
      {
        day: "Day 5", date: "3/23 (二)", area: "宇治・奈良",
        items: [
          { time: "09:00", title: "宇治上神社", note: "日本現存最古老神社建築", transport: "電車", tType: "t-train", must: false, query: "宇治上神社", subItems: [] },
          { time: "11:00", title: "平等院", note: "鳳凰堂與品嚐宇治抹茶", transport: "步行", tType: "t-walk", must: false, query: "平等院", subItems: [] },
          { time: "14:00", title: "奈良公園", note: "與可愛小鹿互動散步", transport: "電車", tType: "t-train", must: false, query: "奈良公園", subItems: [] }
        ]
      },
      {
        day: "Day 6", date: "3/24 (三)", area: "大阪 → 桃園",
        items: [
          { time: "上午", title: "自由活動 & 最後採買", note: "心齋橋或臨空城 Outlet 補貨，前往機場", transport: "電車", tType: "t-train", must: false, query: "關西國際機場", subItems: [] }
        ]
      }
    ];

    let tripDays = JSON.parse(localStorage.getItem("hiroshima_trip_days_v6")) || DEFAULT_TRIP_DAYS;
    let expenseData = JSON.parse(localStorage.getItem("hiroshima_expenses_v1")) || {
      rate: 0.2150,
      members: [],
      items: []
    };

    if (expenseData.members && expenseData.members.length === 1 && expenseData.members[0] === "我" && (!expenseData.items || expenseData.items.length === 0)) {
      expenseData.members = [];
    }

    let activeDayIdx = 0;
    let editingSpotIdx = null;
    let showExpenseList = true;
    const statusBadge = document.getElementById("cloud-status");

    function setCloudStatus(text, bg) {
      statusBadge.textContent = text;
      statusBadge.style.color = bg || "var(--neon-green)";
    }

    function fetchWithTimeout(url, options, timeoutMs) {
      const controller = new AbortController();
      const id = setTimeout(() => controller.abort(), timeoutMs || 5000);
      const opts = Object.assign({}, options, { signal: controller.signal });
      return fetch(url, opts).finally(() => clearTimeout(id));
    }

    document.querySelectorAll(".tab-btn").forEach(btn => {
      btn.addEventListener("click", () => {
        const targetId = btn.dataset.target;
        document.querySelectorAll(".tab-section").forEach(sec => sec.classList.remove("active-section"));
        document.getElementById(targetId).classList.add("active-section");

        document.querySelectorAll(".tab-btn").forEach(b => {
          if (b.dataset.target === targetId) b.classList.add("active");
          else b.classList.remove("active");
        });
        if (targetId === "sec-weather-box" && !weatherLoadedOnce) {
          fetchWeatherForecast(34.3853, 132.4553, "廣島・宮島");
        }
        if (targetId === "sec-food-box" && !foodLoadedOnce) {
          triggerGpsFoodSearch();
        }
        window.scrollTo({ top: 120, behavior: "smooth" });
      });
    });

    /* 天氣預報 */
    let weatherLoadedOnce = false;
    function getWeatherEmoji(code) {
      if (code === 0) return "☀️ 晴朗";
      if (code <= 3) return "⛅ 多雲";
      if (code <= 48) return "🌫️ 起霧";
      if (code <= 67) return "🌧️ 降雨";
      if (code <= 77) return "❄️ 降雪";
      if (code <= 82) return "🌦️ 陣雨";
      return "⛈️ 雷雨";
    }

    async function fetchWeatherForecast(lat, lon, labelName) {
      weatherLoadedOnce = true;
      document.getElementById("weather-city-label").textContent = labelName;
      const thisWeekEl = document.getElementById("weather-this-week");
      const nextWeekEl = document.getElementById("weather-next-week");
      thisWeekEl.innerHTML = '<p style="text-align:center; color:var(--text-sub);">⏳ 正在更新氣象數據...</p>';
      nextWeekEl.innerHTML = '<p style="text-align:center; color:var(--text-sub);">⏳ 載入中...</p>';

      try {
        const url = "https://api.open-meteo.com/v1/forecast?latitude=" + lat + "&longitude=" + lon + "&daily=weathercode,temperature_2m_max,temperature_2m_min,precipitation_probability_max&timezone=Asia%2FTokyo&forecast_days=14";
        const res = await fetchWithTimeout(url, {}, 6000);
        const data = await res.json();
        const d = data.daily;
        const weekDays = ["日", "一", "二", "三", "四", "五", "六"];

        const renderRows = (startIdx, endIdx) => {
          let html = "";
          for (let i = startIdx; i < endIdx && i < d.time.length; i++) {
            const dt = new Date(d.time[i] + "T00:00:00");
            const dateLabel = (dt.getMonth() + 1) + "/" + dt.getDate() + " (" + weekDays[dt.getDay()] + ")";
            const iconText = getWeatherEmoji(d.weathercode[i]);
            const tMin = Math.round(d.temperature_2m_min[i]);
            const tMax = Math.round(d.temperature_2m_max[i]);
            const pop = d.precipitation_probability_max[i] !== null ? d.precipitation_probability_max[i] : 0;
            html += '<div class="weather-row">' +
              '<div style="min-width:85px; font-weight:600;">' + dateLabel + '</div>' +
              '<div style="flex:1;">' + iconText + '</div>' +
              '<div class="weather-temp" style="margin-right:8px;">' + tMin + '° ~ ' + tMax + '°C</div>' +
              '<span class="weather-rain">☔ ' + pop + '%</span>' +
              '</div>';
          }
          return html;
        };

        thisWeekEl.innerHTML = renderRows(0, 7);
        nextWeekEl.innerHTML = renderRows(7, 14);
      } catch (e) {
        thisWeekEl.innerHTML = '<p style="text-align:center; color:var(--neon-pink);">⚠️ 氣象載入逾時</p>';
        nextWeekEl.innerHTML = "";
      }
    }

    document.querySelectorAll(".weather-loc-btn").forEach(btn => {
      btn.addEventListener("click", () => {
        document.querySelectorAll(".weather-loc-btn").forEach(b => b.classList.remove("active-mizu"));
        btn.classList.add("active-mizu");
        if (btn.id === "weather-gps-btn") {
          if (!navigator.geolocation) { alert("瀏覽器不支援定位"); return; }
          document.getElementById("weather-this-week").innerHTML = '<p style="text-align:center;">📡 定位中...</p>';
          navigator.geolocation.getCurrentPosition(
            pos => fetchWeatherForecast(pos.coords.latitude, pos.coords.longitude, "目前位置"),
            () => alert("無法取得定位權限")
          );
        } else {
          fetchWeatherForecast(btn.dataset.lat, btn.dataset.lon, btn.dataset.name);
        }
      });
    });

    /* 美食與地圖 */
    let foodLoadedOnce = false;
    function calcDistanceMeters(lat1, lon1, lat2, lon2) {
      const R = 6371000;
      const toRad = x => x * Math.PI / 180;
      const dLat = toRad(lat2 - lat1);
      const dLon = toRad(lon2 - lon1);
      const a = Math.pow(Math.sin(dLat / 2), 2) + Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) * Math.pow(Math.sin(dLon / 2), 2);
      return Math.round(R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a)));
    }

    function translateCuisine(tagStr, amenityStr) {
      const map = {
        ramen: "🍜 拉麵", sushi: "🍣 壽司", izakaya: "🏮 居酒屋", cafe: "☕ 咖啡",
        japanese: "🍱 和食", okonomiyaki: "🥞 廣島燒", udon: "🍲 烏龍麵",
        soba: "🥢 蕎麥麵", yakiniku: "🥩 燒肉", curry: "🍛 咖哩", burger: "🍔 漢堡"
      };
      if (tagStr) {
        const key = tagStr.split(";")[0].toLowerCase().trim();
        if (map[key]) return map[key];
      }
      if (amenityStr === "cafe") return "☕ 咖啡";
      if (amenityStr === "fast_food") return "🍢 小吃";
      if (amenityStr === "izakaya" || amenityStr === "pub") return "🏮 居酒屋";
      return "🍽️ 餐廳";
    }

    async function fetchNearbyFood(lat, lon, areaLabel) {
      foodLoadedOnce = true;
      const statusEl = document.getElementById("food-status-text");
      const listEl = document.getElementById("food-list-container");
      const topLink = document.getElementById("tabelog-top-ranking-link");

      statusEl.textContent = "⏳ 掃描 " + areaLabel + " 周邊...";
      listEl.innerHTML = '<div class="ehon-card" style="text-align:center; color:var(--text-sub);">📡 連線地圖伺服器中...</div>';
      topLink.href = "https://tabelog.com/rstLst/?vs=1&sa=" + encodeURIComponent(areaLabel) + "&sk=&SrtT=rt&Srt=D";

      const query = '[out:json][timeout:10];(node["amenity"~"restaurant|cafe|fast_food|izakaya"](around:1000,' + lat + ',' + lon + ');way["amenity"~"restaurant|cafe|fast_food|izakaya"](around:1000,' + lat + ',' + lon + '););out center 40;';

      try {
        const res = await fetchWithTimeout("https://overpass-api.de/api/interpreter", {
          method: "POST",
          body: "data=" + encodeURIComponent(query)
        }, 8000);
        const data = await res.json();
        const places = (data.elements || [])
          .filter(el => el.tags && (el.tags.name || el.tags["name:ja"] || el.tags["name:zh"]))
          .map(el => {
            const pLat = el.lat || (el.center && el.center.lat);
            const pLon = el.lon || (el.center && el.center.lon);
            const name = el.tags["name:zh"] || el.tags.name || el.tags["name:ja"];
            const jpName = el.tags["name:ja"] || el.tags.name;
            const dist = calcDistanceMeters(lat, lon, pLat, pLon);
            const cat = translateCuisine(el.tags.cuisine, el.tags.amenity);
            return { name, jpName, dist, cat, pLat, pLon };
          })
          .sort((a, b) => a.dist - b.dist)
          .slice(0, 15);

        if (places.length === 0) {
          statusEl.textContent = "⚠️ 該區域暫無資料";
          listEl.innerHTML = "";
          return;
        }

        statusEl.textContent = "✅ 已找到 " + places.length + " 家周邊美食";
        listEl.innerHTML = places.map((p, idx) => {
          const tabelogSearchUrl = "https://tabelog.com/rstLst/?vs=1&sa=" + encodeURIComponent(areaLabel) + "&sk=" + encodeURIComponent(p.jpName) + "&SrtT=rt&Srt=D";
          const mapUrl = "https://www.google.com/maps/search/?api=1&query=" + encodeURIComponent(p.jpName) + "&center=" + p.pLat + "," + p.pLon;
          return '<div class="food-card">' +
            '<div class="food-title"><span>' + (idx + 1) + '. ' + p.name + '</span><span class="ehon-badge">📏 ' + p.dist + 'm</span></div>' +
            '<div class="food-meta"><span>' + p.cat + '</span></div>' +
            '<div class="food-actions">' +
              '<a href="' + tabelogSearchUrl + '" target="_blank" rel="noopener" class="tabelog-btn">🏆 查 Tabelog ↗</a>' +
              '<a href="' + mapUrl + '" target="_blank" rel="noopener" class="nav-map-btn">🧭 導航</a>' +
            '</div>' +
          '</div>';
        }).join("");
      } catch (err) {
        statusEl.textContent = "⚠️ 伺服器忙碌中";
        listEl.innerHTML = "";
      }
    }

    function triggerGpsFoodSearch() {
      if (!navigator.geolocation) { fetchNearbyFood(34.3955, 132.4536, "廣島"); return; }
      document.getElementById("food-status-text").textContent = "📡 定位中...";
      navigator.geolocation.getCurrentPosition(
        pos => fetchNearbyFood(pos.coords.latitude, pos.coords.longitude, "目前定位"),
        () => fetchNearbyFood(34.3955, 132.4536, "廣島站")
      );
    }

    document.getElementById("locate-food-btn").addEventListener("click", triggerGpsFoodSearch);
    document.querySelectorAll(".preset-food-btn").forEach(btn => {
      btn.addEventListener("click", () => fetchNearbyFood(Number(btn.dataset.lat), Number(btn.dataset.lon), btn.dataset.area));
    });

    /* 雲端同步與行程 */
    async function loadFromGoogleSheet() {
      if (!GOOGLE_SHEET_API_URL.startsWith("http")) return;
      setCloudStatus("☁️ 同步中...", "var(--neon-blue)");
      try {
        const res = await fetchWithTimeout(GOOGLE_SHEET_API_URL, {}, 5000);
        const data = await res.json();
        if (Array.isArray(data) && data.length > 0) {
          tripDays = data;
          localStorage.setItem("hiroshima_trip_days_v6", JSON.stringify(tripDays));
        } else if (data && data.tripDays) {
          tripDays = data.tripDays;
          if (data.expenseData) {
            expenseData = data.expenseData;
            localStorage.setItem("hiroshima_expenses_v1", JSON.stringify(expenseData));
          }
          localStorage.setItem("hiroshima_trip_days_v6", JSON.stringify(tripDays));
        }
        renderDayTabs();
        renderTimeline();
        renderMembers();
        renderExpenses();
        setCloudStatus("☁️ 已同步");
      } catch (err) {
        setCloudStatus("📱 本機離線模式", "var(--neon-yellow)");
      }
    }

    async function saveToGoogleSheet() {
      localStorage.setItem("hiroshima_trip_days_v6", JSON.stringify(tripDays));
      localStorage.setItem("hiroshima_expenses_v1", JSON.stringify(expenseData));
      if (!GOOGLE_SHEET_API_URL.startsWith("http")) return;
      setCloudStatus("☁️ 儲存中...", "var(--neon-yellow)");
      try {
        await fetchWithTimeout(GOOGLE_SHEET_API_URL, { method: "POST", body: JSON.stringify({ tripDays, expenseData }) }, 6000);
        setCloudStatus("☁️ 已同步");
      } catch (err) {
        setCloudStatus("⚠️ 存於本機", "var(--neon-pink)");
      }
    }

    const dayTabsEl = document.getElementById("day-tabs");
    const timelineEl = document.getElementById("timeline-container");
    const areaBadgeEl = document.getElementById("day-area-badge");

    function renderDayTabs() {
      dayTabsEl.innerHTML = tripDays.map((d, idx) =>
        '<button type="button" class="day-tab ' + (idx === activeDayIdx ? "active" : "") + '" data-idx="' + idx + '">' +
          '<span class="day-tab-num">' + d.day + '</span>' +
          '<span class="day-tab-date">' + d.date + '</span>' +
        '</button>'
      ).join("");
      dayTabsEl.querySelectorAll(".day-tab").forEach(btn => {
        btn.addEventListener("click", () => {
          activeDayIdx = Number(btn.dataset.idx);
          renderDayTabs();
          renderTimeline();
        });
      });
    }

    let expandedSpots = {};
    function renderTimeline() {
      const current = tripDays[activeDayIdx];
      areaBadgeEl.textContent = current.area;
      timelineEl.innerHTML = current.items.map((item, idx) => {
        if (!Array.isArray(item.subItems)) item.subItems = [];
        const key = activeDayIdx + "-" + idx;
        const isOpen = !!expandedSpots[key];
        const subCount = item.subItems.length;

        const subHtml = isOpen ? (
          '<div class="sub-items-box">' +
            (subCount === 0
              ? '<div style="font-size:11.5px; color:var(--text-sub); text-align:center;">尚無子行程細節</div>'
              : item.subItems.map((sub, subIdx) =>
                  '<div class="sub-item-row">' +
                    '<div>' + (sub.tag ? '<span class="sub-item-tag">' + sub.tag + '</span>' : '') + '<span>' + sub.text + '</span></div>' +
                    '<button type="button" class="del-btn del-sub-btn" data-sidx="' + idx + '" data-subidx="' + subIdx + '">✕</button>' +
                  '</div>'
                ).join("")
            ) +
            '<button type="button" class="action-btn add-sub-btn" data-sidx="' + idx + '">＋ 新增子細節</button>' +
          '</div>'
        ) : '';

        return '<article class="ehon-card timeline-item">' +
          '<span class="timeline-dot"></span>' +
          '<div class="spot-header">' +
            '<div><div class="spot-time">' + item.time + '</div><h3 class="spot-title">' + item.title + '</h3></div>' +
            '<div style="display:flex; align-items:center; gap:6px;">' +
              (item.must ? '<span class="must-go-badge">★ 必去</span>' : '') +
              '<div class="card-actions">' +
                (idx > 0 ? '<button type="button" class="action-btn move-up-spot" data-sidx="' + idx + '">▲</button>' : '') +
                (idx < current.items.length - 1 ? '<button type="button" class="action-btn move-down-spot" data-sidx="' + idx + '">▼</button>' : '') +
                '<button type="button" class="action-btn edit-spot" data-sidx="' + idx + '">✏️</button>' +
                '<button type="button" class="action-btn del del-spot" data-sidx="' + idx + '">🗑️</button>' +
              '</div>' +
            '</div>' +
          '</div>' +
          '<p class="spot-note">' + item.note + '</p>' +
          '<div class="spot-tags">' +
            '<span class="transport-tag ' + item.tType + '">' + item.transport + '</span>' +
            '<a class="nav-map-btn" href="https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent(item.query) + '" target="_blank" rel="noopener">🧭 導航</a>' +
            '<button type="button" class="action-btn toggle-sub-btn" data-sidx="' + idx + '">細節 (' + subCount + ')</button>' +
          '</div>' +
          subHtml +
        '</article>';
      }).join("");

      timelineEl.querySelectorAll(".toggle-sub-btn").forEach(btn => {
        btn.addEventListener("click", () => {
          const key = activeDayIdx + "-" + btn.dataset.sidx;
          expandedSpots[key] = !expandedSpots[key];
          renderTimeline();
        });
      });

      timelineEl.querySelectorAll(".add-sub-btn").forEach(btn => {
        btn.addEventListener("click", () => {
          const sidx = Number(btn.dataset.sidx);
          const tag = prompt("標籤（如：交通、門票）：", "交通");
          if (tag === null) return;
          const text = prompt("內容：");
          if (!text || !text.trim()) return;
          if (!Array.isArray(tripDays[activeDayIdx].items[sidx].subItems)) tripDays[activeDayIdx].items[sidx].subItems = [];
          tripDays[activeDayIdx].items[sidx].subItems.push({ tag: tag.trim(), text: text.trim() });
          expandedSpots[activeDayIdx + "-" + sidx] = true;
          renderTimeline();
          saveToGoogleSheet();
        });
      });

      timelineEl.querySelectorAll(".del-sub-btn").forEach(btn => {
        btn.addEventListener("click", () => {
          const sidx = Number(btn.dataset.sidx);
          const subidx = Number(btn.dataset.subidx);
          tripDays[activeDayIdx].items[sidx].subItems.splice(subidx, 1);
          renderTimeline();
          saveToGoogleSheet();
        });
      });

      timelineEl.querySelectorAll(".move-up-spot").forEach(btn => {
        const i = Number(btn.dataset.sidx);
        btn.addEventListener("click", () => { const arr = tripDays[activeDayIdx].items; [arr[i - 1], arr[i]] = [arr[i], arr[i - 1]]; renderTimeline(); saveToGoogleSheet(); });
      });
      timelineEl.querySelectorAll(".move-down-spot").forEach(btn => {
        const i = Number(btn.dataset.sidx);
        btn.addEventListener("click", () => { const arr = tripDays[activeDayIdx].items; [arr[i + 1], arr[i]] = [arr[i], arr[i + 1]]; renderTimeline(); saveToGoogleSheet(); });
      });
      timelineEl.querySelectorAll(".edit-spot").forEach(btn => {
        btn.addEventListener("click", () => openSpotModal(Number(btn.dataset.sidx)));
      });
      timelineEl.querySelectorAll(".del-spot").forEach(btn => {
        btn.addEventListener("click", () => {
          if (confirm("刪除此行程？")) {
            tripDays[activeDayIdx].items.splice(Number(btn.dataset.sidx), 1);
            renderTimeline();
            saveToGoogleSheet();
          }
        });
      });
    }

    const spotModal = document.getElementById("spot-modal");
    function openSpotModal(sidx) {
      editingSpotIdx = (sidx !== undefined && sidx !== null) ? sidx : null;
      document.getElementById("modal-title").textContent = editingSpotIdx === null ? "新增行程" : "編輯行程";
      if (editingSpotIdx !== null) {
        const item = tripDays[activeDayIdx].items[editingSpotIdx];
        document.getElementById("m-time").value = item.time;
        document.getElementById("m-title").value = item.title;
        document.getElementById("m-note").value = item.note;
        document.getElementById("m-transport").value = item.transport;
        document.getElementById("m-ttype").value = item.tType;
        document.getElementById("m-must").checked = item.must;
        document.getElementById("m-query").value = item.query;
      } else {
        document.getElementById("m-time").value = "12:00";
        document.getElementById("m-title").value = "";
        document.getElementById("m-note").value = "";
        document.getElementById("m-transport").value = "步行 ・ 15分鐘";
        document.getElementById("m-ttype").value = "t-walk";
        document.getElementById("m-must").checked = false;
        document.getElementById("m-query").value = "";
      }
      spotModal.classList.add("open");
    }

    document.getElementById("add-spot-btn").addEventListener("click", () => openSpotModal(null));
    document.getElementById("m-cancel").addEventListener("click", () => spotModal.classList.remove("open"));
    document.getElementById("refresh-cloud-btn").addEventListener("click", loadFromGoogleSheet);

    document.getElementById("m-save").addEventListener("click", () => {
      const newItem = {
        time: document.getElementById("m-time").value.trim() || "時間",
        title: document.getElementById("m-title").value.trim() || "新景點",
        note: document.getElementById("m-note").value.trim(),
        transport: document.getElementById("m-transport").value.trim() || "步行",
        tType: document.getElementById("m-ttype").value,
        must: document.getElementById("m-must").checked,
        query: document.getElementById("m-query").value.trim() || document.getElementById("m-title").value.trim()
      };
      if (editingSpotIdx === null) {
        newItem.subItems = [];
        tripDays[activeDayIdx].items.push(newItem);
      } else {
        newItem.subItems = tripDays[activeDayIdx].items[editingSpotIdx].subItems || [];
        tripDays[activeDayIdx].items[editingSpotIdx] = newItem;
      }
      renderTimeline();
      spotModal.classList.remove("open");
      saveToGoogleSheet();
    });

    /* 記帳功能 */
    const rateInput = document.getElementById("global-rate-input");
    const rateText = document.getElementById("rate-updated-text");
    rateInput.value = expenseData.rate || 0.2150;

    async function fetchLiveExchangeRate() {
      rateText.textContent = "⏳ 更新中...";
      try {
        const res = await fetchWithTimeout("https://open.er-api.com/v6/latest/JPY", {}, 5000);
        const json = await res.json();
        if (json && json.rates && json.rates.TWD) {
          const liveRate = Number(json.rates.TWD.toFixed(6));
          rateInput.value = liveRate;
          expenseData.rate = liveRate;
          rateText.textContent = "✅ 即時匯率：1 JPY = " + liveRate + " TWD";
          saveToGoogleSheet();
          renderExpenses();
        }
      } catch (e) {
        rateText.textContent = "⚠️ 連線失敗";
      }
    }

    document.getElementById("fetch-rate-btn").addEventListener("click", fetchLiveExchangeRate);
    rateInput.addEventListener("change", () => {
      const val = parseFloat(rateInput.value);
      if (val > 0) { expenseData.rate = val; saveToGoogleSheet(); renderExpenses(); }
    });

    function evalMathExpr(str) {
      const cleaned = String(str).replace(/[^0-9+\-*/().]/g, "");
      if (!cleaned) return 0;
      try {
        const res = Function('"use strict";return (' + cleaned + ')')();
        return isFinite(res) && res > 0 ? Math.round(res * 100) / 100 : 0;
      } catch (e) { return 0; }
    }

    document.getElementById("quick-calc-jpy").addEventListener("input", (e) => {
      const jpy = evalMathExpr(e.target.value);
      const rate = parseFloat(rateInput.value) || 0.215;
      document.getElementById("quick-calc-twd").textContent = "≈ NT$ " + Math.round(jpy * rate).toLocaleString();
    });

    function renderMembers() {
      const wrap = document.getElementById("members-list-wrap");
      if (!expenseData.members || expenseData.members.length === 0) {
        wrap.innerHTML = '<span style="font-size:12px; color:var(--text-sub);">尚未新增旅伴，請在下方輸入</span>';
        return;
      }
      wrap.innerHTML = expenseData.members.map((m, idx) =>
        '<span class="ehon-badge" style="background:var(--card-bg);">' +
          '👤 ' + m + '<button type="button" data-midx="' + idx + '" class="del-member-btn" style="border:none;background:none;color:var(--text-sub);cursor:pointer;margin-left:4px;">✕</button>' +
        '</span>'
      ).join("");
      wrap.querySelectorAll(".del-member-btn").forEach(btn => {
        btn.addEventListener("click", () => {
          expenseData.members.splice(Number(btn.dataset.midx), 1);
          renderMembers(); renderExpenses(); saveToGoogleSheet();
        });
      });
    }

    document.getElementById("add-member-btn").addEventListener("click", () => {
      const inp = document.getElementById("new-member-input");
      const name = inp.value.trim();
      if (name && !expenseData.members.includes(name)) {
        expenseData.members.push(name);
        inp.value = "";
        renderMembers(); renderExpenses(); saveToGoogleSheet();
      }
    });

    const expModal = document.getElementById("expense-modal");
    let selCurrency = "JPY";
    let selCategory = "餐飲";
    let selPayer = "";
    let selSplitMembers = [];

    function updateExpPreview() {
      const amt = evalMathExpr(document.getElementById("exp-amount").value);
      const rate = parseFloat(rateInput.value) || 0.215;
      const twd = selCurrency === "JPY" ? Math.round(amt * rate) : Math.round(amt);
      const jpy = selCurrency === "JPY" ? Math.round(amt) : Math.round(amt / rate);
      document.getElementById("exp-calc-preview").textContent = "換算：¥ " + jpy.toLocaleString() + " ／ NT$ " + twd.toLocaleString();
    }

    document.getElementById("exp-amount").addEventListener("input", updateExpPreview);

    document.querySelectorAll("#exp-currency-group .choice-pill").forEach(btn => {
      btn.addEventListener("click", () => {
        document.querySelectorAll("#exp-currency-group .choice-pill").forEach(b => b.classList.remove("active-mizu"));
        btn.classList.add("active-mizu");
        selCurrency = btn.dataset.cur;
        updateExpPreview();
      });
    });

    document.querySelectorAll("#exp-cat-group .choice-pill").forEach(btn => {
      btn.addEventListener("click", () => {
        document.querySelectorAll("#exp-cat-group .choice-pill").forEach(b => b.classList.remove("active-yama"));
        btn.classList.add("active-yama");
        selCategory = btn.dataset.cat;
      });
    });

    function renderExpModalPeople() {
      const payerGroup = document.getElementById("exp-payer-group");
      const splitGroup = document.getElementById("exp-split-group");
      if (!expenseData.members.includes(selPayer)) selPayer = expenseData.members[0] || "";

      payerGroup.innerHTML = expenseData.members.map(m =>
        '<button type="button" class="choice-pill ' + (m === selPayer ? "active-coral" : "") + '" data-p="' + m + '">' + m + '</button>'
      ).join("");
      payerGroup.querySelectorAll(".choice-pill").forEach(b => b.addEventListener("click", () => {
        selPayer = b.dataset.p; renderExpModalPeople();
      }));

      splitGroup.innerHTML =
        '<button type="button" class="choice-pill ' + (selSplitMembers.length === expenseData.members.length ? "active-waka" : "") + '" id="split-all-btn">全選</button>' +
        expenseData.members.map(m =>
          '<button type="button" class="choice-pill ' + (selSplitMembers.includes(m) ? "active-waka" : "") + '" data-s="' + m + '">' + m + '</button>'
        ).join("");
      document.getElementById("split-all-btn").addEventListener("click", () => {
        selSplitMembers = expenseData.members.slice(); renderExpModalPeople();
      });
      splitGroup.querySelectorAll("[data-s]").forEach(b => b.addEventListener("click", () => {
        const m = b.dataset.s;
        if (selSplitMembers.includes(m)) {
          if (selSplitMembers.length > 1) selSplitMembers = selSplitMembers.filter(x => x !== m);
        } else { selSplitMembers.push(m); }
        renderExpModalPeople();
      }));
    }

    document.getElementById("open-expense-modal-btn").addEventListener("click", () => {
      if (!expenseData.members || expenseData.members.length === 0) {
        alert("請先在上方新增旅伴名稱！");
        document.getElementById("new-member-input").focus();
        return;
      }
      document.getElementById("exp-amount").value = "";
      document.getElementById("exp-note").value = "";
      selPayer = expenseData.members[0];
      selSplitMembers = expenseData.members.slice();
      renderExpModalPeople();
      updateExpPreview();
      expModal.classList.add("open");
    });

    document.getElementById("exp-cancel-btn").addEventListener("click", () => expModal.classList.remove("open"));

    document.getElementById("exp-save-btn").addEventListener("click", () => {
      const amt = evalMathExpr(document.getElementById("exp-amount").value);
      if (amt <= 0) { alert("金額必須大於 0"); return; }
      const rate = parseFloat(rateInput.value) || 0.215;
      const jpy = selCurrency === "JPY" ? Math.round(amt) : Math.round(amt / rate);
      const twd = selCurrency === "JPY" ? Math.round(amt * rate) : Math.round(amt);
      const note = document.getElementById("exp-note").value.trim() || selCategory;
      const dateStr = new Date().toLocaleDateString("zh-TW", { month: "numeric", day: "numeric" });

      expenseData.items.unshift({
        id: Date.now(), date: dateStr, category: selCategory, note: note,
        currency: selCurrency, origAmount: amt, jpy: jpy, twd: twd,
        payer: selPayer, splitFor: selSplitMembers.slice()
      });

      expModal.classList.remove("open");
      renderExpenses();
      saveToGoogleSheet();
    });

    document.getElementById("toggle-exp-list-btn").addEventListener("click", () => {
      showExpenseList = !showExpenseList; renderExpenses();
    });

    function renderExpenses() {
      let totalJpy = 0, totalTwd = 0;
      const paidMap = {}, owedMap = {};
      expenseData.members.forEach(m => { paidMap[m] = 0; owedMap[m] = 0; });

      expenseData.items.forEach(item => {
        totalJpy += item.jpy;
        totalTwd += item.twd;
        if (paidMap[item.payer] !== undefined) paidMap[item.payer] += item.twd;
        const share = item.twd / (item.splitFor.length || 1);
        item.splitFor.forEach(m => { if (owedMap[m] !== undefined) owedMap[m] += share; });
      });

      document.getElementById("total-jpy-display").textContent = "¥ " + totalJpy.toLocaleString();
      document.getElementById("total-twd-display").textContent = "NT$ " + totalTwd.toLocaleString();

      const listEl = document.getElementById("expense-list-container");
      if (!showExpenseList) {
        listEl.innerHTML = '<p style="text-align:center; color:var(--text-sub); font-size:12px;">(明細已收合)</p>';
      } else if (expenseData.items.length === 0) {
        listEl.innerHTML = '<p style="text-align:center; color:var(--text-sub); font-size:13px;">目前尚無消費紀錄</p>';
      } else {
        listEl.innerHTML = expenseData.items.map((item, idx) =>
          '<div class="expense-item">' +
            '<div><div><span class="exp-cat-badge">' + item.category + '</span><strong>' + item.note + '</strong> <small style="color:var(--text-sub);">(' + item.date + ')</small></div>' +
            '<div style="font-size:11.5px; color:var(--text-sub); margin-top:2px;">👤 ' + item.payer + ' 付 ｜ 分攤：' + item.splitFor.join(",") + '</div></div>' +
            '<div style="text-align:right; display:flex; align-items:center; gap:8px;">' +
              '<div><div style="font-weight:700; color:var(--text-main);">¥ ' + item.jpy.toLocaleString() + '</div><div style="font-size:11.5px; color:var(--text-sub);">NT$ ' + item.twd.toLocaleString() + '</div></div>' +
              '<button type="button" class="action-btn del del-exp-btn" data-eidx="' + idx + '">✕</button>' +
            '</div>' +
          '</div>'
        ).join("");

        listEl.querySelectorAll(".del-exp-btn").forEach(btn => {
          btn.addEventListener("click", () => {
            if (confirm("刪除此筆記帳？")) {
              expenseData.items.splice(Number(btn.dataset.eidx), 1);
              renderExpenses(); saveToGoogleSheet();
            }
          });
        });
      }

      const settleEl = document.getElementById("settlement-box");
      if (expenseData.members.length > 1 && expenseData.items.length > 0) {
        settleEl.style.display = "block";
        const lines = expenseData.members.map(m => {
          const diff = Math.round(paidMap[m] - owedMap[m]);
          if (diff > 0) return '• <strong>' + m + '</strong> 應收回 <span style="color:var(--neon-green); font-weight:700;">NT$ ' + diff.toLocaleString() + '</span>';
          if (diff < 0) return '• <strong>' + m + '</strong> 應支付 <span style="color:#FDA4AF; font-weight:700;">NT$ ' + Math.abs(diff).toLocaleString() + '</span>';
          return '• <strong>' + m + '</strong> 已結清';
        });
        settleEl.innerHTML = '<div style="font-weight:700; margin-bottom:4px;">📊 旅伴分帳試算：</div>' + lines.join("<br>");
      } else { settleEl.style.display = "none"; }
    }

    renderDayTabs();
    renderTimeline();
    renderMembers();
    renderExpenses();
    setTimeout(loadFromGoogleSheet, 100);

    /* 任務清單 */
    const DEFAULT_PRE = [
      { text: "護照（效期 6 個月以上）", done: false },
      { text: "填寫 Visit Japan Web", done: false },
      { text: "日本 eSIM / 上網卡", done: false },
      { text: "日幣現鈔與海外信用卡", done: false },
      { text: "電子機票與飯店憑證備份", done: false }
    ];
    let preItems = JSON.parse(localStorage.getItem("hiroshima_pre_v4")) || DEFAULT_PRE;
    function saveAndRenderPre() {
      localStorage.setItem("hiroshima_pre_v4", JSON.stringify(preItems));
      const done = preItems.filter(i => i.done).length;
      document.getElementById("pre-progress-label").textContent = "已完成 " + done + " / " + preItems.length + " 項";
      document.getElementById("pre-progress-bar").style.width = preItems.length ? ((done / preItems.length) * 100) + "%" : "0%";
      document.getElementById("pre-list").innerHTML = preItems.map((i, idx) =>
        '<li class="check-item ' + (i.done ? "done" : "") + '">' +
          '<label class="check-label"><input type="checkbox" ' + (i.done ? "checked" : "") + ' data-idx="' + idx + '" style="display:none;" />' +
          '<span class="check-box">' + (i.done ? "✔" : "") + '</span><span class="check-text">' + i.text + '</span></label>' +
          '<button type="button" class="action-btn del" data-del="' + idx + '">✕</button>' +
        '</li>'
      ).join("");
      document.querySelectorAll("#pre-list input").forEach(cb => cb.addEventListener("change", () => { preItems[cb.dataset.idx].done = cb.checked; saveAndRenderPre(); }));
      document.querySelectorAll("#pre-list .action-btn.del").forEach(btn => btn.addEventListener("click", () => { preItems.splice(btn.dataset.del, 1); saveAndRenderPre(); }));
    }
    document.getElementById("pre-add-btn").addEventListener("click", () => {
      const inp = document.getElementById("pre-input");
      if (inp.value.trim()) { preItems.push({ text: inp.value.trim(), done: false }); inp.value = ""; saveAndRenderPre(); }
    });
    saveAndRenderPre();

    /* 行李清單 */
    const DEFAULT_LUG = [
      { cat: "隨身重要", text: "護照、日幣現鈔、信用卡", done: false },
      { cat: "隨身重要", text: "行動電源", done: false },
      { cat: "衣物穿搭", text: "保暖外套、運動鞋", done: false },
      { cat: "3C電器", text: "充電線、轉接頭", done: false }
    ];
    let lugItems = JSON.parse(localStorage.getItem("hiroshima_lug_v4")) || DEFAULT_LUG;
    let curCat = "all";
    function saveAndRenderLug() {
      localStorage.setItem("hiroshima_lug_v4", JSON.stringify(lugItems));
      const done = lugItems.filter(i => i.done).length;
      document.getElementById("lug-progress-label").textContent = "已完成 " + done + " / " + lugItems.length + " 項";
      document.getElementById("lug-progress-bar").style.width = lugItems.length ? ((done / lugItems.length) * 100) + "%" : "0%";
      document.getElementById("lug-list").innerHTML = lugItems.map((i, realIdx) => Object.assign({}, i, { realIdx }))
        .filter(i => curCat === "all" || i.cat === curCat)
        .map(i =>
          '<li class="check-item ' + (i.done ? "done" : "") + '">' +
            '<label class="check-label"><input type="checkbox" ' + (i.done ? "checked" : "") + ' data-idx="' + i.realIdx + '" style="display:none;" />' +
            '<span class="check-box">' + (i.done ? "✔" : "") + '</span><span class="check-text"><small style="color:var(--text-sub);">[' + i.cat + ']</small> ' + i.text + '</span></label>' +
            '<button type="button" class="action-btn del" data-del="' + i.realIdx + '">✕</button>' +
          '</li>'
        ).join("");
      document.querySelectorAll("#lug-list input").forEach(cb => cb.addEventListener("change", () => { lugItems[cb.dataset.idx].done = cb.checked; saveAndRenderLug(); }));
      document.querySelectorAll("#lug-list .action-btn.del").forEach(btn => btn.addEventListener("click", () => { lugItems.splice(btn.dataset.del, 1); saveAndRenderLug(); }));
    }
    document.querySelectorAll(".lug-tab").forEach(tab => tab.addEventListener("click", () => {
      document.querySelectorAll(".lug-tab").forEach(t => t.classList.remove("active"));
      tab.classList.add("active");
      curCat = tab.dataset.cat;
      saveAndRenderLug();
    }));
    document.getElementById("lug-add-btn").addEventListener("click", () => {
      const inp = document.getElementById("lug-input");
      if (inp.value.trim()) { lugItems.push({ cat: curCat === "all" ? "隨身重要" : curCat, text: inp.value.trim(), done: false }); inp.value = ""; saveAndRenderLug(); }
    });
    saveAndRenderLug();

    /* SOS 與應急字卡 */
    const PHRASES = [
      { jp: "助けてください", furi: "たすけてください", zh: "請幫幫我" },
      { jp: "救急車を呼んでください", furi: "きゅうきゅうしゃをよんでください", zh: "請叫救護車" },
      { jp: "警察を呼んでください", furi: "けいさつをよんでください", zh: "請叫警察" },
      { jp: "道に迷いました", furi: "みちにまよいました", zh: "我迷路了" }
    ];
    const phraseModal = document.getElementById("phrase-modal");
    document.getElementById("phrases-container").innerHTML = PHRASES.map((p, i) =>
      '<article class="ehon-card phrase-card">' +
        '<div><p class="phrase-jp">' + p.jp + '</p><p class="phrase-furi">' + p.furi + '</p><p class="phrase-zh">' + p.zh + '</p></div>' +
        '<button type="button" class="zoom-btn" data-pidx="' + i + '">放大字卡</button>' +
      '</article>'
    ).join("");
    document.querySelectorAll(".zoom-btn").forEach(btn => btn.addEventListener("click", () => {
      const p = PHRASES[btn.dataset.pidx];
      document.getElementById("modal-jp").textContent = p.jp;
      document.getElementById("modal-furi").textContent = p.furi;
      document.getElementById("modal-zh").textContent = p.zh;
      phraseModal.classList.add("open");
    }));
    document.getElementById("close-phrase-btn").addEventListener("click", () => phraseModal.classList.remove("open"));
  </script>
</body>
</html>
