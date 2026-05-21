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

        /* фон в стиле программирования */
        body {
            background: #0a0c10;
            font-family: 'Fira Code', 'JetBrains Mono', 'SF Mono', 'Inter', monospace;
            color: #e3e6ff;
            line-height: 1.5;
            padding: 2rem 1.5rem;
            position: relative;
            min-height: 100vh;
        }

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

        body::after {
            content: ">_ const blockchain = new Chain(); // immutable & transparent";
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
            max-width: 1500px;
            margin: 0 auto;
            position: relative;
            z-index: 2;
        }

        h1 {
            font-size: 2.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #c0ffb0, #6eff8e, #2bffaa);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.01em;
            margin-bottom: 1rem;
            border-left: 4px solid #2eff7a;
            padding-left: 1.2rem;
            font-family: 'Fira Code', monospace;
        }

        /* единый большой контейнер */
        .big-terminal {
            background: rgba(8, 12, 18, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 2.5rem;
            border: 1px solid #2a7f3e80;
            box-shadow: 0 25px 40px -12px black, inset 0 1px 0 rgba(80, 255, 120, 0.2);
            padding: 2rem;
        }

        /* ВСТУПЛЕНИЕ - ЧТО ТАКОЕ БЛОКЧЕЙН */
        .what-is-blockchain {
            background: rgba(10, 20, 15, 0.6);
            border-radius: 1.5rem;
            padding: 1.2rem 1.5rem;
            margin-bottom: 2rem;
            border-left: 5px solid #5eff8c;
            font-family: 'Inter', monospace;
        }
        .what-is-blockchain h2 {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
            color: #b5ffc8;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        .what-is-blockchain p {
            color: #cfecda;
            font-size: 0.95rem;
            line-height: 1.5;
            margin-bottom: 0.5rem;
        }
        .what-is-blockchain .highlight {
            color: #9effb0;
            font-weight: 500;
        }

        /* ---------- ВЕРХНЯЯ ЧАСТЬ: ВЕРТИКАЛЬНЫЕ БЛОКИ + ОПИСАНИЕ ---------- */
        .features-layout {
            display: grid;
            grid-template-columns: 280px 1fr;
            gap: 2rem;
            margin-bottom: 3rem;
            border-bottom: 1px solid #2a7f3e60;
            padding-bottom: 2rem;
        }

        /* вертикальные карточки (колонка) */
        .features-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .feature-item {
            background: rgba(12, 20, 16, 0.7);
            border: 1px solid #2a7f3e70;
            border-radius: 1.5rem;
            padding: 1rem 1.2rem;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }

        .feature-item:hover {
            border-color: #6eff96;
            background: #1e2a24;
            transform: translateX(4px);
        }

        .feature-item.active {
            border-color: #5eff8c;
            background: #1f3a2c;
            box-shadow: 0 0 10px #2eff7a30;
        }

        .feature-icon {
            font-size: 2rem;
        }

        .feature-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: #d0ffd8;
        }

        /* панель описания справа */
        .feature-description-panel {
            background: #0f151f80;
            border-radius: 1.8rem;
            border: 1px solid #3d8b5e80;
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .desc-title {
            font-size: 1.8rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #aaffc3;
        }

        .desc-text {
            font-size: 1rem;
            line-height: 1.5;
            color: #cae6d4;
        }

        /* ---------- ИНТЕРАКТИВНАЯ МОДЕЛЬ (БЛОКЧЕЙН С ТРАНЗАКЦИЯМИ) ---------- */
        .section-title {
            font-size: 1.7rem;
            font-weight: 600;
            margin: 1rem 0 1.5rem;
            border-bottom: 2px solid #2a7f3e60;
            display: inline-block;
            padding-bottom: 0.3rem;
        }

        .chain-controls {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            margin-bottom: 1.5rem;
        }

        .chain-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-start;
            gap: 1.5rem;
            margin: 1.5rem 0;
        }

        .block-card {
            background: #0c111b;
            border-radius: 1.2rem;
            border: 1px solid #2e7a4a;
            width: 320px;
            padding: 1rem;
            transition: 0.1s;
            box-shadow: 0 8px 16px rgba(0,0,0,0.5);
        }

        .block-card.invalid {
            border: 2px solid #ff5f7a;
            background: #1e121a;
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
            font-size: 0.9rem;
            background: #1c2a22;
            padding: 0.2rem 0.7rem;
            border-radius: 30px;
        }

        .block-badge {
            font-size: 0.65rem;
            background: #1f2e24;
            padding: 0.2rem 0.6rem;
            border-radius: 20px;
            color: #a2f5b0;
        }

        .tx-field {
            margin: 0.5rem 0;
        }

        .tx-field label {
            font-size: 0.7rem;
            color: #88dd99;
            display: block;
        }

        .tx-input, .tx-select {
            width: 100%;
            background: #010407;
            border: 1px solid #2a754a;
            border-radius: 0.8rem;
            padding: 0.4rem 0.6rem;
            color: #eef5ff;
            font-size: 0.8rem;
            font-family: monospace;
        }

        .balance-row {
            background: #03070c;
            border-radius: 0.8rem;
            padding: 0.3rem 0.6rem;
            margin: 0.5rem 0;
            font-size: 0.7rem;
            font-family: monospace;
            color: #9dffb5;
        }

        .hash-row {
            background: #03070c;
            border-radius: 0.8rem;
            padding: 0.3rem 0.6rem;
            margin: 0.5rem 0;
            font-size: 0.65rem;
            word-break: break-all;
        }

        .status-badge {
            display: inline-block;
            font-size: 0.7rem;
            padding: 0.2rem 0.5rem;
            border-radius: 20px;
            margin-top: 0.3rem;
        }

        .status-ok {
            background: #1a542a60;
            color: #a3f5b4;
        }

        .status-bad {
            background: #8b2c4440;
            color: #ffaabb;
        }

        .simulator-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin: 1.5rem 0;
        }

        button {
            background: #142015;
            border: 1px solid #3e9e5e;
            padding: 0.6rem 1.2rem;
            border-radius: 2rem;
            font-weight: 600;
            color: #ceffdb;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.8rem;
            font-family: monospace;
        }

        button.primary {
            background: #1c612e;
            border-color: #8effb0;
        }

        button.danger {
            border-color: #ff7f8f;
            color: #ffb7c2;
        }

        button:hover {
            background: #2c4534;
            transform: scale(0.98);
        }

        .add-tx-form {
            background: #0b111c;
            border-radius: 1.5rem;
            padding: 1rem;
            margin-top: 1rem;
            display: flex;
            flex-wrap: wrap;
            gap: 0.8rem;
            align-items: flex-end;
        }

        .add-tx-form .field-group {
            flex: 1;
            min-width: 120px;
        }

        .info-note {
            background: #0c151d;
            border-radius: 1.2rem;
            padding: 0.8rem 1.2rem;
            margin-top: 1.5rem;
            font-size: 0.8rem;
            border-left: 4px solid #5eff8c;
        }

        footer {
            text-align: center;
            font-size: 0.7rem;
            color: #5f8b6f;
            margin-top: 2rem;
        }

        @media (max-width: 850px) {
            .features-layout {
                grid-template-columns: 1fr;
            }
            .block-card {
                width: 100%;
            }
        }
    </style>
</head>
<body>
<div class="container">
    <h1>⚡ Блокчейн: цифровое доверие</h1>
    <div class="big-terminal">
        <!-- НОВЫЙ БЛОК: что такое блокчейн и зачем он нужен -->
        <div class="what-is-blockchain">
            <h2>📦 Что такое блокчейн?</h2>
            <p><span class="highlight">Блокчейн</span> — это распределённая база данных, где информация хранится в виде непрерывной цепочки блоков. Каждый блок содержит набор записей (транзакций) и <span class="highlight">криптографическую ссылку</span> на предыдущий блок.</p>
            <p>🔐 <strong>Главная задача блокчейна</strong> — обеспечить <span class="highlight">доверие без посредников</span>. Благодаря децентрализации и неизменяемости, блокчейн позволяет фиксировать любые ценности (деньги, контракты, активы) так, что их нельзя подделать или удалить. Вы можете проверить всю историю, но не можете изменить прошлое.</p>
            <p>🌐 Блокчейн лежит в основе криптовалют (биткоин, эфир), смарт-контрактов, цифровых удостоверений, логистики и многих других сфер, где важна <span class="highlight">прозрачность и защита от мошенничества</span>.</p>
        </div>

        <!-- Верхняя часть: вертикальные блоки + описание (4 ключевых свойства) -->
        <div class="features-layout">
            <div class="features-list" id="featuresList">
                <div class="feature-item" data-idx="0">
                    <div class="feature-icon">🌍</div>
                    <div class="feature-title">Децентрализация</div>
                </div>
                <div class="feature-item" data-idx="1">
                    <div class="feature-icon">⛓️</div>
                    <div class="feature-title">Неизменяемость</div>
                </div>
                <div class="feature-item" data-idx="2">
                    <div class="feature-icon">🔍</div>
                    <div class="feature-title">Прозрачность</div>
                </div>
                <div class="feature-item" data-idx="3">
                    <div class="feature-icon">📜</div>
                    <div class="feature-title">Смарт-контракты</div>
                </div>
            </div>
            <div class="feature-description-panel" id="featureDescPanel">
                <div class="desc-title">Децентрализация</div>
                <div class="desc-text">Нет единого сервера или банка. Данные хранятся у тысяч участников — никто не может отключить сеть или подделать историю в одиночку.</div>
            </div>
        </div>

        <!-- ИНТЕРАКТИВНАЯ МОДЕЛЬ БЛОКЧЕЙНА С ТРАНЗАКЦИЯМИ -->
        <div>
            <div class="section-title">🔗 Интерактивная цепочка блоков (транзакции)</div>
            <div class="chain-controls">
                <button id="addBlockBtn" class="primary">➕ Добавить блок (транзакцию)</button>
                <button id="removeLastBlockBtn" class="danger">🗑️ Удалить последний блок</button>
                <button id="repairChainBtn">🔧 Перестроить хеши (починить)</button>
                <button id="validateBtn">🔍 Проверить целостность</button>
            </div>
            <div id="blockchainContainer" class="chain-container"></div>

            <!-- форма для добавления новой транзакции -->
            <div class="add-tx-form" id="addTxForm">
                <div class="field-group">
                    <label>От кого</label>
                    <input type="text" id="txFrom" class="tx-input" placeholder="Alice" value="Alice">
                </div>
                <div class="field-group">
                    <label>Кому</label>
                    <input type="text" id="txTo" class="tx-input" placeholder="Bob" value="Bob">
                </div>
                <div class="field-group">
                    <label>Сумма</label>
                    <input type="number" id="txAmount" class="tx-input" value="5">
                </div>
                <div class="field-group">
                    <label>Описание</label>
                    <input type="text" id="txDesc" class="tx-input" placeholder="оплата" value="перевод">
                </div>
                <button id="confirmAddBtn" class="primary">✔ Добавить</button>
            </div>

            <div class="info-note">
                💡 <strong>Как это работает:</strong> Каждый блок содержит транзакцию (от кого, кому, сумма) и хеш. Первый блок (Genesis) имеет баланс 50 у Alice, его нельзя удалить. <br>
                ✏️ <strong>Редактируйте поля</strong> — хеш меняется, цепочка ломается. Нажмите «Починить» — перестроите хеши (в реальном блокчейне это требует колоссальной мощности). <br>
                ➕ <strong>Добавляйте/удаляйте блоки</strong> — балансы пересчитываются автоматически.
            </div>
        </div>
    </div>
    <footer>// образовательный проект — неизменяемость, прозрачность, децентрализация | редактируй, исследуй</footer>
</div>

<script>
    // ----- ХЕШ-ФУНКЦИЯ -----
    function simpleHash(str) {
        let hash = 0;
        for (let i = 0; i < str.length; i++) {
            hash = ((hash << 5) - hash) + str.charCodeAt(i);
            hash |= 0;
        }
        return (hash >>> 0).toString(16).padStart(8, '0').slice(0, 8);
    }

    function computeBlockHash(index, from, to, amount, description, prevHash) {
        const payload = `${index}|${from}|${to}|${amount}|${description}|${prevHash}`;
        return simpleHash(payload);
    }

    // ---------- УПРАВЛЕНИЕ БЛОКАМИ И БАЛАНСАМИ ----------
    let blocks = []; 

    function recomputeBalancesAndChain() {
        if (!blocks.length) return;
        let balances = new Map();
        const genesis = blocks[0];
        if (genesis.from === "Genesis" && genesis.to === "Alice") {
            balances.set("Alice", genesis.amount);
        } else {
            balances.set("Alice", 0);
        }
        for (let i = 1; i < blocks.length; i++) {
            const b = blocks[i];
            const fromBalance = balances.get(b.from) || 0;
            const newFromBalance = fromBalance - b.amount;
            const toBalance = balances.get(b.to) || 0;
            balances.set(b.from, newFromBalance);
            balances.set(b.to, toBalance + b.amount);
            b.senderBalanceAfter = newFromBalance;
        }
        blocks[0].senderBalanceAfter = balances.get("Alice");
    }

    function rebuildChainHashes() {
        let prevHash = "0";
        for (let i = 0; i < blocks.length; i++) {
            const b = blocks[i];
            b.prevHash = prevHash;
            b.hash = computeBlockHash(b.index, b.from, b.to, b.amount, b.description, prevHash);
            prevHash = b.hash;
        }
        recomputeBalancesAndChain();
        renderChain();
    }

    function initGenesis() {
        const genesis = {
            index: 1,
            from: "Genesis",
            to: "Alice",
            amount: 50,
            description: "Начальный баланс (нельзя удалить)",
            prevHash: "0",
            hash: "",
            senderBalanceAfter: 50
        };
        genesis.hash = computeBlockHash(1, genesis.from, genesis.to, genesis.amount, genesis.description, "0");
        blocks = [genesis];
        rebuildChainHashes();
    }

    function addTransaction(from, to, amount, description) {
        const newIndex = blocks.length + 1;
        const prevBlock = blocks[blocks.length - 1];
        const newBlock = {
            index: newIndex,
            from: from,
            to: to,
            amount: Number(amount),
            description: description,
            prevHash: prevBlock.hash,
            hash: "",
            senderBalanceAfter: 0
        };
        newBlock.hash = computeBlockHash(newIndex, from, to, Number(amount), description, prevBlock.hash);
        blocks.push(newBlock);
        rebuildChainHashes();
        renderChain();
    }

    function removeLastBlock() {
        if (blocks.length <= 1) {
            alert("Первый блок (Genesis) нельзя удалить — он содержит начальный баланс 50");
            return;
        }
        blocks.pop();
        rebuildChainHashes();
        renderChain();
    }

    function updateBlock(blockIdx, newFrom, newTo, newAmount, newDesc) {
        const block = blocks.find(b => b.index === blockIdx);
        if (!block) return;
        if (block.index === 1 && (newFrom !== "Genesis" || newTo !== "Alice")) {
            alert("Первый блок (Genesis) нельзя изменять, он фиксирует начальный баланс 50.");
            renderChain();
            return;
        }
        block.from = newFrom;
        block.to = newTo;
        block.amount = Number(newAmount);
        block.description = newDesc;
        block.hash = computeBlockHash(block.index, block.from, block.to, block.amount, block.description, block.prevHash);
        recomputeBalancesAndChain();
        renderChain();
    }

    function validateChain() {
        let expectedPrev = "0";
        for (let i = 0; i < blocks.length; i++) {
            const b = blocks[i];
            const recalcHash = computeBlockHash(b.index, b.from, b.to, b.amount, b.description, expectedPrev);
            if (b.prevHash !== expectedPrev || b.hash !== recalcHash) {
                return false;
            }
            expectedPrev = b.hash;
        }
        return true;
    }

    function getBlockValidityArray() {
        const validity = [];
        let expectedPrev = "0";
        for (let i = 0; i < blocks.length; i++) {
            const b = blocks[i];
            const recalcHash = computeBlockHash(b.index, b.from, b.to, b.amount, b.description, expectedPrev);
            const ok = (b.prevHash === expectedPrev && b.hash === recalcHash);
            validity.push(ok);
            expectedPrev = b.hash;
        }
        return validity;
    }

    function renderChain() {
        const container = document.getElementById('blockchainContainer');
        if (!container) return;
        const validity = getBlockValidityArray();
        container.innerHTML = '';
        blocks.forEach(block => {
            const idx = block.index;
            const isValid = validity[idx-1];
            const blockDiv = document.createElement('div');
            blockDiv.className = `block-card ${isValid ? 'valid' : 'invalid'}`;
            blockDiv.innerHTML = `
                <div class="block-header">
                    <span class="block-index">Блок #${block.index}</span>
                    <span class="block-badge">${isValid ? '✔ валиден' : '⚠ нарушен'}</span>
                </div>
                <div class="tx-field"><label>📤 От кого</label>
                    <input type="text" class="tx-input edit-from" value="${escapeHtml(block.from)}" data-block="${block.index}">
                </div>
                <div class="tx-field"><label>📥 Кому</label>
                    <input type="text" class="tx-input edit-to" value="${escapeHtml(block.to)}" data-block="${block.index}">
                </div>
                <div class="tx-field"><label>💰 Сумма</label>
                    <input type="number" class="tx-input edit-amount" value="${block.amount}" data-block="${block.index}">
                </div>
                <div class="tx-field"><label>📝 Описание</label>
                    <input type="text" class="tx-input edit-desc" value="${escapeHtml(block.description)}" data-block="${block.index}">
                </div>
                <div class="balance-row">💎 Баланс отправителя после транзакции: <strong>${block.senderBalanceAfter ?? '—'}</strong></div>
                <div class="hash-row">🔗 prevHash: ${block.prevHash}</div>
                <div class="hash-row">🔐 hash: ${block.hash}</div>
                <div class="status-badge ${isValid ? 'status-ok' : 'status-bad'}">
                    ${isValid ? '✓ связь цела' : '✗ цепочка сломана (измените хеши)'}
                </div>
            `;
            container.appendChild(blockDiv);
        });

        document.querySelectorAll('.edit-from').forEach(inp => {
            const bid = parseInt(inp.dataset.block);
            inp.addEventListener('change', (e) => updateBlock(bid, e.target.value, getFieldValue(bid, 'to'), getFieldValue(bid, 'amount'), getFieldValue(bid, 'desc')));
        });
        document.querySelectorAll('.edit-to').forEach(inp => {
            const bid = parseInt(inp.dataset.block);
            inp.addEventListener('change', (e) => updateBlock(bid, getFieldValue(bid, 'from'), e.target.value, getFieldValue(bid, 'amount'), getFieldValue(bid, 'desc')));
        });
        document.querySelectorAll('.edit-amount').forEach(inp => {
            const bid = parseInt(inp.dataset.block);
            inp.addEventListener('change', (e) => updateBlock(bid, getFieldValue(bid, 'from'), getFieldValue(bid, 'to'), parseFloat(e.target.value), getFieldValue(bid, 'desc')));
        });
        document.querySelectorAll('.edit-desc').forEach(inp => {
            const bid = parseInt(inp.dataset.block);
            inp.addEventListener('change', (e) => updateBlock(bid, getFieldValue(bid, 'from'), getFieldValue(bid, 'to'), getFieldValue(bid, 'amount'), e.target.value));
        });
    }

    function getFieldValue(blockIdx, field) {
        const block = blocks.find(b => b.index === blockIdx);
        if (!block) return '';
        if (field === 'from') return block.from;
        if (field === 'to') return block.to;
        if (field === 'amount') return block.amount;
        if (field === 'desc') return block.description;
        return '';
    }

    function escapeHtml(str) {
        return String(str).replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    // ----- УПРАВЛЕНИЕ ВЕРХНИМИ КАРТОЧКАМИ (описание) -----
    const featuresData = [
        { title: "Децентрализация", desc: "Нет единого сервера или банка. Данные хранятся у тысяч участников — никто не может отключить сеть или подделать историю в одиночку." },
        { title: "Неизменяемость", desc: "Любая запись остаётся навсегда. Чтобы изменить старый блок, нужно переписать все последующие — вычислительно невозможно в реальной сети." },
        { title: "Прозрачность", desc: "Любой может проверить транзакции. Никакой скрытой бухгалтерии — доверие достигается через математику и открытый код." },
        { title: "Смарт-контракты", desc: "Программируемые соглашения, которые исполняются автоматически без посредников. Революция в логистике, финансах и праве." }
    ];

    function initFeatures() {
        const items = document.querySelectorAll('.feature-item');
        const panel = document.getElementById('featureDescPanel');
        function setActive(index) {
            items.forEach((item, i) => {
                if (i === index) item.classList.add('active');
                else item.classList.remove('active');
            });
            panel.innerHTML = `<div class="desc-title">${featuresData[index].title}</div><div class="desc-text">${featuresData[index].desc}</div>`;
        }
        items.forEach((item, idx) => {
            item.addEventListener('click', () => setActive(idx));
        });
        setActive(0);
    }

    function bindChainEvents() {
        document.getElementById('addBlockBtn').addEventListener('click', () => {
            document.getElementById('confirmAddBtn').click();
        });
        document.getElementById('confirmAddBtn').addEventListener('click', () => {
            const from = document.getElementById('txFrom').value.trim();
            const to = document.getElementById('txTo').value.trim();
            const amount = parseFloat(document.getElementById('txAmount').value);
            const desc = document.getElementById('txDesc').value.trim();
            if (!from || !to || isNaN(amount) || amount <= 0) {
                alert("Заполните все поля корректно (сумма > 0)");
                return;
            }
            addTransaction(from, to, amount, desc);
        });
        document.getElementById('removeLastBlockBtn').addEventListener('click', removeLastBlock);
        document.getElementById('repairChainBtn').addEventListener('click', () => {
            rebuildChainHashes();
        });
        document.getElementById('validateBtn').addEventListener('click', () => {
            const valid = validateChain();
            alert(valid ? "✅ Цепочка валидна! Все хеши и связи корректны." : "❌ Цепочка нарушена! Измените данные или нажмите «Починить».");
            renderChain();
        });
    }

    // старт
    initGenesis();
    bindChainEvents();
    initFeatures();
    renderChain();
</script>
</body>
</html>
