<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <title>TaxMate｜日韓退稅防呆＆分帳管家</title>
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
  <header class="px-5 pt-6 pb-3 flex items-center justify-between sticky top-0 bg-warmBg/90 backdrop-blur-md z-20">
    <div>
      <span class="text-xs font-semibold tracking-wider text-coral uppercase">TaxMate MVP</span>
      <h1 id="headerTitle" class="text-2xl font-bold text-navy">我的行程退稅總覽</h1>
    </div>
    <div class="flex items-center gap-2">
      <select id="currencyRateSelect" onchange="updateRate()" class="text-xs font-medium bg-white border border-gray-200 rounded-full px-3 py-1.5 text-navy shadow-sm focus:outline-none">
        <option value="JPY">🇯🇵 1 JPY ≈ 0.215 TWD</option>
        <option value="KRW">🇰🇷 1 KRW ≈ 0.0235 TWD</option>
      </select>
    </div>
  </header>

  <!-- 主要內容區 -->
  <main class="px-5 flex-1 space-y-5">

    <!-- ================= TAB 1: 首頁總覽 ================= -->
    <section id="tab-home" class="space-y-5">
      <!-- 退稅總額卡片 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-orange-100/80 relative overflow-hidden">
        <div class="flex justify-between items-start">
          <div>
            <p class="text-xs font-medium text-gray-500">預估可拿回退稅總額</p>
            <div class="flex items-baseline gap-1.5 mt-1">
              <span class="text-3xl font-extrabold text-navy" id="totalRefundTWD">NT$ 0</span>
              <span class="text-xs font-semibold text-coral bg-coralLight px-2 py-0.5 rounded-full">含稅消費 <span id="totalSpentTWD">NT$ 0</span></span>
            </div>
            <p class="text-xs text-gray-400 mt-1.5" id="foreignTotalText">日幣退稅約 ¥0 ｜ 韓元退稅約 ₩0</p>
          </div>
          <div class="w-12 h-12 rounded-2xl bg-coralLight flex items-center justify-center text-coral">
            <i data-lucide="badge-japanese-yen" class="w-6 h-6"></i>
          </div>
        </div>

        <!-- 退稅防呆提示條 -->
        <div class="mt-4 pt-3 border-t border-gray-100 flex items-center justify-between text-xs">
          <span class="text-gray-600 flex items-center gap-1.5">
            <span class="w-2 h-2 rounded-full bg-coral inline-block"></span>
            不可拆封消耗品：<strong id="consumableCount" class="text-navy">0 筆</strong>
          </span>
          <button onclick="switchTab('checklist')" class="text-coral font-semibold flex items-center gap-0.5">
            查看打包防呆 <i data-lucide="chevron-right" class="w-3.5 h-3.5"></i>
          </button>
        </div>
      </div>

      <!-- 模擬 AI 拍照掃描按鈕 -->
      <button onclick="openAddModal()" class="w-full bg-coral hover:bg-orange-600 active:scale-[0.99] transition text-white font-bold py-3.5 px-5 rounded-2xl shadow-md shadow-orange-200 flex items-center justify-center gap-2.5">
        <i data-lucide="camera" class="w-5 h-5"></i>
        <span>模擬 AI 拍收據 / 新增退稅消費</span>
      </button>

      <!-- 近期收據列表 -->
      <div>
        <div class="flex items-center justify-between mb-3">
          <h2 class="text-base font-bold text-navy">已記錄收據明細</h2>
          <button onclick="resetDemoData()" class="text-xs text-gray-400 underline">重置範例資料</button>
        </div>
        <div id="receiptList" class="space-y-3">
          <!-- JS 動態渲染 -->
        </div>
      </div>
    </section>

    <!-- ================= TAB 2: AI 收據辨識模擬 ================= -->
    <section id="tab-scanner" class="hidden space-y-4">
      <div class="bg-navy text-white rounded-3xl p-5 shadow-md">
        <div class="flex items-center justify-between mb-3">
          <span class="text-xs bg-white/15 px-2.5 py-1 rounded-full">AI 視覺辨識預覽範例</span>
          <span class="text-xs text-orange-300 font-medium">日本藥妝店收據</span>
        </div>
        <h3 class="text-lg font-bold">スギ薬局 / Sun Drug 新宿店</h3>
        <p class="text-xs text-gray-300 mt-0.5">已自動辨識 4 項商品・達日本退稅門檻 (¥5,500 含稅)</p>

        <div class="mt-4 bg-white text-navy rounded-2xl p-4 space-y-3 text-sm">
          <div class="flex justify-between items-start border-b border-gray-100 pb-2.5">
            <div>
              <span class="text-[11px] font-bold bg-red-100 text-red-600 px-2 py-0.5 rounded">消耗品・不可拆封</span>
              <p class="font-semibold mt-1">Biore 含水防曬保濕水凝乳</p>
            </div>
            <span class="font-bold">¥1,200</span>
          </div>
          <div class="flex justify-between items-start border-b border-gray-100 pb-2.5">
            <div>
              <span class="text-[11px] font-bold bg-red-100 text-red-600 px-2 py-0.5 rounded">消耗品・不可拆封</span>
              <p class="font-semibold mt-1">ANESSA 安耐曬金鑽高效防曬露</p>
            </div>
            <span class="font-bold">¥2,980</span>
          </div>
          <div class="flex justify-between items-start border-b border-gray-100 pb-2.5">
            <div>
              <span class="text-[11px] font-bold bg-blue-100 text-blue-700 px-2 py-0.5 rounded">一般物品・可拆封</span>
              <p class="font-semibold mt-1">象印不鏽鋼保溫瓶 480ml</p>
            </div>
            <span class="font-bold">¥3,200</span>
          </div>
          <div class="flex justify-between items-center pt-1 text-xs text-gray-500">
            <span>含稅總計：¥7,380</span>
            <span class="text-coral font-bold text-sm">預估退稅：¥670 (≈ NT$ 144)</span>
          </div>
        </div>

        <div class="mt-4 bg-orange-500/20 border border-orange-400/40 rounded-xl p-3 text-xs text-orange-100 flex gap-2.5 items-start">
          <i data-lucide="alert-triangle" class="w-4 h-4 text-orange-300 shrink-0 mt-0.5"></i>
          <p><strong>防呆提醒：</strong>此張收據同時包含「消耗品」與「一般物品」，店員會將整袋密封，<strong>離境前絕對不可拆封</strong>，否則整張收據 ¥670 退稅將失效。</p>
        </div>
      </div>
    </section>

    <!-- ================= TAB 3: 分帳與機場防呆 ================= -->
    <section id="tab-checklist" class="hidden space-y-5">
      <!-- 旅伴分帳計算機 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-gray-100">
        <div class="flex items-center justify-between mb-3">
          <h3 class="font-bold text-navy flex items-center gap-2">
            <i data-lucide="users" class="w-4 h-4 text-coral"></i> 旅伴退稅後分帳試算
          </h3>
          <span class="text-xs text-gray-400">自動扣除退稅額</span>
        </div>

        <div class="space-y-3 text-sm">
          <div>
            <label class="text-xs text-gray-500">共同消費含稅金額</label>
            <div class="flex gap-2 mt-1">
              <select id="splitCurrency" onchange="calcSplit()" class="bg-warmBg border border-gray-200 rounded-xl px-3 py-2 font-semibold text-navy">
                <option value="JPY">JPY ¥</option>
                <option value="KRW">KRW ₩</option>
              </select>
              <input type="number" id="splitAmount" value="24000" oninput="calcSplit()" class="w-full bg-warmBg border border-gray-200 rounded-xl px-3 py-2 font-bold text-navy" />
            </div>
          </div>

          <div class="grid grid-cols-2 gap-2">
            <div>
              <label class="text-xs text-gray-500">分帳人數</label>
              <input type="number" id="splitPeople" value="3" min="1" oninput="calcSplit()" class="w-full mt-1 bg-warmBg border border-gray-200 rounded-xl px-3 py-2 font-bold text-navy" />
            </div>
            <div>
              <label class="text-xs text-gray-500">退稅狀態</label>
              <select id="splitTaxMode" onchange="calcSplit()" class="w-full mt-1 bg-warmBg border border-gray-200 rounded-xl px-3 py-2 font-semibold text-navy">
                <option value="refund">已扣預估退稅 (~9%)</option>
                <option value="taxed">無退稅 (含稅原價)</option>
              </select>
            </div>
          </div>

          <div class="bg-navy text-white rounded-2xl p-4 mt-2 flex justify-between items-center">
            <div>
              <p class="text-xs text-gray-300">每人實際應付台幣</p>
              <p class="text-2xl font-extrabold text-coral mt-0.5" id="splitResultTWD">NT$ 1,565</p>
            </div>
            <div class="text-right text-xs text-gray-300">
              <p id="splitResultForeign">每人約 ¥7,280</p>
              <p class="text-[11px] text-gray-400 mt-0.5">含刷卡手續費緩衝</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 機場退稅打包防呆清單 -->
      <div class="bg-white rounded-3xl p-5 shadow-sm border border-gray-100">
        <h3 class="font-bold text-navy flex items-center gap-2 mb-3">
          <i data-lucide="plane-takeoff" class="w-4 h-4 text-coral"></i> 機場退稅通關防呆檢查表
        </h3>
        <div id="checklistContainer" class="space-y-2.5 text-sm">
          <!-- JS 動態渲染 -->
        </div>
      </div>
    </section>

  </main>

  <!-- 底部導覽列 -->
  <nav class="fixed bottom-0 left-1/2 -translate-x-1/2 w-full max-w-md bg-white border-t border-gray-100 px-6 py-2.5 flex justify-around items-center z-30">
    <button onclick="switchTab('home')" id="nav-home" class="flex flex-col items-center gap-1 text-coral font-bold">
      <i data-lucide="home" class="w-5 h-5"></i>
      <span class="text-[11px]">總覽</span>
    </button>
    <button onclick="switchTab('scanner')" id="nav-scanner" class="flex flex-col items-center gap-1 text-gray-400 font-medium">
      <i data-lucide="scan-line" class="w-5 h-5"></i>
      <span class="text-[11px]">AI 辨識範例</span>
    </button>
    <button onclick="switchTab('checklist')" id="nav-checklist" class="flex flex-col items-center gap-1 text-gray-400 font-medium">
      <i data-lucide="calculator" class="w-5 h-5"></i>
      <span class="text-[11px]">分帳＆防呆</span>
    </button>
  </nav>

  <!-- 新增收據彈窗 -->
  <div id="addModal" class="hidden fixed inset-0 bg-black/50 z-40 flex items-end justify-center">
    <div class="bg-white w-full max-w-md rounded-t-3xl p-6 space-y-4 animate-slide-up">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-bold text-navy">新增退稅消費紀錄</h3>
        <button onclick="closeAddModal()" class="text-gray-400 p-1"><i data-lucide="x" class="w-5 h-5"></i></button>
      </div>

      <div class="space-y-3 text-sm">
        <div>
          <label class="text-xs font-semibold text-gray-600">店家名稱</label>
          <input type="text" id="inputStore" placeholder="例如：松本清 心齋橋店 / Olive Young" class="w-full mt-1 border border-gray-200 rounded-xl px-3.5 py-2.5 focus:outline-none focus:border-coral" />
        </div>

        <div class="grid grid-cols-2 gap-3">
          <div>
            <label class="text-xs font-semibold text-gray-600">國家／幣別</label>
            <select id="inputCurrency" class="w-full mt-1 border border-gray-200 rounded-xl px-3 py-2.5 bg-white">
              <option value="JPY">🇯🇵 日本 (JPY)</option>
              <option value="KRW">🇰🇷 韓國 (KRW)</option>
            </select>
          </div>
          <div>
            <label class="text-xs font-semibold text-gray-600">含稅消費金額</label>
            <input type="number" id="inputAmount" placeholder="例如：12000" class="w-full mt-1 border border-gray-200 rounded-xl px-3.5 py-2.5 focus:outline-none focus:border-coral" />
          </div>
        </div>

        <div>
          <label class="text-xs font-semibold text-gray-600">商品類型（防呆分類）</label>
          <select id="inputType" class="w-full mt-1 border border-gray-200 rounded-xl px-3 py-2.5 bg-white">
            <option value="consumable">🔴 消耗品（藥妝/零食 - 離境前不可拆封）</option>
            <option value="general">🔵 一般物品（服飾/電器 - 可拆封使用）</option>
            <option value="mixed">🟠 混合結帳（整袋不可拆封）</option>
          </select>
        </div>
      </div>

      <button onclick="saveReceipt()" class="w-full bg-coral text-white font-bold py-3.5 rounded-2xl shadow-md mt-2">
        儲存並自動試算退稅
      </button>
    </div>
  </div>

  <script>
    const RATES = { JPY: 0.215, KRW: 0.0235 };

    const DEFAULT_RECEIPTS = [
      { id: 1, store: '松本清 東京新宿店', currency: 'JPY', amount: 15800, type: 'consumable', date: '今日 14:20' },
      { id: 2, store: 'Olive Young 明洞旗艦店', currency: 'KRW', amount: 85000, type: 'consumable', date: '昨日 19:45' },
      { id: 3, store: 'BIC CAMERA 難波店', currency: 'JPY', amount: 32000, type: 'general', date: '2 天前' }
    ];

    const DEFAULT_CHECKS = [
      { id: 'c1', text: '所有紅色標籤「消耗品」密封袋保持完整未拆封', done: false },
      { id: 'c2', text: '高單價精品／手錶已放入隨身手提行李（勿託運）', done: false },
      { id: 'c3', text: '確認護照與退稅收據／QR Code 放在隨身口袋', done: true },
      { id: 'c4', text: '日本離境於託運前先至海關 KIOSK 掃描護照', done: false }
    ];

    let receipts = JSON.parse(localStorage.getItem('taxmate_receipts')) || DEFAULT_RECEIPTS;
    let checks = JSON.parse(localStorage.getItem('taxmate_checks')) || DEFAULT_CHECKS;

    function calcTaxRefund(currency, amount) {
      if (currency === 'JPY') {
        // 日本含稅滿 5500 日圓可退約 9.09% (10/110)
        return amount >= 5500 ? Math.round(amount * (10 / 110)) : 0;
      } else {
        // 韓國滿 15000 韓元可退約 6.5% ~ 7%
        return amount >= 15000 ? Math.round(amount * 0.068) : 0;
      }
    }

    function renderAll() {
      const listEl = document.getElementById('receiptList');
      listEl.innerHTML = '';

      let totalSpentTWD = 0;
      let totalRefundTWD = 0;
      let totalRefundJPY = 0;
      let totalRefundKRW = 0;
      let consumableCount = 0;

      receipts.forEach(item => {
        const rate = RATES[item.currency];
        const refundForeign = calcTaxRefund(item.currency, item.amount);
        const isEligible = refundForeign > 0;

        totalSpentTWD += item.amount * rate;
        totalRefundTWD += refundForeign * rate;

        if (item.currency === 'JPY') totalRefundJPY += refundForeign;
        if (item.currency === 'KRW') totalRefundKRW += refundForeign;
        if (item.type === 'consumable' || item.type === 'mixed') consumableCount++;

        const badgeMap = {
          consumable: '<span class="text-[11px] font-semibold bg-red-50 text-red-600 px-2 py-0.5 rounded-md">不可拆封</span>',
          general: '<span class="text-[11px] font-semibold bg-blue-50 text-blue-600 px-2 py-0.5 rounded-md">一般品</span>',
          mixed: '<span class="text-[11px] font-semibold bg-orange-50 text-orange-600 px-2 py-0.5 rounded-md">混合不可拆</span>'
        };

        const symbol = item.currency === 'JPY' ? '¥' : '₩';

        listEl.innerHTML += `
          <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 flex items-center justify-between">
            <div class="space-y-1">
              <div class="flex items-center gap-2">
                <span class="font-bold text-navy text-sm">${item.store}</span>
                ${badgeMap[item.type]}
              </div>
              <p class="text-xs text-gray-400">${item.date} · ${isEligible ? `<span class="text-emerald-600 font-medium">預估退 ${symbol}${refundForeign.toLocaleString()}</span>` : '<span class="text-gray-400">未達退稅門檻</span>'}</p>
            </div>
            <div class="text-right flex items-center gap-3">
              <div>
                <p class="font-extrabold text-navy text-sm">${symbol}${item.amount.toLocaleString()}</p>
                <p class="text-[11px] text-gray-400">≈ NT$ ${Math.round(item.amount * rate).toLocaleString()}</p>
              </div>
              <button onclick="deleteReceipt(${item.id})" class="text-gray-300 hover:text-red-500 p-1">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
        `;
      });

      document.getElementById('totalRefundTWD').innerText = `NT$ ${Math.round(totalRefundTWD).toLocaleString()}`;
      document.getElementById('totalSpentTWD').innerText = `NT$ ${Math.round(totalSpentTWD).toLocaleString()}`;
      document.getElementById('foreignTotalText').innerText = `日幣退稅約 ¥${totalRefundJPY.toLocaleString()} ｜ 韓元退稅約 ₩${totalRefundKRW.toLocaleString()}`;
      document.getElementById('consumableCount').innerText = `${consumableCount} 筆`;

      renderChecklist();
      lucide.createIcons();
    }

    function renderChecklist() {
      const container = document.getElementById('checklistContainer');
      container.innerHTML = checks.map(c => `
        <label onclick="toggleCheck('${c.id}')" class="flex items-start gap-3 p-3 rounded-xl border ${c.done ? 'bg-gray-50 border-gray-100 text-gray-400 line-through' : 'bg-warmBg/60 border-orange-100 text-navy'} cursor-pointer transition">
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

    function saveReceipt() {
      const store = document.getElementById('inputStore').value.trim() || '未命名店家';
      const currency = document.getElementById('inputCurrency').value;
      const amount = parseFloat(document.getElementById('inputAmount').value);
      const type = document.getElementById('inputType').value;

      if (!amount || amount <= 0) {
        alert('請輸入正確的消費金額');
        return;
      }

      receipts.unshift({
        id: Date.now(),
        store,
        currency,
        amount,
        type,
        date: '剛剛新增'
      });

      localStorage.setItem('taxmate_receipts', JSON.stringify(receipts));
      document.getElementById('inputStore').value = '';
      document.getElementById('inputAmount').value = '';
      closeAddModal();
      renderAll();
    }

    function deleteReceipt(id) {
      receipts = receipts.filter(r => r.id !== id);
      localStorage.setItem('taxmate_receipts', JSON.stringify(receipts));
      renderAll();
    }

    function resetDemoData() {
      receipts = [...DEFAULT_RECEIPTS];
      localStorage.setItem('taxmate_receipts', JSON.stringify(receipts));
      renderAll();
    }

    function calcSplit() {
      const currency = document.getElementById('splitCurrency').value;
      const amount = parseFloat(document.getElementById('splitAmount').value) || 0;
      const people = Math.max(1, parseInt(document.getElementById('splitPeople').value) || 1);
      const mode = document.getElementById('splitTaxMode').value;

      const refund = mode === 'refund' ? calcTaxRefund(currency, amount) : 0;
      const netForeign = Math.max(0, amount - refund);
      const perPersonForeign = netForeign / people;
      const perPersonTWD = perPersonForeign * RATES[currency];
      const symbol = currency === 'JPY' ? '¥' : '₩';

      document.getElementById('splitResultTWD').innerText = `NT$ ${Math.round(perPersonTWD).toLocaleString()}`;
      document.getElementById('splitResultForeign').innerText = `每人約 ${symbol}${Math.round(perPersonForeign).toLocaleString()}`;
    }

    function switchTab(tab) {
      ['home', 'scanner', 'checklist'].forEach(t => {
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
        scanner: 'AI 收據視覺辨識',
        checklist: '旅伴分帳＆防呆'
      };
      document.getElementById('headerTitle').innerText = titles[tab];
    }

    function openAddModal() { document.getElementById('addModal').classList.remove('hidden'); }
    function closeAddModal() { document.getElementById('addModal').classList.add('hidden'); }

    // 初始化
    renderAll();
    calcSplit();
  </script>
</body>
</html># taxmate-mvp
