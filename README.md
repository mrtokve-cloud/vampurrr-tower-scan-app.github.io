
    <title>Сканер QR-кодов DNDBOT</title>
    <script src="https://unpkg.com/html5-qrcode"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            color: white;
        }
        
        .container {
            width: 100%;
            max-width: 500px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            text-align: center;
        }
        
        h1 {
            margin-bottom: 10px;
            font-size: 24px;
            color: white;
        }
        
        .subtitle {
            margin-bottom: 15px;
            color: rgba(255, 255, 255, 0.8);
            font-size: 16px;
        }
        
        #camera-selector {
            margin-bottom: 15px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
        }
        
        #camera-select {
            width: 100%;
            padding: 10px;
            border-radius: 8px;
            border: 2px solid rgba(255, 255, 255, 0.3);
            background: rgba(0, 0, 0, 0.3);
            color: white;
            font-size: 14px;
            margin-top: 8px;
        }
        
        #camera-select option {
            background: #333;
            color: white;
        }
        
        #qr-reader {
            width: 100%;
            margin: 15px 0;
            border-radius: 15px;
            overflow: hidden;
            background: black;
            min-height: 300px;
            position: relative;
        }
        
        .camera-placeholder {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 300px;
            background: rgba(0, 0, 0, 0.5);
            color: rgba(255, 255, 255, 0.7);
            border-radius: 15px;
            font-size: 18px;
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
            text-align: left;
        }
        
        .instructions h3 {
            margin-bottom: 10px;
            color: white;
        }
        
        .instructions ul {
            padding-left: 20px;
        }
        
        .instructions li {
            margin: 8px 0;
            color: rgba(255, 255, 255, 0.9);
        }
        
        #result {
            display: none;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
        }
        
        #result.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }
        
        .user-info {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 15px;
            margin: 15px 0;
        }
        
        .currency-form {
            margin-top: 20px;
        }
        
        .currency-buttons {
            display: flex;
            gap: 10px;
            margin: 15px 0;
        }
        
        .currency-btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
        }
        
        .currency-btn:hover {
            transform: translateY(-2px);
            opacity: 0.9;
        }
        
        .kusi-btn {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }
        
        .vcoin-btn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }
        
        .amount-input {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 10px;
            margin: 15px 0;
            font-size: 18px;
            text-align: center;
            background: rgba(255, 255, 255, 0.9);
        }
        
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        .action-btn {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .send-btn {
            background: linear-gradient(135deg, #00b09b 0%, #96c93d 100%);
            color: white;
        }
        
        .cancel-btn {
            background: rgba(255, 255, 255, 0.3);
            color: white;
        }
        
        .status {
            margin-top: 20px;
            padding: 15px;
            border-radius: 10px;
            display: none;
        }
        
        .status.success {
            background: rgba(46, 204, 113, 0.3);
            display: block;
        }
        
        .status.error {
            background: rgba(231, 76, 60, 0.3);
            display: block;
        }
        
        .status.loading {
            background: rgba(52, 152, 219, 0.3);
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .loader {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-top: 4px solid white;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .camera-switch-btn {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            font-size: 20px;
            cursor: pointer;
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .camera-switch-btn:hover {
            background: rgba(0, 0, 0, 0.9);
        }
        
        .debug-info {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.5);
            margin-top: 10px;
            text-align: left;
            background: rgba(0, 0, 0, 0.2);
            padding: 10px;
            border-radius: 5px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📷 Сканер QR-кодов DNDBOT</h1>
        <p class="subtitle">Наведите камеру на QR-код пользователя</p>
        
        <div id="camera-selector" style="display: none;">
            <label for="camera-select">Выберите камеру:</label>
            <select id="camera-select"></select>
        </div>
        
        <div id="qr-reader">
            <div class="camera-placeholder" id="camera-placeholder">
                <div>Инициализация камеры...</div>
            </div>
            <button class="camera-switch-btn" id="switch-camera" title="Переключить камеру" style="display: none;">🔄</button>
        </div>
        
        <div class="instructions">
            <h3>📋 Как использовать:</h3>
            <ul>
                <li>Разрешите доступ к камере</li>
                <li>Наведите камеру на QR-код</li>
                <li>Держите телефон устойчиво</li>
                <li>Дождитесь автоматического сканирования</li>
                <li>Используйте кнопку 🔄 для переключения камеры</li>
            </ul>
        </div>
        
        <div id="result">
            <h3>👤 Найден пользователь:</h3>
            <div class="user-info">
                <p id="user-name">Загрузка...</p>
                <p id="user-balance">Баланс: загрузка...</p>
            </div>
            
            <div class="currency-form">
                <h3>💰 Начисление валюты</h3>
                <div class="currency-buttons">
                    <button class="currency-btn kusi-btn" onclick="selectCurrency('kusi')">💰 Куси</button>
                    <button class="currency-btn vcoin-btn" onclick="selectCurrency('vcoin')">🪙 V-Coin</button>
                </div>
                
                <input type="number" id="amount" class="amount-input" placeholder="Введите сумму" min="-9999" max="9999">
                
                <div class="action-buttons">
                    <button class="action-btn send-btn" onclick="sendCurrency()">📤 Начислить</button>
                    <button class="action-btn cancel-btn" onclick="cancelOperation()">❌ Отмена</button>
                </div>
            </div>
        </div>
        
        <div id="status" class="status"></div>
        
        <div class="debug-info" id="debug-info"></div>
    </div>
    
    <script>
        let currentUserId = null;
        let currentCurrency = null;
        let currentUserName = null;
        let currentBalance = null;
        let availableCameras = [];
        let currentCameraIndex = 0;
        let html5QrCode = null;
        let isCameraSwitching = false;
        let isWaitingForResponse = false;
        
        const qrConfig = { 
            fps: 10,
            qrbox: { width: 250, height: 250 },
            aspectRatio: 1.0,
            disableFlip: false
        };
        
        // Функция для отладки
        function debugLog(message) {
            console.log(message);
            const debugEl = document.getElementById('debug-info');
            debugEl.innerHTML += new Date().toLocaleTimeString() + ': ' + message + '<br>';
            debugEl.scrollTop = debugEl.scrollHeight;
        }
        
        // Проверка Telegram WebApp
        function initTelegramWebApp() {
            if (window.Telegram && Telegram.WebApp) {
                debugLog('Telegram WebApp инициализирован');
                Telegram.WebApp.expand();
                Telegram.WebApp.enableClosingConfirmation();
                
                // Настройка обработчиков событий
                Telegram.WebApp.onEvent('themeChanged', function() {
                    debugLog('Тема изменена');
                });
                
                Telegram.WebApp.onEvent('viewportChanged', function() {
                    debugLog('Viewport изменен');
                });
                
                return true;
            } else {
                debugLog('Telegram WebApp не обнаружен, работаем в тестовом режиме');
                return false;
            }
        }
        
        // Инициализация камер
        function initCameras() {
            Html5Qrcode.getCameras().then(cameras => {
                if (cameras && cameras.length) {
                    availableCameras = cameras;
                    updateCameraSelector(cameras);
                    
                    // Пытаемся найти и использовать заднюю камеру
                    const backCameraIndex = findBackCameraIndex(cameras);
                    
                    if (backCameraIndex >= 0) {
                        currentCameraIndex = backCameraIndex;
                        startCamera(cameras[backCameraIndex].id);
                    } else {
                        // Используем первую камеру
                        startCamera(cameras[0].id);
                    }
                    
                    // Показываем кнопку переключения камеры, если камер больше одной
                    if (cameras.length > 1) {
                        document.getElementById('switch-camera').style.display = 'flex';
                    }
                    
                } else {
                    showError("Камеры не найдены");
                }
            }).catch(err => {
                console.error("Ошибка доступа к камерам: ", err);
                showError("Ошибка доступа к камерам: " + err.message);
            });
        }
        
        // Поиск задней камеры
        function findBackCameraIndex(cameras) {
            for (let i = 0; i < cameras.length; i++) {
                const camera = cameras[i];
                const label = camera.label ? camera.label.toLowerCase() : '';
                
                // Ищем по ключевым словам
                if (label.includes('back') || 
                    label.includes('rear') || 
                    label.includes('environment') ||
                    label.includes('задняя') ||
                    (camera.facingMode && camera.facingMode === 'environment')) {
                    return i;
                }
            }
            
            // Если не нашли по меткам, но есть несколько камер,
            // предполагаем что задняя - вторая (индекс 1) или последняя
            if (cameras.length > 1) {
                return cameras.length - 1;
            }
            
            return -1; // Не найдена
        }
        
        // Обновление селектора камер
        function updateCameraSelector(cameras) {
            const selector = document.getElementById('camera-selector');
            const select = document.getElementById('camera-select');
            
            if (cameras.length > 1) {
                selector.style.display = 'block';
                select.innerHTML = '';
                
                cameras.forEach((camera, index) => {
                    const option = document.createElement('option');
                    option.value = camera.id;
                    let label = camera.label || `Камера ${index + 1}`;
                    
                    // Добавляем пометки для удобства
                    if (index === findBackCameraIndex(cameras)) {
                        label += " (Задняя)";
                    } else if (label.toLowerCase().includes('front') || 
                               (camera.facingMode && camera.facingMode === 'user')) {
                        label += " (Фронтальная)";
                    }
                    
                    option.textContent = label;
                    select.appendChild(option);
                });
                
                // Обработчик изменения выбора
                select.addEventListener('change', function() {
                    if (!isCameraSwitching) {
                        switchCameraById(this.value);
                    }
                });
            }
        }
        
        // Запуск камеры
        function startCamera(cameraId) {
            // Удаляем старый сканер если он есть
            if (html5QrCode && html5QrCode.isScanning) {
                html5QrCode.stop().catch(console.error);
            }
            
            // Создаем новый сканер
            html5QrCode = new Html5Qrcode("qr-reader");
            
            // Скрываем плейсхолдер
            document.getElementById('camera-placeholder').style.display = 'none';
            
            // Обновляем селектор
            const select = document.getElementById('camera-select');
            if (select) {
                for (let i = 0; i < select.options.length; i++) {
                    if (select.options[i].value === cameraId) {
                        select.selectedIndex = i;
                        currentCameraIndex = i;
                        break;
                    }
                }
            }
            
            // Запускаем камеру
            html5QrCode.start(
                cameraId,
                qrConfig,
                onScanSuccess,
                onScanFailure
            ).then(() => {
                debugLog("Камера успешно запущена");
            }).catch(err => {
                console.error("Ошибка запуска камеры: ", err);
                
                // Пробуем следующую камеру если текущая не работает
                if (availableCameras.length > 1) {
                    const nextIndex = (currentCameraIndex + 1) % availableCameras.length;
                    if (nextIndex !== currentCameraIndex) {
                        showError(`Камера не работает, пробуем другую...`);
                        setTimeout(() => {
                            switchCamera(nextIndex);
                        }, 1000);
                    }
                } else {
                    showError("Не удалось запустить камеру: " + err.message);
                }
            });
        }
        
        // Переключение камеры по индексу
        function switchCamera(index) {
            if (index >= 0 && index < availableCameras.length && !isCameraSwitching) {
                isCameraSwitching = true;
                currentCameraIndex = index;
                startCamera(availableCameras[index].id);
                
                // Сбрасываем флаг через небольшую задержку
                setTimeout(() => {
                    isCameraSwitching = false;
                }, 500);
            }
        }
        
        // Переключение камеры по ID
        function switchCameraById(cameraId) {
            for (let i = 0; i < availableCameras.length; i++) {
                if (availableCameras[i].id === cameraId) {
                    switchCamera(i);
                    return;
                }
            }
        }
        
        // Переключение на следующую камеру
        function switchToNextCamera() {
            if (availableCameras.length > 1 && !isCameraSwitching) {
                const nextIndex = (currentCameraIndex + 1) % availableCameras.length;
                switchCamera(nextIndex);
            }
        }
        
        function onScanSuccess(decodedText, decodedResult) {
            debugLog(`Сканирование успешно: ${decodedText}`);
            
            // Останавливаем сканер после успешного сканирования
            html5QrCode.stop().then(() => {
                // Проверяем формат QR-кода
                if (decodedText.startsWith('DNDBOT_USER_')) {
                    const userId = decodedText.replace('DNDBOT_USER_', '');
                    currentUserId = userId;
                    
                    debugLog(`Найден пользователь ID: ${userId}`);
                    
                    // Показываем индикатор загрузки
                    document.getElementById('result').classList.add('active');
                    document.getElementById('user-name').textContent = "Загрузка данных...";
                    document.getElementById('user-balance').textContent = "Запрос к серверу...";
                    
                    // Запрашиваем данные пользователя у Telegram бота
                    getUserData(userId);
                    
                } else {
                    showError("Это не QR-код нашего бота");
                    setTimeout(() => {
                        restartScanner();
                    }, 2000);
                }
            }).catch(err => {
                console.error("Ошибка остановки сканера: ", err);
            });
        }
        
        function onScanFailure(error) {
            // Ошибки сканирования игнорируем - сканер продолжает работать
        }
        
        function getUserData(userId) {
            debugLog(`Запрос данных пользователя: ${userId}`);
            
            const data = {
                action: "get_user_data",
                user_id: userId
            };
            
            // Отправляем данные через Telegram WebApp
            if (window.Telegram && Telegram.WebApp) {
                Telegram.WebApp.sendData(JSON.stringify(data));
                debugLog("Данные отправлены через Telegram WebApp");
                
                // Обработка ответа от бота
                Telegram.WebApp.onEvent('messageReceived', function(event) {
                    debugLog(`Получен ответ: ${event}`);
                    try {
                        const response = JSON.parse(event);
                        if (response.type === 'user_data') {
                            if (response.success) {
                                currentUserName = response.name;
                                currentBalance = response.balance;
                                
                                document.getElementById('user-name').textContent = response.name;
                                document.getElementById('user-balance').innerHTML = 
                                    `💰 Куси: <strong>${response.balance.kusi}</strong><br>` +
                                    `🪙 V-Coin: <strong>${response.balance.vcoin}</strong>`;
                                    
                                debugLog(`Данные пользователя получены: ${response.name}`);
                            } else {
                                showError(response.error || "Пользователь не найден");
                                setTimeout(() => {
                                    restartScanner();
                                }, 2000);
                            }
                        }
                    } catch (e) {
                        console.error("Ошибка обработки данных:", e);
                        setTestUserData();
                    }
                });
                
                // Таймаут на случай отсутствия ответа
                setTimeout(() => {
                    if (!currentUserName) {
                        debugLog("Таймаут запроса, использую тестовые данные");
                        setTestUserData();
                    }
                }, 3000);
                
            } else {
                // Fallback для тестирования
                debugLog("Telegram WebApp не доступен, использую тестовые данные");
                setTestUserData();
            }
        }
        
        function setTestUserData() {
            currentUserName = "Тестовый Пользователь";
            currentBalance = { kusi: 100, vcoin: 50 };
            
            document.getElementById('user-name').textContent = "Тестовый Пользователь";
            document.getElementById('user-balance').innerHTML = 
                `💰 Куси: <strong>100</strong><br>` +
                `🪙 V-Coin: <strong>50</strong>`;
                
            debugLog("Установлены тестовые данные");
        }
        
        function selectCurrency(currency) {
            currentCurrency = currency;
            debugLog(`Выбрана валюта: ${currency}`);
            
            // Обновляем UI
            document.querySelectorAll('.currency-btn').forEach(btn => {
                btn.style.opacity = '0.5';
            });
            
            if (currency === 'kusi') {
                document.querySelector('.kusi-btn').style.opacity = '1';
            } else {
                document.querySelector('.vcoin-btn').style.opacity = '1';
            }
            
            document.getElementById('amount').focus();
        }
        
        function sendCurrency() {
            if (!currentCurrency) {
                showError("Выберите тип валюты");
                return;
            }
            
            const amount = parseInt(document.getElementById('amount').value);
            if (!amount || isNaN(amount)) {
                showError("Введите сумму");
                return;
            }
            
            if (isWaitingForResponse) {
                showError("Пожалуйста, дождитесь ответа от сервера");
                return;
            }
            
            // Показываем индикатор загрузки
            showStatus("Отправка данных на сервер...", "loading");
            isWaitingForResponse = true;
            
            const data = {
                action: "grant_currency",
                user_id: currentUserId,
                currency: currentCurrency,
                amount: amount
            };
            
            debugLog(`Отправка данных: ${JSON.stringify(data)}`);
            
            // Отправляем данные через Telegram WebApp
            if (window.Telegram && Telegram.WebApp) {
                // Отправляем данные
                Telegram.WebApp.sendData(JSON.stringify(data));
                debugLog("Данные отправлены");
                
                // Обработка ответа от бота
                const messageHandler = function(event) {
                    debugLog(`Получен ответ от бота: ${event}`);
                    try {
                        const response = JSON.parse(event);
                        
                        if (response.type === 'success') {
                            showStatus(response.message, "success");
                            isWaitingForResponse = false;
                            
                            // Закрываем WebApp через 3 секунды
                            setTimeout(() => {
                                if (window.Telegram && Telegram.WebApp) {
                                    Telegram.WebApp.close();
                                }
                            }, 3000);
                            
                        } else if (response.type === 'error') {
                            showError(response.message);
                            isWaitingForResponse = false;
                        }
                        
                        // Удаляем обработчик после получения ответа
                        Telegram.WebApp.offEvent('messageReceived', messageHandler);
                        
                    } catch (e) {
                        console.error("Ошибка обработки ответа:", e);
                        showError("Ошибка обработки ответа от сервера");
                        isWaitingForResponse = false;
                    }
                };
                
                Telegram.WebApp.onEvent('messageReceived', messageHandler);
                
                // Таймаут на случай отсутствия ответа
                setTimeout(() => {
                    if (isWaitingForResponse) {
                        debugLog("Таймаут ожидания ответа");
                        showError("Превышено время ожидания ответа от сервера");
                        isWaitingForResponse = false;
                        Telegram.WebApp.offEvent('messageReceived', messageHandler);
                    }
                }, 10000);
                
            } else {
                // Fallback для тестирования
                debugLog("Telegram WebApp не доступен, симулирую успешную отправку");
                showStatus(`✅ Тестовый режим: Начислено ${amount} ${currentCurrency === 'kusi' ? 'Куси' : 'V-Coin'}`, "success");
                isWaitingForResponse = false;
                
                setTimeout(() => {
                    restartScanner();
                }, 3000);
            }
        }
        
        function cancelOperation() {
            debugLog("Операция отменена");
            // Перезагружаем сканер для нового сканирования
            restartScanner();
        }
        
        function restartScanner() {
            debugLog("Перезапуск сканера");
            // Скрываем результат
            document.getElementById('result').classList.remove('active');
            
            // Сбрасываем значения
            currentUserId = null;
            currentCurrency = null;
            currentUserName = null;
            currentBalance = null;
            isWaitingForResponse = false;
            document.getElementById('amount').value = '';
            document.getElementById('status').style.display = 'none';
            
            // Сбрасываем кнопки валют
            document.querySelectorAll('.currency-btn').forEach(btn => {
                btn.style.opacity = '1';
            });
            
            // Перезапускаем камеру
            if (availableCameras.length > 0) {
                startCamera(availableCameras[currentCameraIndex].id);
            }
        }
        
        function showStatus(message, type) {
            const statusEl = document.getElementById('status');
            statusEl.textContent = message;
            statusEl.className = 'status ' + type;
            statusEl.style.display = 'block';
            
            if (type === 'loading') {
                statusEl.innerHTML = `<div class="loader"></div><div>${message}</div>`;
            }
            
            debugLog(`Статус: ${type} - ${message}`);
        }
        
        function showError(message) {
            const statusEl = document.getElementById('status');
            statusEl.innerHTML = `❌ ${message}<br><button onclick="restartScanner()" style="margin-top: 10px; padding: 8px 15px; background: white; border: none; border-radius: 5px; cursor: pointer; color: #333;">Попробовать снова</button>`;
            statusEl.className = 'status error';
            statusEl.style.display = 'block';
            
            debugLog(`Ошибка: ${message}`);
        }
        
        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            // Включаем отладку по умолчанию
            document.getElementById('debug-info').style.display = 'block';
            
            debugLog("Страница загружена");
            
            // Инициализируем Telegram WebApp
            const isWebApp = initTelegramWebApp();
            
            // Назначаем обработчик для кнопки переключения камеры
            document.getElementById('switch-camera').addEventListener('click', switchToNextCamera);
            
            // Запрашиваем разрешение на камеру и инициализируем
            if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
                navigator.mediaDevices.getUserMedia({ video: true })
                    .then(() => {
                        // Разрешение получено, инициализируем камеры
                        initCameras();
                        debugLog("Разрешение на камеру получено");
                    })
                    .catch(err => {
                        debugLog(`Ошибка разрешения камеры: ${err.message}`);
                        showError("Требуется разрешение на использование камеры: " + err.message);
                        document.getElementById('camera-placeholder').textContent = "Требуется разрешение на камеру";
                    });
            } else {
                debugLog("Браузер не поддерживает доступ к камере");
                showError("Ваш браузер не поддерживает доступ к камере");
            }
            
            // Обработчик изменения ориентации устройства
            window.addEventListener('orientationchange', function() {
                debugLog("Ориентация устройства изменена");
                // Перезапускаем камеру при изменении ориентации
                setTimeout(() => {
                    if (availableCameras.length > 0 && html5QrCode) {
                        restartScanner();
                    }
                }, 300);
            });
            
            // Обработчик видимости страницы
            document.addEventListener('visibilitychange', function() {
                if (document.hidden) {
                    debugLog("Страница скрыта");
                } else {
                    debugLog("Страница видима");
                    // При возвращении на страницу перезапускаем сканер
                    if (availableCameras.length > 0 && !currentUserId) {
                        restartScanner();
                    }
                }
            });
        });
    </script>
</body>
</html>
