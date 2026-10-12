<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dora Lin (宥均) | 行程與班表分享站</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Noto+Sans+TC:wght@400;500;700&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', 'Noto Sans TC', sans-serif;
            background-color: #FAFAFC;
            color: #334155;
        }

        /* 馬卡龍卡片配色方案 */
        .badge-work { background-color: #FFE5E8; color: #D93856; border: 1px solid #FECDD3; }
        .badge-class { background-color: #E8F1FF; color: #2563EB; border: 1px solid #BFDBFE; }
        .badge-gathering { background-color: #FEF9C3; color: #CA8A04; border: 1px solid #FEF08A; }
        .badge-off { background-color: #DCFCE7; color: #166534; border: 1px solid #BBF7D0; }
        .badge-other { background-color: #F3E8FF; color: #7C3AED; border: 1px solid #E9D5FF; }

        .btn-macaron-primary {
            background-color: #2563EB;
            color: white;
            transition: all 0.2s ease;
        }
        .btn-macaron-primary:hover {
            background-color: #1D4ED8;
            transform: translateY(-1px);
        }

        /* 九宮格鍵盤按下效果 */
        .keypad-btn:active {
            transform: scale(0.95);
            background-color: #E2E8F0;
        }

        /* 隱藏滾動條但可滾動 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="min-h-screen flex flex-col pb-12">

    <!-- Header / Navbar -->
    <header class="bg-white/80 backdrop-blur-md sticky top-0 z-30 border-b border-slate-100 shadow-sm">
        <div class="max-w-6xl mx-auto px-4 sm:px-6 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-pink-400 via-rose-300 to-indigo-300 flex items-center justify-center text-white font-bold shadow-md shadow-rose-100">
                    D
                </div>
                <div>
                    <h1 class="text-base font-bold text-slate-800 leading-tight">Dora Lin (宥均)</h1>
                    <p class="text-xs text-slate-400">班表與行程分享</p>
                </div>
            </div>

            <!-- 身分狀態與切換按鈕 -->
            <div class="flex items-center gap-2">
                <div id="roleBadge" class="hidden sm:flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-slate-100 text-slate-600">
                    <i data-lucide="user" class="w-3.5 h-3.5"></i>
                    <span id="roleText">訪客檢視模式</span>
                </div>
                <button id="switchRoleBtn" onclick="toggleRoleModal()" class="flex items-center gap-1.5 px-3.5 py-1.5 rounded-xl text-xs font-semibold bg-slate-900 text-white hover:bg-slate-800 transition-all shadow-sm">
                    <i data-lucide="lock" class="w-3.5 h-3.5"></i>
                    <span id="switchBtnText">切換管理者</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-6xl mx-auto px-4 sm:px-6 pt-6 flex-1 w-full space-y-6">

        <!-- Banner / 公告卡片 -->
        <div class="bg-white rounded-3xl p-5 sm:p-6 border border-slate-100 shadow-sm flex flex-col md:flex-row md:items-center justify-between gap-4">
            <div class="space-y-1">
                <div class="flex items-center gap-2">
                    <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-rose-50 text-rose-500 border border-rose-100">Status</span>
                    <h2 class="text-lg font-bold text-slate-800">歡迎找我出去玩！</h2>
                </div>
                <p id="statusNotice" class="text-sm text-slate-500">歡迎查看我的時間表～有空檔的時間隨時提議約我喔！✨</p>
            </div>
            
            <div class="flex items-center gap-2 shrink-0">
                <button onclick="openInviteModal()" class="w-full sm:w-auto px-4 py-2.5 rounded-2xl bg-gradient-to-r from-rose-500 to-indigo-500 text-white text-xs font-bold shadow-md shadow-rose-100 hover:opacity-95 transition-all flex items-center justify-center gap-1.5">
                    <i data-lucide="sparkles" class="w-4 h-4"></i>
                    <span>約 Dora 出去</span>
                </button>
                <button id="adminAddBtn" onclick="openAddModal()" class="hidden w-full sm:w-auto px-4 py-2.5 rounded-2xl bg-slate-900 text-white text-xs font-bold shadow-md hover:bg-slate-800 transition-all flex items-center justify-center gap-1.5">
                    <i data-lucide="plus" class="w-4 h-4"></i>
                    <span>新增行程</span>
                </button>
            </div>
        </div>

        <!-- 管理者專屬：約會提議收件匣 -->
        <div id="adminInboxSection" class="hidden bg-white rounded-3xl p-5 border border-indigo-100 shadow-sm bg-gradient-to-br from-indigo-50/30 to-white">
            <div class="flex items-center justify-between mb-3">
                <div class="flex items-center gap-2">
                    <div class="w-2 h-2 rounded-full bg-indigo-500 animate-pulse"></div>
                    <h3 class="text-sm font-bold text-slate-800">親友邀約收件匣 (<span id="inboxCount">0</span>)</h3>
                </div>
                <span class="text-xs text-slate-400">僅管理者可見</span>
            </div>
            <div id="inboxList" class="space-y-2 max-h-48 overflow-y-auto pr-1">
                <!-- 動態注入邀約列表 -->
            </div>
        </div>

        <!-- 月曆工具列 -->
        <div class="bg-white rounded-3xl p-4 border border-slate-100 shadow-sm flex flex-col sm:flex-row sm:items-center justify-between gap-3">
            <div class="flex items-center justify-between sm:justify-start gap-4">
                <h2 id="currentMonthStr" class="text-lg font-bold text-slate-800">2026 年 10 月</h2>
                <div class="flex items-center gap-1 bg-slate-50 p-1 rounded-xl border border-slate-100">
                    <button onclick="changeMonth(-1)" class="p-1.5 hover:bg-white rounded-lg text-slate-600 transition-all"><i data-lucide="chevron-left" class="w-4 h-4"></i></button>
                    <button onclick="goToday()" class="px-2.5 py-1 text-xs font-semibold text-slate-600 hover:bg-white rounded-lg transition-all">今天</button>
                    <button onclick="changeMonth(1)" class="p-1.5 hover:bg-white rounded-lg text-slate-600 transition-all"><i data-lucide="chevron-right" class="w-4 h-4"></i></button>
                </div>
            </div>

            <!-- 色彩圖例 -->
            <div class="flex flex-wrap items-center gap-2 text-xs">
                <span class="badge-work px-2.5 py-1 rounded-lg font-medium">🔴 上班</span>
                <span class="badge-class px-2.5 py-1 rounded-lg font-medium">🔵 上課</span>
                <span class="badge-gathering px-2.5 py-1 rounded-lg font-medium">🟡 聚會</span>
                <span class="badge-off px-2.5 py-1 rounded-lg font-medium">🟢 休假</span>
                <span class="badge-other px-2.5 py-1 rounded-lg font-medium">🟣 其他</span>
            </div>
        </div>

        <!-- 月曆主要區域 -->
        <div class="bg-white rounded-3xl border border-slate-100 shadow-sm overflow-hidden p-2 sm:p-4">
            <!-- 星期標題 -->
            <div class="grid grid-cols-7 mb-2 text-center text-xs font-semibold text-slate-400">
                <div class="py-2 text-rose-400">日</div>
                <div class="py-2">一</div>
                <div class="py-2">二</div>
                <div class="py-2">三</div>
                <div class="py-2">四</div>
                <div class="py-2">五</div>
                <div class="py-2 text-indigo-400">六</div>
            </div>
            <!-- 日期網格 -->
            <div id="calendarGrid" class="grid grid-cols-7 gap-1 sm:gap-2">
                <!-- 動態生成日期卡片 -->
            </div>
        </div>
    </main>

    <!-- 1. 九宮格數字密碼 Modal -->
    <div id="passwordModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-xs rounded-3xl p-6 shadow-2xl border border-slate-100 text-center space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-slate-100 mx-auto flex items-center justify-center text-slate-700">
                <i data-lucide="shield-check" class="w-6 h-6"></i>
            </div>
            <div>
                <h3 class="font-bold text-slate-800">解鎖管理者權限</h3>
                <p class="text-xs text-slate-400 mt-1">請輸入 Dora 專屬密碼</p>
            </div>

            <!-- 密碼點點點顯示 -->
            <div class="flex justify-center gap-2 py-2">
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
                <div class="pwd-dot w-3.5 h-3.5 rounded-full bg-slate-200"></div>
            </div>

            <!-- 九宮格數字按鈕 -->
            <div class="grid grid-cols-3 gap-2 pt-2">
                <button onclick="pressKey('1')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">1</button>
                <button onclick="pressKey('2')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">2</button>
                <button onclick="pressKey('3')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">3</button>
                <button onclick="pressKey('4')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">4</button>
                <button onclick="pressKey('5')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">5</button>
                <button onclick="pressKey('6')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">6</button>
                <button onclick="pressKey('7')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">7</button>
                <button onclick="pressKey('8')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">8</button>
                <button onclick="pressKey('9')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">9</button>
                <button onclick="clearKeypad()" class="keypad-btn h-12 rounded-2xl bg-rose-50 text-rose-500 font-bold text-xs hover:bg-rose-100 transition-all">清除</button>
                <button onclick="pressKey('0')" class="keypad-btn h-12 rounded-2xl bg-slate-50 text-slate-700 font-bold text-lg hover:bg-slate-100 transition-all">0</button>
                <button onclick="backspaceKey()" class="keypad-btn h-12 rounded-2xl bg-slate-100 text-slate-600 font-bold flex items-center justify-center hover:bg-slate-200 transition-all">
                    <i data-lucide="delete" class="w-5 h-5"></i>
                </button>
            </div>

            <button onclick="closePasswordModal()" class="w-full py-2 text-xs font-semibold text-slate-400 hover:text-slate-600">取消</button>
        </div>
    </div>

    <!-- 2. 新增/編輯行程 Modal -->
    <div id="eventModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-3xl p-6 shadow-2xl border border-slate-100 space-y-4 max-h-[90vh] overflow-y-auto">
            <div class="flex items-center justify-between border-b pb-3">
                <h3 id="eventModalTitle" class="font-bold text-slate-800">新增行程 / 排班</h3>
                <button onclick="closeEventModal()" class="p-1 rounded-lg text-slate-400 hover:bg-slate-100"><i data-lucide="x" class="w-5 h-5"></i></button>
            </div>

            <form id="eventForm" onsubmit="handleSaveEvent(event)" class="space-y-4">
                <input type="hidden" id="eventId">

                <!-- 類別選擇 -->
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1.5">行程類別</label>
                    <div class="grid grid-cols-3 gap-1.5">
                        <label class="flex items-center justify-center p-2 rounded-xl border border-slate-100 text-xs font-medium cursor-pointer has-[:checked]:bg-rose-50 has-[:checked]:text-rose-600 has-[:checked]:border-rose-200">
                            <input type="radio" name="type" value="work" checked class="hidden" onchange="updateTypeStyle()"> 🔴 上班
                        </label>
                        <label class="flex items-center justify-center p-2 rounded-xl border border-slate-100 text-xs font-medium cursor-pointer has-[:checked]:bg-blue-50 has-[:checked]:text-blue-600 has-[:checked]:border-blue-200">
                            <input type="radio" name="type" value="class" class="hidden" onchange="updateTypeStyle()"> 🔵 上課
                        </label>
                        <label class="flex items-center justify-center p-2 rounded-xl border border-slate-100 text-xs font-medium cursor-pointer has-[:checked]:bg-amber-50 has-[:checked]:text-amber-600 has-[:checked]:border-amber-200">
                            <input type="radio" name="type" value="gathering" class="hidden" onchange="updateTypeStyle()"> 🟡 聚會
                        </label>
                        <label class="flex items-center justify-center p-2 rounded-xl border border-slate-100 text-xs font-medium cursor-pointer has-[:checked]:bg-emerald-50 has-[:checked]:text-emerald-600 has-[:checked]:border-emerald-200">
                            <input type="radio" name="type" value="off" class="hidden" onchange="updateTypeStyle()"> 🟢 休假
                        </label>
                        <label class="flex items-center justify-center p-2 rounded-xl border border-slate-100 text-xs font-medium cursor-pointer has-[:checked]:bg-purple-50 has-[:checked]:text-purple-600 has-[:checked]:border-purple-200 col-span-2">
                            <input type="radio" name="type" value="other" class="hidden" onchange="updateTypeStyle()"> 🟣 其他 / 私人
                        </label>
                    </div>
                </div>

                <!-- 名稱與地點 -->
                <div class="space-y-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 mb-1">行程名稱</label>
                        <input type="text" id="eventTitle" placeholder="例如：公司上班、演算法課、與小明吃飯" required class="w-full px-3 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                </div>

                <!-- 連續日期模式切換 -->
                <div class="flex items-center justify-between bg-slate-50 p-2.5 rounded-xl border border-slate-100">
                    <span class="text-xs font-semibold text-slate-600">連續日期（例如出國/多天連續行程）</span>
                    <input type="checkbox" id="isRange" onchange="toggleRangeInput()" class="w-4 h-4 rounded text-blue-600 focus:ring-blue-500">
                </div>

                <!-- 日期選擇 -->
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label id="startDateLabel" class="block text-xs font-semibold text-slate-500 mb-1">日期</label>
                        <input type="date" id="startDate" required class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500/20">
                    </div>
                    <div id="endDateContainer" class="hidden">
                        <label class="block text-xs font-semibold text-slate-500 mb-1">結束日期</label>
                        <input type="date" id="endDate" class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500/20">
                    </div>
                </div>

                <!-- 常用時間範本 + 時間輸入 -->
                <div class="space-y-2">
                    <div class="flex items-center justify-between">
                        <label class="block text-xs font-semibold text-slate-500">時間區間</label>
                        <button type="button" onclick="openTimePresetManageModal()" class="text-xs font-medium text-blue-600 hover:underline flex items-center gap-1">
                            <i data-lucide="settings" class="w-3 h-3"></i> 設定常用時間
                        </button>
                    </div>

                    <!-- 常用時間快捷點擊區 -->
                    <div id="timePresetsContainer" class="flex flex-wrap gap-1.5 py-1">
                        <!-- 動態載入常用時間標籤 -->
                    </div>

                    <div class="grid grid-cols-2 gap-2">
                        <input type="time" id="startTime" class="w-full px-3 py-1.5 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500/20">
                        <input type="time" id="endTime" class="w-full px-3 py-1.5 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500/20">
                    </div>
                </div>

                <!-- 按鈕區 -->
                <div class="flex items-center gap-2 pt-2">
                    <button type="button" id="deleteEventBtn" onclick="handleDeleteEvent()" class="hidden px-4 py-2.5 rounded-xl bg-rose-50 text-rose-600 text-xs font-bold hover:bg-rose-100 transition-all">刪除</button>
                    <button type="submit" class="flex-1 py-2.5 rounded-xl bg-slate-900 text-white text-xs font-bold hover:bg-slate-800 transition-all">儲存行程</button>
                </div>
            </form>
        </div>
    </div>

    <!-- 3. 自訂常用時間管理 Modal -->
    <div id="timePresetManageModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-sm rounded-3xl p-6 shadow-2xl border border-slate-100 space-y-4">
            <div class="flex items-center justify-between border-b pb-3">
                <h3 class="font-bold text-slate-800">管理常用時間範本</h3>
                <button onclick="closeTimePresetManageModal()" class="p-1 rounded-lg text-slate-400 hover:bg-slate-100"><i data-lucide="x" class="w-5 h-5"></i></button>
            </div>

            <div id="presetManageList" class="space-y-2 max-h-48 overflow-y-auto">
                <!-- 動態呈現常用時間列表 -->
            </div>

            <!-- 新增時間範本 -->
            <div class="pt-2 border-t space-y-2">
                <span class="text-xs font-semibold text-slate-500">新增時間範本</span>
                <div class="flex items-center gap-1.5">
                    <input type="text" id="newPresetLabel" placeholder="名稱 (如早班)" class="w-1/3 px-2.5 py-1.5 text-xs rounded-xl border border-slate-200">
                    <input type="time" id="newPresetStart" class="w-1/3 px-2 py-1.5 text-xs rounded-xl border border-slate-200">
                    <input type="time" id="newPresetEnd" class="w-1/3 px-2 py-1.5 text-xs rounded-xl border border-slate-200">
                </div>
                <button onclick="addNewPreset()" class="w-full py-2 rounded-xl bg-blue-600 text-white text-xs font-bold hover:bg-blue-700 transition-all">+ 新增範本</button>
            </div>
        </div>
    </div>

    <!-- 4. 親友「約 Dora 出去」Modal -->
    <div id="inviteModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-3xl p-6 shadow-2xl border border-slate-100 space-y-4">
            <div class="flex items-center justify-between border-b pb-3">
                <div class="flex items-center gap-2">
                    <div class="w-8 h-8 rounded-xl bg-rose-100 text-rose-500 flex items-center justify-center"><i data-lucide="heart" class="w-4 h-4"></i></div>
                    <h3 class="font-bold text-slate-800">約 Dora 出去玩！</h3>
                </div>
                <button onclick="closeInviteModal()" class="p-1 rounded-lg text-slate-400 hover:bg-slate-100"><i data-lucide="x" class="w-5 h-5"></i></button>
            </div>

            <form onsubmit="handleSendInvite(event)" class="space-y-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">你是誰？（暱稱）</label>
                    <input type="text" id="inviteName" placeholder="例如：小明、阿姨" required class="w-full px-3 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500/20">
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 mb-1">想約的日期</label>
                        <input type="date" id="inviteDate" required class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500/20">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 mb-1">時間區間</label>
                        <input type="text" id="inviteTime" placeholder="例如：下午或18:00後" required class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500/20">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">想去哪裡 / 做什麼？</label>
                    <textarea id="inviteMessage" rows="2" placeholder="例如：看電影、吃火鍋、逛街" required class="w-full px-3 py-2 text-sm rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500/20"></textarea>
                </div>

                <div class="pt-2">
                    <button type="submit" class="w-full py-2.5 rounded-xl bg-gradient-to-r from-rose-500 to-indigo-500 text-white text-xs font-bold shadow-md hover:opacity-95 transition-all">送出約會提議 💌</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JavaScript 邏輯處理 -->
    <script>
        // 常數與全域變數
        const MASTER_PASSWORD = "094811";
        let isAdmin = false;
        let enteredPin = "";
        let currentDate = new Date();

        // 預設資料
        let events = JSON.parse(localStorage.getItem('dora_events')) || [
            { id: '1', title: '公司上班', type: 'work', startDate: '2026-10-14', endDate: '2026-10-14', startTime: '08:00', endTime: '17:00' },
            { id: '2', title: '演算法課', type: 'class', startDate: '2026-10-14', endDate: '2026-10-14', startTime: '18:30', endTime: '21:30' },
            { id: '3', title: '朋友聚餐', type: 'gathering', startDate: '2026-10-16', endDate: '2026-10-16', startTime: '19:00', endTime: '21:30' },
            { id: '4', title: '日本東京之旅 ✈️', type: 'off', startDate: '2026-10-22', endDate: '2026-10-26', startTime: '', endTime: '' }
        ];

        let invites = JSON.parse(localStorage.getItem('dora_invites')) || [
            { id: 'inv1', name: '小明', date: '2026-10-17', time: '18:00 後', message: '要不要一起去吃火鍋？' }
        ];

        let timePresets = JSON.parse(localStorage.getItem('dora_time_presets')) || [
            { label: '早班', start: '08:00', end: '17:00' },
            { label: '晚班', start: '13:00', end: '22:00' },
            { label: '晚課', start: '18:30', end: '21:30' },
            { label: '全天', start: '', end: '' }
        ];

        // 初始化頁面
        document.addEventListener('DOMContentLoaded', () => {
            renderCalendar();
            renderTimePresets();
            lucide.createIcons();
        });

        // 儲存資料到 LocalStorage
        function saveData() {
            localStorage.setItem('dora_events', JSON.stringify(events));
            localStorage.setItem('dora_invites', JSON.stringify(invites));
            localStorage.setItem('dora_time_presets', JSON.stringify(timePresets));
        }

        /* ---------------- 管理者權限 & 密碼鎖 ---------------- */
        function toggleRoleModal() {
            if (isAdmin) {
                // 登出管理者
                isAdmin = false;
                updateRoleUI();
            } else {
                // 跳出密碼鎖 Modal
                clearKeypad();
                document.getElementById('passwordModal').classList.remove('hidden');
            }
        }

        function closePasswordModal() {
            document.getElementById('passwordModal').classList.add('hidden');
        }

        function pressKey(num) {
            if (enteredPin.length < 6) {
                enteredPin += num;
                updatePinDots();
                if (enteredPin.length === 6) {
                    setTimeout(verifyPassword, 150);
                }
            }
        }

        function backspaceKey() {
            enteredPin = enteredPin.slice(0, -1);
            updatePinDots();
        }

        function clearKeypad() {
            enteredPin = "";
            updatePinDots();
        }

        function updatePinDots() {
            const dots = document.querySelectorAll('.pwd-dot');
            dots.forEach((dot, idx) => {
                if (idx < enteredPin.length) {
                    dot.classList.remove('bg-slate-200');
                    dot.classList.add('bg-slate-800');
                } else {
                    dot.classList.remove('bg-slate-800');
                    dot.classList.add('bg-slate-200');
                }
            });
        }

        function verifyPassword() {
            if (enteredPin === MASTER_PASSWORD) {
                isAdmin = true;
                closePasswordModal();
                updateRoleUI();
                alert('歡迎回來，Dora！解鎖成功。');
            } else {
                alert('密碼錯誤，請再試一次！');
                clearKeypad();
            }
        }

        function updateRoleUI() {
            const roleBadge = document.getElementById('roleBadge');
            const roleText = document.getElementById('roleText');
            const switchBtnText = document.getElementById('switchBtnText');
            const adminAddBtn = document.getElementById('adminAddBtn');
            const adminInboxSection = document.getElementById('adminInboxSection');

            if (isAdmin) {
                roleText.innerText = "Dora (管理者)";
                roleBadge.className = "hidden sm:flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-semibold bg-rose-50 text-rose-600 border border-rose-100";
                switchBtnText.innerText = "登出管理者";
                adminAddBtn.classList.remove('hidden');
                adminInboxSection.classList.remove('hidden');
                renderInbox();
            } else {
                roleText.innerText = "訪客檢視模式";
                roleBadge.className = "hidden sm:flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-slate-100 text-slate-600";
                switchBtnText.innerText = "切換管理者";
                adminAddBtn.classList.add('hidden');
                adminInboxSection.classList.add('hidden');
            }
            renderCalendar();
            lucide.createIcons();
        }

        /* ---------------- 月曆渲染邏輯 ---------------- */
        function renderCalendar() {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();

            document.getElementById('currentMonthStr').innerText = `${year} 年 ${month + 1} 月`;

            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();

            const grid = document.getElementById('calendarGrid');
            grid.innerHTML = '';

            // 填充上個月空位
            for (let i = 0; i < firstDay; i++) {
                const emptyCell = document.createElement('div');
                emptyCell.className = 'min-h-[90px] sm:min-h-[110px] p-1 bg-slate-50/50 rounded-2xl border border-dashed border-slate-100 opacity-40';
                grid.appendChild(emptyCell);
            }

            const todayStr = new Date().toISOString().split('T')[0];

            // 填充本月日期
            for (let day = 1; day <= daysInMonth; day++) {
                const dateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                const cell = document.createElement('div');
                const isToday = dateStr === todayStr;

                cell.className = `min-h-[90px] sm:min-h-[110px] p-1.5 sm:p-2 rounded-2xl border transition-all flex flex-col justify-between ${
                    isToday ? 'bg-rose-50/30 border-rose-200' : 'bg-white border-slate-100 hover:border-slate-200'
                }`;

                // 日期頂部數字與點擊新增
                let topHtml = `<div class="flex items-center justify-between">
                    <span class="text-xs font-bold ${isToday ? 'w-5 h-5 rounded-full bg-rose-500 text-white flex items-center justify-center' : 'text-slate-700'}">${day}</span>
                    ${isAdmin ? `<button onclick="openAddModal('${dateStr}')" class="text-slate-300 hover:text-slate-600"><i data-lucide="plus-circle" class="w-3.5 h-3.5"></i></button>` : ''}
                </div>`;

                // 搜尋當天行程
                const dayEvents = events.filter(e => {
                    if (e.startDate <= dateStr && e.endDate >= dateStr) return true;
                    return false;
                });

                let eventsHtml = `<div class="space-y-1 my-1 flex-1 overflow-y-auto no-scrollbar">`;
                dayEvents.forEach(ev => {
                    const badgeClass = getBadgeClass(ev.type);
                    const timeText = ev.startTime ? `${ev.startTime}` : '';
                    eventsHtml += `
                        <div onclick="${isAdmin ? `openEditModal('${ev.id}')` : ''}" class="${badgeClass} p-1 rounded-lg text-[10px] font-semibold truncate ${isAdmin ? 'cursor-pointer hover:opacity-80' : ''}">
                            ${timeText ? `<span class="opacity-75">${timeText}</span> ` : ''}${ev.title}
                        </div>
                    `;
                });
                eventsHtml += `</div>`;

                cell.innerHTML = topHtml + eventsHtml;
                grid.appendChild(cell);
            }
            lucide.createIcons();
        }

        function getBadgeClass(type) {
            switch (type) {
                case 'work': return 'badge-work';
                case 'class': return 'badge-class';
                case 'gathering': return 'badge-gathering';
                case 'off': return 'badge-off';
                default: return 'badge-other';
            }
        }

        function changeMonth(delta) {
            currentDate.setMonth(currentDate.getMonth() + delta);
            renderCalendar();
        }

        function goToday() {
            currentDate = new Date();
            renderCalendar();
        }

        /* ---------------- 新增 / 編輯行程 Modal ---------------- */
        function openAddModal(dateStr = '') {
            if (!isAdmin) return;
            document.getElementById('eventModalTitle').innerText = "新增行程 / 排班";
            document.getElementById('eventForm').reset();
            document.getElementById('eventId').value = '';
            document.getElementById('deleteEventBtn').classList.add('hidden');

            const targetDate = dateStr || new Date().toISOString().split('T')[0];
            document.getElementById('startDate').value = targetDate;
            document.getElementById('endDate').value = targetDate;
            
            document.getElementById('isRange').checked = false;
            toggleRangeInput();

            document.getElementById('eventModal').classList.remove('hidden');
        }

        function openEditModal(eventId) {
            if (!isAdmin) return;
            const ev = events.find(e => e.id === eventId);
            if (!ev) return;

            document.getElementById('eventModalTitle').innerText = "編輯行程";
            document.getElementById('eventId').value = ev.id;
            document.getElementById('eventTitle').value = ev.title;
            document.getElementById('startDate').value = ev.startDate;
            document.getElementById('endDate').value = ev.endDate || ev.startDate;
            document.getElementById('startTime').value = ev.startTime || '';
            document.getElementById('endTime').value = ev.endTime || '';

            // Radio button
            const radios = document.querySelectorAll('input[name="type"]');
            radios.forEach(r => { if (r.value === ev.type) r.checked = true; });

            const isRange = ev.startDate !== ev.endDate;
            document.getElementById('isRange').checked = isRange;
            toggleRangeInput();

            document.getElementById('deleteEventBtn').classList.remove('hidden');
            document.getElementById('eventModal').classList.remove('hidden');
        }

        function closeEventModal() {
            document.getElementById('eventModal').classList.add('hidden');
        }

        function toggleRangeInput() {
            const isRange = document.getElementById('isRange').checked;
            const endDateContainer = document.getElementById('endDateContainer');
            const startDateLabel = document.getElementById('startDateLabel');

            if (isRange) {
                endDateContainer.classList.remove('hidden');
                startDateLabel.innerText = "開始日期";
            } else {
                endDateContainer.classList.add('hidden');
                startDateLabel.innerText = "日期";
            }
        }

        function handleSaveEvent(e) {
            e.preventDefault();
            const id = document.getElementById('eventId').value || Date.now().toString();
            const title = document.getElementById('eventTitle').value;
            const type = document.querySelector('input[name="type"]:checked').value;
            const startDate = document.getElementById('startDate').value;
            const isRange = document.getElementById('isRange').checked;
            const endDate = isRange ? document.getElementById('endDate').value : startDate;
            const startTime = document.getElementById('startTime').value;
            const endTime = document.getElementById('endTime').value;

            const newEvent = { id, title, type, startDate, endDate, startTime, endTime };

            const existingIdx = events.findIndex(ev => ev.id === id);
            if (existingIdx >= 0) {
                events[existingIdx] = newEvent;
            } else {
                events.push(newEvent);
            }

            saveData();
            closeEventModal();
            renderCalendar();
        }

        function handleDeleteEvent() {
            const id = document.getElementById('eventId').value;
            if (!id) return;
            if (confirm('確定要刪除這筆行程嗎？')) {
                events = events.filter(e => e.id !== id);
                saveData();
                closeEventModal();
                renderCalendar();
            }
        }

        /* ---------------- 常用時間範本管理 ---------------- */
        function renderTimePresets() {
            const container = document.getElementById('timePresetsContainer');
            container.innerHTML = '';

            timePresets.forEach((pt) => {
                const btn = document.createElement('button');
                btn.type = 'button';
                btn.className = 'px-2.5 py-1 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold transition-all';
                btn.innerText = `${pt.label} (${pt.start || '全天'})`;
                btn.onclick = () => {
                    document.getElementById('startTime').value = pt.start;
                    document.getElementById('endTime').value = pt.end;
                };
                container.appendChild(btn);
            });
        }

        function openTimePresetManageModal() {
            renderPresetManageList();
            document.getElementById('timePresetManageModal').classList.remove('hidden');
        }

        function closeTimePresetManageModal() {
            document.getElementById('timePresetManageModal').classList.add('hidden');
        }

        function renderPresetManageList() {
            const list = document.getElementById('presetManageList');
            list.innerHTML = '';

            timePresets.forEach((pt, idx) => {
                const item = document.createElement('div');
                item.className = 'flex items-center justify-between p-2 rounded-xl bg-slate-50 text-xs border border-slate-100';
                item.innerHTML = `
                    <span class="font-bold text-slate-700">${pt.label} (${pt.start || '全天'} - ${pt.end || '全天'})</span>
                    <button onclick="deletePreset(${idx})" class="text-rose-500 hover:text-rose-700 font-bold">刪除</button>
                `;
                list.appendChild(item);
            });
        }

        function addNewPreset() {
            const label = document.getElementById('newPresetLabel').value;
            const start = document.getElementById('newPresetStart').value;
            const end = document.getElementById('newPresetEnd').value;

            if (!label) {
                alert('請輸入範本名稱！');
                return;
            }

            timePresets.push({ label, start, end });
            saveData();
            renderTimePresets();
            renderPresetManageList();

            document.getElementById('newPresetLabel').value = '';
            document.getElementById('newPresetStart').value = '';
            document.getElementById('newPresetEnd').value = '';
        }

        function deletePreset(idx) {
            timePresets.splice(idx, 1);
            saveData();
            renderTimePresets();
            renderPresetManageList();
        }

        /* ---------------- 約會提議 ---------------- */
        function openInviteModal() {
            document.getElementById('inviteModal').classList.remove('hidden');
        }

        function closeInviteModal() {
            document.getElementById('inviteModal').classList.add('hidden');
        }

        function handleSendInvite(e) {
            e.preventDefault();
            const name = document.getElementById('inviteName').value;
            const date = document.getElementById('inviteDate').value;
            const time = document.getElementById('inviteTime').value;
            const message = document.getElementById('inviteMessage').value;

            const newInvite = {
                id: Date.now().toString(),
                name, date, time, message
            };

            invites.push(newInvite);
            saveData();

            alert('約會提議已送出囉！提議內容僅 Dora 可以看到 ✨');
            closeInviteModal();
            e.target.reset();
        }

        function renderInbox() {
            const container = document.getElementById('inboxList');
            document.getElementById('inboxCount').innerText = invites.length;
            container.innerHTML = '';

            if (invites.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-400 py-2">目前沒有新的約會提議。</p>`;
                return;
            }

            invites.forEach(inv => {
                const card = document.createElement('div');
                card.className = 'bg-white p-3 rounded-2xl border border-indigo-100 flex items-center justify-between gap-3 text-xs';
                card.innerHTML = `
                    <div class="space-y-0.5">
                        <div class="font-bold text-slate-800">${inv.name} <span class="font-normal text-slate-400">約 (${inv.date} ${inv.time})</span></div>
                        <div class="text-slate-600">${inv.message}</div>
                    </div>
                    <div class="flex items-center gap-1.5 shrink-0">
                        <button onclick="acceptInvite('${inv.id}')" class="px-2.5 py-1 rounded-xl bg-emerald-50 text-emerald-600 font-bold hover:bg-emerald-100">接受排入</button>
                        <button onclick="deleteInvite('${inv.id}')" class="px-2.5 py-1 rounded-xl bg-slate-100 text-slate-500 hover:bg-slate-200">忽略</button>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function acceptInvite(id) {
            const inv = invites.find(i => i.id === id);
            if (!inv) return;

            // 自動將約會轉為黃色聚會行程
            events.push({
                id: Date.now().toString(),
                title: `聚會：${inv.name} (${inv.message})`,
                type: 'gathering',
                startDate: inv.date,
                endDate: inv.date,
                startTime: '',
                endTime: ''
            });

            invites = invites.filter(i => i.id !== id);
            saveData();
            renderInbox();
            renderCalendar();
            alert('已接受約會，並自動新增為月曆上的聚會行程！');
        }

        function deleteInvite(id) {
            invites = invites.filter(i => i.id !== id);
            saveData();
            renderInbox();
        }
    </script>
</body>
</html>
