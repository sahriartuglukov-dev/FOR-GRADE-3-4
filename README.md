<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Академия Героев: REBORN</title>
    <style>
        /* Шрифты */
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Exo+2:wght@400;600;800&family=Share+Tech+Mono&display=swap');

        :root {
            --glass: rgba(13, 17, 23, 0.85);
            --accent: #00f2ff;
            --danger: #ff003c;
            --success: #00ff9d;
            --gold: #ffcc00;
            --font-head: 'Orbitron', sans-serif;
            --font-body: 'Exo 2', sans-serif;
            --font-mono: 'Share Tech Mono', monospace;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body {
            font-family: var(--font-body);
            margin: 0; padding: 0;
            color: #e0e6ed;
            min-height: 100vh;
            background-color: #000;
            overflow-x: hidden;
            transition: background 0.5s ease;
        }

        /* --- КАНВАС (Зеленый код для АГЕНТА) --- */
        #hacker-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
            display: none;
        }

        /* --- ФОН ОШИБКИ (Для МАСТЕРА) --- */
        #error-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
            display: none;
            background: #0a0000;
            overflow: hidden;
            opacity: 0.8;
        }
        .error-line {
            white-space: nowrap;
            font-family: var(--font-mono);
            color: rgba(255, 0, 60, 0.3);
            font-size: 1.5rem;
            transform: translateX(-10%);
            animation: glitchText 0.2s infinite;
        }
        @keyframes glitchText {
            0% { transform: translateX(-10%) skewX(0deg); opacity: 0.3; }
            20% { transform: translateX(-11%) skewX(-10deg); opacity: 0.5; }
            40% { transform: translateX(-9%) skewX(10deg); opacity: 0.2; }
            60% { transform: translateX(-10%) skewX(0deg); opacity: 0.3; }
            80% { transform: translateX(-10.5%) skewX(5deg); opacity: 0.6; }
            100% { transform: translateX(-10%) skewX(0deg); opacity: 0.3; }
        }

        /* --- СТИЛИ ТЕМ --- */
        /* 1. РЕКРУТ */
        body.theme-newbie {
            background-color: #050a10;
            background-image: 
                linear-gradient(rgba(0, 242, 255, 0.1) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 242, 255, 0.1) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: gridMove 20s linear infinite;
        }
        @keyframes gridMove { 0% { background-position: 0 0; } 100% { background-position: 50px 50px; } }

        /* 2. АГЕНТ */
        body.theme-agent { background-color: #000; }
        body.theme-agent #hacker-bg { display: block; }

        /* 3. МАСТЕР */
        body.theme-master { background-color: #110000; }
        body.theme-master #error-bg { display: flex; flex-direction: column; justify-content: space-around; }

        /* UI Стили */
        .glass-panel {
            background: rgba(10, 15, 20, 0.7);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 4px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.6);
            position: relative;
        }

        header {
            padding: 15px 20px; display: flex; justify-content: space-between; align-items: center;
            position: sticky; top: 0; z-index: 100; background: rgba(0, 0, 0, 0.9);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stats-box { 
            font-family: var(--font-mono); background: #000; padding: 8px 16px; border-radius: 2px; 
            border: 1px solid #333; display: flex; align-items: center; gap: 8px; font-size: 1rem; color: #fff;
        }

        .hero-section { text-align: center; padding: 40px 20px; animation: fadeIn 1s; }
        .hero-title { font-family: var(--font-head); font-size: 2.5rem; text-transform: uppercase; margin: 0 0 10px 0; color: white; }
        .hero-desc { color: #8899a6; font-size: 1.1rem; max-width: 600px; margin: 0 auto; font-family: var(--font-mono); }

        .grid {
            display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 20px; max-width: 1100px; margin: 0 auto; padding: 20px;
        }

        .card {
            padding: 25px; cursor: pointer; text-align: center; user-select: none; transition: 0.3s;
            border: 1px solid rgba(255,255,255,0.05);
        }
        .card:hover { transform: translateY(-5px); border-color: var(--accent); background: rgba(0, 242, 255, 0.05); }
        .card h3 { font-family: var(--font-head); margin: 15px 0 10px 0; color: #fff; }
        .icon { font-size: 3rem; margin-bottom: 10px; display: block; }
        .card.special { border-color: var(--danger); }
        .card.completed { border-color: var(--success); opacity: 0.7; pointer-events: none; }
        .progress-bar { height: 4px; background: #111; width: 100%; margin-top: 15px; }
        .progress-fill { height: 100%; background: var(--accent); width: 0%; transition: width 0.5s ease; }

        .btn-main {
            padding: 15px 40px; font-size: 1.5rem; font-family: var(--font-head); font-weight: 900;
            background: var(--accent); border: none; cursor: pointer; color: #000;
            text-transform: uppercase; letter-spacing: 2px;
            clip-path: polygon(15px 0, 100% 0, 100% calc(100% - 15px), calc(100% - 15px) 100%, 0 100%, 0 15px);
            transition: all 0.2s;
        }
        .btn-main:hover { transform: scale(1.05); box-shadow: 0 0 20px var(--accent); background: #fff; }

        .screen-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: 5000; display: none; 
            justify-content: center; align-items: center; flex-direction: column;
            background: #000;
        }

        /* ЧИСТЫЙ СТАРТОВЫЙ ЭКРАН */
        #start-screen { display: flex; z-index: 9999; background: #000; }
        
        .story-box {
            max-width: 700px; width: 90%; padding: 40px;
            background: rgba(0, 0, 0, 0.9); border: 1px solid var(--accent);
            text-align: center;
        }
        .story-text { font-size: 1.2rem; line-height: 1.6; color: #fff; margin-bottom: 30px; text-align: left; font-family: var(--font-mono); }

        .modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 6000; display: none;
            justify-content: center; align-items: center; opacity: 0; transition: opacity 0.3s;
        }
        .modal.open { display: flex; opacity: 1; }
        .modal-content {
            background: #080808; border: 1px solid var(--accent);
            padding: 30px; width: 90%; max-width: 600px;
            transform: scale(0.9); transition: transform 0.3s;
        }
        .modal.open .modal-content { transform: scale(1); }

        .ans-btn {
            background: #111; border: 1px solid #333; color: #ccc;
            padding: 15px; width: 100%; text-align: left; margin-bottom: 10px; cursor: pointer;
            font-size: 1rem; font-family: var(--font-mono);
        }
        .ans-btn:hover { border-color: var(--accent); color: var(--accent); }
        .ans-btn.correct { border-color: var(--success); color: var(--success); background: #002200; }
        .ans-btn.wrong { border-color: var(--danger); color: var(--danger); background: #220000; }

        /* ФИНАЛ */
        #final-screen { z-index: 10000; overflow: hidden; background: #000; }
        
        .report-card {
            border: 2px solid #fff; padding: 40px; margin-bottom: 50px; text-align: center;
            font-family: var(--font-mono); width: 90%; max-width: 500px;
            background: #111;
        }
        .score-row { display: flex; justify-content: space-between; margin-bottom: 15px; font-size: 1.2rem; border-bottom: 1px solid #333; padding-bottom: 5px;}
        .final-big { font-family: var(--font-head); font-size: 4rem; color: var(--success); margin-top: 20px; }

        .credits {
            position: absolute; bottom: -100px; width: 100%; text-align: center;
            animation: creditsRoll 15s linear forwards; pointer-events: none;
        }
        @keyframes creditsRoll { from { transform: translateY(100vh); } to { transform: translateY(-120vh); } }
        .cr-role { color: var(--accent); font-size: 0.9rem; letter-spacing: 2px; text-transform: uppercase; margin-top: 40px; }
        .cr-name { font-size: 2rem; font-weight: bold; font-family: var(--font-head); }

        /* УВЕДОМЛЕНИЯ */
        #notif-area { position: fixed; top: 80px; right: 20px; z-index: 7000; display: flex; flex-direction: column; gap: 10px; }
        .notif { padding: 15px 25px; background: #000; border: 1px solid #fff; color: #fff; font-family: var(--font-mono); }
        .notif.success { border-color: var(--success); color: var(--success); }
        .notif.error { border-color: var(--danger); color: var(--danger); }
        .close-icon { position: absolute; top: 15px; right: 20px; font-size: 2rem; cursor: pointer; color: #555; }

    </style>
</head>
<body class="theme-newbie">

    <canvas id="hacker-bg"></canvas>

    <div id="error-bg">
        <div class="error-line">ERROR SYSTEM FAILURE 0x00 ERROR CRITICAL ERROR SYSTEM FAILURE</div>
        <div class="error-line" style="animation-duration: 0.3s;">WARNING VIRUS DETECTED ERROR 0x01 ACCESS DENIED</div>
        <div class="error-line" style="animation-duration: 0.15s; color: rgba(255,0,0,0.5);">FATAL ERROR SYSTEM FAILURE ERROR ERROR ERROR</div>
        <div class="error-line">CONNECTION LOST RECONNECTING... ERROR 404</div>
        <div class="error-line" style="animation-duration: 0.25s;">SYSTEM BREACH DETECTED ERROR SYSTEM FAILURE</div>
        <div class="error-line">CRITICAL FAILURE ERROR ERROR SYSTEM DOWN</div>
        <div class="error-line" style="animation-duration: 0.4s;">ERROR 0x99 FATAL EXCEPTION ERROR</div>
        <div class="error-line">ACCESS DENIED ACCESS DENIED ACCESS DENIED</div>
        <div class="error-line">SYSTEM FAILURE ERROR SYSTEM FAILURE ERROR</div>
        <div class="error-line">ERROR ERROR ERROR ERROR ERROR ERROR ERROR</div>
    </div>

    <div id="start-screen" class="screen-overlay" style="display: flex;">
        <h1 style="font-family: 'Orbitron'; font-size: 3.5rem; text-align: center; margin-bottom: 50px; color: white;">
            АКАДЕМИЯ ГЕРОЕВ
        </h1>
        <button class="btn-main" onclick="startGame()">НАЧАТЬ МИССИЮ</button>
    </div>

    <div id="story-screen" class="screen-overlay">
        <div class="story-box">
            <h2 id="story-title" style="color:var(--gold); font-family: 'Orbitron';">СООБЩЕНИЕ</h2>
            <div id="story-text" class="story-text"></div>
            <button class="btn-main" onclick="closeStory()" style="font-size: 1.2rem;">ПРИНЯТЬ</button>
        </div>
    </div>

    <div id="final-screen" class="screen-overlay">
        <div style="z-index: 2; display: flex; flex-direction: column; align-items: center; margin-top: 10vh; width: 100%;">
            <h2 style="font-family: 'Orbitron'; font-size: 2.5rem; margin-bottom: 20px;">ОТЧЕТ МИССИИ</h2>
            
            <div class="report-card">
                <div class="score-row">
                    <span>Ошибки:</span>
                    <span id="res-mistakes" style="color: var(--danger)">0</span>
                </div>
                <div class="score-row">
                    <span>Баланс крипты:</span>
                    <span id="res-coins" style="color: var(--gold)">0 🪙</span>
                </div>
                <div style="margin-top: 20px; font-size: 1.5rem;">ИТОГОВАЯ ОЦЕНКА</div>
                <div class="final-big" id="res-score">10/10</div>
            </div>
        </div>

        <div class="credits">
            <div class="cr-role">ГЛАВНЫЙ ГЕРОЙ</div><div class="cr-name">ТЫ</div>
            <div class="cr-role">РАЗРАБОТКА</div><div class="cr-name">ТУГЛУКОВ ШАХРИЯР</div>
            <div class="cr-role">СЦЕНАРИЙ</div><div class="cr-name">ТУГЛУКОВ ШАХРИЯР</div>
            <div class="cr-role">AI SYSTEM</div><div class="cr-name">GEMINI</div>
            
            <div style="margin-top: 80px;">
                <button class="btn-main" onclick="fullReset()" style="font-size: 1rem; padding: 10px 30px;">ПЕРЕЗАГРУЗКА</button>
            </div>
        </div>
    </div>

    <div id="game-ui" style="display:none;">
        <header>
            <div style="color: var(--accent); font-family: 'Orbitron'; font-size: 1.4rem; font-weight: bold;">🛡️ ACADEMY OS</div>
            <div style="display: flex; gap: 10px;">
                <div class="stats-box"><span id="rank-name">РЕКРУТ</span></div>
                <div class="stats-box" style="border-color: var(--gold); color: var(--gold);">🪙 <span id="coins">0</span></div>
            </div>
        </header>

        <section class="hero-section">
            <h1 class="hero-title" id="location-name">ХОЛЛ АКАДЕМИИ</h1>
            <p class="hero-desc" id="location-desc">Базовый уровень доступа.</p>
        </section>

        <div class="grid">
            <div class="card glass-panel" onclick="openTask('speech')">
                <span class="icon">🗣️</span>
                <h3>ЭТИКЕТ</h3>
                <div class="progress-bar"><div class="progress-fill" id="bar-speech"></div></div>
            </div>
            <div class="card glass-panel" onclick="openTask('logic')">
                <span class="icon">🧠</span>
                <h3>ЛОГИКА</h3>
                <div class="progress-bar"><div class="progress-fill" id="bar-logic"></div></div>
            </div>
            <div class="card glass-panel" onclick="openTask('crit')">
                <span class="icon">👁️</span>
                <h3>БЕЗОПАСНОСТЬ</h3>
                <div class="progress-bar"><div class="progress-fill" id="bar-crit"></div></div>
            </div>
            
            <div class="card glass-panel special" id="card-hack" onclick="openHack()">
                <span class="icon">🎲</span>
                <h3 id="hack-title">ВЗЛОМ</h3>
                <p id="hack-desc" style="font-size:0.9rem; color:#ff88aa;">Награда: Высокая</p>
            </div>

            <div class="card glass-panel" onclick="checkRankUp()" style="border-color: var(--gold);">
                <span class="icon">🚀</span>
                <h3>ПОВЫШЕНИЕ</h3>
                <p style="font-size:0.9rem; color:#aaa;">След. уровень</p>
            </div>
        </div>
    </div>

    <div id="quiz-modal" class="modal">
        <div class="modal-content">
            <div id="quiz-text" class="q-text" style="font-size:1.3rem; margin-bottom:20px; color:#fff;"></div>
            <div id="quiz-answers"></div>
        </div>
    </div>

    <div id="hack-modal" class="modal">
        <div class="modal-content" style="text-align: center;">
            <div class="close-icon" onclick="closeModal('hack-modal')">&times;</div>
            <h2 style="color:var(--accent); font-family:'Orbitron'">СИСТЕМА ЗАЩИТЫ</h2>
            <div id="hack-content"></div>
        </div>
    </div>

    <div id="store-modal" class="modal">
        <div class="modal-content" style="text-align: center;">
            <div class="close-icon" onclick="closeModal('store-modal')">&times;</div>
            <h2 style="font-family:'Orbitron'">ДОСТУП К СЕТИ</h2>
            <div id="store-content"></div>
        </div>
    </div>

    <div id="notif-area"></div>

    <script>
        // --- МАТРИЦА ---
        const canvas = document.getElementById('hacker-bg');
        const ctx = canvas.getContext('2d');
        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;
        const cols = Math.floor(width / 20) + 1;
        const ypos = Array(cols).fill(0);
        window.addEventListener('resize', () => { width = canvas.width = window.innerWidth; height = canvas.height = window.innerHeight; });
        function drawMatrix() {
            ctx.fillStyle = '#0001'; ctx.fillRect(0, 0, width, height);
            ctx.fillStyle = '#0f0'; ctx.font = '15pt monospace';
            ypos.forEach((y, ind) => {
                const text = String.fromCharCode(Math.random() > 0.9 ? 0x30A0 + Math.random() * 96 : 0x30 + Math.random() * 10);
                const x = ind * 20; ctx.fillText(text, x, y);
                if (y > 100 + Math.random() * 10000) ypos[ind] = 0; else ypos[ind] = y + 20;
            });
        }
        setInterval(drawMatrix, 50);

        // --- ДАННЫЕ ---
        const db = {
            newbie: { 
                speech: [
                    {q:"Учитель вошел в класс. Твои действия?", a:["Встать и поздороваться.", "Сидеть молча."]}, 
                    {q:"Друг уронил пенал. Что сделаешь?", a:["Помогу собрать.", "Посмеюсь."]}, 
                    {q:"Надо спросить время у взрослого.", a:["«Извините, который час?»", "«Эй, время есть?»"]},
                    {q:"Толкнул кого-то случайно.", a:["«Извини, пожалуйста.»", "«Смотри куда прешь!»"]},
                    {q:"Бабушка дала невкусную конфету.", a:["«Спасибо за заботу!»", "«Фу, гадость.»"]}
                ],
                logic: [
                    {q:"2 яблока + 3 груши. Сколько фруктов?", a:["5 фруктов", "2 яблока"]}, 
                    {q:"Что легче: кг ваты или кг гвоздей?", a:["Одинаково", "Гвозди тяжелее"]}, 
                    {q:"У стола 4 угла. Один отпилили. Сколько стало?", a:["5 углов", "3 угла"]},
                    {q:"Сколько пальцев на 2 руках?", a:["10", "2"]},
                    {q:"Лед растаял. Чем он стал?", a:["Водой", "Паром"]}
                ],
                crit: [
                    {q:"Сайт пишет: «Ты выиграл iPhone!». Жмешь?", a:["Нет, это обман.", "Да, забираю!"]}, 
                    {q:"Незнакомец зовет в машину смотреть котят.", a:["Убегу и расскажу взрослым.", "Пойду смотреть."]}, 
                    {q:"Просят пароль от игры, чтобы «прокачать».", a:["Не дам пароль.", "Дам, пусть качает."]},
                    {q:"Нашел чужую карту на улице.", a:["Отдам родителям/полиции.", "Пойду в магазин."]},
                    {q:"Друг зовет гулять на стройку.", a:["Откажусь, там опасно.", "Пойду, я смелый."]}
                ]
            },
            agent: {
                speech: [
                    {q:"Одноклассник грустит. Что скажешь?", a:["«Могу я чем-то помочь?»", "«Чего ноешь?»"]}, 
                    {q:"Тебя ругают ни за что. Реакция?", a:["Спокойно объясню ситуацию.", "Начну кричать."]}, 
                    {q:"Новичок в классе никого не знает.", a:["Подойду познакомиться.", "Буду игнорировать."]},
                    {q:"Опоздал на встречу.", a:["«Простите за опоздание.»", "«Я же пришел, всё ок.»"]},
                    {q:"Мама устала после работы.", a:["Помогу по дому.", "Попрошу кушать."]}
                ],
                logic: [
                    {q:"Что идет после вторника?", a:["Среда", "Четверг"]}, 
                    {q:"Сколько месяцев имеют 28 дней?", a:["Все 12", "Один"]}, 
                    {q:"Сын моего отца, но не я.", a:["Мой брат", "Дядя"]},
                    {q:"Что можно разбить, не трогая?", a:["Слово (или обещание)", "Стакан"]},
                    {q:"Чем больше берешь, тем больше становится.", a:["Яма", "Куча"]}
                ],
                crit: [
                    {q:"Звонят с незнакомого номера и молчат.", a:["Положу трубку.", "Буду кричать «Алло»."]}, 
                    {q:"В интернете просят твое фото.", a:["Не отправлю.", "Отправлю красивое."]}, 
                    {q:"Нашел флешку на улице. Вставишь в ПК?", a:["Нет, там могут быть вирусы.", "Да, посмотрю что там."]},
                    {q:"Предлагают попробовать сигареты.", a:["Твердо откажусь.", "Попробую разок."]},
                    {q:"В чате оскорбляют друга.", a:["Поддержу друга.", "Присоединюсь к травле."]}
                ]
            },
            master: {
                speech: [
                    {q:"Выиграл в конкурсе. Что скажешь?", a:["«Спасибо всем за поддержку!»", "«Я круче всех!»"]}, 
                    {q:"Увидел, как старшие обижают малыша.", a:["Сообщу учителю/охране.", "Пройду мимо."]}, 
                    {q:"Друг рассказал секрет.", a:["Сохраню в тайне.", "Расскажу всем."]},
                    {q:"Нужно отказать в просьбе.", a:["«Извини, я не могу сейчас.»", "«Отстань.»"]},
                    {q:"Разбил вазу в гостях.", a:["Извинюсь и предложу помощь.", "Спрячу осколки."] }
                ],
                logic: [
                    {q:"Города без домов, реки без воды.", a:["Карта", "Сон"]}, 
                    {q:"Что не влезает даже в самую большую кастрюлю?", a:["Её крышка", "Суп"]}, 
                    {q:"Человек под дождем без зонта, но волосы сухие.", a:["Он лысый", "Дождь грибной"]},
                    {q:"Что принадлежит тебе, но другие пользуются чаще?", a:["Имя", "Деньги"]},
                    {q:"Можно ли принести воду в решете?", a:["Да, если она лед", "Нет, вытечет"]}
                ],
                crit: [
                    {q:"«Банк» просит код из СМС.", a:["Никому не скажу код.", "Скажу, это же банк."]}, 
                    {q:"Пришло письмо: «Скачай файл срочно».", a:["Удалю письмо.", "Скачаю."]}, 
                    {q:"В соцсети незнакомец притворяется другом.", a:["Проверю, задав личный вопрос.", "Поверю сразу."]},
                    {q:"Предлагают легкий заработок (закладки).", a:["Откажусь, это преступление.", "Соглашусь ради денег."]},
                    {q:"Твой аккаунт пытались взломать.", a:["Сменю пароль и включу 2FA.", "Ничего не сделаю."]}
                ]
            }
        };

        const ranks = {
            newbie: { name: "РЕКРУТ", theme: "theme-newbie", loc: "СИСТЕМНОЕ ЯДРО", desc: "Докажи, что ты готов к реальному миру.", cost: 1000, rewardQ: 100, rewardH: 500, next: "agent" },
            agent: { name: "АГЕНТ", theme: "theme-agent", loc: "ЦИФРОВАЯ СЕТЬ", desc: "Улицы полны опасностей. Будь начеку.", cost: 2500, rewardQ: 200, rewardH: 1000, next: "master" },
            master: { name: "МАСТЕР", theme: "theme-master", loc: "КРАСНАЯ ЗОНА", desc: "Финальный рубеж. Защити город.", cost: 5000, rewardQ: 300, rewardH: 2000, next: "omega" }
        };

        const stories = {
            start: "Внимание, курсант!<br><br>Ты прибыл в Академию Героев. Здесь мы готовим защитников будущего. Твоя первая задача — пройти базовую подготовку и доказать, что ты достоин носить это звание.<br><br>Удачи.",
            toAgent: "ДОСТУП ПОВЫШЕН<br><br>Ты отлично справился с теорией. Тебя переводят в Цифровую Сеть. Вокруг — потоки данных, вирусы и ловушки. Будь осторожен, Агент.",
            toMaster: "ВНИМАНИЕ!<br><br>Ты показал себя настоящим профессионалом. Город в опасности, и нам нужны лучшие из лучших. Ты получаешь ранг МАСТЕР. Твоя миссия — прямая защита граждан.",
            end: "МИССИЯ ВЫПОЛНЕНА.<br><br>Ты сделал это! Благодаря твоим ответам, логике и честности, система восстановлена. Жители могут спать спокойно.<br><br>Ты — настоящая Легенда."
        };

        const SAVE_KEY = 'heroSaveV17_FinalScore';
        let state = { 
            rank: 'newbie', 
            coins: 0, 
            prog: { speech: 0, logic: 0, crit: 0 }, 
            hackDone: false,
            mistakes: 0 
        };
        let tempNextRank = null;

        function startGame() {
            const save = localStorage.getItem(SAVE_KEY);
            document.getElementById('start-screen').style.display = 'none';

            if (save) {
                let savedState = JSON.parse(save);
                if (savedState.rank === 'omega') {
                    state = { rank: 'newbie', coins: 0, prog: { speech: 0, logic: 0, crit: 0 }, hackDone: false, mistakes: 0 };
                    showStory('start');
                } else {
                    state = savedState;
                    if(!state.mistakes) state.mistakes = 0; // Совместимость
                    initGameUI();
                }
            } else {
                showStory('start');
            }
        }

        function showStory(key) {
            const txt = stories[key];
            document.getElementById('story-text').innerHTML = txt;
            document.getElementById('story-screen').style.display = 'flex';
            if(key === 'end') {
                document.querySelector('#story-screen button').onclick = () => {
                     document.getElementById('story-screen').style.display = 'none';
                     showFinal();
                };
            }
        }

        function closeStory() {
            document.getElementById('story-screen').style.display = 'none';
            if (tempNextRank) {
                state.rank = tempNextRank;
                state.prog = { speech: 0, logic: 0, crit: 0 };
                state.hackDone = false;
                tempNextRank = null;
            }
            initGameUI();
            saveGame();
        }

        function initGameUI() { document.getElementById('game-ui').style.display = 'block'; updateUI(); }

        function updateUI() {
            const rData = ranks[state.rank];
            document.body.className = rData.theme;
            const root = document.documentElement;
            
            if(state.rank === 'newbie') root.style.setProperty('--accent', '#00f2ff'); 
            else if (state.rank === 'agent') root.style.setProperty('--accent', '#0f0');
            else if (state.rank === 'master') root.style.setProperty('--accent', '#ff003c');

            document.getElementById('rank-name').innerText = rData.name;
            document.getElementById('coins').innerText = state.coins;
            document.getElementById('location-name').innerText = rData.loc;
            document.getElementById('location-desc').innerText = rData.desc;

            ['speech', 'logic', 'crit'].forEach(type => {
                const pct = (state.prog[type] / 5) * 100;
                document.getElementById(`bar-${type}`).style.width = pct + '%';
            });

            const hCard = document.getElementById('card-hack');
            if (state.hackDone) {
                hCard.classList.remove('special'); hCard.classList.add('completed');
                document.getElementById('hack-title').innerText = "ВЗЛОМАНО";
                document.getElementById('hack-desc').innerText = "Доступ получен";
                document.getElementById('hack-desc').style.color = "var(--success)";
            } else {
                hCard.classList.add('special'); hCard.classList.remove('completed');
                document.getElementById('hack-title').innerText = "ВЗЛОМ";
                document.getElementById('hack-desc').innerText = `Награда: ${rData.rewardH} 🪙`;
                document.getElementById('hack-desc').style.color = "#ff88aa";
            }
            saveGame();
        }

        function openTask(type) {
            if (state.prog[type] >= 5) { notify("Максимум!", "success"); return; }
            const qData = db[state.rank][type][state.prog[type]];
            document.getElementById('quiz-text').innerText = qData.q;
            const ansDiv = document.getElementById('quiz-answers');
            ansDiv.innerHTML = '';
            let answers = [{ t: qData.a[0], ok: true }, { t: qData.a[1], ok: false }].sort(() => Math.random() - 0.5);
            answers.forEach(ans => {
                let btn = document.createElement('div');
                btn.className = 'ans-btn';
                btn.innerText = ans.t;
                btn.onclick = () => checkAnswer(btn, ans.ok, type);
                ansDiv.appendChild(btn);
            });
            openModal('quiz-modal');
        }

        function checkAnswer(btn, isCorrect, type) {
            document.querySelectorAll('.ans-btn').forEach(b => b.style.pointerEvents = 'none');
            if (isCorrect) {
                btn.classList.add('correct');
                state.coins += ranks[state.rank].rewardQ;
                state.prog[type]++;
                notify("ВЕРНО!", "success");
                setTimeout(() => { closeModal('quiz-modal'); updateUI(); }, 1000);
            } else {
                btn.classList.add('wrong');
                state.mistakes = (state.mistakes || 0) + 1; // Записываем ошибку
                notify("ОШИБКА (-1 балл)", "error");
                setTimeout(() => { closeModal('quiz-modal'); }, 1000);
            }
        }

        function openHack() {
            if(state.hackDone) return;
            let a, b, c, q, correct, options;
            if(state.rank === 'newbie') {
                a = rInt(10, 50); b = rInt(10, 50); correct = a + b; q = `${a} + ${b} = ?`;
                options = [correct, correct+5, correct-2];
            } else if (state.rank === 'agent') {
                a = rInt(3, 10); b = rInt(3, 10); correct = a * b; q = `${a} × ${b} = ?`;
                options = [correct, correct+2, correct-3];
            } else {
                a = rInt(5, 15); b = rInt(2, 5); c = rInt(10, 100); correct = a * b + c; q = `${a} × ${b} + ${c} = ?`;
                options = [correct, correct+10, correct-10];
            }
            options.sort(()=>Math.random()-0.5);
            let html = `<h1 style="color:var(--accent); font-family:'Share Tech Mono'; font-size:3rem; margin:20px 0;">${q}</h1><div style="display:flex; gap:10px; justify-content:center;">`;
            options.forEach(opt => html += `<button class="btn-main" onclick="solveHack(${opt}, ${correct})" style="font-size:1.2rem; min-width:80px;">${opt}</button>`);
            html += `</div>`;
            document.getElementById('hack-content').innerHTML = html;
            openModal('hack-modal');
        }

        function solveHack(val, correct) {
            if (val === correct) {
                state.hackDone = true;
                state.coins += ranks[state.rank].rewardH;
                notify("ВЗЛОМАНО!", "success");
                closeModal('hack-modal');
                updateUI();
            } else {
                notify("ОШИБКА", "error");
                closeModal('hack-modal');
            }
        }

        function checkRankUp() {
            const p = state.prog;
            if (p.speech < 5 || p.logic < 5 || p.crit < 5) { notify("Нужно прокачать все навыки!", "error"); return; }
            const cost = ranks[state.rank].cost;
            let html = `<p style="font-size:1.2rem; color:#aaa;">Требуется: <span style="color:var(--gold); font-weight:bold;">${cost} 🪙</span></p>`;
            if (state.coins >= cost) {
                html += `<button class="btn-main" onclick="buyRank('${ranks[state.rank].next}', ${cost})">ПОДКЛЮЧИТЬ</button>`;
            } else {
                html += `<button class="btn-main" style="background:#444; color:#888;">НЕДОСТАТОЧНО КРИПТЫ</button>`;
            }
            document.getElementById('store-content').innerHTML = html;
            openModal('store-modal');
        }

        function buyRank(nextKey, cost) {
            state.coins -= cost;
            closeModal('store-modal');
            tempNextRank = nextKey;
            document.getElementById('game-ui').style.display = 'none';
            if (nextKey === 'agent') showStory('toAgent');
            else if (nextKey === 'master') showStory('toMaster');
            else if (nextKey === 'omega') showStory('end');
        }

        function showFinal() {
            state.rank = 'omega';
            saveGame();
            document.getElementById('game-ui').style.display = 'none';
            document.getElementById('start-screen').style.display = 'none';
            
            // Расчет баллов
            const mistakes = state.mistakes || 0;
            let finalScore = 10 - mistakes;
            if (finalScore < 0) finalScore = 0;

            document.getElementById('res-mistakes').innerText = mistakes;
            document.getElementById('res-coins').innerText = state.coins + " 🪙";
            document.getElementById('res-score').innerText = finalScore + " / 10";

            document.getElementById('final-screen').style.display = 'flex';
        }

        function fullReset() {
            if(confirm("Сбросить систему и начать заново?")) {
                localStorage.removeItem(SAVE_KEY);
                location.reload();
            }
        }

        function notify(msg, type) {
            const n = document.createElement('div');
            n.className = `notif ${type}`; n.innerText = msg;
            document.getElementById('notif-area').appendChild(n);
            setTimeout(() => n.remove(), 3000);
        }

        function openModal(id) { document.getElementById(id).style.display = 'flex'; setTimeout(() => document.getElementById(id).classList.add('open'), 10); }
        function closeModal(id) { document.getElementById(id).classList.remove('open'); setTimeout(() => document.getElementById(id).style.display = 'none', 300); }
        function saveGame() { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); }
        function rInt(min, max) { return Math.floor(Math.random() * (max - min + 1) ) + min; }
    </script>
</body>
</html>
