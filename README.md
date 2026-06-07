<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOEIC 高效多益單字記憶神器</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700;900&family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', 'Noto Sans TC', sans-serif;
        }
        /* 翻牌特效 */
        .perspective-1000 {
            perspective: 1000px;
        }
        .preserve-3d {
            transform-style: preserve-3d;
        }
        .backface-hidden {
            backface-visibility: hidden;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col">

    <!-- 頂部導覽列 -->
    <header class="bg-indigo-600 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-white text-indigo-600 p-2 rounded-lg font-bold text-xl shadow-inner">
                    <i class="fa-solid fa-graduation-cap"></i> TOEIC
                </div>
                <div>
                    <h1 class="font-bold text-lg md:text-xl tracking-wide">多益高效單字特訓王</h1>
                    <p class="text-xs text-indigo-200">GitHub Pages 專用部署版</p>
                </div>
            </div>
            
            <div class="flex items-center space-x-2 mt-2 sm:mt-0">
                <!-- API 設定按鈕 -->
                <button onclick="toggleApiModal()" class="bg-indigo-700 hover:bg-indigo-800 px-3 py-1.5 rounded-lg text-sm transition-colors flex items-center space-x-1">
                    <i class="fa-solid fa-key"></i>
                    <span>Gemini AI 助手設定</span>
                </button>
            </div>
        </div>
    </header>

    <!-- 主要內容區 -->
    <main class="flex-grow max-w-6xl w-full mx-auto px-4 py-6">
        
        <!-- API 提示 Banner -->
        <div id="api-banner" class="mb-6 bg-amber-50 border border-amber-200 text-amber-800 p-4 rounded-xl flex flex-col md:flex-row justify-between items-start md:items-center shadow-sm">
            <div class="flex items-start space-x-3">
                <i class="fa-solid fa-circle-info text-xl text-amber-500 mt-0.5"></i>
                <div>
                    <h3 class="font-bold text-sm">💡 啟用「Gemini AI 聯想記憶助手」</h3>
                    <p class="text-xs text-amber-700 mt-0.5">設定您的 Gemini API 密鑰，即可在背單字時隨時生成諧音聯想、字首字根拆解與多益模擬情境例句！</p>
                </div>
            </div>
            <button onclick="toggleApiModal()" class="mt-3 md:mt-0 bg-amber-600 hover:bg-amber-700 text-white px-3 py-1.5 rounded-lg text-xs font-bold transition-all shadow-sm">
                立即設定 API
            </button>
        </div>

        <!-- 功能分類標籤 -->
        <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
            
            <!-- 左側控制面板：選擇等級與分類 -->
            <div class="lg:col-span-1 space-y-6">
                <!-- 1. 關卡與主題 -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
                    <h2 class="font-bold text-gray-800 text-base mb-4 flex items-center border-b pb-2">
                        <i class="fa-solid fa-sliders text-indigo-600 mr-2"></i> 篩選單字庫
                    </h2>
                    
                    <!-- 多益等級 -->
                    <div class="mb-4">
                        <label class="block text-xs font-bold text-slate-500 uppercase tracking-wider mb-2">多益等級目標</label>
                        <select id="level-filter" onchange="filterWords()" class="w-full border border-slate-200 rounded-xl p-2.5 bg-slate-50 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none transition-all">
                            <option value="all">全部等級 (550 ~ 950+)</option>
                            <option value="550">初級核心 (550分基礎)</option>
                            <option value="750">中級必備 (750分進階)</option>
                            <option value="950">高階達人 (950分神人級)</option>
                        </select>
                    </div>

                    <!-- 商業主題 -->
                    <div class="mb-4">
                        <label class="block text-xs font-bold text-slate-500 uppercase tracking-wider mb-2">商業商務主題</label>
                        <select id="category-filter" onchange="filterWords()" class="w-full border border-slate-200 rounded-xl p-2.5 bg-slate-50 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none transition-all">
                            <option value="all">所有主題</option>
                            <option value="辦公室">辦公室與通訊 (Office)</option>
                            <option value="商務會議">商務會議與談判 (Meeting)</option>
                            <option value="金融財務">金融財務與投資 (Finance)</option>
                            <option value="行銷採購">行銷與採購 (Marketing)</option>
                            <option value="旅遊餐飲">商務旅遊與餐飲 (Travel)</option>
                            <option value="人事研發">人事與研發 (HR & RD)</option>
                        </select>
                    </div>

                    <!-- 收藏篩選 -->
                    <div class="pt-2 border-t">
                        <button onclick="toggleFavoriteFilter()" id="btn-fav-filter" class="w-full py-2 px-3 border border-slate-200 hover:border-red-200 hover:bg-red-50 rounded-xl text-sm font-medium flex items-center justify-center space-x-2 text-slate-700 transition-all">
                            <i id="fav-filter-icon" class="fa-regular fa-star text-amber-500"></i>
                            <span id="fav-filter-text">只看我收藏的單字</span>
                        </button>
                    </div>
                </div>

                <!-- 2. 學習模式切換 -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
                    <h2 class="font-bold text-gray-800 text-base mb-4 flex items-center border-b pb-2">
                        <i class="fa-solid fa-laptop-code text-indigo-600 mr-2"></i> 選擇學習模式
                    </h2>
                    <div class="space-y-2">
                        <button onclick="switchMode('flashcard')" id="tab-flashcard" class="w-full text-left py-3 px-4 rounded-xl text-sm font-semibold flex items-center justify-between transition-all bg-indigo-50 text-indigo-700 border border-indigo-100">
                            <span class="flex items-center"><i class="fa-solid fa-clone mr-3 text-lg"></i> 單字卡記憶</span>
                            <i class="fa-solid fa-chevron-right text-xs"></i>
                        </button>
                        <button onclick="switchMode('quiz')" id="tab-quiz" class="w-full text-left py-3 px-4 rounded-xl text-sm font-semibold flex items-center justify-between transition-all hover:bg-slate-50 text-slate-600">
                            <span class="flex items-center"><i class="fa-solid fa-circle-question mr-3 text-lg"></i> 四選一挑戰</span>
                            <i class="fa-solid fa-chevron-right text-xs"></i>
                        </button>
                        <button onclick="switchMode('spelling')" id="tab-spelling" class="w-full text-left py-3 px-4 rounded-xl text-sm font-semibold flex items-center justify-between transition-all hover:bg-slate-50 text-slate-600">
                            <span class="flex items-center"><i class="fa-solid fa-keyboard mr-3 text-lg"></i> 拼字特訓營</span>
                            <i class="fa-solid fa-chevron-right text-xs"></i>
                        </button>
                    </div>
                </div>

                <!-- 進度條 -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
                    <h3 class="text-xs font-bold text-slate-500 uppercase tracking-wider mb-2">單字學習進度</h3>
                    <div class="flex justify-between text-xs text-slate-600 mb-1">
                        <span id="progress-text">已學習: 0 / 0</span>
                        <span id="progress-percent" class="font-bold text-indigo-600">0%</span>
                    </div>
                    <div class="w-full bg-slate-100 h-2.5 rounded-full overflow-hidden">
                        <div id="progress-bar" class="bg-indigo-600 h-full transition-all duration-300" style="width: 0%"></div>
                    </div>
                    <p class="text-[10px] text-slate-400 mt-2">＊翻閱過的單字將計入您的背誦進度中喔！</p>
                </div>
            </div>

            <!-- 右側主要互動視窗 -->
            <div class="lg:col-span-3 space-y-6">

                <!-- 模式 A：單字卡模式 -->
                <div id="panel-flashcard" class="space-y-6">
                    <!-- 進度面板 -->
                    <div class="flex justify-between items-center bg-white px-5 py-3 rounded-2xl shadow-sm border border-slate-100 text-sm">
                        <span class="text-slate-500">目前單字：<b id="word-index-disp" class="text-indigo-600">1</b> / <span id="word-total-disp">0</span></span>
                        <div class="flex items-center space-x-2">
                            <span id="card-level-tag" class="px-2 py-0.5 rounded text-xs font-semibold bg-green-100 text-green-700">Level</span>
                            <span id="card-category-tag" class="px-2 py-0.5 rounded text-xs font-semibold bg-blue-100 text-blue-700">主題</span>
                        </div>
                    </div>

                    <!-- 3D 翻轉卡片 -->
                    <div class="perspective-1000 w-full h-80 sm:h-96 cursor-pointer" onclick="flipCard()">
                        <div id="flashcard-inner" class="preserve-3d duration-500 relative w-full h-full rounded-3xl shadow-lg border border-slate-100">
                            
                            <!-- 卡片正面 (英文) -->
                            <div class="backface-hidden absolute w-full h-full bg-white rounded-3xl p-8 flex flex-col justify-between">
                                <div class="flex justify-between items-start">
                                    <button onclick="toggleFavorite(event)" class="text-2xl text-slate-300 hover:text-amber-400 transition-colors">
                                        <i id="card-fav-icon-front" class="fa-regular fa-star"></i>
                                    </button>
                                    <span class="text-xs text-slate-400 font-medium">💡 點擊卡片可翻看中文意思</span>
                                </div>

                                <div class="text-center my-auto">
                                    <h2 id="card-word" class="text-4xl sm:text-5xl font-bold text-indigo-900 tracking-wide">loading...</h2>
                                    <p id="card-phonetic" class="text-slate-400 mt-2 text-lg">/ ... /</p>
                                    <p id="card-part-of-speech" class="inline-block bg-slate-100 text-slate-600 px-3 py-1 rounded-full text-xs font-medium mt-3">n.</p>
                                </div>

                                <div class="flex justify-center items-center space-x-4">
                                    <!-- 發音按鈕 -->
                                    <button onclick="speakWord(event)" class="w-12 h-12 rounded-full bg-indigo-50 hover:bg-indigo-100 text-indigo-600 flex items-center justify-center transition-all shadow-sm" title="語音朗讀">
                                        <i class="fa-solid fa-volume-high text-lg"></i>
                                    </button>
                                </div>
                            </div>

                            <!-- 卡片背面 (中文) -->
                            <div class="backface-hidden rotate-y-180 absolute w-full h-full bg-indigo-900 text-white rounded-3xl p-8 flex flex-col justify-between">
                                <div class="flex justify-between items-start">
                                    <span class="text-xs text-indigo-300">中文釋義與例句</span>
                                    <button onclick="toggleFavorite(event)" class="text-2xl text-indigo-300 hover:text-amber-400 transition-colors">
                                        <i id="card-fav-icon-back" class="fa-regular fa-star"></i>
                                    </button>
                                </div>

                                <div class="text-center my-auto px-2">
                                    <p id="card-translation" class="text-3xl font-bold tracking-wide">載入中...</p>
                                    <div class="mt-6 max-w-md mx-auto text-left bg-indigo-800/40 p-4 rounded-xl border border-indigo-700/50">
                                        <p class="text-xs text-indigo-300 font-bold mb-1">💡 實用例句：</p>
                                        <p id="card-sentence-en" class="text-sm font-medium text-indigo-100">Loading sentence...</p>
                                        <p id="card-sentence-zh" class="text-xs text-indigo-300 mt-1">載入翻譯中...</p>
                                    </div>
                                </div>

                                <div class="text-center text-xs text-indigo-300">
                                    再次點擊卡片翻回英文
                                </div>
                            </div>

                        </div>
                    </div>

                    <!-- 卡片控制與 AI 助記鈕 -->
                    <div class="grid grid-cols-3 gap-4">
                        <button onclick="prevCard()" class="bg-white hover:bg-slate-100 text-slate-700 py-4 px-6 rounded-2xl font-bold border border-slate-200 transition-all flex flex-col items-center justify-center space-y-1 shadow-sm">
                            <i class="fa-solid fa-arrow-left text-lg"></i>
                            <span class="text-xs">上一個</span>
                        </button>

                        <button onclick="askGeminiAi()" class="bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-700 hover:to-indigo-700 text-white py-4 px-4 rounded-2xl font-bold transition-all flex flex-col items-center justify-center space-y-1 shadow-md transform hover:-translate-y-0.5">
                            <div class="relative">
                                <i class="fa-solid fa-wand-magic-sparkles text-lg animate-pulse"></i>
                            </div>
                            <span class="text-xs">AI 聯想背法</span>
                        </button>

                        <button onclick="nextCard()" class="bg-indigo-600 hover:bg-indigo-700 text-white py-4 px-6 rounded-2xl font-bold transition-all flex flex-col items-center justify-center space-y-1 shadow-md transform hover:-translate-y-0.5">
                            <i class="fa-solid fa-arrow-right text-lg"></i>
                            <span class="text-xs">下一個</span>
                        </button>
                    </div>

                    <!-- AI 回應展示面板 -->
                    <div id="ai-response-box" class="hidden bg-white rounded-3xl p-6 border border-purple-100 shadow-md space-y-4">
                        <div class="flex justify-between items-center border-b border-purple-50 pb-3">
                            <div class="flex items-center space-x-2">
                                <div class="bg-purple-100 text-purple-600 p-1.5 rounded-lg">
                                    <i class="fa-solid fa-brain"></i>
                                </div>
                                <h4 class="font-bold text-purple-900">Gemini AI 創意聯想記憶</h4>
                            </div>
                            <button onclick="closeAiBox()" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark"></i></button>
                        </div>
                        <div id="ai-loading" class="hidden py-8 flex flex-col items-center justify-center space-y-3">
                            <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-purple-600"></div>
                            <p class="text-xs text-purple-600 font-medium">Gemini 正在為您撰寫記憶聯想秘笈...</p>
                        </div>
                        <div id="ai-content" class="text-sm text-slate-700 leading-relaxed whitespace-pre-wrap font-light">
                            <!-- AI 回應內容 -->
                        </div>
                        <div class="bg-purple-50 rounded-xl p-3 text-[11px] text-purple-700 flex items-start space-x-1.5">
                            <i class="fa-solid fa-lightbulb mt-0.5"></i>
                            <span>這項功能是由 Gemini-2.5-flash AI 自動生成。提示：設定個人的 Gemini API Key 可獲得極速的回應。</span>
                        </div>
                    </div>
                </div>

                <!-- 模式 B：四選一測驗 -->
                <div id="panel-quiz" class="hidden bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-100 space-y-6">
                    <div class="flex justify-between items-center border-b pb-4">
                        <div>
                            <h3 class="font-bold text-lg text-slate-800">🎯 多益核心單字測驗</h3>
                            <p class="text-xs text-slate-400">系統將自動從符合當前篩選條件的單字庫出題</p>
                        </div>
                        <div class="text-right">
                            <div class="text-xs text-slate-500">積分</div>
                            <div class="text-xl font-bold text-green-600" id="quiz-score">0 / 0</div>
                        </div>
                    </div>

                    <!-- 題目區 -->
                    <div class="bg-slate-50 p-6 rounded-2xl border border-slate-100 text-center">
                        <span class="text-xs text-indigo-600 font-bold uppercase tracking-wider">請問此單字的意思是？</span>
                        <h2 id="quiz-question" class="text-3xl font-bold text-indigo-900 mt-2">loading...</h2>
                        <p id="quiz-part-of-speech" class="inline-block bg-white text-slate-500 px-2.5 py-0.5 rounded-full text-[10px] font-semibold mt-2 border">n.</p>
                    </div>

                    <!-- 選項區 -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4" id="quiz-options">
                        <!-- 選項會動態渲染 -->
                    </div>

                    <!-- 測驗下方回饋 -->
                    <div id="quiz-feedback" class="hidden p-4 rounded-xl text-center font-bold">
                        <!-- 對/錯 訊息 -->
                    </div>

                    <!-- 下一題按鈕 -->
                    <div class="flex justify-end">
                        <button onclick="nextQuiz()" id="btn-next-quiz" class="hidden bg-indigo-600 hover:bg-indigo-700 text-white px-6 py-3 rounded-xl font-bold text-sm shadow-md transition-all flex items-center space-x-2">
                            <span>進入下一題</span>
                            <i class="fa-solid fa-arrow-right"></i>
                        </button>
                    </div>
                </div>

                <!-- 模式 C：拼字特訓營 -->
                <div id="panel-spelling" class="hidden bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-100 space-y-6">
                    <div class="flex justify-between items-center border-b pb-4">
                        <div>
                            <h3 class="font-bold text-lg text-slate-800">⌨️ 拼字特訓營</h3>
                            <p class="text-xs text-slate-400">根據中文提示與例句，拼寫出正確的英文單字</p>
                        </div>
                        <div class="text-right">
                            <div class="text-xs text-slate-500">正確率</div>
                            <div class="text-xl font-bold text-indigo-600" id="spell-score">0 / 0</div>
                        </div>
                    </div>

                    <!-- 提示卡片 -->
                    <div class="bg-slate-50 p-6 rounded-2xl border border-slate-100 space-y-4">
                        <div class="text-center">
                            <span class="text-xs text-indigo-600 font-bold uppercase tracking-wider">中文意思</span>
                            <h2 id="spell-chinese" class="text-2xl font-bold text-indigo-900 mt-1">載入中...</h2>
                            <p id="spell-part-of-speech" class="inline-block bg-white text-slate-500 px-2.5 py-0.5 rounded-full text-[10px] font-semibold mt-2 border">n.</p>
                        </div>
                        <div class="border-t border-dashed pt-4 max-w-md mx-auto">
                            <span class="text-xs text-slate-400 font-bold">💡 例句填空：</span>
                            <p id="spell-sentence" class="text-sm font-medium text-slate-700 mt-1">Please enter your password to ______ access.</p>
                            <p id="spell-sentence-zh" class="text-xs text-slate-400 mt-1">（請輸入您的密碼以取得存取權。）</p>
                        </div>
                    </div>

                    <!-- 輸入欄與對答區 -->
                    <div class="max-w-md mx-auto space-y-3">
                        <div class="relative">
                            <input type="text" id="spell-input" onkeydown="handleSpellKeydown(event)" autocomplete="off" placeholder="在此輸入英文單字..." class="w-full text-center border-2 border-slate-200 rounded-2xl py-3 px-4 font-bold text-lg tracking-wider focus:border-indigo-500 focus:outline-none transition-all">
                            <button onclick="speakSpellWord()" class="absolute right-3 top-3.5 text-slate-400 hover:text-indigo-600 transition-colors">
                                <i class="fa-solid fa-volume-high text-lg"></i>
                            </button>
                        </div>

                        <!-- 顯示提示字首與字母數 -->
                        <div class="flex justify-between items-center px-1 text-xs text-slate-400">
                            <span id="spell-length-hint">長度：0 個字母</span>
                            <button onclick="revealLetterHint()" class="hover:text-indigo-600 font-medium transition-colors">
                                <i class="fa-regular fa-lightbulb"></i> 顯示字首提示
                            </button>
                        </div>

                        <div id="spell-hint-disp" class="hidden text-center text-indigo-600 font-bold tracking-wider text-sm bg-indigo-50 py-2 rounded-xl">
                            字首提示：A
                        </div>

                        <button onclick="checkSpelling()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white py-3 rounded-2xl font-bold shadow-md transition-all">
                            檢查答案 (或按 Enter)
                        </button>
                    </div>

                    <!-- 拼字回饋區 -->
                    <div id="spell-feedback" class="hidden p-4 rounded-xl text-center font-bold max-w-md mx-auto">
                        <!-- 對錯回饋 -->
                    </div>

                    <!-- 下一題按鈕 -->
                    <div class="flex justify-center">
                        <button onclick="nextSpelling()" id="btn-next-spell" class="hidden bg-indigo-600 hover:bg-indigo-700 text-white px-6 py-2.5 rounded-xl font-bold text-sm shadow-md transition-all flex items-center space-x-2">
                            <span>繼續下一題</span>
                            <i class="fa-solid fa-chevron-right"></i>
                        </button>
                    </div>
                </div>

            </div>
        </div>

    </main>

    <!-- 頁尾 -->
    <footer class="bg-slate-900 text-slate-400 py-8 border-t border-slate-800 mt-12 text-xs md:text-sm">
        <div class="max-w-6xl mx-auto px-4 text-center space-y-3">
            <p class="font-medium text-slate-300">🎓 TOEIC 高效多益單字記憶神器</p>
            <p>專為課堂專案、GitHub Pages 線上靜態網頁快速部署所設計。內建完整的多益核心字彙、3D閃卡與AI聯想功能。</p>
            <div class="flex justify-center space-x-4 text-xs text-slate-500">
                <span>單一 index.html 架構</span>
                <span>•</span>
                <span>支援 RWD 手機版</span>
                <span>•</span>
                <span>語音免 API 即時發音</span>
            </div>
            <p class="text-slate-600 text-[10px] mt-4">© 2026. All rights reserved.</p>
        </div>
    </footer>

    <!-- Gemini API Key 彈出視窗設定 -->
    <div id="api-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 shadow-2xl border border-slate-100 relative animate-in fade-in duration-200">
            <button onclick="toggleApiModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600 transition-colors">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
            
            <div class="text-center mb-6">
                <div class="bg-indigo-50 text-indigo-600 w-12 h-12 rounded-2xl flex items-center justify-center mx-auto mb-3">
                    <i class="fa-solid fa-wand-magic-sparkles text-xl"></i>
                </div>
                <h3 class="font-bold text-lg text-slate-800">啟用 Gemini AI 助記功能</h3>
                <p class="text-xs text-slate-500 mt-1">這將啟用諧音聯想、字根字首拆解及多益客製化例句！</p>
            </div>

            <div class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-500 uppercase tracking-wider mb-2">輸入您的 Gemini API Key</label>
                    <input type="password" id="api-key-input" placeholder="AIzaSy..." class="w-full border border-slate-200 rounded-xl p-3 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none transition-all">
                </div>

                <div class="bg-slate-50 rounded-xl p-3 border text-[11px] text-slate-500 space-y-1">
                    <p class="font-semibold text-slate-700">🔒 安全與隱私保護：</p>
                    <p>1. 您的 API Key 將會**安全的儲存在您的本地瀏覽器 (localStorage)** 中，絕對不會上傳到任何第三方的伺服器。</p>
                    <p>2. 如果沒有設定 Key，系統仍將為您提供完整的「字卡」、「四選一」及「拼字」三大基礎功能。</p>
                </div>

                <div class="grid grid-cols-2 gap-3 pt-2">
                    <button onclick="clearApiKey()" class="py-2.5 px-4 border border-slate-200 hover:bg-slate-50 text-slate-600 rounded-xl text-sm font-semibold transition-all">
                        清除 Key
                    </button>
                    <button onclick="saveApiKey()" class="py-2.5 px-4 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-sm font-semibold transition-all shadow-md">
                        儲存並啟用
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- 訊息提示通知 Modal (取代 Browser Alert) -->
    <div id="notice-modal" class="fixed inset-0 bg-slate-900/50 z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-xl text-center space-y-4">
            <div id="notice-icon-wrapper" class="w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl">
                <i id="notice-icon" class="fa-solid"></i>
            </div>
            <div>
                <h4 id="notice-title" class="font-bold text-slate-800 text-base">提示</h4>
                <p id="notice-message" class="text-xs text-slate-500 mt-1">訊息內容</p>
            </div>
            <button onclick="closeNoticeModal()" class="w-full py-2 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-sm font-semibold transition-colors">
                確認
            </button>
        </div>
    </div>


    <!-- ========================================================= -->
    <!-- JAVASCRIPT: 程式邏輯與多益單字庫庫 -->
    <!-- ========================================================= -->
    <script>
        // 1. 本地多益經典單字資料庫 (包含550、750、950三個分數等級)
        const toeicWordList = [
            // 550 分基礎等級
            { id: 1, word: "agenda", phonetic: "/əˈdʒen.də/", partOfSpeech: "n.", translation: "議程；待議事項", level: "550", category: "商務會議", sentenceEn: "The chairperson distributed the agenda for today's meeting.", sentenceZh: "主席分發了今天會議的議程。" },
            { id: 2, word: "notify", phonetic: "/ˈnoʊ.t̬ə.faɪ/", partOfSpeech: "v.", translation: "通知；告知", level: "550", category: "辦公室", sentenceEn: "Please notify the HR department if your address changes.", sentenceZh: "如果您的地址有變更，請通知人力資源部。" },
            { id: 3, word: "postpone", phonetic: "/poʊstˈpoʊn/", partOfSpeech: "v.", translation: "延期；延遲", level: "550", category: "商務會議", sentenceEn: "We had to postpone the product launch due to technical issues.", sentenceZh: "由於技術問題，我們不得不推遲產品發表。" },
            { id: 4, word: "budget", phonetic: "/ˈbʌdʒ.ɪt/", partOfSpeech: "n.", translation: "預算", level: "550", category: "金融財務", sentenceEn: "The marketing team prepared a detailed budget proposal.", sentenceZh: "行銷團隊準備了一份詳細的預算提案。" },
            { id: 5, word: "confirm", phonetic: "/kənˈfɝːm/", partOfSpeech: "v.", translation: "確認；證實", level: "550", category: "辦公室", sentenceEn: "Could you please confirm your reservation by email?", sentenceZh: "能否請您透過電子郵件確認您的預約？" },
            { id: 6, word: "passenger", phonetic: "/ˈpæs.ən.dʒɚ/", partOfSpeech: "n.", translation: "乘客；旅客", level: "550", category: "旅遊餐飲", sentenceEn: "Passengers are requested to remain seated during takeoff.", sentenceZh: "乘客在起飛期間請保持就座。" },
            { id: 7, word: "purchase", phonetic: "/ˈpɝː.tʃəs/", partOfSpeech: "v. / n.", translation: "購買；採購", level: "550", category: "行銷採購", sentenceEn: "You can purchase the tickets online or at the counter.", sentenceZh: "您可以上網或在櫃檯購買門票。" },
            { id: 8, word: "recruitment", phonetic: "/rɪˈkruːt.mənt/", partOfSpeech: "n.", translation: "招聘；新兵募集", level: "550", category: "人事研發", sentenceEn: "The agency is handling the recruitment for our new branch.", sentenceZh: "該機構正在處理我們新分公司的招聘事宜。" },

            // 750 分進階等級
            { id: 9, word: "negotiate", phonetic: "/nəˈɡoʊ.ʃi.eɪt/", partOfSpeech: "v.", translation: "談判；協商", level: "750", category: "商務會議", sentenceEn: "We managed to negotiate a lower price with the supplier.", sentenceZh: "我們成功與供應商談判，爭取到了更低的價格。" },
            { id: 10, word: "collaborate", phonetic: "/kəˈlæb.ə.reɪt/", partOfSpeech: "v.", translation: "合作；協作", level: "750", category: "辦公室", sentenceEn: "Engineers from both companies will collaborate on the design.", sentenceZh: "兩家公司的工程師將在設計上進行合作。" },
            { id: 11, word: "beverage", phonetic: "/ˈbev.ɚ.ɪdʒ/", partOfSpeech: "n.", translation: "飲料", level: "750", category: "旅遊餐飲", sentenceEn: "Complimentary beverages will be served during the flight.", sentenceZh: "飛行期間將免費提供飲料。" },
            { id: 12, word: "subsidiary", phonetic: "/səbˈsɪd.i.er.i/", partOfSpeech: "n.", translation: "子公司；分支機構", level: "750", category: "金融財務", sentenceEn: "The multinational group opened a new subsidiary in Tokyo.", sentenceZh: "該跨國集團在東京開設了一家新的子公司。" },
            { id: 13, word: "monopolize", phonetic: "/məˈnɑː.pəl.aɪz/", partOfSpeech: "v.", translation: "獨佔；壟斷", level: "750", category: "行銷採購", sentenceEn: "They are trying to monopolize the local telecommunications market.", sentenceZh: "他們正試圖壟斷當地的電信市場。" },
            { id: 14, word: "reimburse", phonetic: "/ˌriː.ɪmˈbɝːs/", partOfSpeech: "v.", translation: "報銷；補償", level: "750", category: "金融財務", sentenceEn: "The company will reimburse your travel expenses.", sentenceZh: "公司將會報銷您的差旅費用。" },
            { id: 15, word: "evaluation", phonetic: "/ɪˌvæl.juˈeɪ.ʃən/", partOfSpeech: "n.", translation: "評估；評價", level: "750", category: "人事研發", sentenceEn: "Employee performance evaluations are conducted annually.", sentenceZh: "員工績效評估每年進行一次。" },

            // 950 分神人等級
            { id: 16, word: "unprecedented", phonetic: "/ʌnˈpres.ə.den.t̬ɪd/", partOfSpeech: "adj.", translation: "史無前例的；空前的", level: "950", category: "行銷採購", sentenceEn: "The company experienced unprecedented growth during the first quarter.", sentenceZh: "該公司在第一季度經歷了空前的成長。" },
            { id: 17, word: "discrepancy", phonetic: "/dɪˈskrep.ən.si/", partOfSpeech: "n.", translation: "不一致；差異；出入", level: "950", category: "金融財務", sentenceEn: "There was a discrepancy between the two financial reports.", sentenceZh: "這兩份財務報告之間存在出入。" },
            { id: 18, word: "demolish", phonetic: "/dɪˈmɑː.lɪʃ/", partOfSpeech: "v.", translation: "拆除；推翻(理論)", level: "950", category: "人事研發", sentenceEn: "The old factory building was demolished to make way for a new park.", sentenceZh: "舊廠房被拆除以建造新公園。" },
            { id: 19, word: "procrastinate", phonetic: "/proʊˈkræs.tə.neɪt/", partOfSpeech: "v.", translation: "拖延；延宕", level: "950", category: "辦公室", sentenceEn: "If you procrastinate on this project, we will miss the deadline.", sentenceZh: "如果你對這個專案拖延，我們就會錯過截止日期。" },
            { id: 20, word: "conglomerate", phonetic: "/kəŋˈɡlɑː.mɚ.ət/", partOfSpeech: "n.", translation: "企業集團；複合企業群", level: "950", category: "金融財務", sentenceEn: "The business conglomerate has holdings in media, shipping, and energy.", sentenceZh: "該企業集團旗下擁有媒體、航運和能源等產業。" }
        ];

        // 2. 狀態管理器 (State)
        let filteredWords = [...toeicWordList]; // 當前經篩選過後的單字數
        let currentIndex = 0;                  // 目前單字卡索引
        let isFlipped = false;                 // 單字卡是否翻轉
        let favoriteIds = [];                  // 收藏單字ID
        let currentMode = 'flashcard';         // 當前模式：flashcard / quiz / spelling
        let viewedWordIds = new Set();         // 記錄已背過(瀏覽過)的單字
        let apiKey = "";                       // Gemini API key

        // 測驗相關狀態
        let quizScore = 0;
        let quizTotal = 0;
        let currentQuizWord = null;

        // 拼字相關狀態
        let spellScore = 0;
        let spellTotal = 0;
        let currentSpellWord = null;
        let isLetterHintUsed = false;

        // 初始化
        window.onload = function() {
            // 從 LocalStorage 載入 API Key 與收藏資料
            const storedKey = localStorage.getItem("gemini_toeic_api_key");
            if (storedKey) {
                apiKey = storedKey;
                document.getElementById("api-key-input").value = apiKey;
                document.getElementById("api-banner").classList.add("hidden");
            }

            const storedFavs = localStorage.getItem("gemini_toeic_fav_ids");
            if (storedFavs) {
                favoriteIds = JSON.parse(storedFavs);
            }

            const storedViewed = localStorage.getItem("gemini_toeic_viewed_ids");
            if (storedViewed) {
                viewedWordIds = new Set(JSON.parse(storedViewed));
            }

            // 更新篩選單字與畫面
            filterWords();
        };

        // 3. 多功能控制方法
        function filterWords() {
            const levelVal = document.getElementById("level-filter").value;
            const catVal = document.getElementById("category-filter").value;
            const isFavOnly = document.getElementById("btn-fav-filter").classList.contains("bg-red-50");

            filteredWords = toeicWordList.filter(item => {
                const levelMatch = (levelVal === "all") || (item.level === levelVal);
                const catMatch = (catVal === "all") || (item.category === catVal);
                const favMatch = (!isFavOnly) || (favoriteIds.includes(item.id));
                return levelMatch && catMatch && favMatch;
            });

            // 重設當前索引
            currentIndex = 0;
            isFlipped = false;
            const inner = document.getElementById("flashcard-inner");
            if (inner) inner.classList.remove("rotate-y-180");

            // 更新進度與UI
            updateProgress();
            renderModeContent();
        }

        function toggleFavoriteFilter() {
            const btn = document.getElementById("btn-fav-filter");
            const icon = document.getElementById("fav-filter-icon");
            const text = document.getElementById("fav-filter-text");

            if (btn.classList.contains("bg-red-50")) {
                // 取消只看收藏
                btn.className = "w-full py-2 px-3 border border-slate-200 hover:border-red-200 hover:bg-red-50 rounded-xl text-sm font-medium flex items-center justify-center space-x-2 text-slate-700 transition-all";
                icon.className = "fa-regular fa-star text-amber-500";
                text.textContent = "只看我收藏的單字";
            } else {
                // 設為只看收藏
                btn.className = "w-full py-2 px-3 border-2 border-red-400 bg-red-50 rounded-xl text-sm font-bold flex items-center justify-center space-x-2 text-red-700 transition-all shadow-sm";
                icon.className = "fa-solid fa-star text-amber-500";
                text.textContent = "正在檢視收藏單字";
            }
            filterWords();
        }

        // 切換學習模式
        function switchMode(mode) {
            currentMode = mode;
            
            // UI tabs 更新
            const tabs = {
                'flashcard': document.getElementById("tab-flashcard"),
                'quiz': document.getElementById("tab-quiz"),
                'spelling': document.getElementById("tab-spelling")
            };

            const panels = {
                'flashcard': document.getElementById("panel-flashcard"),
                'quiz': document.getElementById("panel-quiz"),
                'spelling': document.getElementById("panel-spelling")
            };

            // 復原所有 tab 為預設樣式
            for (let m in tabs) {
                tabs[m].className = "w-full text-left py-3 px-4 rounded-xl text-sm font-semibold flex items-center justify-between transition-all hover:bg-slate-50 text-slate-600";
                panels[m].classList.add("hidden");
            }

            // 啟用當前的
            tabs[mode].className = "w-full text-left py-3 px-4 rounded-xl text-sm font-semibold flex items-center justify-between transition-all bg-indigo-50 text-indigo-700 border border-indigo-100 shadow-sm";
            panels[mode].classList.remove("hidden");

            // 渲染當前模式的內容
            renderModeContent();
        }

        // 渲染對應模式內容
        function renderModeContent() {
            // 重置 AI 回應框
            closeAiBox();

            if (filteredWords.length === 0) {
                showEmptyState();
                return;
            }

            if (currentMode === 'flashcard') {
                showFlashcard(currentIndex);
            } else if (currentMode === 'quiz') {
                initQuiz();
            } else if (currentMode === 'spelling') {
                initSpelling();
            }
        }

        // 無單字時的空狀態顯示
        function showEmptyState() {
            const emptyHtml = `
                <div class="text-center py-16 bg-white rounded-3xl border border-slate-100 shadow-sm space-y-4">
                    <div class="bg-indigo-50 text-indigo-600 w-16 h-16 rounded-full flex items-center justify-center mx-auto text-2xl">
                        <i class="fa-solid fa-folder-open"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-slate-800 text-lg">沒有符合條件的單字</h3>
                        <p class="text-xs text-slate-400 mt-1">請嘗試調整您的篩選等級、主題，或新增一些收藏單字！</p>
                    </div>
                    <button onclick="resetFilters()" class="bg-indigo-600 hover:bg-indigo-700 text-white px-5 py-2 rounded-xl text-xs font-semibold shadow transition-all">
                        重設篩選條件
                    </button>
                </div>
            `;

            if (currentMode === 'flashcard') {
                document.getElementById("panel-flashcard").innerHTML = emptyHtml;
            } else if (currentMode === 'quiz') {
                document.getElementById("panel-quiz").innerHTML = emptyHtml;
            } else if (currentMode === 'spelling') {
                document.getElementById("panel-spelling").innerHTML = emptyHtml;
            }
        }

        function resetFilters() {
            // 如果面板內容被空狀態取代了，需要還原面板結構再進行 filter
            location.reload(); // 最簡單且完全無痛的重置方式
        }


        // =========================================================
        // 模式 A: 單字卡 (Flashcards) 邏輯
        // =========================================================
        function showFlashcard(index) {
            if (filteredWords.length === 0) return;
            
            // 確保 index 範圍正確
            if (index < 0) {
                currentIndex = filteredWords.length - 1;
            } else if (index >= filteredWords.length) {
                currentIndex = 0;
            } else {
                currentIndex = index;
            }

            const current = filteredWords[currentIndex];

            // 記錄至已瀏覽
            viewedWordIds.add(current.id);
            localStorage.setItem("gemini_toeic_viewed_ids", JSON.stringify([...viewedWordIds]));
            updateProgress();

            // 正面渲染
            document.getElementById("card-word").textContent = current.word;
            document.getElementById("card-phonetic").textContent = current.phonetic;
            document.getElementById("card-part-of-speech").textContent = current.partOfSpeech;

            // 背面渲染
            document.getElementById("card-translation").textContent = current.translation;
            document.getElementById("card-sentence-en").textContent = current.sentenceEn;
            document.getElementById("card-sentence-zh").textContent = current.sentenceZh;

            // 等級與主題標籤
            document.getElementById("card-level-tag").textContent = `${current.level} 分`;
            document.getElementById("card-category-tag").textContent = current.category;

            // 頁碼指示器
            document.getElementById("word-index-disp").textContent = currentIndex + 1;
            document.getElementById("word-total-disp").textContent = filteredWords.length;

            // 更新愛心星號狀態
            updateFavoriteIcons(current.id);

            // 翻牌復位
            isFlipped = false;
            document.getElementById("flashcard-inner").classList.remove("rotate-y-180");
        }

        function flipCard() {
            isFlipped = !isFlipped;
            const inner = document.getElementById("flashcard-inner");
            if (isFlipped) {
                inner.classList.add("rotate-y-180");
            } else {
                inner.classList.remove("rotate-y-180");
            }
        }

        function nextCard() {
            currentIndex++;
            showFlashcard(currentIndex);
        }

        function prevCard() {
            currentIndex--;
            showFlashcard(currentIndex);
        }

        // =========================================================
        // 模式 B: 四選一測驗 (Quiz) 邏輯
        // =========================================================
        function initQuiz() {
            if (filteredWords.length === 0) return;
            
            document.getElementById("quiz-feedback").className = "hidden p-4 rounded-xl text-center font-bold";
            document.getElementById("btn-next-quiz").classList.add("hidden");

            // 隨機抽選一題
            const randIdx = Math.floor(Math.random() * filteredWords.length);
            currentQuizWord = filteredWords[randIdx];

            document.getElementById("quiz-question").textContent = currentQuizWord.word;
            document.getElementById("quiz-part-of-speech").textContent = currentQuizWord.partOfSpeech;

            // 生成四個選項 (1正3反)
            let options = [currentQuizWord.translation];
            
            // 從全部多益庫中隨機挑選不同的翻譯來混淆
            const allTranslations = toeicWordList.map(w => w.translation);
            while (options.length < 4) {
                const randomTrans = allTranslations[Math.floor(Math.random() * allTranslations.length)];
                if (!options.includes(randomTrans)) {
                    options.push(randomTrans);
                }
            }

            // 打亂選項順序
            options.sort(() => Math.random() - 0.5);

            // 渲染選項
            const optionsContainer = document.getElementById("quiz-options");
            optionsContainer.innerHTML = "";
            
            options.forEach((opt, idx) => {
                const button = document.createElement("button");
                button.className = "w-full text-left py-4 px-6 border-2 border-slate-100 hover:border-indigo-100 hover:bg-slate-50 rounded-2xl font-medium transition-all text-sm flex items-center space-x-3";
                button.onclick = () => selectQuizOption(button, opt);
                button.innerHTML = `
                    <span class="w-7 h-7 rounded-full bg-slate-100 text-slate-500 font-bold flex items-center justify-center text-xs border">${String.fromCharCode(65 + idx)}</span>
                    <span class="text-slate-700 font-semibold">${opt}</span>
                `;
                optionsContainer.appendChild(button);
            });
        }

        function selectQuizOption(buttonElement, selectedText) {
            const optionsContainer = document.getElementById("quiz-options");
            const buttons = optionsContainer.getElementsByTagName("button");
            
            // 停用所有按鈕，避免二次點擊
            for (let btn of buttons) {
                btn.disabled = true;
            }

            const feedback = document.getElementById("quiz-feedback");
            feedback.classList.remove("hidden");
            
            quizTotal++;

            if (selectedText === currentQuizWord.translation) {
                // 答對了
                quizScore++;
                buttonElement.className = "w-full text-left py-4 px-6 border-2 border-emerald-500 bg-emerald-50 rounded-2xl font-medium transition-all text-sm flex items-center space-x-3";
                feedback.className = "p-4 rounded-xl text-center font-bold bg-emerald-50 text-emerald-800 border border-emerald-200 text-sm";
                feedback.innerHTML = `🎉 答對了！非常棒！朗讀發音：<button onclick="speakWordText('${currentQuizWord.word}')" class="ml-1 text-indigo-600 underline hover:text-indigo-800"><i class="fa-solid fa-volume-high"></i> ${currentQuizWord.word}</button>`;
            } else {
                // 答錯了
                buttonElement.className = "w-full text-left py-4 px-6 border-2 border-rose-500 bg-rose-50 rounded-2xl font-medium transition-all text-sm flex items-center space-x-3";
                feedback.className = "p-4 rounded-xl text-center font-bold bg-rose-50 text-rose-800 border border-rose-200 text-sm";
                feedback.innerHTML = `❌ 答錯囉！正確答案是：<span class="text-indigo-700">「${currentQuizWord.translation}」</span>`;
                
                // 標示出正確的那一項
                for (let btn of buttons) {
                    if (btn.innerText.includes(currentQuizWord.translation)) {
                        btn.className = "w-full text-left py-4 px-6 border-2 border-emerald-500 bg-emerald-50 rounded-2xl font-medium transition-all text-sm flex items-center space-x-3";
                    }
                }
            }

            // 更新積分板
            document.getElementById("quiz-score").textContent = `${quizScore} / ${quizTotal}`;
            
            // 秀出下一題按鈕
            document.getElementById("btn-next-quiz").classList.remove("hidden");
        }

        function nextQuiz() {
            initQuiz();
        }


        // =========================================================
        // 模式 C: 拼字特訓營 (Spelling) 邏輯
        // =========================================================
        function initSpelling() {
            if (filteredWords.length === 0) return;

            document.getElementById("spell-feedback").className = "hidden p-4 rounded-xl text-center font-bold max-w-md mx-auto";
            document.getElementById("btn-next-spell").classList.add("hidden");
            document.getElementById("spell-input").value = "";
            document.getElementById("spell-input").disabled = false;
            document.getElementById("spell-input").focus();
            document.getElementById("spell-hint-disp").classList.add("hidden");
            isLetterHintUsed = false;

            // 隨機選一個單字
            const randIdx = Math.floor(Math.random() * filteredWords.length);
            currentSpellWord = filteredWords[randIdx];

            document.getElementById("spell-chinese").textContent = currentSpellWord.translation;
            document.getElementById("spell-part-of-speech").textContent = currentSpellWord.partOfSpeech;
            
            // 隱藏英文，用底線代替
            const hiddenWordPattern = "_____";
            const replacedEn = currentSpellWord.sentenceEn.replace(new RegExp(currentSpellWord.word, 'gi'), hiddenWordPattern);
            
            document.getElementById("spell-sentence").textContent = replacedEn;
            document.getElementById("spell-sentence-zh").textContent = currentSpellWord.sentenceZh;

            document.getElementById("spell-length-hint").textContent = `長度：${currentSpellWord.word.length} 個字母`;
        }

        function revealLetterHint() {
            if (!currentSpellWord) return;
            const hintDisp = document.getElementById("spell-hint-disp");
            hintDisp.textContent = `字首提示：『 ${currentSpellWord.word[0].toUpperCase()} 』，總長 ${currentSpellWord.word.length} 個字`;
            hintDisp.classList.remove("hidden");
            isLetterHintUsed = true;
        }

        function checkSpelling() {
            const userAns = document.getElementById("spell-input").value.trim().toLowerCase();
            const correctAns = currentSpellWord.word.trim().toLowerCase();
            
            if (!userAns) {
                showNotice("info", "請先輸入您的答案！");
                return;
            }

            document.getElementById("spell-input").disabled = true;
            spellTotal++;

            const feedback = document.getElementById("spell-feedback");
            feedback.classList.remove("hidden");

            if (userAns === correctAns) {
                spellScore++;
                feedback.className = "p-4 rounded-xl text-center font-bold bg-emerald-50 text-emerald-800 border border-emerald-200 text-sm max-w-md mx-auto";
                feedback.innerHTML = `🎉 太厲害了！完全正確！`;
                speakWordText(currentSpellWord.word);
            } else {
                feedback.className = "p-4 rounded-xl text-center font-bold bg-rose-50 text-rose-800 border border-rose-200 text-sm max-w-md mx-auto";
                feedback.innerHTML = `❌ 拼錯了。正確應為：<span class="text-indigo-600 font-bold">${currentSpellWord.word}</span>`;
            }

            document.getElementById("spell-score").textContent = `${spellScore} / ${spellTotal}`;
            document.getElementById("btn-next-spell").classList.remove("hidden");
        }

        function handleSpellKeydown(e) {
            if (e.key === "Enter") {
                checkSpelling();
            }
        }

        function speakSpellWord() {
            if (currentSpellWord) {
                speakWordText(currentSpellWord.word);
            }
        }

        function nextSpelling() {
            initSpelling();
        }


        // =========================================================
        // 我的最愛 (收藏夾) 功能
        // =========================================================
        function toggleFavorite(event) {
            event.stopPropagation(); // 防止翻轉卡片觸發
            if (filteredWords.length === 0) return;

            const current = filteredWords[currentIndex];
            const idx = favoriteIds.indexOf(current.id);

            if (idx > -1) {
                // 取消收藏
                favoriteIds.splice(idx, 1);
            } else {
                // 加入收藏
                favoriteIds.push(current.id);
            }

            localStorage.setItem("gemini_toeic_fav_ids", JSON.stringify(favoriteIds));
            updateFavoriteIcons(current.id);
        }

        function updateFavoriteIcons(wordId) {
            const isFav = favoriteIds.includes(wordId);
            const frontIcon = document.getElementById("card-fav-icon-front");
            const backIcon = document.getElementById("card-fav-icon-back");

            if (frontIcon && backIcon) {
                if (isFav) {
                    frontIcon.className = "fa-solid fa-star text-amber-400";
                    backIcon.className = "fa-solid fa-star text-amber-400";
                } else {
                    frontIcon.className = "fa-regular fa-star text-slate-300 hover:text-amber-400";
                    backIcon.className = "fa-regular fa-star text-indigo-300 hover:text-amber-400";
                }
            }
        }


        // =========================================================
        // 瀏覽器 Text-to-Speech 即時語音朗讀
        // =========================================================
        function speakWord(event) {
            if (event) event.stopPropagation(); // 防止點發音時翻轉卡片
            if (filteredWords.length === 0) return;
            const word = filteredWords[currentIndex].word;
            speakWordText(word);
        }

        function speakWordText(text) {
            if ('speechSynthesis' in window) {
                // 先取消其他發音
                window.speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'en-US';
                utterance.rate = 0.85; // 稍慢一點，方便聽懂發音
                window.speechSynthesis.speak(utterance);
            } else {
                showNotice("error", "您的瀏覽器不支援 Web Speech 朗讀 API 😭");
            }
        }


        // =========================================================
        // 系統進度更新
        // =========================================================
        function updateProgress() {
            const total = toeicWordList.length;
            const viewedCount = viewedWordIds.size;
            const percent = Math.round((viewedCount / total) * 100);

            const progText = document.getElementById("progress-text");
            const progPercent = document.getElementById("progress-percent");
            const progBar = document.getElementById("progress-bar");

            if (progText) progText.textContent = `已學習: ${viewedCount} / ${total}`;
            if (progPercent) progPercent.textContent = `${percent}%`;
            if (progBar) progBar.style.width = `${percent}%`;
        }


        // =========================================================
        // Gemini API 密鑰管理視窗
        // =========================================================
        function toggleApiModal() {
            const modal = document.getElementById("api-modal");
            modal.classList.toggle("hidden");
        }

        function saveApiKey() {
            const keyVal = document.getElementById("api-key-input").value.trim();
            if (!keyVal) {
                showNotice("warning", "請輸入有效的 API Key！");
                return;
            }
            apiKey = keyVal;
            localStorage.setItem("gemini_toeic_api_key", apiKey);
            toggleApiModal();
            document.getElementById("api-banner").classList.add("hidden");
            showNotice("success", "API Key 設定成功，已為您解鎖 AI 助記助手！");
        }

        function clearApiKey() {
            localStorage.removeItem("gemini_toeic_api_key");
            apiKey = "";
            document.getElementById("api-key-input").value = "";
            toggleApiModal();
            document.getElementById("api-banner").classList.remove("hidden");
            showNotice("info", "已清除 API Key。");
        }


        // =========================================================
        // Gemini AI 單字高效聯想生成 (API 整合)
        // =========================================================
        async function askGeminiAi() {
            if (filteredWords.length === 0) return;
            const currentWordObj = filteredWords[currentIndex];
            const targetWord = currentWordObj.word;
            const trans = currentWordObj.translation;

            // 開啟展示區並顯示 Loading
            const aiBox = document.getElementById("ai-response-box");
            const aiLoading = document.getElementById("ai-loading");
            const aiContent = document.getElementById("ai-content");

            aiBox.classList.remove("hidden");
            aiLoading.classList.remove("hidden");
            aiContent.textContent = "";

            // 自動平滑滑動到 AI 區塊
            aiBox.scrollIntoView({ behavior: 'smooth' });

            // 準備 Prompt 提示詞
            const systemPrompt = "您是一個幽默風趣且專業的多益英文名師。您的任務是幫助學生使用『諧音記憶、字首字根拆解、故事聯想法』，快速牢記多益單字。";
            const userQuery = `請針對多益單字『 ${targetWord} 』（中文意思：${trans}）進行創意拆解，並以下列格式產出（繁體中文）：
1. 💡 諧音/故事聯想記憶：(請提供生動、好記的諧音或畫面)
2. 🔍 字根字首剖析：(若合適，請拆解字首、字根或字尾)
3. 💼 多益情境短句：(提供 1 個貼近多益商業辦公室風格的模擬造句，並附上翻譯)`;

            try {
                // 如果用戶沒有輸入自訂 key，我們會告知並回退到友好提示
                if (!apiKey) {
                    // 自動生成一個簡單的模擬回應
                    setTimeout(() => {
                        aiLoading.classList.add("hidden");
                        aiContent.innerHTML = `
<div class="text-amber-800 bg-amber-50 p-4 rounded-xl border border-amber-200 mb-3 text-xs">
    ⚠️ <b>偵測到您尚未設定自己的 Gemini API Key：</b><br>
    此處展示的是<b>靜態離線預覽範本</b>。請點擊上方「Gemini AI 助手設定」輸入 API 密鑰，即可啟用即時線上 AI 聯想！
</div>
<strong class="text-indigo-900 text-base">【預覽範本】${targetWord} (${trans}) 記憶法</strong>

<b class="text-purple-700">1. 💡 諧音/故事聯想記憶：</b>
想成：${targetWord} 讀音像什麼？試著用畫面把字義關聯起來。例如 ${targetWord} 在辦公室非常重要。

<b class="text-purple-700">2. 🔍 字根字首剖析：</b>
拆解此單字的拼字結構，找出常規字根字尾（如 -ate v., -ment n.）幫你記住詞性與字義。

<b class="text-purple-700">3. 💼 多益情境短句：</b>
"We must complete the project on time to satisfy our client."
（我們必須按時完成項目，以滿足我們的客戶。）
                        `;
                    }, 1200);
                    return;
                }

                // 呼叫 Gemini 2.5 Flash 
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: userQuery }] }],
                        systemInstruction: { parts: [{ text: systemPrompt }] }
                    })
                });

                if (!response.ok) {
                    throw new Error("API 請求失敗，請檢查 API Key 是否正確或過期。");
                }

                const data = await response.json();
                const aiText = data.candidates?.[0]?.content?.parts?.[0]?.text;

                aiLoading.classList.add("hidden");
                if (aiText) {
                    aiContent.innerHTML = aiText.replace(/\n/g, '<br>');
                } else {
                    aiContent.textContent = "AI 助手今日有點疲倦，未能產生聯想，請再試一次。";
                }

            } catch (error) {
                aiLoading.classList.add("hidden");
                aiContent.innerHTML = `
                    <div class="text-rose-600 bg-rose-50 p-4 rounded-xl border border-rose-200 text-xs">
                        ❌ <b>呼叫 Gemini API 時發生錯誤：</b><br>
                        ${error.message}<br><br>
                        請確認您的 API Key 設定正確且網路通暢。
                    </div>
                `;
            }
        }

        function closeAiBox() {
            document.getElementById("ai-response-box").classList.add("hidden");
        }


        // =========================================================
        // 自定義 Notice Modal 提示通知窗
        // =========================================================
        function showNotice(type, message) {
            const modal = document.getElementById("notice-modal");
            const iconWrapper = document.getElementById("notice-icon-wrapper");
            const icon = document.getElementById("notice-icon");
            const title = document.getElementById("notice-title");
            const msg = document.getElementById("notice-message");

            msg.textContent = message;

            if (type === "success") {
                iconWrapper.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-green-100 text-green-600";
                icon.className = "fa-solid fa-circle-check";
                title.textContent = "成功";
            } else if (type === "error") {
                iconWrapper.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-rose-100 text-rose-600";
                icon.className = "fa-solid fa-circle-xmark";
                title.textContent = "錯誤";
            } else if (type === "warning") {
                iconWrapper.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-amber-100 text-amber-600";
                icon.className = "fa-solid fa-triangle-exclamation";
                title.textContent = "警告";
            } else {
                iconWrapper.className = "w-12 h-12 rounded-full mx-auto flex items-center justify-center text-xl bg-blue-100 text-indigo-600";
                icon.className = "fa-solid fa-info";
                title.textContent = "通知";
            }

            modal.classList.remove("hidden");
        }

        function closeNoticeModal() {
            document.getElementById("notice-modal").classList.add("hidden");
        }
    </script>
</body>
</html>
