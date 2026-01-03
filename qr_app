<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Сканер QR-кодов</title>
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
            margin-bottom: 25px;
            color: rgba(255, 255, 255, 0.8);
            font-size: 16px;
        }
        
        #qr-reader {
            width: 100%;
            margin: 20px 0;
            border-radius: 15px;
            overflow: hidden;
            background: black;
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
    </style>
</head>
<body>
    <div class="container">
        <h1>📷 Сканер QR-кодов</h1>
        <p class="subtitle">Наведите камеру на QR-код пользователя</p>
        
        <div id="qr-reader"></div>
        
        <div class="instructions">
            <h3>📋 Как использовать:</h3>
            <ul>
                <li>Разрешите доступ к камере</li>
                <li>Наведите камеру на QR-код</li>
                <li>Держите телефон устойчиво</li>
                <li>Дождитесь автоматического сканирования</li>
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
    </div>
    
    <script>
        let currentUserId = null;
        let currentCurrency = null;
        let currentUserName = null;
        let currentBalance = null;
        
        // Инициализация сканера QR-кодов
        const html5QrCode = new Html5Qrcode("qr-reader");
        
        const qrConfig = { 
            fps: 10,
            qrbox: { width: 250, height: 250 },
            aspectRatio: 1.0,
            disableFlip: false
        };
        
        // Начинаем сканирование при загрузке страницы
        Html5Qrcode.getCameras().then(cameras => {
            if (cameras && cameras.length) {
                const cameraId = cameras[0].id; // Используем заднюю камеру
                
                html5QrCode.start(
                    cameraId,
                    qrConfig,
                    onScanSuccess,
                    onScanFailure
                ).catch(err => {
                    showError("Не удалось запустить камеру: " + err);
                });
            } else {
                showError("Камера не найдена");
            }
        }).catch(err => {
            showError("Ошибка доступа к камере: " + err);
        });
        
        function onScanSuccess(decodedText, decodedResult) {
            // Останавливаем сканер после успешного сканирования
            html5QrCode.stop().then(() => {
                // Проверяем формат QR-кода
                if (decodedText.startsWith('DNDBOT_USER_')) {
                    const userId = decodedText.replace('DNDBOT_USER_', '');
                    currentUserId = userId;
                    
                    // Показываем индикатор загрузки
                    document.getElementById('result').classList.add('active');
                    document.getElementById('user-name').textContent = "Загрузка данных...";
                    document.getElementById('user-balance').textContent = "Запрос к серверу...";
                    
                    // Запрашиваем данные пользователя у Telegram бота
                    getUserData(userId);
                    
                } else {
                    showError("Это не QR-код нашего бота");
                    setTimeout(() => {
                        location.reload();
                    }, 2000);
                }
            }).catch(err => {
                console.error("Ошибка остановки сканера: ", err);
            });
        }
        
        function onScanFailure(error) {
            // Ошибки игнорируем - сканер продолжает работать
        }
        
        function getUserData(userId) {
            // Используем Telegram WebApp для отправки данных боту
            if (window.Telegram && Telegram.WebApp) {
                Telegram.WebApp.sendData(JSON.stringify({
                    action: "get_user_data",
                    user_id: userId
                }));
                
                // Слушаем ответ от бота
                Telegram.WebApp.onEvent('userDataReceived', function(event) {
                    try {
                        const userData = JSON.parse(event);
                        if (userData.success) {
                            currentUserName = userData.name;
                            currentBalance = userData.balance;
                            
                            document.getElementById('user-name').textContent = userData.name;
                            document.getElementById('user-balance').innerHTML = 
                                `💰 Куси: <strong>${userData.balance.kusi}</strong><br>` +
                                `🪙 V-Coin: <strong>${userData.balance.vcoin}</strong>`;
                        } else {
                            showError(userData.error || "Пользователь не найден");
                        }
                    } catch (e) {
                        showError("Ошибка обработки данных");
                    }
                });
            } else {
                // Fallback для тестирования
                setTimeout(() => {
                    currentUserName = "Тестовый Пользователь";
                    currentBalance = { kusi: 100, vcoin: 50 };
                    
                    document.getElementById('user-name').textContent = "Тестовый Пользователь";
                    document.getElementById('user-balance').innerHTML = 
                        `💰 Куси: <strong>100</strong><br>` +
                        `🪙 V-Coin: <strong>50</strong>`;
                }, 1000);
            }
        }
        
        function selectCurrency(currency) {
            currentCurrency = currency;
            
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
            
            // Показываем индикатор загрузки
            showStatus("Отправка данных...", "loading");
            
            // Отправляем данные в бота через Telegram WebApp
            if (window.Telegram && Telegram.WebApp) {
                Telegram.WebApp.sendData(JSON.stringify({
                    action: "grant_currency",
                    user_id: currentUserId,
                    currency: currentCurrency,
                    amount: amount,
                    admin_id: Telegram.WebApp.initDataUnsafe.user?.id
                }));
                
                // Закрываем WebApp после успешной отправки
                setTimeout(() => {
                    Telegram.WebApp.close();
                }, 2000);
                
            } else {
                // Fallback для тестирования
                showStatus(`✅ Успешно! Начислено ${amount} ${currentCurrency === 'kusi' ? 'Куси' : 'V-Coin'}`, "success");
                
                setTimeout(() => {
                    // Перезагружаем сканер для нового сканирования
                    location.reload();
                }, 3000);
            }
        }
        
        function cancelOperation() {
            // Перезагружаем страницу для нового сканирования
            location.reload();
        }
        
        function showStatus(message, type) {
            const statusEl = document.getElementById('status');
            statusEl.textContent = message;
            statusEl.className = 'status ' + type;
            statusEl.style.display = 'block';
        }
        
        function showError(message) {
            const statusEl = document.getElementById('status');
            statusEl.innerHTML = `❌ ${message}<br><button onclick="location.reload()" style="margin-top: 10px; padding: 8px 15px; background: white; border: none; border-radius: 5px; cursor: pointer;">Попробовать снова</button>`;
            statusEl.className = 'status error';
            statusEl.style.display = 'block';
        }
        
        // Обработка сообщений от бота
        window.addEventListener('message', function(event) {
            if (event.data && event.data.type === 'user_data') {
                const userData = event.data.data;
                if (userData.success) {
                    currentUserName = userData.name;
                    currentBalance = userData.balance;
                    
                    document.getElementById('user-name').textContent = userData.name;
                    document.getElementById('user-balance').innerHTML = 
                        `💰 Куси: <strong>${userData.balance.kusi}</strong><br>` +
                        `🪙 V-Coin: <strong>${userData.balance.vcoin}</strong>`;
                } else {
                    showError(userData.error || "Пользователь не найден");
                }
            }
        });
    </script>
</body>
</html>
