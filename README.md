<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Công cụ Bảo hiểm Vốn (Tối ưu)</title>
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
            transition: background-color 0.3s;
            white-space: nowrap;
        }

        button:hover {
            background-color: var(--btn-hover);
        }

        button.listening {
            background-color: #e74c3c;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
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
            <button id="micBtn" type="button" title="Bấm để nói">🎤 Nói</button>
        </div>
    </div>

    <div class="result-card">
        <div class="result-label">Lệnh tôi cần đi (Làm tròn 100k)</div>
        <div class="result-value" id="userOrderDisplay">0 VNĐ</div>
    </div>
    
    <div class="hint-text">
        💡 Mẹo: Nhấn phím <b>Space (Phím cách)</b> bên ngoài các ô nhập để bật Micro nhanh.
    </div>
</div>

<script>
    // Định dạng tiền tệ VNĐ
    function formatCurrency(amount) {
        return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
    }

    // Tính toán
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

    // Cài đặt Micro
    const micBtn = document.getElementById('micBtn');
    const bossOrderInput = document.getElementById('bossOrder');

    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        
        recognition.continuous = false;
        recognition.interimResults = false;
        recognition.lang = 'vi-VN';

        micBtn.onclick = () => {
            try {
                recognition.start();
                micBtn.innerText = "Đang nghe...";
                micBtn.classList.add('listening');
            } catch (e) {
                console.error("Microphone đang bận hoặc lỗi: ", e);
            }
        };

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript.toLowerCase().trim();
            let cleanString = transcript.replace(',', '.');
            let matchedNumbers = cleanString.match(/\d+(\.\d+)?/);

            if (matchedNumbers) {
                bossOrderInput.value = matchedNumbers[0];
                calculate();
            } else {
                alert("Không nhận diện được con số: '" + transcript + "'. Vui lòng thử lại!");
            }
            resetMicState();
        };

        recognition.onerror = (event) => {
            resetMicState();
        };

        recognition.onend = () => {
            resetMicState();
        };

        function resetMicState() {
            micBtn.innerText = "🎤 Nói";
            micBtn.classList.remove('listening');
        }

        // Lắng nghe phím Space để bật Micro
        document.addEventListener('keydown', function(event) {
            // Kiểm tra nếu bấm phím Space và không đang ở trong ô input nào
            if (event.code === 'Space' && event.target.tagName !== 'INPUT') {
                event.preventDefault(); // Ngăn trình duyệt cuộn trang
                if (!micBtn.classList.contains('listening')) {
                    micBtn.click(); // Kích hoạt nút Nói
                }
            }
        });

    } else {
        micBtn.style.display = 'none';
    }
</script>

</body>
</html>
