<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Công cụ Bảo hiểm Vốn (Tối ưu Tốc độ)</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --accent-color: #3498db;
            --bg-color: #f4f7f6;
            --card-bg: #ffffff;
            --text-color: #333;
            --btn-hover: #2980b9;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            user-select: none; /* Tránh bôi đen chữ khi nhấn giữ chuột */
        }

        .container {
            background-color: var(--card-bg);
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 500px;
        }

        h2 {
            text-align: center;
            color: var(--primary-color);
            margin-bottom: 5px;
        }

        .subtitle {
            text-align: center;
            color: #7f8c8d;
            font-size: 14px;
            margin-bottom: 25px;
            border-bottom: 2px solid var(--accent-color);
            padding-bottom: 10px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        .input-with-btn {
            display: flex;
            gap: 10px;
        }

        input {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            box-sizing: border-box;
            transition: border-color 0.3s;
        }

        input:focus {
            outline: none;
            border-color: var(--accent-color);
        }

        button {
            background-color: var(--accent-color);
            color: white;
            border: none;
            padding: 0 15px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            transition: background-color 0.3s, transform 0.1s;
            white-space: nowrap;
        }

        button:hover {
            background-color: var(--btn-hover);
        }

        button.listening {
            background-color: #e74c3c;
            transform: scale(0.95); /* Hiệu ứng lún xuống khi đang nhấn giữ */
        }

        .info-box {
            background-color: #eef2f7;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .info-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }

        .info-row:last-child {
            margin-bottom: 0;
        }

        .label-info {
            color: #666;
        }

        .value-info {
            font-weight: bold;
            color: var(--primary-color);
        }

        .result-card {
            background-color: #d4edda;
            border: 1px solid #c3e6cb;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }

        .result-label {
            color: #155724;
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 5px;
        }

        .result-value {
            color: #155724;
            font-size: 28px;
            font-weight: 800;
        }
        
        .hint-text {
            text-align: center;
            font-size: 12px;
            color: #888;
            margin-top: 15px;
            line-height: 1.5;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Bảo Hiểm Vốn</h2>
    <div class="subtitle">Đơn vị nhập: Triệu VNĐ (Ví dụ: Nhập 100 = 100,000,000đ)</div>
    
    <div class="input-group">
        <label for="bossCapital">Vốn của Boss (Triệu VNĐ)</label>
        <input type="number" id="bossCapital" placeholder="Ví dụ: 100" oninput="calculate()">
    </div>

    <div class="input-group">
        <label for="userCapital">Vốn của tôi (Triệu VNĐ)</label>
        <input type="number" id="userCapital" placeholder="Ví dụ: 20" oninput="calculate()">
    </div>

    <div class="info-box">
        <div class="info-row">
            <span class="label-info">Tỷ lệ vốn (Tôi/Boss):</span>
            <span class="value-info" id="ratioDisplay">0%</span>
        </div>
    </div>

    <div class="input-group">
        <label for="bossOrder">Lệnh Boss đi (Triệu VNĐ)</label>
        <div class="input-with-btn">
            <input type="number" step="0.1" id="bossOrder" placeholder="Ví dụ: 5.5" oninput="calculate()">
            <button id="micBtn" type="button" title="Nhấn giữ để nói">🎤 Giữ & Nói</button>
        </div>
    </div>

    <div class="result-card">
        <div class="result-label">Lệnh tôi cần đi (Làm tròn 100k)</div>
        <div class="result-value" id="userOrderDisplay">0 VNĐ</div>
    </div>
    
    <div class="hint-text">
        💡 <b>Bộ đàm:</b> Nhấn GIỮ phím <b>Space</b> (hoặc giữ chuột vào nút) để đọc số.<br>
        Buông tay ra là hệ thống ngắt và nhận lệnh ngay lập tức!
    </div>
</div>

<script>
    function formatCurrency(amount) {
        return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
    }

    function calculate() {
        const bossCapital = (parseFloat(document.getElementById('bossCapital').value) || 0) * 1000000;
        const userCapital = (parseFloat(document.getElementById('userCapital').value) || 0) * 1000000;
        const bossOrder = (parseFloat(document.getElementById('bossOrder').value) || 0) * 1000000;

        let ratio = 0;
        if (bossCapital > 0) {
            ratio = userCapital / bossCapital;
        }

        document.getElementById('ratioDisplay').innerText = (ratio * 100).toFixed(2) + '%';
        const rawUserOrder = bossOrder * ratio;
        const roundedUserOrder = Math.round(rawUserOrder / 100000) * 100000;
        document.getElementById('userOrderDisplay').innerText = formatCurrency(roundedUserOrder);
    }

    const micBtn = document.getElementById('micBtn');
    const bossOrderInput = document.getElementById('bossOrder');
    let isListening = false;

    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        
        recognition.continuous = false;
        recognition.interimResults = false;
        recognition.lang = 'vi-VN';

        // Hàm bắt đầu nghe
        function startListening() {
            if (isListening) return; // Nếu đang nghe rồi thì bỏ qua
            try {
                recognition.start();
                isListening = true;
                micBtn.innerText = "Đang nghe...";
                micBtn.classList.add('listening');
            } catch (e) {
                console.error(e);
            }
        }

        // Hàm ép dừng khẩn cấp để lấy kết quả ngay
        function stopListening() {
            if (!isListening) return;
            recognition.stop(); // Ép dừng, API sẽ trả về kết quả nó đang thu được ngay lập tức
            isListening = false;
            resetMicState();
        }

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript.toLowerCase().trim();
            let cleanString = transcript.replace(',', '.');
            let matchedNumbers = cleanString.match(/\d+(\.\d+)?/);

            if (matchedNumbers) {
                bossOrderInput.value = matchedNumbers[0];
                calculate();
            } else {
                console.log("Không nhận diện được số: " + transcript);
            }
        };

        recognition.onerror = () => resetMicState();
        recognition.onend = () => {
            isListening = false;
            resetMicState();
        };

        function resetMicState() {
            micBtn.innerText = "🎤 Giữ & Nói";
            micBtn.classList.remove('listening');
        }

        // 1. Thao tác bằng chuột (Nhấn giữ chuột trái)
        micBtn.addEventListener('mousedown', startListening);
        micBtn.addEventListener('mouseup', stopListening);
        micBtn.addEventListener('mouseleave', stopListening); // Đề phòng kéo chuột ra ngoài nút

        // 2. Thao tác trên điện thoại (Chạm và giữ)
        micBtn.addEventListener('touchstart', (e) => { e.preventDefault(); startListening(); });
        micBtn.addEventListener('touchend', (e) => { e.preventDefault(); stopListening(); });

        // 3. Thao tác bằng bàn phím (Nhấn giữ phím Space)
        document.addEventListener('keydown', function(event) {
            // !event.repeat giúp tránh việc spam lệnh bật micro khi giữ phím quá lâu
            if (event.code === 'Space' && event.target.tagName !== 'INPUT' && !event.repeat) {
                event.preventDefault();
                startListening();
            }
        });

        // Buông phím Space
        document.addEventListener('keyup', function(event) {
            if (event.code === 'Space' && event.target.tagName !== 'INPUT') {
                event.preventDefault();
                stopListening();
            }
        });

    } else {
        micBtn.style.display = 'none';
    }
</script>

</body>
</html>
