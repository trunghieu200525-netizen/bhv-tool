<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Công cụ Bảo hiểm Vốn (Fintech Edition)</title>
    <style>
        /* --- ĐỊNH NGHĨA BIẾN MÀU SẮC (MÀU SẮC CHUYÊN NGHIỆP) --- */
        :root {
            --primary-blue: #007bff; /* Xanh chính */
            --bg-body: #f8fafc; /* Nền trắng xám */
            --bg-card: #ffffff; /* Nền card trắng tinh */
            --text-dark: #334155; /* Chữ chính */
            --text-light: #64748b; /* Chữ phụ */
            --border-color: #e2e8f0; /* Viền ô nhập */
            --shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            
            /* Màu sắc cho các trạng thái tính toán */
            --ratio-bg: #e0f2fe;
            --ratio-text: #0369a1;
            --result-bg: #ecfdf5;
            --result-border: #a7f3d0;
            --result-text: #047857;
            --listen-red: #ef4444;
        }

        /* --- STYLES CHO TOÀN TRANG --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-body);
            color: var(--text-dark);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 10px;
            box-sizing: border-box;
            user-select: none; /* Tránh bôi đen chữ khi nhấn giữ Space */
        }

        /* --- STYLES CHO CONTAINER CHÍNH --- */
        .container {
            background-color: var(--bg-card);
            padding: 30px;
            border-radius: 16px;
            box-shadow: var(--shadow);
            width: 100%;
            max-width: 480px;
        }

        h2 {
            text-align: center;
            color: var(--text-dark);
            margin-bottom: 5px;
            font-weight: 700;
        }

        .subtitle {
            text-align: center;
            color: var(--text-light);
            font-size: 14px;
            margin-bottom: 25px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
        }

        /* --- STYLES CHO CÁC NHÓM NHẬP LIỆU --- */
        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--text-dark);
            font-size: 15px;
        }

        .input-with-btn {
            display: flex;
            gap: 10px;
            align-items: stretch;
        }

        input {
            width: 100%;
            padding: 14px;
            border: 1.5px solid var(--border-color);
            border-radius: 10px;
            font-size: 16px;
            color: var(--text-dark);
            box-sizing: border-box;
            transition: border-color 0.2s, box-shadow 0.2s;
        }

        input:focus {
            outline: none;
            border-color: var(--primary-blue);
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
        }

        button {
            background-color: var(--primary-blue);
            color: white;
            border: none;
            padding: 0 20px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            transition: background-color 0.2s, transform 0.1s;
            white-space: nowrap;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        button:hover {
            background-color: #006ae0;
        }

        /* --- STYLES TRẠNG THÁI "ĐANG NGHE" MỚI --- */
        button.listening {
            background-color: var(--listen-red);
            transform: scale(0.96); /* Hiệu ứng lún nút */
            animation: pulse-red 1.5s infinite; /* Hiệu ứng nhấp nháy mềm */
        }

        @keyframes pulse-red {
            0% { box-shadow: 0 0 0 0px rgba(239, 68, 68, 0.5); }
            100% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0.0); }
        }

        /* --- STYLES CHO HIỂN THỊ TỶ LỆ --- */
        .ratio-display {
            background-color: var(--ratio-bg);
            color: var(--ratio-text);
            padding: 10px 15px;
            border-radius: 10px;
            display: inline-flex;
            font-weight: 700;
            margin-bottom: 25px;
            font-size: 16px;
            align-items: center;
            gap: 5px;
        }

        /* --- STYLES CHO CARD KẾT QUẢ CUỐI CÙNG --- */
        .result-card {
            background-color: var(--result-bg);
            border: 1.5px solid var(--result-border);
            padding: 25px;
            border-radius: 12px;
            text-align: center;
            margin-top: 10px;
        }

        .result-label {
            color: var(--result-text);
            font-size: 13px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1.2px;
            margin-bottom: 10px;
        }

        .result-value {
            color: var(--result-text);
            font-size: 32px;
            font-weight: 800;
            line-height: 1;
        }
        
        /* --- STYLES CHO DÒNG CHỮ GỢI Ý PHÍA DƯỚI --- */
        .hint-text {
            text-align: center;
            font-size: 12px;
            color: var(--text-light);
            margin-top: 20px;
            line-height: 1.6;
            background-color: #f1f5f9;
            padding: 10px;
            border-radius: 8px;
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
        <input type="number" id="userCapital" placeholder="Ví dụ: 5" oninput="calculate()">
    </div>

    <div style="text-align: center;">
        <div class="ratio-display">
            Tỷ lệ vốn (Tôi/Boss): <span id="ratioDisplay">0%</span>
        </div>
    </div>

    <div class="input-group">
        <label for="bossOrder">Lệnh Boss đi</label>
        <div class="input-with-btn">
            <input type="number" step="0.1" id="bossOrder" placeholder="Ví dụ: 20" oninput="calculate()">
            <button id="micBtn" type="button" title="Nhấn giữ để nói">
                🎤 <span id="btnText">Giữ & Nói</span>
            </button>
        </div>
    </div>

    <div class="result-card">
        <div class="result-label">Lệnh tôi cần đi (Làm tròn 100k)</div>
        <div class="result-value" id="userOrderDisplay">0 VNĐ</div>
    </div>
    
    <div class="hint-text">
        💡 <b>Chế độ bộ đàm:</b> Nhấn GIỮ phím <b>Space (Phím cách)</b> bên ngoài các ô nhập (hoặc giữ chuột/chạm vào nút).<br>
        Có thể đọc: "100", "Một tỷ", "2 chục triệu", "Tỷ hai"...<br>
        Buông tay ra là hệ thống ngắt và nhận lệnh ngay lập tức!
    </div>
</div>

<script>
    // Định dạng tiền tệ VNĐ (ví dụ: 1.000.000 đ)
    function formatCurrency(amount) {
        return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
    }

    // Hàm tính toán chính
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
    const btnText = document.getElementById('btnText');
    const bossOrderInput = document.getElementById('bossOrder');
    let isListening = false;

    // Tích hợp giọng nói
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        
        recognition.continuous = false;
        recognition.interimResults = false;
        recognition.lang = 'vi-VN';

        // Bật micro
        function startListening() {
            if (isListening) return;
            try {
                recognition.start();
                isListening = true;
                btnText.innerText = "Đang nghe...";
                micBtn.classList.add('listening');
            } catch (e) {
                console.error(e);
            }
        }

        // Tắt micro và lấy kết quả ngay
        function stopListening() {
            if (!isListening) return;
            recognition.stop();
            isListening = false;
            resetMicState();
        }

        // Xử lý kết quả giọng nói
        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript.toLowerCase().trim();
            console.log("Giọng nói ghi nhận: ", transcript);
            
            // Xử lý chuyển chữ thành số cơ bản
            let text = transcript
                .replace(/một/g, '1').replace(/hai/g, '2').replace(/ba/g, '3')
                .replace(/bốn/g, '4').replace(/năm/g, '5').replace(/sáu/g, '6')
                .replace(/bảy/g, '7').replace(/tám/g, '8').replace(/chín/g, '9')
                .replace(/mười/g, '10').replace(/rưỡi/g, '.5').replace(/,/g, '.');

            // Xử lý tiếng lóng "chục"
            text = text
                .replace(/1 chục/g, '10')
                .replace(/2 chục/g, '20')
                .replace(/3 chục/g, '30')
                .replace(/4 chục/g, '40')
                .replace(/5 chục/g, '50')
                .replace(/6 chục/g, '60')
                .replace(/7 chục/g, '70')
                .replace(/8 chục/g, '80')
                .replace(/9 chục/g, '90')
                .replace(/chục/g, '10');

            let finalValue = 0;

            // Xử lý TỶ
            let tyMatch = text.match(/(\d+(\.\d+)?)\s*(tỷ|tỉ)/);
            if (tyMatch) {
                finalValue = parseFloat(tyMatch[1]) * 1000;
                let textConLai = text.replace(tyMatch[0], '');
                let leMatch = textConLai.match(/\d+(\.\d+)?/);
                if (leMatch) {
                    let le = parseFloat(leMatch[0]);
                    if (le > 0 && le < 10) finalValue += le * 100;
                    else if (le >= 10 && le < 100) finalValue += le * 10;
                    else finalValue += le;
                }
            } 
            // Xử lý NGÀN
            else if (text.includes('ngàn') || text.includes('nghìn') || text.includes('k')) {
                let numMatch = text.match(/\d+(\.\d+)?/);
                if (numMatch) finalValue = parseFloat(numMatch[0]) / 1000;
            }
            // Mặc định TRIỆU
            else {
                let numMatch = text.match(/\d+(\.\d+)?/);
                if (numMatch) finalValue = parseFloat(numMatch[0]);
            }

            if (finalValue > 0) {
                bossOrderInput.value = finalValue;
                calculate();
            }
        };

        // Reset trạng thái micro
        recognition.onerror = () => resetMicState();
        recognition.onend = () => {
            isListening = false;
            resetMicState();
        };

        function resetMicState() {
            btnText.innerText = "Giữ & Nói";
            micBtn.classList.remove('listening');
        }

        // --- CÁC SỰ KIỆN KÍCH HOẠT (CHẾ ĐỘ BỘ ĐÀM) ---

        // 1. Chuột
        micBtn.addEventListener('mousedown', startListening);
        micBtn.addEventListener('mouseup', stopListening);
        micBtn.addEventListener('mouseleave', stopListening);

        // 2. Điện thoại
        micBtn.addEventListener('touchstart', (e) => { e.preventDefault(); startListening(); });
        micBtn.addEventListener('touchend', (e) => { e.preventDefault(); stopListening(); });

        // 3. Phím cách (Space) - Tốc độ cao nhất
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
