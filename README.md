<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>洛克人風格射擊遊戲 - 衛教小知識問答</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #0a0f2a 0%, #0c1225 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Courier New', 'Press Start 2P', 'Fira Code', monospace;
            padding: 20px;
        }

        .game-container {
            background: #000000aa;
            border-radius: 32px;
            padding: 20px;
            box-shadow: 0 20px 35px rgba(0, 0, 0, 0.5), inset 0 1px 0 rgba(255,255,255,0.1);
            backdrop-filter: blur(2px);
        }

        canvas {
            display: block;
            margin: 0 auto;
            border-radius: 20px;
            box-shadow: 0 0 0 4px #ffcc44, 0 10px 25px rgba(0,0,0,0.5);
            cursor: none;
        }

        .info-panel {
            display: flex;
            justify-content: space-between;
            margin-top: 18px;
            margin-bottom: 8px;
            background: #1a1f3ad9;
            padding: 12px 20px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
            border: 1px solid #ffdd88;
            font-weight: bold;
            color: #ffecb3;
            text-shadow: 2px 2px 0 #3a2c1a;
        }

        .info-item {
            background: #00000077;
            padding: 6px 16px;
            border-radius: 32px;
            font-size: 1.25rem;
            letter-spacing: 1px;
        }

        .info-item span {
            color: #f5d742;
            font-size: 1.5rem;
            font-weight: bold;
            margin-left: 8px;
        }

        .controls {
            margin-top: 16px;
            display: flex;
            gap: 28px;
            justify-content: center;
            font-size: 0.8rem;
            background: #0c0e1ad9;
            padding: 10px 18px;
            border-radius: 48px;
            color: #ffdd99;
        }

        .controls kbd {
            background: #2a2e4a;
            border-radius: 8px;
            padding: 4px 10px;
            font-weight: bold;
            color: #ffdf8c;
            margin: 0 4px;
            box-shadow: inset 0 1px 0 #5f6b9f, 0 1px 2px black;
        }

        button {
            background: #ffb347;
            border: none;
            font-family: monospace;
            font-weight: bold;
            padding: 6px 14px;
            border-radius: 40px;
            cursor: pointer;
            transition: 0.1s linear;
            box-shadow: 0 3px 0 #9b4b1e;
        }
        button:active {
            transform: translateY(2px);
            box-shadow: 0 1px 0 #9b4b1e;
        }

        /* 模態框 - 衛教問答 */
        .quiz-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            font-family: 'Segoe UI', 'Courier New', monospace;
            backdrop-filter: blur(6px);
            transition: all 0.2s;
        }

        .quiz-card {
            background: linear-gradient(135deg, #f9f3e0 0%, #fff2df 100%);
            max-width: 520px;
            width: 85%;
            border-radius: 48px;
            padding: 25px 20px 30px;
            text-align: center;
            box-shadow: 0 25px 45px rgba(0,0,0,0.5), inset 0 1px 4px rgba(255,255,200,0.8);
            border: 2px solid #ffcd7e;
        }

        .quiz-card h3 {
            font-size: 1.9rem;
            color: #bc6f1a;
            margin-bottom: 12px;
            border-bottom: 3px dashed #ffb347;
            display: inline-block;
            padding: 0 15px;
        }

        .quiz-question {
            font-size: 1.4rem;
            font-weight: bold;
            margin: 20px 0;
            color: #2d2b1f;
            background: #fff0cc;
            padding: 18px 12px;
            border-radius: 50px;
            line-height: 1.3;
        }

        .quiz-options {
            display: flex;
            flex-direction: column;
            gap: 14px;
            margin: 20px 0 15px;
        }

        .quiz-opt-btn {
            background: #e4d5b0;
            border: none;
            font-size: 1rem;
            padding: 12px 18px;
            border-radius: 60px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.07s linear;
            text-align: left;
            display: flex;
            align-items: center;
            gap: 12px;
            color: #2c2a1f;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .quiz-opt-btn:hover {
            background: #fedb8c;
            transform: scale(0.98);
        }

        .prefix {
            background: #c9992e;
            width: 32px;
            height: 32px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            border-radius: 40px;
            color: white;
            font-weight: bold;
        }

        .feedback {
            margin-top: 10px;
            font-size: 0.9rem;
            height: 28px;
            color: #b45f1b;
        }

        .restart-btn {
            background: #3a6ea5;
            color: white;
            border-radius: 32px;
            padding: 8px 20px;
            margin-top: 10px;
            border: none;
            font-weight: bold;
        }
    </style>
</head>
<body>
<div>
    <div class="game-container">
        <canvas id="gameCanvas" width="1000" height="500"></canvas>
        <div class="info-panel">
            <div class="info-item">❤️ 生命 <span id="livesValue">3</span></div>
            <div class="info-item">⭐ 分數 <span id="scoreValue">0</span></div>
            <div class="info-item">🔫 擊敗問答 <span id="quizCount">0</span></div>
        </div>
        <div class="controls">
            <span>🎮 <kbd>A/D</kbd> 左右移動</span>
            <span>⚡ <kbd>J</kbd> / <kbd>K</kbd> 射擊</span>
            <span>📚 擊敗敵人 → 回答衛教知識</span>
            <button id="resetGameBtn">🔄 重新開始</button>
        </div>
    </div>
</div>

<script>
    (function(){
        // ---------- CANVAS ----------
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // 遊戲尺寸
        const W = 1000, H = 500;
        canvas.width = W;
        canvas.height = H;

        // ---------- 衛教問答庫 (健康教育小知識，共20題，繁體中文) ----------
        const QUIZ_LIBRARY = [
            {
                question: "預防流感最有效的方法是？",
                options: ["服用抗生素", "每年接種流感疫苗", "多喝板藍根", "只吃維生素C"],
                correct: 1,
                explanation: "接種流感疫苗是預防流感及其嚴重併發症的最有效手段。"
            },
            {
                question: "關於洗手，哪一項正確？",
                options: ["只用清水沖洗10秒", "使用肥皂或洗手液搓揉至少20秒", "洗手後不用擦乾", "戴手套代替洗手"],
                correct: 1,
                explanation: "正確洗手需用肥皂搓揉20秒以上，才能有效去除病菌。"
            },
            {
                question: "咳嗽或打噴嚏時正確的禮儀是？",
                options: ["用手掌直接摀住口鼻", "朝向他人咳嗽", "用紙巾或手肘內側遮掩", "摘下口罩再咳嗽"],
                correct: 2,
                explanation: "用紙巾或手肘內側遮掩，可以避免飛沫傳播疾病。"
            },
            {
                question: "關於口罩佩戴，哪項錯誤？",
                options: ["遮住口鼻和下巴", "佩戴前洗手", "口罩變濕後繼續使用", "一次性口罩不重複使用"],
                correct: 2,
                explanation: "口罩變濕或汙染後應立即更換，否則防護效果下降。"
            },
            {
                question: "保持腸道健康，建議如何做？",
                options: ["每天喝含糖飲料", "多吃高纖維蔬果", "只吃肉食", "不吃早餐"],
                correct: 1,
                explanation: "膳食纖維能促進腸道蠕動，減少便秘和腸道疾病風險。"
            },
            {
                question: "預防登革熱，以下哪種方法最有效？",
                options: ["消滅蚊子孳生源", "吃消炎藥", "注射登革熱疫苗(目前尚無廣泛疫苗)", "關閉門窗"],
                correct: 0,
                explanation: "清除積水、消滅蚊蟲孳生地是預防登革熱的關鍵。"
            },
            {
                question: "關於近視防控，哪項正確？",
                options: ["每天戶外活動2小時", "長時間看手機不休息", "在昏暗燈光下閱讀", "多吃甜食"],
                correct: 0,
                explanation: "每天至少2小時戶外光照能有效降低近視發生率。"
            },
            {
                question: "預防骨質疏鬆，下列哪種營養素最重要？",
                options: ["鐵質", "鈣質與維生素D", "蛋白質", "碘"],
                correct: 1,
                explanation: "充足的鈣質和維生素D有助於維持骨骼健康，預防骨質疏鬆。"
            },
            {
                question: "關於安全性行為，以下何者正確？",
                options: ["僅靠體外射精可避孕", "全程正確使用保險套", "性伴侶越多人越好", "服用事後藥可當常規避孕"],
                correct: 1,
                explanation: "全程正確使用保險套可有效預防性傳染病並降低懷孕風險。"
            },
            {
                question: "哪一項是「戒菸」的立即好處？",
                options: ["體重立即下降", "心跳與血壓趨於正常", "肺癌完全消失", "牙齒變白"],
                correct: 1,
                explanation: "停止吸菸20分鐘後心跳與血壓就會開始恢復，降低心血管負擔。"
            },
            {
                question: "下列何者為「代謝症候群」的指標？",
                options: ["身高過高", "腰圍過粗、血壓偏高", "視力良好", "頭髮茂密"],
                correct: 1,
                explanation: "腰圍過粗、血糖血脂異常、血壓偏高都是代謝症候群的警示信號。"
            },
            {
                question: "預防大腸癌，最推薦的習慣是？",
                options: ["經常熬夜", "定期大腸癌篩檢及高纖飲食", "只吃保健食品", "不吃蔬菜"],
                correct: 1,
                explanation: "定期篩檢與多攝取膳食纖維可顯著降低大腸癌發生率。"
            },
            {
                question: "哪一種方式可以有效緩解壓力？",
                options: ["持續熬夜工作", "規律運動與充足睡眠", "過量飲酒", "暴飲暴食"],
                correct: 1,
                explanation: "規律運動、充足睡眠及良好社交有助於調節壓力荷爾蒙。"
            },
            {
                question: "關於抗生素的使用，何者正確？",
                options: ["感冒就要立刻吃抗生素", "細菌感染才需依醫囑使用", "自行增減劑量", "分享給朋友吃"],
                correct: 1,
                explanation: "抗生素僅對細菌有效，濫用會產生抗藥性，須遵醫囑使用。"
            },
            {
                question: "預防跌倒對長者很重要，下列何者正確？",
                options: ["居家保持昏暗燈光", "地面保持濕滑", "移除通道障礙物，加裝扶手", "家具隨意堆放"],
                correct: 2,
                explanation: "改善居家環境、增強肌力訓練可有效預防長者跌倒。"
            },
            {
                question: "關於飲水健康，建議每日喝多少水？",
                options: ["500ml", "約1500~2000ml", "4000ml以上", "口渴再喝即可"],
                correct: 1,
                explanation: "一般成人每日建議飲用1500~2000毫升水，視活動量調整。"
            },
            {
                question: "哪一項是正確的用藥觀念？",
                options: ["藥物過期繼續吃", "依醫師藥師指示服用", "把藥品磨粉隨便吃", "自己加藥比較快"],
                correct: 1,
                explanation: "遵照醫囑使用藥品，不自行增減或停藥，才能確保療效與安全。"
            },
            {
                question: "預防熱傷害，哪項錯誤？",
                options: ["穿著淺色透氣衣物", "定時補充水分", "高溫下激烈運動且不休息", "避免在正午烈日下活動"],
                correct: 2,
                explanation: "高溫環境應適時休息、補充水分，避免長時間劇烈運動。"
            },
            {
                question: "關於口腔衛生，何者正確？",
                options: ["一天刷牙一次就夠", "使用牙線清潔牙縫", "喝完糖水不需刷牙", "牙刷用到開花再換"],
                correct: 1,
                explanation: "每天至少刷牙兩次並使用牙線，可預防蛀牙與牙周病。"
            },
            {
                question: "下列哪項是正確的飲食均衡原則？",
                options: ["每天吃大量炸雞", "每餐以全穀雜糧、蛋白質、蔬菜搭配", "只吃水果減肥", "完全不吃澱粉"],
                correct: 1,
                explanation: "均衡飲食包含多元營養，全穀、蛋白質、蔬菜水果缺一不可。"
            }
        ];

        // ---------- 遊戲全域狀態 ----------
        let gameRunning = true;      // 遊戲是否進行中 (未結束)
        let gamePaused = false;      // 問答暫停標誌
        let gameOverFlag = false;    // 遊戲結束標誌
        
        // 玩家屬性
        let player = {
            x: 80,
            y: H - 70,
            width: 28,
            height: 32,
            hp: 3,
            invincibleTimer: 0,      // 無敵幀計數
            speed: 5.2
        };
        
        // 動態數據
        let enemies = [];     // 敵人 {x, y, w, h, hp, color}
        let bullets = [];     // 子彈 {x, y, w, h, speedX}
        let score = 0;        // 衛教學分 (回答正確加分)
        let defeatedCount = 0; // 累積擊敗次數（完成答題次數）
        
        // 射擊冷卻
        let shootCooldown = 0;
        const SHOOT_DELAY_FRAMES = 12;   // 幀數冷卻
        
        // 敵人生成計時 (基於時間差)
        let lastTimestamp = 0;
        let enemySpawnAccumulator = 0;
        const ENEMY_SPAWN_INTERVAL_MS = 1800;  // 1.8秒生成一個敵人
        let lastFrameTime = 0;
        
        // 敵人基本屬性
        const ENEMY_BASE_WIDTH = 32;
        const ENEMY_BASE_HEIGHT = 32;
        const ENEMY_BASE_HP = 2;      // 需要擊中2次才會擊敗
        
        // ---------- 輔助函式 ----------
        function updateUI() {
            document.getElementById('livesValue').innerText = player.hp;
            document.getElementById('scoreValue').innerText = score;
            document.getElementById('quizCount').innerText = defeatedCount;
        }
        
        // 重置遊戲
        function resetGame() {
            // 關閉任何現有模態框
            const existingModal = document.querySelector('.quiz-modal');
            if(existingModal) existingModal.remove();
            
            gameRunning = true;
            gamePaused = false;
            gameOverFlag = false;
            player = {
                x: 80,
                y: H - 70,
                width: 28,
                height: 32,
                hp: 3,
                invincibleTimer: 0,
                speed: 5.2
            };
            enemies = [];
            bullets = [];
            score = 0;
            defeatedCount = 0;
            shootCooldown = 0;
            enemySpawnAccumulator = 0;
            lastFrameTime = 0;
            updateUI();
        }
        
        // 衛教彈窗 (擊敗敵人時呼叫)
        function showHealthQuiz() {
            if (!gameRunning || gameOverFlag) return;
            // 隨機從題庫抽一題
            const randomIndex = Math.floor(Math.random() * QUIZ_LIBRARY.length);
            const quiz = QUIZ_LIBRARY[randomIndex];
            
            // 遊戲暫停
            gamePaused = true;
            
            // 建立模態div
            const modalDiv = document.createElement('div');
            modalDiv.className = 'quiz-modal';
            modalDiv.id = 'dynamicQuizModal';
            
            // 構建選項HTML
            let optionsHtml = '';
            const letters = ['A', 'B', 'C', 'D'];
            quiz.options.forEach((opt, idx) => {
                optionsHtml += `
                    <button class="quiz-opt-btn" data-opt-index="${idx}">
                        <span class="prefix">${letters[idx]}</span>
                        <span style="flex:1">${escapeHtml(opt)}</span>
                    </button>
                `;
            });
            
            modalDiv.innerHTML = `
                <div class="quiz-card">
                    <h3>📖 衛教小挑戰</h3>
                    <div class="quiz-question">❓ ${escapeHtml(quiz.question)}</div>
                    <div class="quiz-options" id="quizOptionsContainer">
                        ${optionsHtml}
                    </div>
                    <div class="feedback" id="quizFeedback"></div>
                </div>
            `;
            
            document.body.appendChild(modalDiv);
            
            // 綁定選項事件
            const optionsBtns = modalDiv.querySelectorAll('.quiz-opt-btn');
            let answered = false;
            
            const handleAnswer = (btn) => {
                if(answered) return;
                answered = true;
                const selectedIdx = parseInt(btn.getAttribute('data-opt-index'));
                const isCorrect = (selectedIdx === quiz.correct);
                const feedbackDiv = modalDiv.querySelector('#quizFeedback');
                
                if(isCorrect) {
                    // 回答正確增加衛教學分 20分
                    score += 20;
                    feedbackDiv.innerHTML = `✅ 正確！ +20 衛教學分<br>📘 ${escapeHtml(quiz.explanation)}`;
                    feedbackDiv.style.color = "#2a6817";
                    feedbackDiv.style.fontWeight = "bold";
                } else {
                    // 回答錯誤扣5分，但最少0分，教育為主
                    score = Math.max(0, score - 5);
                    const correctAns = quiz.options[quiz.correct];
                    feedbackDiv.innerHTML = `❌ 錯誤！正確答案是: ${escapeHtml(correctAns)}<br>📚 小知識: ${escapeHtml(quiz.explanation)}`;
                    feedbackDiv.style.color = "#aa3838";
                    feedbackDiv.style.fontWeight = "bold";
                }
                defeatedCount++;    // 累計完成一次問答（無論對錯都算完成一次戰鬥學習）
                updateUI();
                
                // 禁用所有選項，避免重複點擊
                optionsBtns.forEach(btn => btn.disabled = true);
                
                // 1.2秒後關閉模態框並恢復遊戲
                setTimeout(() => {
                    if(modalDiv && modalDiv.parentNode) modalDiv.remove();
                    // 恢復遊戲
                    gamePaused = false;
                    // 如果遊戲已經結束，不恢復移動
                    if(gameOverFlag || player.hp <= 0) {
                        gameRunning = false;
                        gameOverFlag = true;
                    } else {
                        gameRunning = true;
                    }
                }, 1200);
            };
            
            optionsBtns.forEach(btn => {
                btn.addEventListener('click', () => handleAnswer(btn));
            });
        }
        
        // 簡易防XSS
        function escapeHtml(str) {
            return str.replace(/[&<>]/g, function(m) {
                if(m === '&') return '&amp;';
                if(m === '<') return '&lt;';
                if(m === '>') return '&gt;';
                return m;
            }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
                return c;
            });
        }
        
        // 遊戲結束邏輯
        function endGame() {
            if(gameOverFlag) return;
            gameRunning = false;
            gameOverFlag = true;
            gamePaused = false; // 避免卡模態框
            const modalExists = document.querySelector('.quiz-modal');
            if(modalExists) modalExists.remove();
            // 顯示遊戲結束覆蓋層
            const gameOverMsg = document.createElement('div');
            gameOverMsg.className = 'quiz-modal';
            gameOverMsg.innerHTML = `
                <div class="quiz-card" style="background:#e6cc99;">
                    <h3>💀 GAME OVER 💀</h3>
                    <div style="font-size:1.8rem; margin:20px">🏆 總分: ${score}</div>
                    <div>完成衛教問答: ${defeatedCount} 次</div>
                    <button id="gameOverRestart" class="restart-btn" style="margin-top:30px;">🔁 重新開始遊戲</button>
                </div>
            `;
            document.body.appendChild(gameOverMsg);
            const restartBtn = document.getElementById('gameOverRestart');
            if(restartBtn) {
                restartBtn.addEventListener('click', () => {
                    gameOverMsg.remove();
                    resetGame();
                    // 額外重置場景
                    cancelAnimationFrame(gameLoopId);
                    lastFrameTime = 0;
                    requestAnimationFrame(animate);
                });
            }
        }
        
        // ---------- 遊戲機制: 生成敵人 ----------
        function spawnEnemy() {
            let randY = 40 + Math.random() * (H - ENEMY_BASE_HEIGHT - 70);
            enemies.push({
                x: W - 50,
                y: randY,
                w: ENEMY_BASE_WIDTH,
                h: ENEMY_BASE_HEIGHT,
                hp: ENEMY_BASE_HP,
                color: "#DD5656",
                speedX: -2.3    // 向左移動
            });
        }
        
        // 更新敵人移動 & 碰撞擊敗
        function updateEnemiesAndCollision() {
            // 敵人移動
            for(let i=0; i<enemies.length; i++) {
                const e = enemies[i];
                e.x += e.speedX;
                if(e.x + e.w < 0) {
                    enemies.splice(i,1);
                    i--;
                    continue;
                }
            }
            
            // 子彈與敵人碰撞
            for(let i=0; i<bullets.length; i++) {
                const b = bullets[i];
                let hitIndex = -1;
                for(let j=0; j<enemies.length; j++) {
                    const e = enemies[j];
                    if(b.x < e.x+e.w && b.x+b.w > e.x && b.y < e.y+e.h && b.y+b.h > e.y) {
                        hitIndex = j;
                        break;
                    }
                }
                if(hitIndex !== -1) {
                    const enemy = enemies[hitIndex];
                    enemy.hp -= 1;
                    bullets.splice(i,1);
                    i--;
                    if(enemy.hp <= 0) {
                        enemies.splice(hitIndex,1);
                        // 擊敗敵人 => 觸發衛教問答
                        if(gameRunning && !gamePaused && !gameOverFlag) {
                            showHealthQuiz();
                        } else if(!gamePaused && !gameOverFlag) {
                            showHealthQuiz();
                        }
                    }
                    break;
                }
            }
        }
        
        // 玩家與敵人的碰撞（傷害+擊退）
        function handlePlayerCollision() {
            for(let i=0; i<enemies.length; i++) {
                const e = enemies[i];
                if(player.x < e.x+e.w && player.x+player.width > e.x && player.y < e.y+e.h && player.y+player.height > e.y) {
                    if(player.invincibleTimer <= 0 && player.hp > 0) {
                        player.hp--;
                        updateUI();
                        player.invincibleTimer = 45;
                        // 擊退效果
                        if(player.x < e.x) player.x -= 38;
                        else player.x += 38;
                        player.x = Math.max(10, Math.min(W - player.width - 10, player.x));
                        if(player.hp <= 0) {
                            endGame();
                            return;
                        }
                    }
                    if(player.invincibleTimer > 0) {
                        if(player.x < e.x) player.x = Math.max(5, e.x - player.width - 5);
                        else player.x = Math.min(W - player.width - 5, e.x + e.w + 5);
                    }
                    break;
                }
            }
            if(player.invincibleTimer > 0) player.invincibleTimer--;
        }
        
        // 發射子彈
        function shoot() {
            if(shootCooldown > 0) return;
            bullets.push({
                x: player.x + player.width,
                y: player.y + player.height/2 - 4,
                w: 12,
                h: 6,
                speedX: 9
            });
            shootCooldown = SHOOT_DELAY_FRAMES;
        }
        
        // 更新子彈移動/邊界
        function updateBullets() {
            for(let i=0; i<bullets.length; i++) {
                bullets[i].x += bullets[i].speedX;
                if(bullets[i].x > W || bullets[i].x + bullets[i].w < 0) {
                    bullets.splice(i,1);
                    i--;
                }
            }
        }
        
        // 生成敵人計時 (基於時間差)
        let lastSpawnTime = 0;
        function updateSpawner(nowMs) {
            if(lastSpawnTime === 0) {
                lastSpawnTime = nowMs;
                return;
            }
            let diff = nowMs - lastSpawnTime;
            if(diff >= ENEMY_SPAWN_INTERVAL_MS) {
                if(enemies.length < 6) {
                    spawnEnemy();
                }
                lastSpawnTime = nowMs;
            }
        }
        
        // 鍵盤控制
        const keys = {
            ArrowLeft: false, ArrowRight: false, KeyA: false, KeyD: false,
            KeyJ: false, KeyK: false, Space: false
        };
        
        function handleInput() {
            if(gamePaused || !gameRunning || gameOverFlag) return;
            let move = 0;
            if(keys.KeyA || keys.ArrowLeft) move = -1;
            if(keys.KeyD || keys.ArrowRight) move = 1;
            if(move !== 0) {
                player.x += move * player.speed;
                player.x = Math.max(12, Math.min(W - player.width - 12, player.x));
            }
            if((keys.KeyJ || keys.KeyK || keys.Space) && shootCooldown <= 0 && !gamePaused && gameRunning) {
                shoot();
            }
            if(shootCooldown > 0) shootCooldown--;
        }
        
        // ---------- 繪圖模組 洛克人風格 ----------
        function drawPlayer() {
            let alpha = 1;
            if(player.invincibleTimer > 0 && (Math.floor(Date.now()/50)%3 === 0)) alpha = 0.6;
            ctx.save();
            if(alpha !== 1) ctx.globalAlpha = alpha;
            ctx.fillStyle = "#2A6FDB";
            ctx.shadowBlur = 6;
            ctx.shadowColor = "#0099ff";
            ctx.fillRect(player.x, player.y, player.width, player.height);
            ctx.fillStyle = "#FFBB44";
            ctx.fillRect(player.x+4, player.y-6, 20, 8);
            ctx.fillStyle = "#FFFFFF";
            ctx.fillRect(player.x+20, player.y+8, 5, 8);
            ctx.fillStyle = "#CC6600";
            ctx.fillRect(player.x+5, player.y+player.height-5, 18, 5);
            ctx.fillStyle = "#AA5555";
            ctx.fillRect(player.x+player.width-4, player.y+12, 8, 12);
            ctx.restore();
        }
        
        function drawEnemy() {
            for(let e of enemies) {
                ctx.fillStyle = e.hp === 2 ? "#C73D3D" : "#A53333";
                ctx.shadowBlur = 4;
                ctx.fillRect(e.x, e.y, e.w, e.h);
                ctx.fillStyle = "#3A2A1A";
                ctx.fillRect(e.x+6, e.y+6, 6, 8);
                ctx.fillRect(e.x+e.w-12, e.y+6, 6, 8);
                ctx.fillStyle = "#FFFFFF";
                if(e.hp === 2) ctx.fillStyle = "#FFD966";
                ctx.font = "bold 14 monospace";
                ctx.fillText("⚡"+e.hp, e.x+10, e.y-4);
                ctx.fillStyle = "#000";
                ctx.fillRect(e.x+8, e.y+18, 5, 5);
                ctx.fillRect(e.x+e.w-13, e.y+18, 5, 5);
            }
        }
        
        function drawBullets() {
            for(let b of bullets) {
                ctx.fillStyle = "#FFE484";
                ctx.shadowBlur = 5;
                ctx.fillRect(b.x, b.y, b.w, b.h);
                ctx.fillStyle = "#FF9900";
                ctx.fillRect(b.x+2, b.y+2, 4, 2);
            }
        }
        
        function drawBackground() {
            ctx.fillStyle = "#1a281f";
            ctx.fillRect(0, H-48, W, 48);
            ctx.fillStyle = "#6b4c3b";
            for(let i=0;i<12;i++) {
                ctx.fillRect(40+i*80, H-52, 35, 8);
            }
            ctx.fillStyle = "#aaaa77";
            ctx.font = "12px monospace";
            ctx.fillText("ROCKMAN STYLE · 衛教射擊", 20, 35);
        }
        
        // 遊戲主要更新邏輯 (僅在未暫停且遊戲活躍時)
        function updateGame(nowMs) {
            if(gamePaused || !gameRunning || gameOverFlag) return;
            
            handleInput();
            updateBullets();
            updateEnemiesAndCollision();
            handlePlayerCollision();
            updateSpawner(nowMs);
            
            if(player.hp <= 0) {
                endGame();
            }
        }
        
        let animFrameId = null;
        function animate(timestamp) {
            if(!lastFrameTime) lastFrameTime = timestamp;
            const now = timestamp;
            let delta = Math.min(100, now - lastFrameTime);
            if(delta < 0) delta = 16;
            updateGame(now);
            
            ctx.clearRect(0, 0, W, H);
            drawBackground();
            drawPlayer();
            drawEnemy();
            drawBullets();
            
            if(gamePaused && !gameOverFlag) {
                ctx.font = "bold 26 monospace";
                ctx.fillStyle = "#FFFFBB";
                ctx.shadowBlur = 0;
                ctx.fillText("📖 衛教問答中...", W/2-140, 70);
            }
            if(gameOverFlag && !document.querySelector('.quiz-modal')) {
                ctx.font = "30px monospace";
                ctx.fillStyle = "#EE4444";
                ctx.fillText("GAME OVER", W/2-100, H/2);
            }
            
            lastFrameTime = now;
            animFrameId = requestAnimationFrame(animate);
        }
        
        // ---------- 鍵盤綁定 ----------
        function setupControls() {
            window.addEventListener('keydown', (e) => {
                let code = e.code;
                if(keys.hasOwnProperty(code)) {
                    keys[code] = true;
                    e.preventDefault();
                }
                if(code === 'Space') {
                    keys.Space = true;
                    e.preventDefault();
                }
            });
            window.addEventListener('keyup', (e) => {
                let code = e.code;
                if(keys.hasOwnProperty(code)) keys[code] = false;
                if(code === 'Space') keys.Space = false;
            });
            document.getElementById('resetGameBtn').addEventListener('click', () => {
                const modal = document.querySelector('.quiz-modal');
                if(modal) modal.remove();
                resetGame();
                gamePaused = false;
                gameRunning = true;
                gameOverFlag = false;
                lastSpawnTime = 0;
                enemies = [];
                bullets = [];
                player.hp = 3; player.invincibleTimer = 0;
                score = 0; defeatedCount = 0;
                updateUI();
            });
        }
        
        // 初始化
        function init() {
            resetGame();
            setupControls();
            lastSpawnTime = 0;
            setTimeout(() => { if(!gameOverFlag && enemies.length===0 && !gamePaused) spawnEnemy(); }, 500);
            animate();
        }
        
        let gameLoopId;
        function startGame() {
            gameLoopId = requestAnimationFrame(animate);
        }
        init();
        startGame();
    })();
</script>
</body>
</html>
