<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Công cụ Bảo hiểm Vốn (Tối ưu Tốc độ & Giọng nói)</title>
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
            user-select: none;
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
            transform: scale(0.95);
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
        <label for="bossOrder">Lệnh Boss đi</label>
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
        💡 <b>Bộ đàm:</b> Nhấn GIỮ phím <b>Space</b> (hoặc giữ chuột) để đọc số.<br>
        Có thể đọc: "100", "Một tỷ", "1 tỷ rưỡi", "Tỷ hai"...
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

        function startListening() {
            if (isListening) return;
            try {
                recognition.start();
                isListening = true;
                micBtn.innerText = "Đang nghe...";
                micBtn.classList.add('listening');
            } catch (e) {
                console.error(e);
            }
        }

        function stopListening() {
            if (!isListening) return;
            recognition.stop();
            isListening = false;
            resetMicState();
        }

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript.toLowerCase().trim();
            console.log("Giọng nói ghi nhận: ", transcript);
            
            // Xử lý chuyển chữ thành số cơ bản
            let text = transcript
                .replace(/một/g, '1').replace(/hai/g, '2').replace(/ba/g, '3')
                .replace(/bốn/g, '4').replace(/năm/g, '5').replace(/sáu/g, '6')
                .replace(/bảy/g, '7').replace(/tám/g, '8').replace(/chín/g, '9')
                .replace(/mười/g, '10').replace(/rưỡi/g, '.5').replace(/,/g, '.');

            let finalValue = 0;

            // Xử lý nếu có chữ TỶ / TỈ
            let tyMatch = text.match(/(\d+(\.\d+)?)\s*(tỷ|tỉ)/);
            if (tyMatch) {
                finalValue = parseFloat(tyMatch[1]) * 1000;
                
                // Bắt thêm số lẻ phía sau (ví dụ: "1 tỷ 2")
                let textConLai = text.replace(tyMatch[0], '');
                let leMatch = textConLai.match(/\d+(\.\d+)?/);
                if (leMatch) {
                    let le = parseFloat(leMatch[0]);
                    if (le > 0 && le < 10) finalValue += le * 100; // Đọc "1 tỷ 2" -> cộng 200tr
                    else if (le >= 10 && le < 100) finalValue += le * 10; // Đọc "1 tỷ 25" -> cộng 250tr
                    else finalValue += le; // Đọc "1 tỷ 200" -> cộng 200tr
                }
            } 
            // Xử lý nếu có chữ NGÀN / NGHÌN (ví dụ Boss đi nhỏ 500 ngàn)
            else if (text.includes('ngàn') || text.includes('nghìn') || text.includes('k')) {
                let numMatch = text.match(/\d+(\.\d+)?/);
                if (numMatch) finalValue = parseFloat(numMatch[0]) / 1000;
            }
            // Mặc định không nói đơn vị hoặc nói TRIỆU
            else {
                let numMatch = text.match(/\d+(\.\d+)?/);
                if (numMatch) finalValue = parseFloat(numMatch[0]);
            }

            // Ghi kết quả vào ô nhập và tự tính
            if (finalValue > 0) {
                bossOrderInput.value = finalValue;
                calculate();
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

        micBtn.addEventListener('mousedown', startListening);
        micBtn.addEventListener('mouseup', stopListening);
        micBtn.addEventListener('mouseleave', stopListening);

        micBtn.addEventListener('touchstart', (e) => { e.preventDefault(); startListening(); });
        micBtn.addEventListener('touchend', (e) => { e.preventDefault(); stopListening(); });

        document.addEventListener('keydown', function(event) {
            if (event.code === 'Space' && event.target.tagName !== 'INPUT' && !event.repeat) {
                event.preventDefault();
                startListening();
            }
        });

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
