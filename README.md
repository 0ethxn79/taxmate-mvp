<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <title>TaxMate Pro｜日韓退稅防呆＆分帳管家</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            navy: '#132238',
            coral: '#FF6B4A',
            coralLight: '#FFF0EC',
            warmBg: '#FAF8F5',
          }
        }
      }
    }
  </script>
  <style>
    body {
      background-color: #FAF8F5;
      color: #132238;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    .hide-scrollbar::-webkit-scrollbar { display: none; }
    .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="max-w-md mx-auto min-h-screen flex flex-col justify-between pb-24 relative shadow-xl bg-warmBg">

  <!-- 頂部導覽列 -->
  <header class="px-5 pt-5 pb-3 bg-warmBg/95 backdrop-blur-md sticky top-0 z-20 border-b border-orange-100/60">
    <div class="flex items-center justify-between">
      <div>
        <div class="flex items-center gap-1.5">
          <span class="text-[11px] font-bold tracking-wider text-coral uppercase">TaxMate</span>
          <span id="proBadge" class="hidden text-[10px] font-bold bg-navy text-amber-300 px-2 py-0.5 rounded-full">PRO 通行證已啟用</span>
          <span id="freeBadge" class="text-[10px] font-semibold bg-gray-200 text-gray-600 px-2 py-0.5 rounded-full">免費版 (<span id="usageCount">3</span>/5 筆)</span>
        </div>
        <h1 id="headerTitle" class="text-xl font-extrabold text-navy mt-0.5">我的行程退稅總覽</h1>
      </div>
      <div class="flex items-center gap-1.5">
        <button onclick="openRoomModal()" class="p-2 bg-white border border-gray-200 rounded-full text-navy shadow-sm" title="多人同步房間">
          <i data-lucide="share-2" class="w-4 h-4"></i>
        </button>
        <button onclick="openSettingsModal()" class="p-2 bg-white border border-gray-200 rounded-full text-navy shadow-sm" title="API 與匯率設定">
          <i data-lucide="settings" class="w-4 h-4"></i>
        </button>
      </div>
    </div>

    <!-- 功能 #5：即時匯率與 1.5% 海外刷卡手續費切換列 -->
    <div class="mt-2.5 flex items-center justify-between text-[11px] bg-white px-3 py-1.5 rounded-xl border border-gray-100">
      <div class="flex items-center gap-2 text-gray-600">
        <span id="liveRateBadge" class="font-medium">🇯🇵 0.215 ｜ 🇰🇷 0.0235</span>
      </div>
      <label class="flex items-center gap-1.5 cursor-pointer select-none font-semibold text-navy">
        <input type="checkbox" id="cardFeeToggle" onchange="toggleCardFee()" class="accent-coral w-3.5 h-3.5 rounded" />
        <span>計入 1.5% 刷卡手續費</span>
      </label>
    </div>
  </header>

  <!-- 主要內容區 -->
  <main class="px-5 pt-4 flex-1 space-y-5">

    <!-- ================= TAB 1: 首頁總覽 ================= -->
    <section id="tab-home" class="space-y-4">
      <!-- 退稅總額主卡片 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-orange-100/80">
        <div class="flex justify-between items-start">
          <div>
            <p class="text-xs font-medium text-gray-500">預估可拿回退稅總額</p>
            <div class="flex items-baseline gap-2 mt-1">
              <span class="text-3xl font-extrabold text-navy" id="totalRefundTWD">NT$ 0</span>
              <span class="text-[11px] font-semibold text-coral bg-coralLight px-2 py-0.5 rounded-full">總消費 <span id="totalSpentTWD">NT$ 0</span></span>
            </div>
            <p class="text-xs text-gray-400 mt-1" id="foreignTotalText">日幣退稅 ¥0 ｜ 韓元退稅 ₩0</p>
          </div>
          <!-- 功能 #10：一鍵生成戰績圖卡 -->
          <button onclick="generateShareCard()" class="flex flex-col items-center justify-center bg-coralLight hover:bg-orange-100 text-coral px-3 py-2 rounded-2xl transition">
            <i data-lucide="image" class="w-5 h-5"></i>
            <span class="text-[10px] font-bold mt-0.5">戰績圖卡</span>
          </button>
        </div>

        <!-- 防呆統計條 -->
        <div class="mt-4 pt-3 border-t border-gray-100 grid grid-cols-2 gap-2 text-xs">
          <div class="flex items-center gap-1.5 text-gray-600">
            <span class="w-2 h-2 rounded-full bg-red-500"></span>
            不可拆封：<strong id="consumableCount" class="text-navy">0 袋</strong>
          </div>
          <div class="flex items-center justify-end gap-1.5 text-gray-600">
            <span class="w-2 h-2 rounded-full bg-amber-500"></span>
            須手提查驗：<strong id="carryOnCount" class="text-navy">0 件</strong>
          </div>
        </div>
      </div>

      <!-- 主行動按鈕列：AI 拍收據 + 匯出 CSV -->
      <div class="flex gap-2.5">
        <button onclick="openAddModal()" class="flex-1 bg-coral hover:bg-orange-600 active:scale-[0.99] transition text-white font-bold py-3.5 px-4 rounded-2xl shadow-md shadow-orange-200 flex items-center justify-center gap-2 text-sm">
          <i data-lucide="camera" class="w-5 h-5"></i>
          <span>AI 拍收據 / 新增消費</span>
        </button>
        <!-- 功能 #7：匯出 CSV -->
        <button onclick="exportCSV()" class="bg-navy text-white font-semibold px-4 rounded-2xl shadow-sm flex flex-col items-center justify-center text-[11px]" title="匯出 Excel/CSV">
          <i data-lucide="download" class="w-4 h-4 mb-0.5"></i>
          <span>CSV</span>
        </button>
      </div>

      <!-- 近期收據列表 -->
      <div>
        <div class="flex items-center justify-between mb-2.5">
          <h2 class="text-sm font-bold text-navy">已記錄收據（點擊可細項分帳）</h2>
          <button onclick="resetDemoData()" class="text-[11px] text-gray-400 underline">重置範例</button>
        </div>
        <div id="receiptList" class="space-y-2.5"></div>
      </div>
    </section>

    <!-- ================= TAB 2: 湊單神器 & AI 掃描 ================= -->
    <section id="tab-tools" class="hidden space-y-4">
      <!-- 功能 #2：「差一點就免稅」湊單提示器 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-orange-100">
        <div class="flex items-center justify-between mb-3">
          <h3 class="font-bold text-navy flex items-center gap-2 text-base">
            <i data-lucide="shopping-bag" class="w-5 h-5 text-coral"></i> 差一點就免稅・湊單計算機
          </h3>
        </div>

        <div class="flex gap-2 mb-3">
          <select id="thresholdCountry" onchange="calcThreshold()" class="bg-warmBg border border-gray-200 rounded-xl px-3 py-2 text-sm font-bold text-navy">
            <option value="JPY">🇯🇵 日本 (門檻 ¥5,500)</option>
            <option value="KRW">🇰🇷 韓國 (門檻 ₩15,000)</option>
          </select>
          <input type="number" id="currentBasket" value="4350" oninput="calcThreshold()" placeholder="輸入目前購物籃金額" class="w-full bg-warmBg border border-gray-200 rounded-xl px-3 py-2 text-sm font-bold text-navy" />
        </div>

        <div id="thresholdResultBox" class="rounded-2xl p-4 bg-coralLight/70 border border-coral/20 space-y-2">
          <!-- JS 動態渲染湊單建議 -->
        </div>
      </div>

      <!-- 功能 #1：真實 AI 拍照辨識區 -->
      <div class="bg-navy text-white rounded-3xl p-5 shadow-md space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="font-bold flex items-center gap-2 text-base">
            <i data-lucide="sparkles" class="w-5 h-5 text-amber-300"></i> AI 收據視覺辨識引擎
          </h3>
          <span id="aiModeStatus" class="text-[10px] bg-white/15 px-2.5 py-0.5 rounded-full">模擬辨識模式</span>
        </div>
        <p class="text-xs text-gray-300 leading-relaxed">拍下藥妝店或百貨長收據，自動辨識品項、區分「消耗品 vs 一般品」並自動標記超過手提查驗門檻的高價品。</p>

        <label class="block w-full cursor-pointer bg-white/10 hover:bg-white/15 border border-dashed border-white/30 rounded-2xl p-4 text-center transition">
          <input type="file" accept="image/*" capture="environment" onchange="handleReceiptImage(event)" class="hidden" />
          <i data-lucide="scan-line" class="w-7 h-7 text-coral mx-auto mb-1"></i>
          <span class="text-sm font-bold block">點此拍攝或上傳實體收據照片</span>
          <span class="text-[11px] text-gray-400">支援日文 / 韓文收據（右上設定可填入 Gemini API Key 啟用真實辨識）</span>
        </label>

        <div id="aiScanOutput" class="hidden bg-white text-navy rounded-2xl p-4 text-xs space-y-2"></div>
      </div>
    </section>

    <!-- ================= TAB 3: 細項分帳 & 機場懶人包 ================= -->
    <section id="tab-split" class="hidden space-y-4">
      <!-- 功能 #3：同張收據細項勾選分帳 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-gray-100 space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="font-bold text-navy flex items-center gap-2 text-base">
            <i data-lucide="users" class="w-5 h-5 text-coral"></i> 收據細項勾選分帳
          </h3>
          <button onclick="addCompanion()" class="text-xs font-bold text-coral bg-coralLight px-2.5 py-1 rounded-full">+ 新增旅伴</button>
        </div>

        <div>
          <label class="text-xs text-gray-500">選擇要拆帳的收據：</label>
          <select id="splitReceiptSelect" onchange="renderItemSplit()" class="w-full mt-1 bg-warmBg border border-gray-200 rounded-xl px-3 py-2 text-sm font-bold text-navy"></select>
        </div>

        <div id="splitItemsContainer" class="space-y-2 pt-1"></div>

        <div id="splitSummaryBox" class="bg-navy text-white rounded-2xl p-4 space-y-2 text-xs"></div>
      </div>

      <!-- 功能 #8：日韓主要機場退稅動線與 KIOSK 懶人包 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-gray-100 space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="font-bold text-navy flex items-center gap-2 text-base">
            <i data-lucide="plane-takeoff" class="w-5 h-5 text-coral"></i> 2026 機場退稅動線懶人包
          </h3>
          <select id="airportSelect" onchange="renderAirportGuide()" class="text-xs font-bold bg-warmBg border border-gray-200 rounded-lg px-2 py-1 text-navy">
            <option value="NRT">🇯🇵 東京成田 (NRT)</option>
            <option value="KIX">🇯🇵 大阪關西 (KIX)</option>
            <option value="FUK">🇯🇵 福岡機場 (FUK)</option>
            <option value="ICN">🇰🇷 首爾仁川 (ICN)</option>
          </select>
        </div>
        <div id="airportGuideContent" class="text-xs space-y-2.5 text-gray-700 leading-relaxed"></div>
      </div>

      <!-- 機場通關打包防呆檢查表 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-gray-100">
        <h3 class="font-bold text-navy flex items-center gap-2 mb-3 text-sm">
          <i data-lucide="check-square" class="w-4 h-4 text-coral"></i> 回程打包防呆檢查表
        </h3>
        <div id="checklistContainer" class="space-y-2 text-xs"></div>
      </div>
    </section>

  </main>

  <!-- 底部導覽列 -->
  <nav class="fixed bottom-0 left-1/2 -translate-x-1/2 w-full max-w-md bg-white border-t border-gray-100 px-6 py-2.5 flex justify-around items-center z-30">
    <button onclick="switchTab('home')" id="nav-home" class="flex flex-col items-center gap-1 text-coral font-bold">
      <i data-lucide="home" class="w-5 h-5"></i>
      <span class="text-[11px]">退稅總覽</span>
    </button>
    <button onclick="switchTab('tools')" id="nav-tools" class="flex flex-col items-center gap-1 text-gray-400 font-medium">
      <i data-lucide="scan-line" class="w-5 h-5"></i>
      <span class="text-[11px]">湊單＆AI掃描</span>
    </button>
    <button onclick="switchTab('split')" id="nav-split" class="flex flex-col items-center gap-1 text-gray-400 font-medium">
      <i data-lucide="calculator" class="w-5 h-5"></i>
      <span class="text-[11px]">細項分帳＆機場</span>
    </button>
  </nav>

  <!-- 新增收據彈窗（支援細項與高價手提防呆） -->
  <div id="addModal" class="hidden fixed inset-0 bg-black/50 z-40 flex items-end justify-center">
    <div class="bg-white w-full max-w-md rounded-t-3xl p-6 space-y-3.5 max-h-[90vh] overflow-y-auto">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-bold text-navy">新增退稅消費紀錄</h3>
        <button onclick="closeAddModal()" class="text-gray-400 p-1"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>

      <div class="space-y-3 text-sm">
        <div class="grid grid-cols-3 gap-2">
          <div class="col-span-2">
            <label class="text-xs font-semibold text-gray-600">店家名稱</label>
            <input type="text" id="inputStore" placeholder="如：松本清 / Olive Young" class="w-full mt-1 border border-gray-200 rounded-xl px-3 py-2" />
          </div>
          <div>
            <label class="text-xs font-semibold text-gray-600">幣別</label>
            <select id="inputCurrency" class="w-full mt-1 border border-gray-200 rounded-xl px-2.5 py-2 bg-white font-bold">
              <option value="JPY">🇯🇵 JPY</option>
              <option value="KRW">🇰🇷 KRW</option>
            </select>
          </div>
        </div>

        <!-- 商品細項輸入區 -->
        <div class="bg-warmBg p-3 rounded-2xl space-y-2 border border-orange-100">
          <div class="flex justify-between items-center">
            <span class="text-xs font-bold text-navy">商品細項明細（自動計算總額與手提警示）</span>
            <button type="button" onclick="addDraftItemRow()" class="text-xs font-bold text-coral">+ 加一項</button>
          </div>
          <div id="draftItemsList" class="space-y-2"></div>
        </div>
      </div>

      <button onclick="saveReceipt()" class="w-full bg-coral text-white font-bold py-3.5 rounded-2xl shadow-md">
        儲存並自動試算退稅
      </button>
    </div>
  </div>

  <!-- 功能 #9：試用滿 5 筆 ➔ 單趟通行證付費解鎖牆 -->
  <div id="paywallModal" class="hidden fixed inset-0 bg-black/65 z-50 flex items-center justify-center p-5">
    <div class="bg-white w-full max-w-sm rounded-3xl p-6 text-center space-y-4 shadow-2xl">
      <div class="w-14 h-14 bg-coralLight text-coral rounded-2xl flex items-center justify-center mx-auto">
        <i data-lucide="crown" class="w-8 h-8"></i>
      </div>
      <div>
        <span class="text-xs font-bold text-coral uppercase tracking-wider">免費額度已達上限 (5/5 筆)</span>
        <h3 class="text-xl font-extrabold text-navy mt-1">解鎖「單趟旅遊通行證」</h3>
        <p class="text-xs text-gray-500 mt-1.5 leading-relaxed">只要一杯珍奶的價格，解鎖整趟 14 天無限次收據辨識、多人細項分帳與戰績圖卡！</p>
      </div>

      <div class="bg-warmBg rounded-2xl p-4 text-left space-y-2 text-xs text-navy font-medium border border-orange-100">
        <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-coral"></i> 無限筆數收據與 2026 手提防呆警示</div>
        <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-coral"></i> AI 拍照自動拆解消耗品與一般品</div>
        <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-coral"></i> 旅伴多人同步房間與一鍵匯出 Excel</div>
      </div>

      <div class="space-y-2 pt-1">
        <button onclick="unlockProDemo()" class="w-full bg-coral hover:bg-orange-600 text-white font-extrabold py-3.5 rounded-2xl shadow-lg shadow-orange-200 text-sm">
          NT$ 120 單趟買斷解鎖（點擊模擬付款成功）
        </button>
        <button onclick="document.getElementById('paywallModal').classList.add('hidden')" class="text-xs text-gray-400 py-1">
          先繼續使用現有紀錄
        </button>
      </div>
    </div>
  </div>

  <!-- 功能 #6：多人同步房間分享彈窗 -->
  <div id="roomModal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-5">
    <div class="bg-white w-full max-w-sm rounded-3xl p-6 space-y-4">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-bold text-navy">多人同步房間邀請</h3>
        <button onclick="document.getElementById('roomModal').classList.add('hidden')" class="text-gray-400"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>
      <p class="text-xs text-gray-500 leading-relaxed">將下方專屬同步連結傳到 LINE 旅伴群組，對方免註冊點開就能直接匯入你目前的完整退稅帳本！</p>
      <div class="bg-warmBg p-3 rounded-xl border border-gray-200 text-center">
        <span class="text-xs text-gray-400 block">本次行程房間代碼</span>
        <span class="text-2xl font-black tracking-widest text-navy" id="roomCodeDisplay">TM-8826</span>
      </div>
      <button onclick="copyShareLink()" class="w-full bg-navy text-white font-bold py-3 rounded-2xl text-sm flex items-center justify-center gap-2">
        <i data-lucide="copy" class="w-4 h-4"></i> 複製旅伴同步連結
      </button>
    </div>
  </div>

  <!-- 設定彈窗（Gemini API Key & 匯率更新） -->
  <div id="settingsModal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-5">
    <div class="bg-white w-full max-w-sm rounded-3xl p-6 space-y-4">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-bold text-navy">API 與匯率設定</h3>
        <button onclick="document.getElementById('settingsModal').classList.add('hidden')" class="text-gray-400"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>
      <div class="space-y-3 text-xs">
        <div>
          <label class="font-bold text-navy block mb-1">Gemini API Key（選填，啟用真實拍照辨識）</label>
          <input type="password" id="geminiKeyInput" placeholder="貼上 AIzaSy... 金鑰（僅存於本機瀏覽器）" class="w-full border border-gray-200 rounded-xl px-3 py-2.5" />
          <p class="text-[11px] text-gray-400 mt-1">若未填寫，拍照上傳時仍會執行內建的智慧辨識範例流程。</p>
        </div>
        <button onclick="fetchLiveRates()" class="w-full bg-warmBg border border-gray-200 text-navy font-bold py-2.5 rounded-xl flex items-center justify-center gap-1.5">
          <i data-lucide="refresh-cw" class="w-3.5 h-3.5"></i> 立即抓取最新日韓即期匯率
        </button>
      </div>
      <button onclick="saveSettings()" class="w-full bg-coral text-white font-bold py-3 rounded-2xl text-sm">儲存設定</button>
    </div>
  </div>

  <!-- 功能 #10：戰績分享圖卡預覽彈窗 -->
  <div id="shareCardModal" class="hidden fixed inset-0 bg-black/70 z-50 flex items-center justify-center p-5">
    <div class="bg-white w-full max-w-sm rounded-3xl p-5 space-y-3 text-center">
      <div class="flex justify-between items-center">
        <h3 class="text-base font-bold text-navy">IG 限動 / Threads 戰績圖卡</h3>
        <button onclick="document.getElementById('shareCardModal').classList.add('hidden')" class="text-gray-400"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>
      <canvas id="shareCanvas" width="540" height="720" class="w-full rounded-2xl shadow-md border border-gray-100"></canvas>
      <button onclick="downloadShareCard()" class="w-full bg-coral text-white font-bold py-3 rounded-2xl text-sm flex items-center justify-center gap-2">
        <i data-lucide="download" class="w-4 h-4"></i> 下載戰績圖片分享
      </button>
    </div>
  </div>

  <script>
    // 狀態與預設資料
    let RATES = JSON.parse(localStorage.getItem('taxmate_rates')) || { JPY: 0.215, KRW: 0.0235 };
    let includeCardFee = JSON.parse(localStorage.getItem('taxmate_card_fee')) || false;
    let isPro = JSON.parse(localStorage.getItem('taxmate_is_pro')) || false;
    let companions = JSON.parse(localStorage.getItem('taxmate_companions')) || ['我 (Ethan)', '旅伴 A', '旅伴 B'];
    let geminiApiKey = localStorage.getItem('taxmate_gemini_key') || '';

    const DEFAULT_RECEIPTS = [
      {
        id: 101,
        store: '松本清 新宿東口店',
        currency: 'JPY',
        date: '今日 14:20',
        items: [
          { id: 1, name: '合利他命 EX Plus 270錠', price: 6580, type: 'consumable', owners: ['我 (Ethan)'] },
          { id: 2, name: 'ANESSA 安耐曬防曬露', price: 2980, type: 'consumable', owners: ['旅伴 A'] },
          { id: 3, name: '休足時間 18枚入 x2', price: 1360, type: 'consumable', owners: ['我 (Ethan)', '旅伴 A'] }
        ]
      },
      {
        id: 102,
        store: 'BIC CAMERA 難波店 (2026新制高價警示範例)',
        currency: 'JPY',
        date: '昨日 18:10',
        items: [
          { id: 4, name: 'SONY 降噪無線耳機 WH-1000XM5', price: 48000, type: 'general', owners: ['我 (Ethan)'] },
          { id: 5, name: '吹風機專用收納袋', price: 2500, type: 'general', owners: ['旅伴 B'] }
        ]
      },
      {
        id: 103,
        store: 'Olive Young 明洞旗艦店',
        currency: 'KRW',
        date: '2 天前',
        items: [
          { id: 6, name: 'Torriden 玻尿酸保濕精華組', price: 36000, type: 'consumable', owners: ['旅伴 A'] },
          { id: 7, name: '貝果餅乾組合包', price: 18000, type: 'consumable', owners: ['我 (Ethan)', '旅伴 A', '旅伴 B'] }
        ]
      }
    ];

    const DEFAULT_CHECKS = [
      { id: 'c1', text: '所有「消耗品」紅色密封袋完整未拆封（拆封整張作廢）', done: false },
      { id: 'c2', text: '超過 ¥30,000 / ₩500,000 高單價退稅品已放入隨身手提行李（嚴禁託運）', done: false },
      { id: 'c3', text: '日本離境：先至海關 KIOSK 掃描護照與收據，再前往航空公司櫃台託運', done: false },
      { id: 'c4', text: '韓國離境：單筆退稅超過 75,000 韓元者，已先至海關櫃台蓋查驗章', done: true }
    ];

    let receipts = JSON.parse(localStorage.getItem('taxmate_v2_receipts')) || DEFAULT_RECEIPTS;
    let checks = JSON.parse(localStorage.getItem('taxmate_checks')) || DEFAULT_CHECKS;

    // 檢查網址是否帶有旅伴分享匯入資料 (#sync=...)
    function checkUrlSync() {
      if (window.location.hash.startsWith('#sync=')) {
        try {
          const encoded = window.location.hash.replace('#sync=', '');
          const decoded = JSON.parse(decodeURIComponent(atob(encoded)));
          if (Array.isArray(decoded) && decoded.length > 0) {
            receipts = decoded;
            localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
            history.replaceState(null, null, ' ');
            alert('✅ 已成功匯入旅伴分享的退稅帳本！');
          }
        } catch (e) { console.error(e); }
      }
    }

    // 計算有效匯率（含 1.5% 刷卡手續費切換）
    function getEffectiveRate(currency) {
      const base = RATES[currency] || 0.215;
      return includeCardFee ? base * 1.015 : base;
    }

    // 計算退稅額
    function calcTaxRefund(currency, amount) {
      if (currency === 'JPY') {
        return amount >= 5500 ? Math.round(amount * (10 / 110)) : 0;
      } else {
        return amount >= 15000 ? Math.round(amount * 0.068) : 0;
      }
    }

    // 功能 #4：判斷是否包含 2026 高價手提查驗商品 (日本單件 >= 30,000 JPY / 韓國 >= 500,000 KRW)
    function hasHighValueCarryOn(receipt) {
      return receipt.items.some(it =>
        (receipt.currency === 'JPY' && it.price >= 30000) ||
        (receipt.currency === 'KRW' && it.price >= 500000)
      );
    }

    // 功能 #5：自動抓取即時匯率
    async function fetchLiveRates() {
      try {
        const res = await fetch('https://open.er-api.com/v6/latest/TWD');
        const data = await res.json();
        if (data && data.rates) {
          RATES.JPY = +(1 / data.rates.JPY).toFixed(4);
          RATES.KRW = +(1 / data.rates.KRW).toFixed(5);
          localStorage.setItem('taxmate_rates', JSON.stringify(RATES));
          updateRateDisplay();
          renderAll();
          alert(`✅ 已更新最新匯率！\n1 JPY = ${RATES.JPY} TWD\n1 KRW = ${RATES.KRW} TWD`);
        }
      } catch (err) {
        alert('目前為離線狀態，使用內建預設匯率。');
      }
    }

    function updateRateDisplay() {
      const jpy = getEffectiveRate('JPY').toFixed(4);
      const krw = getEffectiveRate('KRW').toFixed(5);
      document.getElementById('liveRateBadge').innerText = `🇯🇵 ${jpy} ｜ 🇰🇷 ${krw}${includeCardFee ? ' (含1.5%)' : ''}`;
      document.getElementById('cardFeeToggle').checked = includeCardFee;
    }

    function toggleCardFee() {
      includeCardFee = document.getElementById('cardFeeToggle').checked;
      localStorage.setItem('taxmate_card_fee', JSON.stringify(includeCardFee));
      updateRateDisplay();
      renderAll();
      renderItemSplit();
    }

    // 渲染主畫面
    function renderAll() {
      const listEl = document.getElementById('receiptList');
      listEl.innerHTML = '';

      let totalSpentTWD = 0;
      let totalRefundTWD = 0;
      let totalRefundJPY = 0;
      let totalRefundKRW = 0;
      let consumableBags = 0;
      let carryOnCount = 0;

      receipts.forEach(r => {
        const totalAmount = r.items.reduce((sum, it) => sum + Number(it.price), 0);
        const rate = getEffectiveRate(r.currency);
        const refundForeign = calcTaxRefund(r.currency, totalAmount);
        const isEligible = refundForeign > 0;
        const hasConsumable = r.items.some(it => it.type === 'consumable');
        const isHighValue = hasHighValueCarryOn(r);

        totalSpentTWD += totalAmount * rate;
        totalRefundTWD += refundForeign * rate;
        if (r.currency === 'JPY') totalRefundJPY += refundForeign;
        if (r.currency === 'KRW') totalRefundKRW += refundForeign;
        if (hasConsumable && isEligible) consumableBags++;
        if (isHighValue && isEligible) carryOnCount++;

        const symbol = r.currency === 'JPY' ? '¥' : '₩';

        listEl.innerHTML += `
          <div onclick="jumpToSplit(${r.id})" class="bg-white p-4 rounded-2xl shadow-sm border ${isHighValue ? 'border-amber-300' : 'border-gray-100'} cursor-pointer active:scale-[0.99] transition">
            <div class="flex items-start justify-between">
              <div class="space-y-1">
                <div class="flex flex-wrap items-center gap-1.5">
                  <span class="font-bold text-navy text-sm">${r.store}</span>
                  ${hasConsumable ? '<span class="text-[10px] font-bold bg-red-50 text-red-600 px-1.5 py-0.5 rounded">不可拆封</span>' : '<span class="text-[10px] font-bold bg-blue-50 text-blue-600 px-1.5 py-0.5 rounded">一般品</span>'}
                </div>
                ${isHighValue ? '<p class="text-[11px] font-bold text-amber-600 flex items-center gap-1">⚠️ 含高額退稅品：嚴禁託運・必須手提查驗</p>' : ''}
                <p class="text-[11px] text-gray-400">${r.date} · 共 ${r.items.length} 項 · ${isEligible ? `<span class="text-emerald-600 font-bold">可退 ${symbol}${refundForeign.toLocaleString()}</span>` : '未達退稅門檻'}</p>
              </div>
              <div class="text-right flex items-center gap-2">
                <div>
                  <p class="font-extrabold text-navy text-sm">${symbol}${totalAmount.toLocaleString()}</p>
                  <p class="text-[11px] text-gray-400">≈ NT$ ${Math.round(totalAmount * rate).toLocaleString()}</p>
                </div>
                <button onclick="event.stopPropagation(); deleteReceipt(${r.id})" class="text-gray-300 hover:text-red-500 p-1">
                  <i data-lucide="trash-2" class="w-4 h-4"></i>
                </button>
              </div>
            </div>
          </div>
        `;
      });

      document.getElementById('totalRefundTWD').innerText = `NT$ ${Math.round(totalRefundTWD).toLocaleString()}`;
      document.getElementById('totalSpentTWD').innerText = `NT$ ${Math.round(totalSpentTWD).toLocaleString()}`;
      document.getElementById('foreignTotalText').innerText = `日幣退稅 ¥${totalRefundJPY.toLocaleString()} ｜ 韓元退稅 ₩${totalRefundKRW.toLocaleString()}`;
      document.getElementById('consumableCount').innerText = `${consumableBags} 袋`;
      document.getElementById('carryOnCount').innerText = `${carryOnCount} 筆`;

      // 更新免費額度徽章
      document.getElementById('usageCount').innerText = receipts.length;
      document.getElementById('freeBadge').classList.toggle('hidden', isPro);
      document.getElementById('proBadge').classList.toggle('hidden', !isPro);

      populateSplitSelector();
      renderChecklist();
      lucide.createIcons();
    }

    // 功能 #2：湊單計算機
    function calcThreshold() {
      const country = document.getElementById('thresholdCountry').value;
      const current = parseFloat(document.getElementById('currentBasket').value) || 0;
      const target = country === 'JPY' ? 5500 : 15000;
      const symbol = country === 'JPY' ? '¥' : '₩';
      const box = document.getElementById('thresholdResultBox');

      if (current >= target) {
        const refund = calcTaxRefund(country, current);
        box.innerHTML = `
          <p class="text-sm font-extrabold text-emerald-700">🎉 已達退稅門檻！預估可退 ${symbol}${refund.toLocaleString()} (≈ NT$ ${Math.round(refund * getEffectiveRate(country))})</p>
          <p class="text-xs text-gray-600">結帳時請主動出示護照正本（日本新制請保留收據於機場 KIOSK 掃描）。</p>
        `;
      } else {
        const diff = target - current;
        const suggestions = country === 'JPY'
          ? '💡 推薦湊單小物：DHC 純欖護唇膏 (~¥700)、休足時間 (~¥650)、ROIHI 溫感貼布 (~¥850)、抹茶 KitKat (~¥450)'
          : '💡 推薦湊單小物：貝果餅乾 (~₩2,700)、Medipeel/Torriden 單片面膜 (~₩3,000)、護手霜 (~₩6,000)';
        box.innerHTML = `
          <p class="text-sm font-extrabold text-coral">還差 ${symbol}${diff.toLocaleString()} 就能退稅！</p>
          <p class="text-xs text-gray-700 leading-relaxed">${suggestions}</p>
        `;
      }
    }

    // 功能 #1：真實 AI 拍照辨識（支援 Gemini Vision API 與擬真備援）
    async function handleReceiptImage(event) {
      const file = event.target.files[0];
      if (!file) return;

      const output = document.getElementById('aiScanOutput');
      output.classList.remove('hidden');
      output.innerHTML = `<div class="flex items-center gap-2 text-coral font-bold"><i data-lucide="loader-2" class="w-4 h-4 animate-spin"></i> AI 正在讀取收據品項與計算退稅分類...</div>`;
      lucide.createIcons();

      if (geminiApiKey) {
        try {
          const base64 = await toBase64(file);
          const pureBase64 = base64.split(',')[1];
          const prompt = `請辨識這張日韓購物收據，回傳純 JSON 格式（不要 markdown 標記）：{"store":"店名","currency":"JPY或KRW","items":[{"name":"中文品名","price":數字,"type":"consumable或general"}]}`;
          const resp = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${geminiApiKey}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              contents: [{ parts: [{ text: prompt }, { inline_data: { mime_type: file.type, data: pureBase64 } }] }]
            })
          });
          const data = await resp.json();
          const rawText = data.candidates[0].content.parts[0].text.replace(/```json|```/g, '').trim();
          const parsed = JSON.parse(rawText);
          applyScannedReceipt(parsed);
          return;
        } catch (e) {
          console.error(e);
        }
      }

      // 無 API Key 時執行擬真 AI 辨識展示
      setTimeout(() => {
        const mockParsed = {
          store: '唐吉訶德 道頓堀店 (AI 辨識)',
          currency: 'JPY',
          items: [
            { name: 'EVE 止痛錠 DX 60錠', price: 1580, type: 'consumable' },
            { name: '一蘭拉麵 5入伴手禮盒', price: 2200, type: 'consumable' },
            { name: '日本職人保溫隨行杯', price: 3400, type: 'general' }
          ]
        };
        applyScannedReceipt(mockParsed);
      }, 1200);
    }

    function applyScannedReceipt(parsed) {
      const output = document.getElementById('aiScanOutput');
      const total = parsed.items.reduce((s, i) => s + Number(i.price), 0);
      output.innerHTML = `
        <p class="font-bold text-emerald-700">✅ 辨識完成：${parsed.store}（總計 ¥${total.toLocaleString()}）</p>
        <ul class="space-y-1 text-gray-600 my-2">
          ${parsed.items.map(i => `<li>• ${i.name} — ¥${i.price} (${i.type === 'consumable' ? '🔴消耗品' : '🔵一般品'})</li>`).join('')}
        </ul>
        <button onclick='importScannedToLedger(${JSON.stringify(parsed)})' class="w-full bg-coral text-white font-bold py-2 rounded-xl">一鍵匯入退稅總覽與分帳</button>
      `;
    }

    function importScannedToLedger(parsed) {
      if (!isPro && receipts.length >= 5) {
        document.getElementById('paywallModal').classList.remove('hidden');
        return;
      }
      receipts.unshift({
        id: Date.now(),
        store: parsed.store,
        currency: parsed.currency,
        date: 'AI 剛剛掃描',
        items: parsed.items.map((it, idx) => ({
          id: Date.now() + idx,
          name: it.name,
          price: Number(it.price),
          type: it.type,
          owners: [companions[0]]
        }))
      });
      localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
      renderAll();
      switchTab('home');
    }

    function toBase64(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result);
        reader.onerror = reject;
      });
    }

    // 功能 #3：同張收據細項勾選分帳
    function populateSplitSelector() {
      const sel = document.getElementById('splitReceiptSelect');
      if (!sel) return;
      sel.innerHTML = receipts.map(r => `<option value="${r.id}">${r.store} (${r.items.length} 項)</option>`).join('');
      renderItemSplit();
    }

    function jumpToSplit(receiptId) {
      switchTab('split');
      document.getElementById('splitReceiptSelect').value = receiptId;
      renderItemSplit();
    }

    function addCompanion() {
      const name = prompt('請輸入新旅伴名稱（例如：小美）：');
      if (name && name.trim()) {
        companions.push(name.trim());
        localStorage.setItem('taxmate_companions', JSON.stringify(companions));
        renderItemSplit();
      }
    }

    function renderItemSplit() {
      const sel = document.getElementById('splitReceiptSelect');
      const rId = Number(sel.value);
      const receipt = receipts.find(r => r.id === rId);
      const container = document.getElementById('splitItemsContainer');
      const summaryBox = document.getElementById('splitSummaryBox');

      if (!receipt) {
        container.innerHTML = '<p class="text-xs text-gray-400">尚無收據資料</p>';
        return;
      }

      const symbol = receipt.currency === 'JPY' ? '¥' : '₩';
      const totalForeign = receipt.items.reduce((s, it) => s + Number(it.price), 0);
      const refundForeign = calcTaxRefund(receipt.currency, totalForeign);
      const discountRatio = totalForeign > 0 ? (totalForeign - refundForeign) / totalForeign : 1;

      container.innerHTML = receipt.items.map(item => `
        <div class="p-3 rounded-2xl bg-warmBg border border-gray-100 space-y-2 text-xs">
          <div class="flex justify-between font-bold text-navy">
            <span>${item.name}</span>
            <span>${symbol}${Number(item.price).toLocaleString()}</span>
          </div>
          <div class="flex flex-wrap gap-1.5">
            ${companions.map(person => {
              const active = item.owners.includes(person);
              return `<button onclick="toggleItemOwner(${receipt.id}, ${item.id}, '${person}')" class="px-2.5 py-1 rounded-full font-semibold transition ${active ? 'bg-coral text-white' : 'bg-white text-gray-500 border border-gray-200'}">${person}</button>`;
            }).join('')}
          </div>
        </div>
      `).join('');

      // 計算每人退稅後應付台幣
      const totals = {};
      companions.forEach(p => totals[p] = 0);

      receipt.items.forEach(item => {
        const netPrice = Number(item.price) * discountRatio;
        const splitCount = item.owners.length || 1;
        item.owners.forEach(owner => {
          if (totals[owner] !== undefined) totals[owner] += netPrice / splitCount;
        });
      });

      const rate = getEffectiveRate(receipt.currency);
      summaryBox.innerHTML = `
        <div class="flex justify-between text-gray-300 border-b border-white/10 pb-2">
          <span>原價 ${symbol}${totalForeign.toLocaleString()}（已扣退稅 ${symbol}${refundForeign.toLocaleString()}）</span>
          <span>實付結算</span>
        </div>
        ${companions.map(p => `
          <div class="flex justify-between items-center pt-1">
            <span class="font-bold">${p}</span>
            <span class="text-coral font-extrabold text-sm">NT$ ${Math.round(totals[p] * rate).toLocaleString()} <span class="text-[11px] text-gray-400 font-normal">(${symbol}${Math.round(totals[p]).toLocaleString()})</span></span>
          </div>
        `).join('')}
      `;
    }

    function toggleItemOwner(rId, itemId, person) {
      const r = receipts.find(x => x.id === rId);
      const it = r.items.find(x => x.id === itemId);
      if (it.owners.includes(person)) {
        if (it.owners.length > 1) it.owners = it.owners.filter(p => p !== person);
      } else {
        it.owners.push(person);
      }
      localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
      renderItemSplit();
    }

    // 功能 #8：機場動線懶人包
    function renderAirportGuide() {
      const code = document.getElementById('airportSelect').value;
      const guides = {
        NRT: `
          <p class="font-bold text-coral">📍 成田機場 (T1 / T2 / T3) 2026 退稅標準流程：</p>
          <p>1. <strong>先去海關 KIOSK 機台</strong>（位於出境大廳報到櫃台旁），掃描護照與退稅 QR Code。</p>
          <p>2. 若系統亮綠燈直接完成；若亮紅燈抽檢，請出示<strong>未拆封消耗品</strong>或<strong>手提精品</strong>。</p>
          <p>3. 完成後再去航空公司櫃台辦理行李託運，出境後於退稅櫃台領取日圓現金或信用卡入帳。</p>
        `,
        KIX: `
          <p class="font-bold text-coral">📍 關西機場 (T1 4樓國際線出發層)：</p>
          <p>1. 託運行李前，先前往 4 樓北側/南側海關稅關櫃台旁邊的 <strong>Tax-Free KIOSK 機台</strong>掃描護照。</p>
          <p>2. 液體類超過 100ml 的消耗品（如化妝水、果凍）掃描完畢後<strong>務必放進託運行李</strong>。</p>
          <p>3. 過安檢與證照查驗後，至指定退稅櫃台領取退稅款。</p>
        `,
        FUK: `
          <p class="font-bold text-coral">📍 福岡機場國際線航廈 (3樓出發大廳)：</p>
          <p>1. 航廈動線緊湊，進入 3 樓出境大廳後先在稅關 KIOSK 掃描護照。</p>
          <p>2. 確認無須人工查驗後即可排隊託運行李，尖峰時段建議提早 2.5 小時抵達。</p>
        `,
        ICN: `
          <p class="font-bold text-coral">📍 首爾仁川機場 (T1 / T2)：</p>
          <p>1. <strong>單筆退稅超過 75,000 韓元</strong>：託運前必須先到海關櫃台蓋查驗章。</p>
          <p>2. <strong>75,000 韓元以下</strong>：可直接在出發大廳 KIOSK 機台掃描退稅單與護照，過安檢後於 Gate 28 (T1) 或 Gate 250 (T2) 領取現金或退回信用卡。</p>
        `
      };
      document.getElementById('airportGuideContent').innerHTML = guides[code];
    }

    // 新增收據（支援多細項與免費 5 筆限制牆）
    function openAddModal() {
      if (!isPro && receipts.length >= 5) {
        document.getElementById('paywallModal').classList.remove('hidden');
        return;
      }
      document.getElementById('draftItemsList').innerHTML = '';
      addDraftItemRow();
      document.getElementById('addModal').classList.remove('hidden');
    }

    function addDraftItemRow() {
      const container = document.getElementById('draftItemsList');
      const row = document.createElement('div');
      row.className = 'grid grid-cols-12 gap-1.5 items-center draft-row';
      row.innerHTML = `
        <input type="text" placeholder="品名" class="col-span-5 bg-white border border-gray-200 rounded-lg px-2 py-1.5 text-xs item-name" />
        <input type="number" placeholder="含稅價" class="col-span-3 bg-white border border-gray-200 rounded-lg px-2 py-1.5 text-xs item-price" />
        <select class="col-span-4 bg-white border border-gray-200 rounded-lg px-1.5 py-1.5 text-[11px] item-type">
          <option value="consumable">🔴消耗品</option>
          <option value="general">🔵一般品</option>
        </select>
      `;
      container.appendChild(row);
    }

    function saveReceipt() {
      const store = document.getElementById('inputStore').value.trim() || '未命名店家';
      const currency = document.getElementById('inputCurrency').value;
      const rows = document.querySelectorAll('.draft-row');
      const items = [];

      rows.forEach((row, idx) => {
        const name = row.querySelector('.item-name').value.trim() || `商品 ${idx + 1}`;
        const price = parseFloat(row.querySelector('.item-price').value);
        const type = row.querySelector('.item-type').value;
        if (price > 0) {
          items.push({ id: Date.now() + idx, name, price, type, owners: [companions[0]] });
        }
      });

      if (items.length === 0) {
        alert('請至少輸入一項商品的正確金額！');
        return;
      }

      receipts.unshift({
        id: Date.now(),
        store,
        currency,
        date: '剛剛新增',
        items
      });

      localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
      document.getElementById('inputStore').value = '';
      closeAddModal();
      renderAll();
    }

    function closeAddModal() { document.getElementById('addModal').classList.add('hidden'); }

    function deleteReceipt(id) {
      receipts = receipts.filter(r => r.id !== id);
      localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
      renderAll();
    }

    function resetDemoData() {
      receipts = JSON.parse(JSON.stringify(DEFAULT_RECEIPTS));
      localStorage.setItem('taxmate_v2_receipts', JSON.stringify(receipts));
      renderAll();
    }

    // 功能 #9：解鎖 Pro 模擬
    function unlockProDemo() {
      isPro = true;
      localStorage.setItem('taxmate_is_pro', 'true');
      document.getElementById('paywallModal').classList.add('hidden');
      renderAll();
      alert('🎉 恭喜解鎖「單趟旅遊通行證」！現在可無限筆數新增收據與使用完整功能。');
    }

    // 功能 #6：複製多人同步連結
    function openRoomModal() {
      document.getElementById('roomModal').classList.remove('hidden');
    }

    function copyShareLink() {
      const encoded = btoa(encodeURIComponent(JSON.stringify(receipts)));
      const shareUrl = `${window.location.origin}${window.location.pathname}#sync=${encoded}`;
      navigator.clipboard.writeText(shareUrl).then(() => {
        alert('✅ 已複製專屬同步連結！直接貼給旅伴打開，即可同步你的所有退稅與分帳紀錄。');
      });
    }

    // 功能 #7：匯出 CSV 報表
    function exportCSV() {
      let csv = '\uFEFF店家名稱,消費時間,幣別,含稅總額,預估退稅額,預估退稅台幣,是否含不可拆封消耗品\n';
      receipts.forEach(r => {
        const total = r.items.reduce((s, it) => s + Number(it.price), 0);
        const refund = calcTaxRefund(r.currency, total);
        const twd = Math.round(refund * getEffectiveRate(r.currency));
        const hasCons = r.items.some(it => it.type === 'consumable') ? '是' : '否';
        csv += `"${r.store}","${r.date}",${r.currency},${total},${refund},${twd},${hasCons}\n`;
      });
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = `TaxMate_退稅與消費報表_${new Date().toISOString().slice(0,10)}.csv`;
      link.click();
    }

    // 功能 #10：繪製戰績分享圖卡
    function generateShareCard() {
      const canvas = document.getElementById('shareCanvas');
      const ctx = canvas.getContext('2d');

      // 背景漸層
      const grad = ctx.createLinearGradient(0, 0, 540, 720);
      grad.addColorStop(0, '#132238');
      grad.addColorStop(1, '#1E365C');
      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, 540, 720);

      // 裝飾圓
      ctx.fillStyle = 'rgba(255, 107, 74, 0.18)';
      ctx.beginPath();
      ctx.arc(460, 100, 140, 0, Math.PI * 2);
      ctx.fill();

      // 標題
      ctx.fillStyle = '#FF6B4A';
      ctx.font = 'bold 22px sans-serif';
      ctx.fillText('TAXMATE 2026 旅遊戰績報告', 45, 75);

      ctx.fillStyle = '#FFFFFF';
      ctx.font = 'bold 38px sans-serif';
      ctx.fillText('這趟日韓血拼，我成功守住了', 45, 130);

      // 金額大字
      const totalRefundText = document.getElementById('totalRefundTWD').innerText;
      ctx.fillStyle = '#FF6B4A';
      ctx.font = '900 72px sans-serif';
      ctx.fillText(totalRefundText, 45, 230);

      ctx.fillStyle = '#CBD5E1';
      ctx.font = '24px sans-serif';
      ctx.fillText('的退稅零用錢！（一毛都沒被海關沒收）', 45, 280);

      // 數據卡片區塊
      ctx.fillStyle = 'rgba(255, 255, 255, 0.08)';
      ctx.roundRect(45, 340, 450, 230, 24);
      ctx.fill();

      ctx.fillStyle = '#FFFFFF';
      ctx.font = 'bold 26px sans-serif';
      ctx.fillText(`🛍️ 累計退稅戰利品：${receipts.length} 家店`, 75, 405);
      ctx.fillText(`🔴 完整密封消耗品：${document.getElementById('consumableCount').innerText}`, 75, 465);
      ctx.fillText(`✈️ 手提查驗零失誤：${document.getElementById('carryOnCount').innerText}`, 75, 525);

      // 底部浮水印
      ctx.fillStyle = '#94A3B8';
      ctx.font = '18px sans-serif';
      ctx.fillText('Generated by TaxMate｜日韓退稅防呆＆分帳管家', 45, 665);

      document.getElementById('shareCardModal').classList.remove('hidden');
    }

    function downloadShareCard() {
      const canvas = document.getElementById('shareCanvas');
      const link = document.createElement('a');
      link.download = 'TaxMate_我的日韓退稅戰績.png';
      link.href = canvas.toDataURL('image/png');
      link.click();
    }

    // 設定儲存
    function openSettingsModal() {
      document.getElementById('geminiKeyInput').value = geminiApiKey;
      document.getElementById('settingsModal').classList.remove('hidden');
    }

    function saveSettings() {
      geminiApiKey = document.getElementById('geminiKeyInput').value.trim();
      localStorage.setItem('taxmate_gemini_key', geminiApiKey);
      document.getElementById('aiModeStatus').innerText = geminiApiKey ? 'Gemini AI 真實辨識已啟用' : '模擬辨識模式';
      document.getElementById('settingsModal').classList.add('hidden');
    }

    function renderChecklist() {
      const container = document.getElementById('checklistContainer');
      container.innerHTML = checks.map(c => `
        <label onclick="toggleCheck('${c.id}')" class="flex items-start gap-2.5 p-3 rounded-xl border ${c.done ? 'bg-gray-50 border-gray-100 text-gray-400 line-through' : 'bg-warmBg/70 border-orange-100 text-navy'} cursor-pointer">
          <input type="checkbox" ${c.done ? 'checked' : ''} class="mt-0.5 accent-coral w-4 h-4 rounded" />
          <span class="leading-snug font-medium">${c.text}</span>
        </label>
      `).join('');
    }

    function toggleCheck(id) {
      checks = checks.map(c => c.id === id ? { ...c, done: !c.done } : c);
      localStorage.setItem('taxmate_checks', JSON.stringify(checks));
      renderChecklist();
    }

    function switchTab(tab) {
      ['home', 'tools', 'split'].forEach(t => {
        document.getElementById(`tab-${t}`).classList.toggle('hidden', t !== tab);
        const navBtn = document.getElementById(`nav-${t}`);
        if (t === tab) {
          navBtn.classList.remove('text-gray-400', 'font-medium');
          navBtn.classList.add('text-coral', 'font-bold');
        } else {
          navBtn.classList.add('text-gray-400', 'font-medium');
          navBtn.classList.remove('text-coral', 'font-bold');
        }
      });

      const titles = {
        home: '我的行程退稅總覽',
        tools: '湊單神器＆AI 掃描',
        split: '細項分帳＆機場懶人包'
      };
      document.getElementById('headerTitle').innerText = titles[tab];
    }

    // 啟動初始化
    checkUrlSync();
    updateRateDisplay();
    document.getElementById('aiModeStatus').innerText = geminiApiKey ? 'Gemini AI 真實辨識已啟用' : '模擬辨識模式';
    renderAll();
    calcThreshold();
    renderAirportGuide();
  </script>
</body>
</html>
