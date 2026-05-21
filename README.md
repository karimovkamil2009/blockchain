<!DOCTYPE html>
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

        body {
            background: linear-gradient(145deg, #0a0c12 0%, #0f1119 100%);
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            color: #eef2ff;
            line-height: 1.5;
            padding: 2rem 1.5rem;
        }

        /* гладкий скролл */
        html {
            scroll-behavior: smooth;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* заголовки */
        h1 {
            font-size: 3rem;
            font-weight: 800;
            background: linear-gradient(135deg, #ffffff, #a5f0ff, #6c63ff);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.02em;
            margin-bottom: 1rem;
        }

        .subhead {
            font-size: 1.25rem;
            color: #9ca3cf;
            max-width: 680px;
            margin-bottom: 3rem;
            border-left: 3px solid #4f46e5;
            padding-left: 1.25rem;
        }

        /* секции */
        section {
            margin-bottom: 4rem;
        }

        .section-title {
            font-size: 1.8rem;
            font-weight: 600;
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        .section-title span {
            background: #1e1f2c;
            padding: 0.2rem 0.7rem;
            border-radius: 40px;
            font-size: 1rem;
            font-weight: 500;
            color: #a5b4fc;
        }

        /* карточки преимуществ */
        .grid-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
            gap: 1.5rem;
            margin-top: 1rem;
        }

        .benefit-card {
            background: rgba(20, 22, 36, 0.7);
            backdrop-filter: blur(2px);
            border: 1px solid rgba(79, 70, 229, 0.3);
            border-radius: 2rem;
            padding: 1.8rem 1.5rem;
            transition: all 0.2s ease;
        }

        .benefit-card:hover {
            border-color: #4f46e5;
            background: rgba(30, 32, 50, 0.8);
            transform: translateY(-4px);
        }

        .benefit-icon {
            font-size: 2.4rem;
            margin-bottom: 1rem;
        }

        .benefit-card h3 {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 0.75rem;
        }

        .benefit-card p {
            color: #b9c0e3;
            font-size: 0.95rem;
        }

        /* интерактивный блокчейн симулятор */
        .simulator-wrapper {
            background: #0b0d14;
            border-radius: 2rem;
            border: 1px solid #282c3a;
            padding: 1.8rem;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.5);
        }

        .chain-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1.8rem;
            margin: 2rem 0 1.5rem;
        }

        .block-card {
            background: #11131f;
            border-radius: 1.5rem;
            border: 1px solid #2a2e42;
            width: 280px;
            padding: 1rem 1.2rem 1.2rem;
            transition: all 0.2s;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        .block-card.invalid {
            border: 2px solid #f43f5e;
            background: #1f1420;
            box-shadow: 0 0 0 2px rgba(244, 63, 94, 0.3);
        }

        .block-card.valid {
            border: 1px solid #2a2e42;
        }

        .block-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            border-bottom: 1px dashed #2c2f42;
            padding-bottom: 0.5rem;
            margin-bottom: 1rem;
        }

        .block-index {
            font-weight: 700;
            font-size: 1.2rem;
            background: #1e2132;
            padding: 0.2rem 0.7rem;
            border-radius: 30px;
        }

        .block-badge {
            font-size: 0.7rem;
            background: #2d2f42;
            padding: 0.2rem 0.6rem;
            border-radius: 20px;
            color: #a5b4fc;
        }

        .data-field {
            margin: 0.8rem 0;
        }

        .data-field label {
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 500;
            color: #8b92b5;
            display: block;
            margin-bottom: 0.4rem;
        }

        .block-data-input {
            width: 100%;
            background: #080a12;
            border: 1px solid #2d304a;
            border-radius: 1rem;
            padding: 0.6rem 0.8rem;
            color: #f0f3ff;
            font-size: 0.85rem;
            font-family: monospace;
            resize: vertical;
        }

        .block-data-input:focus {
            outline: none;
            border-color: #6c63ff;
        }

        .hash-row {
            background: #07090f;
            border-radius: 1rem;
            padding: 0.5rem 0.7rem;
            margin: 0.6rem 0;
            font-family: monospace;
            font-size: 0.7rem;
            word-break: break-all;
        }

        .hash-label {
            color: #7c82a1;
            font-size: 0.65rem;
        }

        .hash-value {
            color: #bbd9ff;
            font-weight: 500;
        }

        .invalid-hash {
            color: #f97386;
        }

        .status-badge {
            display: inline-block;
            font-size: 0.7rem;
            padding: 0.2rem 0.5rem;
            border-radius: 20px;
            margin-top: 0.5rem;
        }

        .status-ok {
            background: #15803d30;
            color: #4ade80;
            border: 1px solid #22c55e30;
        }

        .status-bad {
            background: #be123c30;
            color: #fb7185;
            border: 1px solid #f43f5e50;
        }

        .simulator-actions {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 1rem;
            margin-top: 2rem;
        }

        button {
            background: #252a41;
            border: none;
            padding: 0.7rem 1.2rem;
            border-radius: 2rem;
            font-weight: 600;
            color: #e2e8ff;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.85rem;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        button.primary {
            background: #4f46e5;
            box-shadow: 0 2px 8px #4f46e550;
        }

        button.primary:hover {
            background: #6366f1;
            transform: scale(0.98);
        }

        button:hover {
            background: #363c5e;
        }

        .info-note {
            background: #131825;
            border-radius: 1.2rem;
            padding: 1rem 1.4rem;
            margin-top: 1.8rem;
            font-size: 0.85rem;
            border-left: 4px solid #4f46e5;
            color: #cbd5ff;
        }

        footer {
            text-align: center;
            font-size: 0.8rem;
            color: #5b638b;
            margin-top: 4rem;
            padding-top: 2rem;
            border-top: 1px solid #202333;
        }

        @media (max-width: 780px) {
            body {
                padding: 1rem;
            }
            h1 {
                font-size: 2.2rem;
            }
            .block-card {
                width: 100%;
                max-width: 320px;
            }
        }
    </style>
</head>
<body>
<div class="container">
    <h1>⚡ Блокчейн: цифровое доверие</h1>
    <div class="subhead">
        Интерактивное объяснение, зачем нужен блокчейн и как неразрывная цепочка блоков защищает данные.
        Редактируйте информацию — и увидите, как ломается связь!
    </div>

    <!-- Зачем нужен блокчейн: преимущества -->
    <section>
        <div class="section-title">
            <span>01</span> Зачем нужен блокчейн?
        </div>
        <div class="grid-cards">
            <div class="benefit-card">
                <div class="benefit-icon">🔗</div>
                <h3>Децентрализация</h3>
                <p>Нет единого сервера или банка. Данные хранятся у тысяч участников — никто не может отключить сеть или подделать историю в одиночку.</p>
            </div>
            <div class="benefit-card">
                <div class="benefit-icon">🛡️</div>
                <h3>Неизменяемость</h3>
                <p>Любая запись остаётся навсегда. Чтобы изменить старый блок, нужно переписать все последующие — вычислительно невозможно в реальной сети.</p>
            </div>
            <div class="benefit-card">
                <div class="benefit-icon">👁️</div>
                <h3>Прозрачность</h3>
                <p>Любой может проверить транзакции. Никакой скрытой бухгалтерии — доверие достигается через математику и открытый код.</p>
            </div>
            <div class="benefit-card">
                <div class="benefit-icon">⚙️</div>
                <h3>Смарт-контракты</h3>
                <p>Программируемые соглашения, которые исполняются автоматически без посредников. Революция в логистике, финансах и праве.</p>
            </div>
        </div>
    </section>

    <!-- ИНТЕРАКТИВ: как работает блокчейн -->
    <section>
        <div class="section-title">
            <span>02</span> Как работает цепочка блоков?
        </div>
        <div class="simulator-wrapper">
            <p style="margin-bottom: 1rem; font-weight: 500;">✨ <strong>Интерактивная модель:</strong> каждый блок содержит <strong>данные</strong> (например, переводы) и <strong>уникальный хеш</strong>. Хеш следующего блока зависит от хеша предыдущего (prevHash). Попробуйте изменить любую запись — цепочка станет красной!</p>
            
            <div id="blockchainContainer" class="chain-container">
                <!-- сюда динамически рендерятся блоки из JS -->
            </div>

            <div class="simulator-actions">
                <button id="resetChainBtn" class="primary">🔄 Сбросить цепочку (пример)</button>
                <button id="validateChainBtn">🔍 Проверить целостность</button>
                <button id="repairChainBtn">🔧 «Починить» цепочку (перестроить хеши)</button>
            </div>

            <div class="info-note">
                💡 <strong>Как это работает:</strong> Каждый блок хранит хеш предыдущего. Если изменить данные в блоке №2, его хеш изменится, но блок №3 всё ещё хранит старый «prevHash». Цепочка разрывается — система сигнализирует о подделке. В реальном блокчейне для подмены нужно пересчитать хеши всех следующих блоков, а это требует колоссальной вычислительной мощности (Proof-of-Work). 
                <br><br>
                🧪 <strong>Интерактив:</strong> Редактируйте текст в любом блоке → поле ввода автоматически пересчитает хеш и проверит валидность. Нажмите «Починить цепочку» — это как если бы злоумышленник перезаписал всё подряд, но в реальной сети это невозможно.
            </div>
        </div>
    </section>
    <footer>
        🔬 Образовательный проект — принцип связки хешей и неизменяемости блокчейна наглядно.
    </footer>
</div>

<script>
    // ------------------- ПРОСТАЯ, НО НАГЛЯДНАЯ ХЕШ-ФУНКЦИЯ -------------------
    // Эмуляция хеша: превращаем строку в детерминированный короткий hex
    function simpleHash(str) {
        let hash = 0;
        for (let i = 0; i < str.length; i++) {
            const char = str.charCodeAt(i);
            hash = ((hash << 5) - hash) + char;
            hash |= 0; // 32-bit integer
        }
        // переводим в беззнаковый hex и обрезаем до 8 символов для читаемости
        const hex = (hash >>> 0).toString(16).padStart(8, '0');
        return hex.slice(0, 8);
    }

    // Вычисление хеша блока на основе его данных
    function computeBlockHash(index, data, previousHash) {
        const payload = `${index}|${data}|${previousHash}`;
        return simpleHash(payload);
    }

    // Исходные данные примеров (блокчейн с 3 блоками)
    const DEFAULT_DATA = [
        "🏦 Алиса → Бобу: 10 BTC | Транзакция #001",
        "🔄 Боб → Карлу: 5 BTC | оплата услуг",
        "🍕 Карл → Алисе: 2 BTC | пицца вечером"
    ];

    // Структура хранения блокчейна
    let blocks = [];

    // Функция перестроить всю цепочку с нуля на основе текущих данных блоков (сохраняя пользовательский текст)
    // prevHash первого блока всегда "0"
    function rebuildChainFromData() {
        if (!blocks.length) return;
        let prevHash = "0";
        for (let i = 0; i < blocks.length; i++) {
            const block = blocks[i];
            // вычисляем новый хеш с текущими данными и предыдущим хешом
            const newHash = computeBlockHash(block.index, block.data, prevHash);
            block.prevHash = prevHash;
            block.hash = newHash;
            prevHash = newHash;
        }
    }

    // инициализация / сброс к примеру
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

    // Проверка целостности цепочки: для каждого блока валидируем prevHash и собственный хеш
    // Возвращает массив булевых значений (валиден ли блок)
    function validateChainIntegrity() {
        const validity = [];
        if (!blocks.length) return validity;
        // Проверяем первый блок: его prevHash должен быть "0" и его хеш должен совпадать с вычисленным
        let expectedPrev = "0";
        for (let i = 0; i < blocks.length; i++) {
            const block = blocks[i];
            const recalculatedHash = computeBlockHash(block.index, block.data, expectedPrev);
            const isPrevValid = (block.prevHash === expectedPrev);
            const isHashValid = (block.hash === recalculatedHash);
            const valid = isPrevValid && isHashValid;
            validity.push(valid);
            // следующий ожидаемый prevHash — это текущий (корректный) хеш, НО если этот блок невалиден, то цепочка дальше автоматически невалидна
            // но для проверки используем реальный hash блока (даже если он неправильный? нет, по логике блокчейна следующий блок должен хранить предыдущий хеш.
            // Для индикации следующего блока мы передаем реальное поле prevHash следующего блока сравниваем с текущим фактическим hash блока (не recalculated)
            // Доп. проверка: для следующего блока expectedPrev должен быть РЕАЛЬНЫМ хешем текущего блока (block.hash)
            expectedPrev = block.hash;
        }
        // Дополнительно проверим связь между блоками (предыдущий хеш следующего == хеш текущего)
        for (let i = 1; i < blocks.length; i++) {
            const prevBlock = blocks[i-1];
            const currBlock = blocks[i];
            if (currBlock.prevHash !== prevBlock.hash) {
                validity[i] = false; // если связь нарушена — блок невалиден
            }
        }
        // Перепроверим первый блок
        if (blocks[0] && blocks[0].prevHash !== "0") validity[0] = false;
        return validity;
    }

    // Обновление конкретного блока: пересчитать его хеш и проверить цепочку (но не трогать чужие prevHash, чтобы показать разрыв)
    // При изменении данных пользователем мы пересчитываем ТОЛЬКО хеш текущего блока, prevHash оставляем старым.
    // Это ломает связь с последующими блоками. Идеально для демонстрации.
    function updateBlockData(blockIndex, newData) {
        const block = blocks.find(b => b.index === blockIndex);
        if (!block) return;
        block.data = newData;
        // Пересчитываем хеш этого блока, используя его текущий prevHash (который не меняется)
        const recalculatedHash = computeBlockHash(block.index, block.data, block.prevHash);
        block.hash = recalculatedHash;
        
        // После изменения данных последующие блоки становятся потенциально невалидными, но их данные и хеши не трогаем.
        // Однако для корректной проверки связей мы не перестраиваем цепочку автоматически.
        // Просто рендерим с новой валидацией.
        renderChain();
    }

    // "Починить цепочку" — перестроить хеши и prevHash начиная с самого первого блока (как будто вся сеть пересоздаёт блоки)
    // Это демонстрирует, что чтобы изменить один блок пришлось бы переписать всё что после него.
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

    // Рендер с актуальным состоянием валидации
    function renderChain() {
        const container = document.getElementById('blockchainContainer');
        if (!container) return;
        const validityArray = validateChainIntegrity();
        
        container.innerHTML = '';
        blocks.forEach((block, idx) => {
            const isValid = validityArray[idx] === true;
            const blockDiv = document.createElement('div');
            blockDiv.className = `block-card ${isValid ? 'valid' : 'invalid'}`;
            
            // Создаём редактируемое поле data (textarea для наглядности)
            const dataValue = block.data;
            
            // визуализация
            blockDiv.innerHTML = `
                <div class="block-header">
                    <span class="block-index">Блок #${block.index}</span>
                    <span class="block-badge">${isValid ? '✔ валиден' : '⚠ нарушен'}</span>
                </div>
                <div class="data-field">
                    <label>📦 ДАННЫЕ (измени текст)</label>
                    <textarea class="block-data-input" data-index="${block.index}" rows="2">${escapeHtml(dataValue)}</textarea>
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
        
        // Добавляем слушатели событий на все textarea после рендера
        document.querySelectorAll('.block-data-input').forEach(textarea => {
            const blockIdx = parseInt(textarea.getAttribute('data-index'), 10);
            textarea.addEventListener('input', (e) => {
                const newVal = e.target.value;
                updateBlockData(blockIdx, newVal);
            });
        });
    }
    
    // вспомогательная функция для безопасности вывода
    function escapeHtml(str) {
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }
    
    // кнопка для валидации (просто перерендер с подсветкой)
    function revalidateAndRender() {
        renderChain();
    }
    
    // установка обработчиков кнопок
    function bindEvents() {
        const resetBtn = document.getElementById('resetChainBtn');
        if (resetBtn) resetBtn.addEventListener('click', () => {
            resetToExample();
        });
        const validateBtn = document.getElementById('validateChainBtn');
        if (validateBtn) validateBtn.addEventListener('click', () => {
            renderChain(); // просто заново применяет валидацию
        });
        const repairBtn = document.getElementById('repairChainBtn');
        if (repairBtn) repairBtn.addEventListener('click', () => {
            repairChain();
        });
    }
    
    // инициализация
    function init() {
        resetToExample(); // создаст блоки и отрендерит
        bindEvents();
    }
    
    init();
</script>
</body>
</html>
