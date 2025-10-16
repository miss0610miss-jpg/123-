<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>萬聖節驚魂之夜</title>
    <!-- 載入 Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- 載入 Noto Sans TC 作為主要字體 -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;700&family=Creepster&display=swap" rel="stylesheet">
    <style>
        /* 使用 Noto Sans TC 作為主要字體，並在標題使用 Creepster 增加萬聖節氛圍 */
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #0d0d0d; /* 深黑背景 */
            background-image: radial-gradient(circle, rgba(29, 29, 29, 0.8) 0%, rgba(13, 13, 13, 1) 70%);
        }
        .spooky-title {
            font-family: 'Creepster', cursive;
            text-shadow: 4px 4px 0px #8b5cf6, 8px 8px 0px #000; /* 紫色陰影 */
        }
        /* 響應式按鈕陰影效果 */
        .cta-button {
            transition: all 0.3s ease;
            box-shadow: 0 5px #9a3412; /* 深橘色陰影 */
        }
        .cta-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 7px #9a3412;
        }
        .cta-button:active {
            transform: translateY(3px);
            box-shadow: 0 2px #9a3412;
        }
    </style>
</head>
<body class="min-h-screen text-gray-100 flex flex-col antialiased">

    <!-- Firebase 初始化區塊 (必備) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // 設定 Firestore 偵錯等級
        setLogLevel('Debug');
        
        // 全局變數初始化
        let app = null;
        let db = null;
        let auth = null;
        let userId = 'anonymous';

        // 確保 __app_id 和 __firebase_config 存在
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        if (firebaseConfig) {
            try {
                // 1. 初始化 Firebase App
                app = initializeApp(firebaseConfig);
                
                // 2. 初始化服務
                db = getFirestore(app);
                auth = getAuth(app);
                
                // 3. 執行認證
                if (initialAuthToken) {
                    signInWithCustomToken(auth, initialAuthToken)
                        .then((userCredential) => {
                            userId = userCredential.user.uid;
                            console.log("Firebase: 已使用自訂 Token 登入。", userId);
                        })
                        .catch((error) => {
                            console.error("Firebase: 自訂 Token 登入失敗。", error.message);
                            // 如果 Token 登入失敗，嘗試匿名登入
                            signInAnonymously(auth)
                                .then(() => {
                                    userId = auth.currentUser.uid;
                                    console.log("Firebase: 已切換為匿名登入。");
                                });
                        });
                } else {
                    signInAnonymously(auth)
                        .then(() => {
                            userId = auth.currentUser.uid;
                            console.log("Firebase: 已使用匿名登入。");
                        })
                        .catch((error) => {
                            console.error("Firebase: 匿名登入失敗。", error.message);
                        });
                }
            } catch (e) {
                console.error("Firebase: 初始化失敗。", e);
            }
        } else {
            console.error("Firebase: 缺少設定，無法初始化。");
        }
    </script>

    <!-- 主內容區塊 -->
    <main class="container mx-auto px-4 py-8 flex-grow">
        
        <!-- 英雄區塊 (Hero) -->
        <header class="text-center py-12 md:py-16">
            <h1 class="spooky-title text-5xl sm:text-7xl md:text-8xl text-orange-400 mb-4 tracking-widest leading-none">
                萬聖節驚魂之夜
            </h1>
            <p class="text-xl sm:text-2xl text-purple-400 font-bold mb-8 italic">
                不給糖就搗蛋，或準備尖叫！
            </p>
            <!-- 南瓜/幽靈 SVG 裝飾 -->
            <div class="flex justify-center space-x-6">
                <!-- 幽靈 SVG -->
                <svg class="w-10 h-10 sm:w-16 sm:h-16 text-gray-100 animate-pulse" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2C6.48 2 2 6.48 2 12c0 3.31 1.6 6.27 4.07 8.16L6 22l2-2h8l2 2 .07-1.84C20.4 18.27 22 15.31 22 12c0-5.52-4.48-10-10-10zm0 17.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z"/>
                </svg>
                <!-- 南瓜 SVG -->
                <svg class="w-12 h-12 sm:w-20 sm:h-20 text-orange-400" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1.2 16.5c-1.89 0-3.46-1.3-3.72-3.05L12 15l2.5 2-1.3 1.5zm3-5c.46-1.55-.46-3.23-1.85-3.69l-1.65-.43L12 11.5l1.5-1.5 2.15.56c1.28.33 2.11 1.53 1.85 2.82-.26 1.29-1.29 2.15-2.57 2.15h-2.13l-1.3 1.5h3.43c2.21 0 4-1.79 4-4s-1.79-4-4-4h-2.18l-1.3-1.5h-1.36c-2.21 0-4 1.79-4 4s1.79 4 4 4h1.36l1.3-1.5H14c1.1 0 2-.9 2-2s-.9-2-2-2z"/>
                </svg>
            </div>
        </header>

        <!-- 倒數計時器區塊 -->
        <section class="text-center mb-12">
            <h2 class="text-3xl sm:text-4xl text-purple-300 font-extrabold mb-6 border-b-2 border-purple-500 inline-block px-4 pb-2">
                倒數計時！恐怖降臨！
            </h2>
            <div id="countdown" class="grid grid-cols-4 gap-4 max-w-xl mx-auto">
                <div class="bg-gray-800 p-4 rounded-lg shadow-xl border-2 border-orange-400">
                    <div id="days" class="text-4xl sm:text-6xl font-mono text-orange-400 font-bold">00</div>
                    <div class="text-sm sm:text-lg text-gray-400 mt-2">天</div>
                </div>
                <div class="bg-gray-800 p-4 rounded-lg shadow-xl border-2 border-orange-400">
                    <div id="hours" class="text-4xl sm:text-6xl font-mono text-orange-400 font-bold">00</div>
                    <div class="text-sm sm:text-lg text-gray-400 mt-2">時</div>
                </div>
                <div class="bg-gray-800 p-4 rounded-lg shadow-xl border-2 border-orange-400">
                    <div id="minutes" class="text-4xl sm:text-6xl font-mono text-orange-400 font-bold">00</div>
                    <div class="text-sm sm:text-lg text-gray-400 mt-2">分</div>
                </div>
                <div class="bg-gray-800 p-4 rounded-lg shadow-xl border-2 border-orange-400">
                    <div id="seconds" class="text-4xl sm:text-6xl font-mono text-orange-400 font-bold">00</div>
                    <div class="text-sm sm:text-lg text-gray-400 mt-2">秒</div>
                </div>
            </div>
        </section>

        <!-- 活動詳情區塊 -->
        <section class="max-w-3xl mx-auto bg-gray-900/70 p-6 sm:p-10 rounded-xl shadow-2xl border-4 border-purple-500/50 mb-12">
            <h2 class="text-2xl sm:text-3xl text-orange-300 font-extrabold mb-6 text-center">
                活動詳情
            </h2>
            <div class="space-y-4 text-center sm:text-left">
                <div class="flex flex-col sm:flex-row items-center sm:space-x-4">
                    <span class="text-purple-400 text-xl font-bold w-full sm:w-32 mb-1 sm:mb-0">
                        日期
                    </span>
                    <span class="text-xl text-gray-200">
                        <span class="font-mono text-orange-400">10月31日</span> 星期四
                    </span>
                </div>
                <div class="flex flex-col sm:flex-row items-center sm:space-x-4">
                    <span class="text-purple-400 text-xl font-bold w-full sm:w-32 mb-1 sm:mb-0">
                        時間
                    </span>
                    <span class="text-xl text-gray-200">
                        晚上 <span class="font-mono text-orange-400">8:00</span> 至 凌晨
                    </span>
                </div>
                <div class="flex flex-col sm:flex-row items-center sm:space-x-4">
                    <span class="text-purple-400 text-xl font-bold w-full sm:w-32 mb-1 sm:mb-0">
                        地點
                    </span>
                    <span class="text-xl text-gray-200">
                        <span class="font-bold text-orange-400">幽靈古堡</span> (Ravenwood Lane 13號)
                    </span>
                </div>
                <div class="flex flex-col sm:flex-row items-center sm:space-x-4">
                    <span class="text-purple-400 text-xl font-bold w-full sm:w-32 mb-1 sm:mb-0">
                        服裝要求
                    </span>
                    <span class="text-xl text-gray-200">
                        盡情打扮，最具創意的服裝將有獎！
                    </span>
                </div>
            </div>
        </section>

        <!-- 行動呼籲 (CTA) 區塊 -->
        <section class="text-center pt-4 pb-12">
            <p class="text-2xl text-gray-300 mb-6 font-semibold">
                您是否敢於面對恐懼，並加入這場狂歡？
            </p>
            <button id="rsvp-button" class="cta-button bg-orange-500 hover:bg-orange-400 text-gray-900 font-extrabold text-2xl py-4 px-10 rounded-full border-2 border-gray-900 uppercase tracking-wider transition duration-300 transform">
                🎃 膽敢報名 (Dare to RSVP) 💀
            </button>
            <p id="message-box" class="mt-4 text-lg text-purple-300 font-semibold hidden">
                感謝您的報名，您已經被列入賓客名單中... 祝您萬聖節快樂！
            </p>
        </section>

    </main>

    <!-- 頁腳 -->
    <footer class="bg-gray-900/50 text-center py-6 border-t-2 border-purple-500/30">
        <p class="text-sm text-gray-500">
            請注意：進入古堡後果自負。
        </p>
        <p class="text-xs text-gray-600 mt-1">
            由 VIBE CODING 生成的萬聖節頁面
        </p>
    </footer>

    <!-- JavaScript 倒數計時器及互動邏輯 -->
    <script>
        // 倒數計時器邏輯
        function startCountdown() {
            // 設定萬聖節日期 (10月31日 當地時間晚上 8:00)
            const halloweenDate = new Date('October 31, 2025 20:00:00').getTime();

            const timer = setInterval(function() {
                const now = new Date().getTime();
                const distance = halloweenDate - now;

                // 時間計算
                const days = Math.floor(distance / (1000 * 60 * 60 * 24));
                const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((distance % (1000 * 60)) / 1000);

                // 格式化數字 (確保兩位數)
                const formatNumber = (num) => String(num).padStart(2, '0');

                // 更新 DOM
                if (document.getElementById('days')) {
                    document.getElementById('days').innerHTML = formatNumber(days);
                    document.getElementById('hours').innerHTML = formatNumber(hours);
                    document.getElementById('minutes').innerHTML = formatNumber(minutes);
                    document.getElementById('seconds').innerHTML = formatNumber(seconds);
                }

                // 活動開始時
                if (distance < 0) {
                    clearInterval(timer);
                    document.getElementById('countdown').innerHTML = '<div class="col-span-4 text-4xl text-red-500 font-bold bg-gray-800 p-6 rounded-lg shadow-inner">👻 派對開始了！快加入我們！ 😈</div>';
                    document.getElementById('rsvp-button').style.display = 'none';
                }
            }, 1000);
        }

        // RSVP 按鈕點擊處理
        function handleRsvpClick() {
            const rsvpButton = document.getElementById('rsvp-button');
            const messageBox = document.getElementById('message-box');
            
            // 隱藏按鈕
            rsvpButton.style.display = 'none';
            // 顯示感謝訊息
            messageBox.style.display = 'block';
        }

        // 頁面載入完成後啟動所有功能
        window.onload = function() {
            startCountdown();
            const rsvpButton = document.getElementById('rsvp-button');
            if (rsvpButton) {
                rsvpButton.addEventListener('click', handleRsvpClick);
            }
        };
    </script>
</body>
</html>
