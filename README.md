<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Блокчейн: интерактивное объяснение</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* ----- ФОН В СТИЛЕ ПРОГРАММИРОВАНИЯ (код/матрица) ----- */
        body {
            background: #0a0c10;
            font-family: 'Fira Code', 'JetBrains Mono', 'SF Mono', 'Inter', monospace;
            color: #e3e6ff;
            line-height: 1.5;
            padding: 2rem 1.5rem;
            position: relative;
            min-height: 100vh;
        }

        /* анимированный фон с "кодом" */
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                repeating-linear-gradient(0deg, rgba(0, 255, 65, 0.03) 0px, rgba(0, 255, 65, 0.03) 2px, transparent 2px, transparent 6px),
                repeating-linear-gradient(90deg, rgba(0, 255, 65, 0.02) 0px, rgba(0, 255, 65, 0.02) 1px, transparent 1px, transparent 8px);
            pointer-events: none;
            z-index: 0;
        }

        /* "шум" из символов */
        body::after {
            content: "console.log('blockchain ready'); >_ const hash = sha256(block); // immutable  ฿  </>";
            position: fixed;
            bottom: 20px;
            right: 20px;
            font-size: 0.7rem;
            color: #2a7f3e;
            background: #0a0c10aa;
            padding: 6px 12px;
            border-radius: 20px;
            font-family: monospace;
            backdrop-filter: blur(4px);
            pointer-events: none;
            z-index: 1;
            white-space: pre;
            opacity: 0.5;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            z-index: 2;
        }

        /* Заголовок главный */
        h1 {
            font-size: 2.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #c0ffb0, #6eff8e, #2bffaa);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.01em;
            margin-bottom: 2rem;
            border-left: 4px solid #2eff7a;
            padding-left: 1.2rem;
            font-family: 'Fira Code', monospace;
            text-shadow: 0 0 5px #1eff6e30;
        }

        /* ----- ОДИН БОЛЬШОЙ ПРЯМОУГОЛЬНИК (все блоки внутри) ----- */
        .big-terminal {
            background: rgba(8, 12, 18, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 2.5rem;
            border: 1px solid #2a7f3e80;
            box-shadow: 0 25px 40px -12px black, inset 0 1px 0 rgba(80, 255, 120, 0.2);
            padding: 2rem 2rem 2.2rem;
            transition: all 0.2s;
        }

        /* секции без номеров (убрали 01,02) */
        .section {
            margin-bottom: 3rem;
        }

        .section-title {
            font-size: 1.7rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            border-bottom: 2px solid #2a7f3e60;
            display: inline-block;
            padding-bottom: 0.3rem;
            letter-spacing: -0.3px;
        }

        /* карточки преимуществ (сетка) */
        .grid-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
            gap: 1.5rem;
            margin-top: 1rem;
        }

        .benefit-card {
            background: rgba(12, 18, 26, 0.8);
            backdrop-filter: blur(2px);
            border: 1px solid #2a7f3e70;
            border-radius: 1.5rem;
            padding: 1.5rem;
            transition: all 0.2s ease;
            font-family: 'Inter', 'Fira Code', monospace;
        }

        .benefit-card:hover {
            border-color: #4eff7a;
            background: rgba(20, 30, 28, 0.9);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(46, 255, 122, 0.1);
        }

        .benefit-icon {
            font-size: 2.2rem;
            margin-bottom: 0.75rem;
        }

        .benefit-card h3 {
            font-size: 1.4rem;
            font-weight: 600;
            margin-bottom: 0.65rem;
            color: #b9ffc4;
        }

        .benefit-card p {
            color: #b8c7e7;
            font-size: 0.9rem;
            line-height: 1.45;
        }

        /* интерактивный симулятор блокчейна */
        .simulator-wrapper {
            background: #050a0f70;
            border-radius: 1.8rem;
            border: 1px solid #2a7f3e60;
            padding: 1.5rem;
            margin-top: 0.5rem;
        }

        .chain-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1.8rem;
            margin: 1.8rem 0 1.5rem;
        }

        .block-card {
            background: #0c111b;
            border-radius: 1.2rem;
            border: 1px solid #2e7a4a;
            width: 290px;
            padding: 1rem;
            transition: 0.1s;
            box-shadow: 0 8px 16px rgba(0,0,0,0.5);
        }

        .block-card.invalid {
            border: 2px solid #ff5f7a;
            background: #1e121a;
            box-shadow: 0 0 0 2px #ff3b5c30;
        }

        .block-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            border-bottom: 1px dashed #3c6e4f;
            padding-bottom: 0.5rem;
            margin-bottom: 0.8rem;
        }

        .block-index {
            font-weight: 700;
            font-size: 1rem;
            background: #1c2a22;
            padding: 0.2rem 0.7rem;
            border-radius: 30px;
            font-family: monospace;
        }

        .block-badge {
            font-size: 0.65rem;
            background: #1f2e24;
            padding: 0.2rem 0.6rem;
            border-radius: 20px;
            color: #a2f5b0;
        }

        .data-field {
            margin: 0.8rem 0;
        }

        .data-field label {
            font-size: 0.7rem;
            letter-spacing: 0.5px;
            font-weight: 500;
            color: #88dd99;
            display: block;
            margin-bottom: 0.3rem;
        }

        .block-data-input {
            width: 100%;
            background: #010407;
            border: 1px solid #2a754a;
            border-radius: 1rem;
            padding: 0.6rem 0.8rem;
            color: #eef5ff;
            font-size: 0.8rem;
            font-family: 'Fira Code', monospace;
            resize: vertical;
        }

        .block-data-input:focus {
            outline: none;
            border-color: #6eff96;
            box-shadow: 0 0 0 2px #44ff7730;
        }

        .hash-row {
            background: #03070c;
            border-radius: 1rem;
            padding: 0.4rem 0.7rem;
            margin: 0.6rem 0;
            font-family: monospace;
            font-size: 0.7rem;
            word-break: break-all;
        }

        .hash-label {
            color: #7cae8a;
            font-size: 0.6rem;
        }

        .hash-value {
            color: #cafcd8;
            font-weight: 500;
        }

        .invalid-hash {
            color: #ff95aa;
        }

        .status-badge {
            display: inline-block;
            font-size: 0.7rem;
            padding: 0.2rem 0.6rem;
            border-radius: 20px;
            margin-top: 0.5rem;
            font-family: monospace;
        }

        .status-ok {
            background: #1a542a60;
            color: #a3f5b4;
            border: 1px solid #4bff7840;
        }

        .status-bad {
            background: #8b2c4440;
            color: #ffaabb;
            border: 1px solid #ff667b60;
        }

        .simulator-actions {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1rem;
            margin-top: 2rem;
        }

        button {
            background: #142015;
            border: 1px solid #3e9e5e;
            padding: 0.7rem 1.2rem;
            border-radius: 2rem;
            font-weight: 600;
            color: #ceffdb;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.85rem;
            font-family: monospace;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        button.primary {
            background: #1c612e;
            border-color: #8effb0;
            box-shadow: 0 2px 8px #2eff6e30;
        }

        button.primary:hover {
            background: #2f8244;
            transform: scale(0.98);
        }

        button:hover {
            background: #2c4534;
            border-color: #86ffa8;
        }

        .info-note {
            background: #0c151d;
            border-radius: 1.2rem;
            padding: 1rem 1.4rem;
            margin-top: 1.8rem;
            font-size: 0.85rem;
            border-left: 4px solid #5eff8c;
            color: #c6e6d0;
            font-family: monospace;
        }

        footer {
            text-align: center;
            font-size: 0.75rem;
            color: #5f8b6f;
            margin-top: 2rem;
            padding-top: 1rem;
            border-top: 1px solid #2a543a;
            font-family: monospace;
        }

        @media (max-width: 800px) {
            body {
                padding: 1rem;
            }
            h1 {
                font-size: 2rem;
            }
            .big-terminal {
                padding: 1.2rem;
            }
            .block-card {
                width: 100%;
                max-width: 340px;
            }
        }

        /* убираем стандартный подзаголовок — его нет в разметке */
    </style>
</head>
<body>
<div class="container">
    <h1>⚡ Блокчейн: цифровое доверие</h1>

    <!-- ЕДИНЫЙ БОЛЬШОЙ ПРЯМОУГОЛЬНИК (скруглённые углы) -->
    <div class="big-terminal">
        <!-- Блок "Зачем нужен блокчейн" (без цифр) -->
        <div class="section">
            <div class="section-title">🔗 Зачем нужен блокчейн?</div>
            <div class="grid-cards">
                <div class="benefit-card">
                    <div class="benefit-icon">🌍</div>
                    <h3>Децентрализация</h3>
                    <p>Нет единого сервера или банка. Данные хранятся у тысяч участников — никто не может отключить сеть или подделать историю в одиночку.</p>
                </div>
                <div class="benefit-card">
                    <div class="benefit-icon">⛓️</div>
                    <h3>Неизменяемость</h3>
                    <p>Любая запись остаётся навсегда. Чтобы изменить старый блок, нужно переписать все последующие — вычислительно невозможно в реальной сети.</p>
                </div>
                <div class="benefit-card">
                    <div class="benefit-icon">🔍</div>
                    <h3>Прозрачность</h3>
                    <p>Любой может проверить транзакции. Никакой скрытой бухгалтерии — доверие достигается через математику и открытый код.</p>
                </div>
                <div class="benefit-card">
                    <div class="benefit-icon">📜</div>
                    <h3>Смарт-контракты</h3>
                    <p>Программируемые соглашения, которые исполняются автоматически без посредников. Революция в логистике, финансах и праве.</p>
                </div>
            </div>
        </div>

        <!-- Блок "Как работает цепочка блоков?" (без цифр) -->
        <div class="section">
            <div class="section-title">⚙️ Как работает цепочка блоков?</div>
            <div class="simulator-wrapper">
                <p style="margin-bottom: 1rem; font-size: 0.9rem; font-family: monospace;">✨ <strong>Интерактивная модель:</strong> каждый блок содержит <strong>данные</strong> и <strong>хеш</strong>. Хеш следующего блока зависит от предыдущего. Измените текст — и цепочка станет красной!</p>
                
                <div id="blockchainContainer" class="chain-container">
                    <!-- блоки рендерятся динамически -->
                </div>

                <div class="simulator-actions">
                    <button id="resetChainBtn" class="primary">⟳ Сбросить цепочку (пример)</button>
                    <button id="validateChainBtn">🔎 Проверить целостность</button>
                    <button id="repairChainBtn">🔧 «Починить» цепочку</button>
                </div>

                <div class="info-note">
                    💡 <strong>Как это работает:</strong> Каждый блок хранит хеш предыдущего. Если изменить данные в блоке #2, его хеш меняется, но блок #3 всё ещё хранит старый prevHash → связь разрывается. В реальном блокчейне подмена одного блока потребовала бы пересчёта всех следующих — это требует колоссальной мощности (Proof-of-Work). 
                    <br><br>
                    🧪 <strong>Попробуйте:</strong> Редактируйте текст в любом блоке → хеш меняется, цепочка ломается. Кнопка «Починить» перестраивает всё подряд — в реальной сети это невозможно без контроля 51% мощности.
                </div>
            </div>
        </div>
    </div>
    <footer>
        // образовательный проект — принцип неразрывной цепочки хешей | редактируй, ломай, исследуй
    </footer>
</div>

<script>
    // ----- ХЕШ-ФУНКЦИЯ (простая, детерминированная) -----
    function simpleHash(str) {
        let hash = 0;
        for (let i = 0; i < str.length; i++) {
            const char = str.charCodeAt(i);
            hash = ((hash << 5) - hash) + char;
            hash |= 0;
        }
        const hex = (hash >>> 0).toString(16).padStart(8, '0');
        return hex.slice(0, 8);
    }

    function computeBlockHash(index, data, previousHash) {
        const payload = `${index}|${data}|${previousHash}`;
        return simpleHash(payload);
    }

    const DEFAULT_DATA = [
        "🏦 Алиса → Бобу: 10 BTC | транзакция #001",
        "🔄 Боб → Карлу: 5 BTC | оплата услуг",
        "🍕 Карл → Алисе: 2 BTC | пицца вечером"
    ];

    let blocks = [];

    function rebuildChainFromData() {
        if (!blocks.length) return;
        let prevHash = "0";
        for (let i = 0; i < blocks.length; i++) {
            const block = blocks[i];
            const newHash = computeBlockHash(block.index, block.data, prevHash);
            block.prevHash = prevHash;
            block.hash = newHash;
            prevHash = newHash;
        }
    }

    function resetToExample() {
        const newBlocks = [];
        let prev = "0";
        for (let i = 0; i < DEFAULT_DATA.length; i++) {
            const idx = i + 1;
            const dataStr = DEFAULT_DATA[i];
            const hashValue = computeBlockHash(idx, dataStr, prev);
            newBlocks.push({
                index: idx,
                data: dataStr,
                prevHash: prev,
                hash: hashValue,
            });
            prev = hashValue;
        }
        blocks = newBlocks;
        renderChain();
    }

    function validateChainIntegrity() {
        const validity = new Array(blocks.length).fill(false);
        if (!blocks.length) return validity;
        let expectedPrev = "0";
        for (let i = 0; i < blocks.length; i++) {
            const block = blocks[i];
            const recalculatedHash = computeBlockHash(block.index, block.data, expectedPrev);
            const isPrevValid = (block.prevHash === expectedPrev);
            const isHashValid = (block.hash === recalculatedHash);
            let valid = isPrevValid && isHashValid;
            // дополнительно проверим, что следующий блок (если есть) ссылается на текущий реальный хеш
            if (valid && i + 1 < blocks.length) {
                if (blocks[i+1].prevHash !== block.hash) {
                    valid = false;
                }
            }
            validity[i] = valid;
            expectedPrev = block.hash; // идём по фактическому хешу для след. проверки
        }
        // проверка первого блока на prevHash = "0"
        if (blocks[0] && blocks[0].prevHash !== "0") validity[0] = false;
        return validity;
    }

    function updateBlockData(blockIndex, newData) {
        const block = blocks.find(b => b.index === blockIndex);
        if (!block) return;
        block.data = newData;
        const recalculatedHash = computeBlockHash(block.index, block.data, block.prevHash);
        block.hash = recalculatedHash;
        renderChain();
    }

    function repairChain() {
        if (!blocks.length) return;
        let prev = "0";
        for (let i = 0; i < blocks.length; i++) {
            const block = blocks[i];
            block.prevHash = prev;
            const newHash = computeBlockHash(block.index, block.data, prev);
            block.hash = newHash;
            prev = newHash;
        }
        renderChain();
    }

    function escapeHtml(str) {
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    function renderChain() {
        const container = document.getElementById('blockchainContainer');
        if (!container) return;
        const validityArray = validateChainIntegrity();
        container.innerHTML = '';
        blocks.forEach((block, idx) => {
            const isValid = validityArray[idx] === true;
            const blockDiv = document.createElement('div');
            blockDiv.className = `block-card ${isValid ? 'valid' : 'invalid'}`;
            blockDiv.innerHTML = `
                <div class="block-header">
                    <span class="block-index">Блок #${block.index}</span>
                    <span class="block-badge">${isValid ? '✔ валиден' : '⚠ нарушен'}</span>
                </div>
                <div class="data-field">
                    <label>📦 ДАННЫЕ (измени текст)</label>
                    <textarea class="block-data-input" data-index="${block.index}" rows="2">${escapeHtml(block.data)}</textarea>
                </div>
                <div class="hash-row">
                    <div class="hash-label">🔗 PREV HASH</div>
                    <div class="hash-value">${block.prevHash}</div>
                </div>
                <div class="hash-row">
                    <div class="hash-label">🔐 ТЕКУЩИЙ HASH</div>
                    <div class="hash-value ${!isValid ? 'invalid-hash' : ''}">${block.hash}</div>
                </div>
                <div class="status-badge ${isValid ? 'status-ok' : 'status-bad'}">
                    ${isValid ? '✓ цепочка цела' : '✗ связь нарушена / хеш не совпадает'}
                </div>
            `;
            container.appendChild(blockDiv);
        });

        document.querySelectorAll('.block-data-input').forEach(textarea => {
            const blockIdx = parseInt(textarea.getAttribute('data-index'), 10);
            textarea.addEventListener('input', (e) => {
                updateBlockData(blockIdx, e.target.value);
            });
        });
    }

    function bindEvents() {
        const resetBtn = document.getElementById('resetChainBtn');
        if (resetBtn) resetBtn.addEventListener('click', () => resetToExample());
        const validateBtn = document.getElementById('validateChainBtn');
        if (validateBtn) validateBtn.addEventListener('click', () => renderChain());
        const repairBtn = document.getElementById('repairChainBtn');
        if (repairBtn) repairBtn.addEventListener('click', () => repairChain());
    }

    function init() {
        resetToExample();
        bindEvents();
    }
    init();
</script>
</body>
</html>
