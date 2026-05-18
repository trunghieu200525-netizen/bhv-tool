<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <title>Hệ Thống ALong - Smart Shield</title>
    <style>
        /* DARK MODE STYLES */
        body { background-color: #111827; color: #f3f4f6; font-family: sans-serif; overflow-x: hidden; }
        .bg-card { background-color: #1f2937; border: 1px solid #374151; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.5); }
        .btn-speed { transition: transform 0.1s; }
        .btn-speed:active { transform: scale(0.92); }
        
        @keyframes pulse-lock {
            0% { box-shadow: 0 0 0 0 rgba(220, 38, 38, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(220, 38, 38, 0); }
            100% { box-shadow: 0 0 0 0 rgba(220, 38, 38, 0); }
        }
        .locked-signal { animation: pulse-lock 1.5s infinite; border-color: #ef4444; background-color: rgba(127, 29, 29, 0.2); }
        
        @keyframes pulse-safe {
            0% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(34, 197, 94, 0); }
            100% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0); }
        }
        .safe-signal { animation: pulse-safe 1.5s infinite; border-color: #22c55e; background-color: rgba(20, 83, 45, 0.2); }

        @keyframes pulse-cooldown {
            0% { box-shadow: 0 0 0 0 rgba(234, 179, 8, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(234, 179, 8, 0); }
            100% { box-shadow: 0 0 0 0 rgba(234, 179, 8, 0); }
        }
        .cooldown-signal { animation: pulse-cooldown 1.5s infinite; border-color: #eab308; background-color: rgba(113, 63, 18, 0.2); }

        @keyframes pulse-breaker {
            0% { box-shadow: 0 0 0 0 rgba(168, 85, 247, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(168, 85, 247, 0); }
            100% { box-shadow: 0 0 0 0 rgba(168, 85, 247, 0); }
        }
        .breaker-signal { animation: pulse-breaker 1.5s infinite; border-color: #a855f7; background-color: rgba(88, 28, 135, 0.2); }

        @keyframes preview-blink {
            0% { opacity: 1; filter: brightness(1.5); }
            50% { opacity: 0.2; filter: brightness(0.5); }
            100% { opacity: 1; filter: brightness(1.5); }
        }
        .preview-blink { animation: preview-blink 0.6s infinite; border-color: #fde047 !important; border-width: 2px !important; }

        @keyframes text-blink-red {
            0%, 100% { color: #f87171; text-shadow: 0 0 5px rgba(239, 68, 68, 0.5); }
            50% { color: #ef4444; text-shadow: none; opacity: 0.7; }
        }
        .text-blink-red { animation: text-blink-red 0.8s infinite; }

        .method-card { transition: all 0.3s ease; }
        .method-dimmed { opacity: 0.4; filter: grayscale(50%); }

        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        .big-road-grid { border-top: 1px solid #374151; border-left: 1px solid #374151; background-color: #111827; }
        .big-road-cell { border-right: 1px solid #374151; border-bottom: 1px solid #374151; }

        .toggle-checkbox:checked { right: 0; border-color: #22c55e; }
        .toggle-checkbox:checked + .toggle-label { background-color: #22c55e; }

        /* NÚT BẤM CHO BẢNG VỊ */
        .vi-btn {
            background-color: #374151; /* gray-700 */
            border: 1px solid #4b5563; /* gray-600 */
            border-radius: 0.25rem;
            color: #d1d5db; /* gray-300 */
            font-weight: 700;
            font-size: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            user-select: none;
            height: 22px;
        }
        .vi-btn:active { transform: scale(0.92); }
        
        .vi-score-con-active { background-color: #2563eb !important; border-color: #3b82f6 !important; color: white !important; }
        .vi-score-cai-active { background-color: #dc2626 !important; border-color: #ef4444 !important; color: white !important; }
        .vi-card-con-active { background-color: #06b6d4 !important; border-color: #22d3ee !important; color: #0f172a !important; font-weight: 900 !important; }
        .vi-card-cai-active { background-color: #ff4d4f !important; border-color: #ff7875 !important; color: #fff !important; font-weight: 900 !important; }
    </style>
</head>
<body class="flex justify-center p-2 text-gray-200">
    <div class="w-full max-w-md space-y-2">
        
        <div class="bg-card p-1.5 rounded-xl space-y-1">
            <div class="flex items-center justify-between pb-1 border-b border-gray-700">
                <div class="text-[10px] text-green-400 font-black uppercase tracking-wider ml-1">HỆ THỐNG ALong</div>
                <div class="flex items-center gap-2">
                    <span id="live-mode-text" class="text-[9px] font-bold text-gray-400 uppercase">Mô phỏng Data</span>
                    <div class="relative inline-block w-8 h-4 align-middle select-none transition duration-200 ease-in mr-1">
                        <input type="checkbox" name="toggle" id="live-toggle" onclick="toggleLiveMode()" class="toggle-checkbox absolute block w-4 h-4 rounded-full bg-gray-200 border-2 border-gray-500 appearance-none cursor-pointer transition-transform duration-200 ease-in-out z-10" />
                        <label for="live-toggle" class="toggle-label block overflow-hidden h-4 rounded-full bg-gray-600 cursor-pointer"></label>
                    </div>
                </div>
            </div>

            <div class="flex gap-1 w-full pt-0.5">
                <div class="flex-1 overflow-x-auto no-scrollbar rounded border border-gray-700" id="draw-main-scroll">
                    <div id="draw-main" class="flex big-road-grid w-max min-w-full h-full"></div>
                </div>
                <div class="w-[50px] shrink-0 flex flex-col gap-1">
                    <button onmousedown="showPreview('O')" onmouseup="clearPreview()" onmouseleave="clearPreview()" ontouchstart="showPreview('O')" ontouchend="clearPreview()" ontouchcancel="clearPreview()" class="flex-1 flex flex-col items-center justify-center bg-blue-900/30 border border-blue-500/50 rounded text-[9px] font-black text-blue-400 shadow-sm active:bg-blue-800 uppercase tracking-tight transition-colors select-none leading-tight py-1">
                        Hỏi<br>Con
                    </button>
                    <button onmousedown="showPreview('C')" onmouseup="clearPreview()" onmouseleave="clearPreview()" ontouchstart="showPreview('C')" ontouchend="clearPreview()" ontouchcancel="clearPreview()" class="flex-1 flex flex-col items-center justify-center bg-red-900/30 border border-red-500/50 rounded text-[9px] font-black text-red-400 shadow-sm active:bg-red-800 uppercase tracking-tight transition-colors select-none leading-tight py-1">
                        Hỏi<br>Cái
                    </button>
                </div>
            </div>

            <div class="flex gap-1 w-full">
                <div class="flex-1 overflow-x-auto no-scrollbar rounded border border-gray-700" id="draw-m3-scroll">
                    <div id="draw-m3" class="flex big-road-grid w-max min-w-full"></div>
                </div>
                <div class="flex-1 overflow-x-auto no-scrollbar rounded border border-gray-700" id="draw-m4-scroll">
                    <div id="draw-m4" class="flex big-road-grid w-max min-w-full"></div>
                </div>
                <div class="flex-1 overflow-x-auto no-scrollbar rounded border border-gray-700" id="draw-m5-scroll">
                    <div id="draw-m5" class="flex big-road-grid w-max min-w-full"></div>
                </div>
            </div>
            <div class="flex justify-between px-2 text-[7px] text-gray-500 font-bold uppercase pb-0.5">
                <span class="flex-1 text-center"></span>
                <span class="flex-1 text-center"></span>
                <span class="flex-1 text-center"></span>
            </div>
        </div>

        <div id="master-box" class="bg-gray-800 border-2 border-gray-600 rounded-xl p-2 my-2 shadow-lg transition-all duration-300">
            
            <div class="grid grid-cols-2 gap-1.5 mb-2">
                <div class="bg-gray-900/80 p-1.5 rounded border border-gray-700">
                    <div class="flex justify-between items-center text-[9px] font-bold mb-1">
                        <span class="text-blue-400 flex items-center gap-1"><div class="w-1.5 h-1.5 rounded-full bg-blue-500"></div> CON: <span id="lbl-score-con" class="text-white text-[10px]">-</span></span>
                        <span class="text-gray-400"><span id="lbl-cards-con" class="text-gray-200">-</span> Lá</span>
                    </div>
                    <div class="grid grid-cols-5 gap-0.5 mb-1" id="grid-score-con">
                        <button onclick="setScore('con', 0)" id="btn-score-con-0" class="vi-btn">0</button>
                        <button onclick="setScore('con', 1)" id="btn-score-con-1" class="vi-btn">1</button>
                        <button onclick="setScore('con', 2)" id="btn-score-con-2" class="vi-btn">2</button>
                        <button onclick="setScore('con', 3)" id="btn-score-con-3" class="vi-btn">3</button>
                        <button onclick="setScore('con', 4)" id="btn-score-con-4" class="vi-btn">4</button>
                        <button onclick="setScore('con', 5)" id="btn-score-con-5" class="vi-btn">5</button>
                        <button onclick="setScore('con', 6)" id="btn-score-con-6" class="vi-btn">6</button>
                        <button onclick="setScore('con', 7)" id="btn-score-con-7" class="vi-btn">7</button>
                        <button onclick="setScore('con', 8)" id="btn-score-con-8" class="vi-btn">8</button>
                        <button onclick="setScore('con', 9)" id="btn-score-con-9" class="vi-btn">9</button>
                    </div>
                    <div class="grid grid-cols-2 gap-0.5">
                        <button onclick="setCards('con', 2)" id="btn-cards-con-2" class="vi-btn">2 Lá</button>
                        <button onclick="setCards('con', 3)" id="btn-cards-con-3" class="vi-btn">3 Lá</button>
                    </div>
                </div>

                <div class="bg-gray-900/80 p-1.5 rounded border border-gray-700">
                    <div class="flex justify-between items-center text-[9px] font-bold mb-1">
                        <span class="text-red-400 flex items-center gap-1"><div class="w-1.5 h-1.5 rounded-full bg-red-500"></div> CÁI: <span id="lbl-score-cai" class="text-white text-[10px]">-</span></span>
                        <span class="text-gray-400"><span id="lbl-cards-cai" class="text-gray-200">-</span> Lá</span>
                    </div>
                    <div class="grid grid-cols-5 gap-0.5 mb-1" id="grid-score-cai">
                        <button onclick="setScore('cai', 0)" id="btn-score-cai-0" class="vi-btn">0</button>
                        <button onclick="setScore('cai', 1)" id="btn-score-cai-1" class="vi-btn">1</button>
                        <button onclick="setScore('cai', 2)" id="btn-score-cai-2" class="vi-btn">2</button>
                        <button onclick="setScore('cai', 3)" id="btn-score-cai-3" class="vi-btn">3</button>
                        <button onclick="setScore('cai', 4)" id="btn-score-cai-4" class="vi-btn">4</button>
                        <button onclick="setScore('cai', 5)" id="btn-score-cai-5" class="vi-btn">5</button>
                        <button onclick="setScore('cai', 6)" id="btn-score-cai-6" class="vi-btn">6</button>
                        <button onclick="setScore('cai', 7)" id="btn-score-cai-7" class="vi-btn">7</button>
                        <button onclick="setScore('cai', 8)" id="btn-score-cai-8" class="vi-btn">8</button>
                        <button onclick="setScore('cai', 9)" id="btn-score-cai-9" class="vi-btn">9</button>
                    </div>
                    <div class="grid grid-cols-2 gap-0.5">
                        <button onclick="setCards('cai', 2)" id="btn-cards-cai-2" class="vi-btn">2 Lá</button>
                        <button onclick="setCards('cai', 3)" id="btn-cards-cai-3" class="vi-btn">3 Lá</button>
                    </div>
                </div>
            </div>

            <div class="flex gap-1 items-center justify-center bg-gray-900 p-1.5 rounded-lg border border-gray-700 mb-2 shadow-inner">
                <button onclick="addCau('con')" class="flex-1 bg-blue-900/40 border border-blue-500 py-2 rounded text-[11px] font-black text-blue-400 shadow-sm active:scale-95 uppercase">Con</button>
                <button onclick="addCau('hoa')" class="flex-1 bg-green-900/40 border border-green-500 py-2 rounded text-[11px] font-black text-green-400 shadow-sm active:scale-95 uppercase">Hòa</button>
                <button onclick="addCau('cai')" class="flex-1 bg-red-900/40 border border-red-500 py-2 rounded text-[11px] font-black text-red-400 shadow-sm active:scale-95 uppercase">Cái</button>
                
                <div class="w-px h-6 bg-gray-600 mx-0.5"></div>
                
                <button id="btn-hardstop" onclick="toggleHardStop()" class="bg-red-900/50 text-red-300 px-1.5 py-1.5 rounded text-[8px] font-bold border border-red-600 active:scale-95 whitespace-nowrap leading-tight">🛑 D.LỖ<br>BẬT</button>
                <button id="btn-shield" onclick="toggleShield()" class="bg-purple-900/50 text-purple-300 px-1.5 py-1.5 rounded text-[8px] font-bold border border-purple-600 active:scale-95 whitespace-nowrap leading-tight">🛡️ CHẮN<br>BẬT</button>
                
                <div class="w-px h-6 bg-gray-600 mx-0.5"></div>
                <button onclick="undoCau()" class="bg-gray-700 text-gray-300 px-1.5 py-1.5 rounded text-[8px] font-bold border border-gray-600 active:scale-95 whitespace-nowrap leading-tight text-center">⌫<br>XÓA</button>
            </div>

            <div class="text-center mb-1">
                <span id="gate-status" class="text-[10px] font-black uppercase tracking-widest text-gray-400 bg-gray-700 px-2 py-0.5 rounded border border-gray-600 inline-block mb-0.5">ĐANG GOM DATA...</span>
            </div>
            
            <div class="flex flex-col items-center justify-center py-1">
                <div id="master-decision" class="text-3xl font-black text-gray-500 drop-shadow-sm flex items-center justify-center leading-none">CHỜ DATA</div>
                <div id="master-reason" class="text-[10.5px] font-bold text-gray-400 mt-1.5 text-center w-full px-1">Cần tối thiểu 2 Cột để kích hoạt Ma Trận</div>
            </div>

            <div id="core-alerts" class="hidden flex-col gap-1 mt-1 mb-1"></div>

            <div id="master-history-container" class="border-t border-gray-700 pt-1.5 mt-1">
                <div class="text-[10px] text-gray-500 italic text-center py-1">Chưa có lệnh nào được khớp.</div>
            </div>
        </div>

        <div class="bg-card p-1.5 rounded-xl">
            <div class="text-[10px] text-gray-400 font-bold uppercase tracking-wider mb-1.5 flex justify-between border-b border-gray-700 pb-1 px-1">
                <span>10 Lõi Tham Chiếu MTF & Vị</span>
            </div>
            <div id="methods-grid" class="grid grid-cols-2 gap-1.5"></div>
        </div>

        <div class="flex gap-2 items-center mt-2">
            <div class="bg-card p-1.5 rounded-xl border border-dashed border-gray-600 bg-gray-800 flex-1 flex gap-1.5 items-center">
                <span class="text-[9px] text-gray-400 font-bold ml-1">TEST:</span>
                <input type="text" id="bulk-input" placeholder="VD: CCOOCH" class="w-20 text-[10px] px-1 py-1 bg-gray-900 border border-gray-600 text-gray-100 rounded uppercase">
                <button onclick="processBulkData()" class="bg-green-700 text-white font-bold py-1 px-2 rounded text-[9px]">CHẠY</button>
            </div>
            <button onclick="confirmReset()" class="py-2.5 px-3 text-[9px] text-red-400 uppercase font-bold bg-gray-800 rounded-xl border border-gray-700 shadow-sm whitespace-nowrap">🗑️ Xóa Bàn</button>
        </div>
    </div>

    <script>
        let vConScore = null;
        let vCaiScore = null;
        let vConCards = 3; 
        let vCaiCards = 3;

        let isHardStopEnabled = true;
        let isShieldEnabled = true;
        let isHardStop = false;
        let hardStopReason = "";
        let consecutiveRealLosses = 0;
        let totalPnL = 0;
        let liveBetRecords = []; 

        function setScore(side, val) {
            if (side === 'con') vConScore = val;
            else vCaiScore = val;
            renderViUI();
        }

        function setCards(side, val) {
            if (side === 'con') vConCards = val;
            else vCaiCards = val;
            renderViUI();
        }

        function renderViUI() {
            document.getElementById('lbl-score-con').innerText = (vConScore !== null) ? vConScore : "-";
            document.getElementById('lbl-cards-con').innerText = vConCards;
            document.getElementById('lbl-score-cai').innerText = (vCaiScore !== null) ? vCaiScore : "-";
            document.getElementById('lbl-cards-cai').innerText = vCaiCards;

            for(let i=0; i<=9; i++) {
                let btnCon = document.getElementById('btn-score-con-' + i);
                let btnCai = document.getElementById('btn-score-cai-' + i);
                
                btnCon.className = "vi-btn";
                if (i === vConScore) btnCon.classList.add("vi-score-con-active");
                
                btnCai.className = "vi-btn";
                if (i === vCaiScore) btnCai.classList.add("vi-score-cai-active");
            }

            [2, 3].forEach(val => {
                let bCardCon = document.getElementById('btn-cards-con-' + val);
                let bCardCai = document.getElementById('btn-cards-cai-' + val);
                
                bCardCon.className = "vi-btn";
                if (val === vConCards) bCardCon.classList.add("vi-card-con-active");

                bCardCai.className = "vi-btn";
                if (val === vCaiCards) bCardCai.classList.add("vi-card-cai-active");
            });
        }

        const VI_MATRIX = {
            "0_0_3_3": { pred: 'C', pct: '89%' }, "0_1_3_3": { pred: 'O', pct: '62%' }, "0_2_3_3": { pred: 'C', pct: '89%' },
            "0_4_3_3": { pred: 'O', pct: '78%' }, "0_4_3_2": { pred: 'O', pct: '68%' }, "0_5_3_3": { pred: 'C', pct: '79%' },
            "0_5_3_2": { pred: 'C', pct: '65%' }, "0_6_3_3": { pred: 'C', pct: '75%' }, "0_6_3_2": { pred: 'C', pct: '59%', note: '+Hòa' },
            "0_7_3_3": { pred: 'O', pct: '70%' }, "0_7_3_2": { pred: 'C', pct: '79%' }, "0_8_3_3": { pred: 'C', pct: '98%' },
            "0_8_2_2": { pred: 'C', pct: '90%' }, "0_9_3_3": { pred: 'O', pct: '60%' }, "0_9_2_2": { pred: 'C', pct: '75%', note: '+Hòa' },
            "1_0_3_3": { pred: 'C', pct: '78%' }, "1_1_3_3": { pred: 'O', pct: '78%' }, "1_2_3_3": { pred: 'C', pct: '45%', note: '+Hòa' },
            "1_3_3_3": { pred: 'C', pct: '75%' }, "1_3_3_2": { pred: 'O', pct: '69%' }, "1_4_3_3": { pred: 'O', pct: '89%' },
            "1_4_3_2": { pred: 'C', pct: '65%' }, "1_5_3_3": { pred: 'O', pct: '73%' }, "1_5_3_2": { pred: 'O', pct: '98%' },
            "1_6_3_3": { pred: 'C', pct: '75%' }, "1_6_3_2": { pred: 'C', pct: '55%' }, "1_7_3_3": { pred: 'C', pct: '70%' },
            "1_7_3_2": { pred: 'O', pct: '75%' }, "1_8_3_3": { pred: 'C', pct: '75%' }, "1_8_2_2": { pred: 'C', pct: '60%' },
            "1_9_3_3": { pred: 'O', pct: '75%' }, "1_9_2_2": { pred: 'C', pct: '55%' },
            "2_0_3_3": { pred: 'O', pct: '79%' }, "2_1_3_3": { pred: 'C', pct: '65%' }, "2_2_3_3": { pred: 'O', pct: '55%', note: '+Hòa' },
            "2_3_3_3": { pred: 'C', pct: '65%' }, "2_3_3_2": { pred: 'C', pct: '85%' }, "2_4_3_3": { pred: 'C', pct: '75%' },
            "2_4_3_2": { pred: 'O', pct: '69%' }, "2_5_3_3": { pred: 'C', pct: '85%' }, "2_5_3_2": { pred: 'O', pct: '70%' },
            "2_6_3_3": { pred: 'C', pct: '75%' }, "2_6_3_2": { pred: 'O', pct: '65%' }, "2_7_3_3": { pred: 'C', pct: '60%', note: '+Hòa' },
            "2_7_3_2": { pred: 'C', pct: '85%' }, "2_8_3_3": { pred: 'C', pct: '75%' }, "2_8_2_2": { pred: 'O', pct: '75%' },
            "2_9_3_3": { pred: 'O', pct: '90%' }, "2_9_2_2": { pred: 'C', pct: '80%' },
            "3_0_3_3": { pred: 'O', pct: '75%' }, "3_1_3_3": { pred: 'O', pct: '70%' }, "3_2_3_3": { pred: 'O', pct: '80%' },
            "3_3_3_3": { pred: 'O', pct: '80%' }, "3_3_3_2": { pred: 'O', pct: '55%', note: '+Hòa' }, "3_4_3_3": { pred: 'O', pct: '80%' },
            "3_4_3_2": { pred: 'O', pct: '65%' }, "3_5_3_3": { pred: 'O', pct: '75%' }, "3_5_3_2": { pred: 'C', pct: '89%' },
            "3_6_3_3": { pred: 'O', pct: '80%' }, "3_6_3_2": { pred: 'O', pct: '70%' }, "3_7_3_3": { pred: 'O', pct: '75%' },
            "3_7_3_2": { pred: 'C', pct: '65%' }, "3_8_3_3": { pred: 'O', pct: '70%' }, "3_8_2_2": { pred: 'O', pct: '80%' },
            "3_9_3_3": { pred: 'O', pct: '85%' }, "3_9_2_2": { pred: 'C', pct: '90%' },
            "4_0_3_3": { pred: 'O', pct: '70%' }, "4_1_3_3": { pred: 'C', pct: '85%' }, "4_2_3_3": { pred: 'C', pct: '85%' },
            "4_3_3_3": { pred: 'O', pct: '70%' }, "4_3_3_2": { pred: 'C', pct: '55%' }, "4_4_3_3": { pred: 'C', pct: '70%' },
            "4_4_3_2": { pred: 'O', pct: '65%', note: '+Hòa' }, "4_5_3_3": { pred: 'C', pct: '75%' }, "4_5_3_2": { pred: 'C', pct: '85%' },
            "4_6_3_3": { pred: 'O', pct: '80%' }, "4_6_3_2": { pred: 'O', pct: '60%' }, "4_7_3_3": { pred: 'C', pct: '75%' },
            "4_7_3_2": { pred: 'O', pct: '65%', note: '+Hòa' }, "4_8_3_3": { pred: 'C', pct: '85%' }, "4_8_2_2": { pred: 'O', pct: '80%' },
            "4_9_3_3": { pred: 'C', pct: '80%' }, "4_9_2_2": { pred: 'C', pct: '55%' },
            "5_0_3_3": { pred: 'O', pct: '80%' }, "5_1_3_3": { pred: 'C', pct: '90%' }, "5_2_3_3": { pred: 'O', pct: '95%' },
            "5_3_3_3": { pred: 'C', pct: '80%' }, "5_3_3_2": { pred: 'C', pct: '90%' }, "5_4_3_3": { pred: 'C', pct: '75%' },
            "5_4_3_2": { pred: 'O', pct: '70%' }, "5_5_3_3": { pred: 'O', pct: '90%' }, "5_5_3_2": { pred: 'O', pct: '70%' },
            "5_6_3_3": { pred: 'O', pct: '95%' }, "5_6_3_2": { pred: 'O', pct: '70%' }, "5_7_3_3": { pred: 'C', pct: '75%' },
            "5_7_3_2": { pred: 'O', pct: '80%' }, "5_8_3_3": { pred: 'O', pct: '90%' }, "5_8_2_2": { pred: 'C', pct: '85%' },
            "5_9_3_3": { pred: 'O', pct: '60%' }, "5_9_2_2": { pred: 'C', pct: '90%' },
            "6_0_3_3": { pred: 'C', pct: '90%' }, "6_0_2_3": { pred: 'O', pct: '70%' }, "6_1_3_3": { pred: 'C', pct: '80%' },
            "6_1_2_3": { pred: 'C', pct: '75%' }, "6_2_3_3": { pred: 'O', pct: '90%' }, "6_2_2_3": { pred: 'O', pct: '80%' },
            "6_3_3_3": { pred: 'O', pct: '90%' }, "6_3_2_3": { pred: 'C', pct: '80%' }, "6_4_3_3": { pred: 'O', pct: '85%' },
            "6_4_2_3": { pred: 'C', pct: '70%' }, "6_5_3_3": { pred: 'C', pct: '75%' }, "6_5_3_2": { pred: 'O', pct: '70%' },
            "6_5_2_3": { pred: 'C', pct: '55%' }, "6_6_3_3": { pred: 'O', pct: '65%' }, "6_6_3_2": { pred: 'C', pct: '90%' },
            "6_6_2_3": { pred: 'C', pct: '75%' }, "6_6_2_2": { pred: 'C', pct: '65%' }, "6_7_3_3": { pred: 'C', pct: '65%' },
            "6_7_3_2": { pred: 'C', pct: '75%' }, "6_7_2_3": { pred: 'C', pct: '75%' }, "6_7_2_2": { pred: 'C', pct: '55%' },
            "6_8_3_3": { pred: 'C', pct: '65%' }, "6_8_2_2": { pred: 'O', pct: '80%' }, "6_9_3_3": { pred: 'C', pct: '90%' },
            "6_9_2_2": { pred: 'O', pct: '60%', note: '+Hòa' },
            "7_0_3_3": { pred: 'C', pct: '75%' }, "7_0_2_3": { pred: 'C', pct: '80%' }, "7_1_3_3": { pred: 'O', pct: '90%' },
            "7_1_2_3": { pred: 'C', pct: '65%' }, "7_2_3_3": { pred: 'C', pct: '85%' }, "7_2_2_3": { pred: 'O', pct: '75%' },
            "7_3_3_3": { pred: 'C', pct: '80%' }, "7_3_2_3": { pred: 'O', pct: '80%' }, "7_4_3_3": { pred: 'C', pct: '90%' },
            "7_4_2_3": { pred: 'C', pct: '80%' }, "7_5_3_3": { pred: 'O', pct: '75%' }, "7_5_2_3": { pred: 'O', pct: '80%' },
            "7_6_3_3": { pred: 'O', pct: '55%' }, "7_6_2_2": { pred: 'C', pct: '80%' }, "7_6_3_2": { pred: 'O', pct: '90%' },
            "7_7_3_3": { pred: 'C', pct: '90%' }, "7_7_3_2": { pred: 'O', pct: '70%' }, "7_7_2_3": { pred: 'O', pct: '80%' },
            "7_7_2_2": { pred: 'O', pct: '85%' }, "7_8_3_3": { pred: 'O', pct: '80%' }, "7_8_2_2": { pred: 'O', pct: '65%', note: '+ Hòa' },
            "7_8_2_3": { pred: 'C', pct: '95%' }, "7_9_3_3": { pred: 'C', pct: '75%' }, "7_9_2_2": { pred: 'C', pct: '90%' },
            "7_9_2_3": { pred: 'O', pct: '85%' },
            "8_0_3_3": { pred: 'C', pct: '75%' }, "8_0_2_2": { pred: 'O', pct: '80%' }, "8_1_3_3": { pred: 'O', pct: '90%' },
            "8_1_2_2": { pred: 'C', pct: '90%' }, "8_2_3_3": { pred: 'O', pct: '85%' }, "8_2_2_2": { pred: 'C', pct: '55%' },
            "8_3_3_3": { pred: 'C', pct: '95%' }, "8_3_2_2": { pred: 'O', pct: '65%' }, "8_3_3_2": { pred: 'O', pct: '95%' },
            "8_4_3_3": { pred: 'C', pct: '65%' }, "8_4_2_2": { pred: 'O', pct: '80%' }, "8_4_3_2": { pred: 'C', pct: '55%', note: '+Hòa' },
            "8_5_3_3": { pred: 'C', pct: '85%' }, "8_5_2_2": { pred: 'C', pct: '80%' }, "8_5_3_2": { pred: 'C', pct: '55%', note: '+ Hòa' },
            "8_6_3_3": { pred: 'C', pct: '90%' }, "8_6_2_2": { pred: 'O', pct: '90%' }, "8_6_3_2": { pred: 'O', pct: '75%' },
            "8_7_3_3": { pred: 'O', pct: '95%' }, "8_7_2_2": { pred: 'C', pct: '75%' }, "8_7_3_2": { pred: 'O', pct: '90%' },
            "8_8_3_3": { pred: 'C', pct: '90%' }, "8_8_2_2": { pred: 'O', pct: '75%' }, "8_9_3_3": { pred: 'O', pct: '80%' },
            "8_9_2_2": { pred: 'C', pct: '95%' },
            "9_0_3_3": { pred: 'C', pct: '80%', note: '+Hòa' }, "9_0_2_2": { pred: 'O', pct: '70%' }, "9_1_3_3": { pred: 'O', pct: '70%' },
            "9_1_2_2": { pred: 'C', pct: '80%', note: '+Hòa' }, "9_2_3_3": { pred: 'C', pct: '90%' }, "9_2_2_2": { pred: 'C', pct: '85%' },
            "9_3_3_3": { pred: 'C', pct: '65%' }, "9_3_2_2": { pred: 'C', pct: '65%', note: '+Hòa' }, "9_3_3_2": { pred: 'C', pct: '95%' },
            "9_4_3_3": { pred: 'O', pct: '75%' }, "9_4_2_2": { pred: 'C', pct: '80%' }, "9_4_3_2": { pred: 'C', pct: '75%' },
            "9_5_3_3": { pred: 'C', pct: '80%' }, "9_5_2_2": { pred: 'O', pct: '90%', note: '+Hòa' }, "9_5_3_2": { pred: 'O', pct: '95%' },
            "9_6_3_3": { pred: 'C', pct: '70%' }, "9_6_2_2": { pred: 'O', pct: '80%' }, "9_6_3_2": { pred: 'C', pct: '80%' },
            "9_7_3_3": { pred: 'C', pct: '60%' }, "9_7_2_2": { pred: 'O', pct: '90%' }, "9_7_3_2": { pred: 'O', pct: '70%' },
            "9_8_3_3": { pred: 'O', pct: '85%' }, "9_8_2_2": { pred: 'C', pct: '85%' }, "9_9_3_3": { pred: 'C', pct: '90%' },
            "9_9_2_2": { pred: 'O', pct: '90%' }
        };

        function getOpposite(char) { return char === 'C' ? 'O' : (char === 'O' ? 'C' : char); }
        
        function getDerivedRoad(str, offset) {
            let chunks = str.match(/(C+|O+)/g) || [];
            let road = [];
            for (let c = 0; c < chunks.length; c++) {
                for (let r = 0; r < chunks[c].length; r++) {
                    if (c < offset) continue;
                    if (c === offset && r === 0) continue;
                    if (r === 0) {
                        let pLen = chunks[c-1].length;
                        let cmpLen = chunks[c-offset-1].length;
                        road.push(pLen === cmpLen ? 'R' : 'B');
                    } else {
                        let refLen = chunks[c-offset] ? chunks[c-offset].length : 0;
                        if (refLen >= r + 1) road.push('R');
                        else if (refLen === r) road.push('B');
                        else road.push('R');
                    }
                }
            }
            return road;
        }

        function getDerivedPrediction(str, offset) {
            let roadCai = getDerivedRoad(str + 'C', offset);
            let roadCon = getDerivedRoad(str + 'O', offset);
            if(roadCai.length === 0 || roadCon.length === 0) return null;
            let currentRoad = getDerivedRoad(str, offset).join('');
            if(currentRoad.length < 2) return null; 
            
            let lastColor = currentRoad.slice(-1);
            let matchResult = currentRoad.match(new RegExp(lastColor + "+$", "g"));
            let cLen = matchResult ? matchResult[0].length : 0;
            
            let ppLen = 1;
            for(let i = currentRoad.length - 2; i >= 0; i--) {
                if (currentRoad[i] !== currentRoad[i+1]) ppLen++;
                else break;
            }

            let expectedDot = 'R';
            if (cLen >= 2) { expectedDot = lastColor; } 
            else if (ppLen >= 3) { expectedDot = (lastColor === 'R') ? 'B' : 'R'; } 
            else { expectedDot = 'R'; }

            let dotCai = roadCai[roadCai.length - 1];
            let dotCon = roadCon[roadCon.length - 1];
            if(dotCai === expectedDot && dotCon !== expectedDot) return 'C';
            if(dotCon === expectedDot && dotCai !== expectedDot) return 'O';
            return null; 
        }

        const ALGORITHMS = [
            { id: 'M1', name: 'Bám Xu Hướng', predict: (str) => { if(str.length < 2) return null; let last = str.slice(-1); let prev = str.slice(-2, -1); return last === prev ? last : getOpposite(last); } },
            { id: 'M2', name: 'Đo Nhịp Ping-Pong', predict: (str) => { if(str.length < 2) return null; return getOpposite(str.slice(-1)); } },
            { id: 'M3', name: 'Bi Đặc (Big Eye)', predict: (str) => getDerivedPrediction(str, 1) },
            { id: 'M4', name: 'Bi Rỗng (Small)', predict: (str) => getDerivedPrediction(str, 2) },
            { id: 'M5', name: 'Phẩy Mưa (Cockroach)', predict: (str) => getDerivedPrediction(str, 3) },
            { id: 'M6', name: 'So Khớp Đối Xứng', predict: (str) => { if(str.length < 4) return null; let last3 = str.slice(-3); if(last3 === "COC") return "O"; if(last3 === "OCO") return "C"; let last4 = str.slice(-4); if(last4 === "COOC") return "O"; if(last4 === "OCCO") return "C"; return null; } },
            { id: 'M7', name: 'Chuyển Pha (Phase)', predict: (str) => { let chunks = str.match(/(C+|O+)/g) || []; if(chunks.length < 3) return null; let avg = chunks.slice(-5).reduce((a, b) => a + b.length, 0) / Math.min(5, chunks.length); if(avg < 1.6) return getOpposite(str.slice(-1)); return str.slice(-1); } },
            { id: 'M8', name: 'Chuỗi Markov', predict: (str) => { if(str.length < 5) return null; let cc=0, co=0, oc=0, oo=0; for(let i=0; i<str.length-1; i++) { let pair = str.slice(i, i+2); if(pair==='CC') cc++; if(pair==='CO') co++; if(pair==='OC') oc++; if(pair==='OO') oo++; } let last = str.slice(-1); if(last === 'C') return cc >= co ? 'C' : 'O'; return oo >= oc ? 'O' : 'C'; } },
            { id: 'M9', name: 'Cộng Hưởng Màu', predict: (str) => { if(str.length < 4) return null; return str.slice(-3, -2); } },
            { id: 'M10', name: 'Vị Thế Bài Matrix', predict: (str) => {
                if (detailedHistory.length === 0) return null;
                let last = detailedHistory[detailedHistory.length - 1];
                if (!last || last.scoreCon === null || last.scoreCai === null) return null;
                let key = `${last.scoreCon}_${last.scoreCai}_${last.cardsCon}_${last.cardsCai}`;
                let match = VI_MATRIX[key];
                return match ? match.pred : null;
            }}
        ];

        function getExplanation(id, pred, pureStr, customIndex = null) {
            if (!pred) return '-';
            let pStr = pred === 'C' ? 'Cái' : 'Con';
            if (id === 'M10') {
                let idx = (customIndex !== null) ? customIndex : (detailedHistory.length - 1);
                let info = detailedHistory[idx];
                if (!info || info.scoreCon === null || info.scoreCai === null) return 'Chưa đủ data điểm số';
                let key = `${info.scoreCon}_${info.scoreCai}_${info.cardsCon}_${info.cardsCai}`;
                let match = VI_MATRIX[key];
                if (match) return `Thế bài ${info.scoreCon}-${info.scoreCai} (${info.cardsCon}L-${info.cardsCai}L) chốt ${pStr} ${match.pct} ${match.note || ''}`;
                return `Vị thế ${info.scoreCon}-${info.scoreCai} chưa kích hoạt`;
            }
            switch (id) {
                case 'M1': return `Đang bệt, theo đuôi ${pStr}`;
                case 'M2': return `Nhịp ngắn, bẻ cầu sang ${pStr}`;
                case 'M3': return `Cột 1 (Big Eye) thuận nhịp, chốt ${pStr}`;
                case 'M4': return `Cột 2 (Small Road) thuận nhịp, chốt ${pStr}`;
                case 'M5': return `Cột 3 (Cockroach) thuận nhịp, chốt ${pStr}`;
                case 'M6': return `Khớp form đối xứng ra ${pStr}`;
                case 'M7': return `Đo pha nhịp thở ngả về ${pStr}`;
                case 'M8': return `Cặp màu Markov nghiêng về ${pStr}`;
                case 'M9': return `Nhịp 3 tay trước chốt ${pStr}`;
                default: return '';
            }
        }

        let cauHistory = [];
        let detailedHistory = []; 
        let methodsState = ALGORITHMS.map(algo => ({ ...algo, history: [], currentPred: null, explainText: '-' }));
        let masterBets = []; 
        let currentMasterSignal = null; 
        let isLiveMode = false;
        let previewDot = null; 
        let isForcedVirtual = false; 

        document.addEventListener('keydown', function(event) {
            if(document.activeElement.id === 'bulk-input') return;
            if (event.key === 'ArrowLeft') { event.preventDefault(); addCau('con'); } 
            else if (event.key === 'ArrowRight') { event.preventDefault(); addCau('cai'); } 
            else if (event.key === 'ArrowDown') { event.preventDefault(); addCau('hoa'); } 
            else if (event.key === 'Backspace') { event.preventDefault(); undoCau(); }
        });

        function toggleHardStop() {
            isHardStopEnabled = !isHardStopEnabled;
            const btn = document.getElementById('btn-hardstop');
            if (isHardStopEnabled) {
                btn.innerHTML = '🛑 D.LỖ<br>BẬT';
                btn.className = 'bg-red-900/50 text-red-300 px-1.5 py-1.5 rounded text-[8px] font-bold border border-red-600 active:scale-95 whitespace-nowrap leading-tight';
            } else {
                btn.innerHTML = '⭕ D.LỖ<br>TẮT';
                btn.className = 'bg-gray-700 text-gray-400 px-1.5 py-1.5 rounded text-[8px] font-bold border border-gray-600 active:scale-95 whitespace-nowrap leading-tight';
            }
            recalculateState();
            updateUI();
        }

        function toggleShield() {
            isShieldEnabled = !isShieldEnabled;
            const btn = document.getElementById('btn-shield');
            if (isShieldEnabled) {
                btn.innerHTML = '🛡️ CHẮN<br>BẬT';
                btn.className = 'bg-purple-900/50 text-purple-300 px-1.5 py-1.5 rounded text-[8px] font-bold border border-purple-600 active:scale-95 whitespace-nowrap leading-tight';
            } else {
                btn.innerHTML = '🛡️ CHẮN<br>TẮT';
                btn.className = 'bg-gray-700 text-gray-400 px-1.5 py-1.5 rounded text-[8px] font-bold border border-gray-600 active:scale-95 whitespace-nowrap leading-tight';
            }
            updateUI();
        }

        function showPreview(val) {
            previewDot = val;
            redrawRoads();
            scrollToRight();
        }
        function clearPreview() {
            previewDot = null;
            redrawRoads();
        }

        function scrollToRight() {
            setTimeout(() => {
                const scrollIds = ['master-history-scroll', 'draw-main-scroll', 'draw-m3-scroll', 'draw-m4-scroll', 'draw-m5-scroll'];
                scrollIds.forEach(id => {
                    let el = document.getElementById(id);
                    if(el) el.scrollLeft = el.scrollWidth;
                });
            }, 15);
        }

        function processBulkData() {
            let inputField = document.getElementById('bulk-input');
            let rawData = inputField.value.toUpperCase();
            let processedData = rawData.replace(/B/g, 'C').replace(/P/g, 'O').replace(/H/g, 'T');
            let validChars = processedData.replace(/[^COT]/g, '');
            if(validChars.length === 0) { alert("Chuỗi không hợp lệ!"); return; }
            for(let i = 0; i < validChars.length; i++) {
                let type = validChars[i] === 'C' ? 'cai' : (validChars[i] === 'O' ? 'con' : 'hoa');
                addCau(type);
            }
            inputField.value = ''; 
        }

        function toggleLiveMode() {
            isLiveMode = document.getElementById('live-toggle').checked;
            let textSpan = document.getElementById('live-mode-text');
            if (isLiveMode) {
                textSpan.innerText = "ĐANG VÀO LỆNH THỰC TẾ";
                textSpan.className = "text-[9px] font-black text-green-400 uppercase";
            } else {
                textSpan.innerText = "Mô phỏng Nhập Data";
                textSpan.className = "text-[9px] font-bold text-gray-400 uppercase";
            }
            updateUI();
        }

        function recalculateState() {
            isHardStop = false;
            hardStopReason = "";
            consecutiveRealLosses = 0;
            totalPnL = 0;
            liveBetRecords = [];

            for (let i = 0; i < masterBets.length; i++) {
                let b = masterBets[i];
                if (b.type === 'SKIP' || b.type === 'VIRTUAL_WIN' || b.type === 'VIRTUAL_LOSS' || b.type === 'TIE') continue;

                if (b.type === 'REAL_WIN') {
                    consecutiveRealLosses = 0;
                    totalPnL += 5;
                    liveBetRecords.push({ result: 'W', vol: 5 });
                } else if (b.type === 'REAL_LOSS') {
                    consecutiveRealLosses++;
                    totalPnL -= 5;
                    liveBetRecords.push({ result: 'L', vol: 5 });

                    if (isHardStopEnabled && consecutiveRealLosses >= 3) {
                        isHardStop = true;
                        hardStopReason = "CHỐT LỖ: Gãy thông 3 lệnh! Hãy đổi bàn.";
                    }
                }
            }
        }

        function addCau(type) {
            let sCon = (vConScore !== null) ? vConScore : 0;
            let sCai = (vCaiScore !== null) ? vCaiScore : 0;
            let cCon = vConCards;
            let cCai = vCaiCards;

            let actual = type === 'cai' ? 'C' : (type === 'con' ? 'O' : 'T');
            let isRealHand = (actual === 'C' || actual === 'O');

            detailedHistory.push({ scoreCon: sCon, scoreCai: sCai, cardsCon: cCon, cardsCai: cCai });

            if (isRealHand) {
                methodsState.forEach(m => {
                    if (m.currentPred === 'C' || m.currentPred === 'O') {
                        m.history.push(m.currentPred === actual);
                        if (m.history.length > 30) m.history.shift(); 
                    }
                });
            }

            if (currentMasterSignal !== null && currentMasterSignal !== undefined) {
                if (actual === 'T') {
                    masterBets.push({ type: 'TIE', pred: currentMasterSignal, details: {sCon, sCai, cCon, cCai} });
                } else {
                    let isWin = (currentMasterSignal === actual);
                    if (isHardStop) {
                        masterBets.push({ type: 'SKIP', pred: null, details: {sCon, sCai, cCon, cCai} });
                    } else {
                        if (isLiveMode && !isForcedVirtual) masterBets.push({ type: isWin ? 'REAL_WIN' : 'REAL_LOSS', pred: currentMasterSignal, details: {sCon, sCai, cCon, cCai} });
                        else masterBets.push({ type: isWin ? 'VIRTUAL_WIN' : 'VIRTUAL_LOSS', pred: currentMasterSignal, details: {sCon, sCai, cCon, cCai} });
                    }
                }
            } else {
                masterBets.push({ type: 'SKIP', pred: null, details: {sCon, sCai, cCon, cCai} });
            }

            cauHistory.push(actual);
            let pureStr = cauHistory.filter(x => x !== 'T').join('');

            if (isRealHand) {
                methodsState.forEach(m => {
                    if(m.id === 'M10') {
                        let lastInfo = detailedHistory[detailedHistory.length - 1];
                        let key = `${lastInfo.scoreCon}_${lastInfo.scoreCai}_${lastInfo.cardsCon}_${lastInfo.cardsCai}`;
                        let match = VI_MATRIX[key];
                        m.currentPred = match ? match.pred : null;
                        m.explainText = getExplanation(m.id, m.currentPred, pureStr);
                    } else {
                        m.currentPred = m.predict(pureStr); 
                        m.explainText = getExplanation(m.id, m.currentPred, pureStr);
                    }
                });
            }
            
            recalculateState();
            updateUI();
        }

        function undoCau() {
            if (cauHistory.length === 0) return;
            cauHistory.pop();
            detailedHistory.pop(); 
            let newHistory = [...cauHistory];
            masterBets.pop();

            methodsState.forEach(m => { m.history = []; m.currentPred = null; m.explainText = '-'; });
            let pureSoFar = "";
            for(let i=0; i<newHistory.length; i++) {
                let char = newHistory[i];
                if (char === 'T') continue;

                methodsState.forEach(m => {
                    if (m.currentPred === 'C' || m.currentPred === 'O') {
                        m.history.push(m.currentPred === char);
                        if (m.history.length > 30) m.history.shift();
                    }
                });
                pureSoFar += char;
                methodsState.forEach(m => {
                    if (m.id === 'M10') {
                        let roundInfo = detailedHistory[i];
                        if(roundInfo) {
                            let key = `${roundInfo.scoreCon}_${roundInfo.scoreCai}_${roundInfo.cardsCon}_${roundInfo.cardsCai}`;
                            let match = VI_MATRIX[key];
                            m.currentPred = match ? match.pred : null;
                            m.explainText = getExplanation(m.id, m.currentPred, pureSoFar, i);
                        }
                    } else {
                        m.currentPred = m.predict(pureSoFar);
                        m.explainText = getExplanation(m.id, m.currentPred, pureSoFar);
                    }
                });
            }
            recalculateState();
            updateUI();
        }

        function redrawRoads() {
            drawMainDots();
            drawDerivedRoad('draw-m3', 1, 'hollow');
            drawDerivedRoad('draw-m4', 2, 'solid');
            drawDerivedRoad('draw-m5', 3, 'slash');
        }

        function updateUI() {
            redrawRoads();
            evaluateAndRender();
            renderMasterHistory();
            scrollToRight();
        }

        function drawMainDots() {
            const container = document.getElementById('draw-main');
            container.innerHTML = '';
            
            let mappedData = cauHistory.map(val => ({ val, isPreview: false }));
            if (previewDot) mappedData.push({ val: previewDot, isPreview: true });

            let consolidated = [];
            let tempTies = 0;
            mappedData.forEach(item => {
                if (item.val === 'T') {
                    if (consolidated.length === 0) tempTies++;
                    else consolidated[consolidated.length - 1].ties = (consolidated[consolidated.length - 1].ties || 0) + 1;
                } else {
                    consolidated.push({ val: item.val, isPreview: item.isPreview, ties: tempTies });
                    tempTies = 0;
                }
            });
            if (consolidated.length === 0 && tempTies > 0) {
                consolidated.push({ val: 'T', isPreview: false, ties: tempTies });
            }

            let cols = []; 
            let currentGroup = [];
            let lastVal = null;

            consolidated.forEach(item => {
                if (item.val === 'T') {
                    currentGroup.push(item);
                } else {
                    if (lastVal === null) {
                        if (currentGroup.length > 0 && currentGroup[0].val === 'T') {
                            item.ties += currentGroup[0].ties;
                            currentGroup[0] = item;
                        } else {
                            currentGroup.push(item);
                        }
                    } else if (lastVal === item.val) {
                        currentGroup.push(item);
                    } else {
                        cols.push(currentGroup);
                        currentGroup = [item];
                    }
                    lastVal = item.val;
                }
            });
            if(currentGroup.length > 0) cols.push(currentGroup);

            let grid = Array.from({length: 6}, () => Array(200).fill(null));
            let maxCol = 0;
            for(let i = 0; i < cols.length; i++) {
                let colData = cols[i];
                let startC = i;
                while(grid[0][startC] !== null) startC++; 
                let r = 0;
                let c = startC;
                for(let j = 0; j < colData.length; j++) {
                    grid[r][c] = colData[j];
                    maxCol = Math.max(maxCol, c);
                    if (r + 1 < 6 && grid[r+1][c] === null) r++; 
                    else c++; 
                }
            }

            let totalColsToDraw = Math.max(18, maxCol + 2);
            for (let c = 0; c < totalColsToDraw; c++) {
                let colDiv = document.createElement('div');
                colDiv.className = 'flex flex-col flex-shrink-0';
                for (let r = 0; r < 6; r++) {
                    let cellDiv = document.createElement('div');
                    cellDiv.className = 'w-5 h-5 flex-shrink-0 flex items-center justify-center border-r border-b border-gray-700/50 relative';
                    let item = grid[r][c];
                    
                    if (item) {
                        let dotContainer = document.createElement('div');
                        dotContainer.className = 'relative flex items-center justify-center w-full h-full';

                        if (item.val === 'T') {
                            let line = document.createElement('div');
                            line.className = 'absolute w-[14px] h-[2.5px] bg-green-500 transform -rotate-45';
                            dotContainer.appendChild(line);
                            if (item.ties > 1) {
                                let tCount = document.createElement('span');
                                tCount.className = 'absolute text-[7px] text-green-400 font-bold z-10 bg-gray-900/80 rounded-full w-3 h-3 flex items-center justify-center';
                                tCount.innerText = item.ties;
                                dotContainer.appendChild(tCount);
                            }
                        } else {
                            let dot = document.createElement('div');
                            let colorClass = item.val === 'C' ? 'border-red-500 bg-red-900/50' : 'border-blue-500 bg-blue-900/50';
                            if (item.isPreview) colorClass += ' preview-blink';
                            dot.className = `w-[14px] h-[14px] rounded-full border-[2.5px] ${colorClass} shadow-sm`;
                            dotContainer.appendChild(dot);

                            if (item.ties > 0) {
                                let line = document.createElement('div');
                                line.className = 'absolute w-[16px] h-[2.5px] bg-green-500 transform -rotate-45 z-10 shadow-sm border-[0.5px] border-gray-900 rounded-sm';
                                dotContainer.appendChild(line);
                                if (item.ties > 1) {
                                    let tCount = document.createElement('span');
                                    tCount.className = 'absolute -bottom-0.5 -right-0.5 text-[7px] text-green-400 font-black z-20 bg-gray-900 rounded-full w-3 h-3 flex items-center justify-center border border-green-500 leading-none';
                                    tCount.innerText = item.ties;
                                    dotContainer.appendChild(tCount);
                                }
                            }
                        }
                        cellDiv.appendChild(dotContainer);
                    }
                    colDiv.appendChild(cellDiv);
                }
                container.appendChild(colDiv);
            }
        }

        function drawDerivedRoad(containerId, offset, styleType) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            let str = cauHistory.filter(x => x !== 'T').join('');
            let rawData = getDerivedRoad(str, offset).map(val => ({ val, isPreview: false }));

            if (previewDot) {
                let previewStr = str + previewDot;
                let previewRawData = getDerivedRoad(previewStr, offset);
                if (previewRawData.length > rawData.length) {
                    rawData.push({ val: previewRawData[previewRawData.length - 1], isPreview: true });
                }
            }

            if(rawData.length === 0) return;

            let cols = [];
            let currentGroup = [];
            rawData.forEach(item => {
                if(currentGroup.length === 0) currentGroup.push(item);
                else if(currentGroup[0].val === item.val) currentGroup.push(item);
                else { cols.push(currentGroup); currentGroup = [item]; }
            });
            if(currentGroup.length > 0) cols.push(currentGroup);

            let grid = Array.from({length: 6}, () => Array(200).fill(null));
            let maxCol = 0;
            for(let i = 0; i < cols.length; i++) {
                let colData = cols[i];
                let startC = i;
                while(grid[0][startC] !== null) startC++; 
                let r = 0;
                let c = startC;
                for(let j = 0; j < colData.length; j++) {
                    grid[r][c] = colData[j];
                    maxCol = Math.max(maxCol, c);
                    if (r + 1 < 6 && grid[r+1][c] === null) r++; 
                    else c++; 
                }
            }

            let totalColsToDraw = Math.max(10, maxCol + 2);
            for (let c = 0; c < totalColsToDraw; c++) {
                let colDiv = document.createElement('div');
                colDiv.className = 'flex flex-col flex-shrink-0';
                for (let r = 0; r < 6; r++) {
                    let cellDiv = document.createElement('div');
                    cellDiv.className = 'w-[10px] h-[10px] flex-shrink-0 flex items-center justify-center border-r border-b border-gray-700/50';
                    let item = grid[r][c];
                    if (item) {
                        let val = item.val;
                        let pClass = item.isPreview ? 'preview-blink' : '';
                        if (styleType === 'hollow') {
                            cellDiv.innerHTML = `<div class="w-[6px] h-[6px] rounded-full border-[1.5px] ${val === 'R' ? 'border-red-500' : 'border-blue-500'} bg-transparent ${pClass}"></div>`;
                        } else if (styleType === 'solid') {
                            cellDiv.innerHTML = `<div class="w-[6px] h-[6px] rounded-full ${val === 'R' ? 'bg-red-500' : 'bg-blue-500'} ${pClass}"></div>`;
                        } else if (styleType === 'slash') {
                            cellDiv.innerHTML = `<div class="w-[8px] h-[8px] overflow-hidden relative ${pClass}"><div class="absolute w-[12px] h-[1.5px] ${val === 'R' ? 'bg-red-500' : 'bg-blue-500'} top-[3px] -left-[2px] transform rotate-45"></div></div>`;
                        }
                    }
                    colDiv.appendChild(cellDiv);
                }
                container.appendChild(colDiv);
            }
        }

        function evaluateAndRender() {
            let pureStr = cauHistory.filter(x => x !== 'T').join('');
            let chunks = pureStr.match(/(C+|O+)/g) || [];
            let numCols = chunks.length;

            if (isHardStop) currentMasterSignal = null;

            if (numCols < 2) {
                currentMasterSignal = null;
                renderDefault();
                return;
            }

            let recentLosses = 0;
            for(let i = masterBets.length - 1; i >= 0; i--) {
                let bt = masterBets[i].type;
                if (bt === 'SKIP' || bt === 'TIE') continue;
                if (bt === 'REAL_LOSS' || bt === 'VIRTUAL_LOSS') recentLosses++;
                if (bt === 'REAL_WIN' || bt === 'VIRTUAL_WIN') break;
            }
            isForcedVirtual = (recentLosses >= 2);

            let rankedMethods = methodsState.map(m => {
                let hasCommand = (m.currentPred === 'C' || m.currentPred === 'O');
                let totalHands = m.history.length;
                let totalWins = m.history.filter(w => w).length;
                let macroWR = totalHands > 0 ? Math.round((totalWins / totalHands) * 100) : 0;

                let framesToTest = [2, 3, 4];
                let frameResults = [];

                framesToTest.forEach(f => {
                    if (numCols >= f) {
                        let handsInFrame = chunks.slice(-f).join('').length;
                        if (m.history.length >= handsInFrame && handsInFrame > 0) {
                            let frameHistory = m.history.slice(-handsInFrame);
                            let wins = frameHistory.filter(w => w).length;
                            let frameWR = Math.round((wins / handsInFrame) * 100);
                            let delta = frameWR - macroWR;
                            let lastHandWon = frameHistory.length > 0 ? frameHistory[frameHistory.length - 1] === true : false;
                            let isMomentumGood = (delta > 0) || (frameWR >= 60);
                            let isQualified = (frameWR > 39) && isMomentumGood && lastHandWon && hasCommand;
                            frameResults.push({ frame: f, hands: handsInFrame, wr: frameWR, delta: delta, isQualified: isQualified, lastHandWon: lastHandWon });
                        }
                    }
                });

                let selectedFrame = null;
                if (frameResults.length > 0) selectedFrame = frameResults.reduce((prev, curr) => (prev.wr > curr.wr) ? prev : curr);
                if (!selectedFrame) return { ...m, wr: 0, total: 0, delta: 0, isQualified: false, frameInfo: 'Chờ Cột' };
                return { ...m, wr: selectedFrame.wr, total: selectedFrame.hands, delta: selectedFrame.delta, isQualified: selectedFrame.isQualified, frameInfo: `${selectedFrame.frame} Cột` };
            });

            rankedMethods.sort((a, b) => {
                if (a.isQualified && !b.isQualified) return -1;
                if (!a.isQualified && b.isQualified) return 1;
                if (b.delta !== a.delta) return b.delta - a.delta; 
                return b.wr - a.wr;
            });

            let qualifiedMethods = rankedMethods.filter(m => m.isQualified);
            let votesC = qualifiedMethods.filter(m => m.currentPred === 'C').length;
            let votesO = qualifiedMethods.filter(m => m.currentPred === 'O').length;

            let m3 = qualifiedMethods.find(m => m.id === 'M3');
            let m4 = qualifiedMethods.find(m => m.id === 'M4');
            let m5 = qualifiedMethods.find(m => m.id === 'M5');
            let isGoldenSignal = (m3 && m4 && m5 && m3.currentPred && m3.currentPred === m4.currentPred && m4.currentPred === m5.currentPred);
            let goldenSignalTarget = isGoldenSignal ? m3.currentPred : null;

            currentMasterSignal = null;
            let winningVotes = 0;

            if (!isHardStop) {
                if (isGoldenSignal) {
                    currentMasterSignal = goldenSignalTarget;
                    winningVotes = (goldenSignalTarget === 'C') ? votesC : votesO;
                } else {
                    if (votesC > votesO && votesC >= 2) { currentMasterSignal = 'C'; winningVotes = votesC; }
                    else if (votesO > votesC && votesO >= 2) { currentMasterSignal = 'O'; winningVotes = votesO; }
                }
            }

            const alertsBox = document.getElementById('core-alerts');
            alertsBox.innerHTML = '';
            alertsBox.className = 'hidden flex-col gap-1 mt-1 mb-1';

            let m1 = qualifiedMethods.find(m => m.id === 'M1');
            let m2 = qualifiedMethods.find(m => m.id === 'M2');
            if (m1 && m2 && m1.currentPred !== m2.currentPred) {
                alertsBox.innerHTML += `<div class="text-[9px] font-bold text-yellow-600 bg-yellow-900/30 border border-yellow-700 px-2 py-1 rounded">⚠️ RỦI RO TRÁI CHIỀU: Phe Trend và Ping-Pong đang đá phiếu nhau.</div>`;
                alertsBox.classList.remove('hidden');
                alertsBox.classList.add('flex');
            }

            let hasValidSignal = (currentMasterSignal !== null);
            let isCircuitBreaker = false;
            let breakerMsg = "";

            if (!isHardStop && hasValidSignal && pureStr.length > 0) {
                let matchEnds = pureStr.match(/(C+|O+)$/);
                let lastChar = pureStr.slice(-1);
                let currentBiet = matchEnds ? matchEnds[0].length : 0;
                
                let currentPP = 0;
                for(let i = pureStr.length - 1; i >= 1; i--) {
                    if(pureStr[i] !== pureStr[i-1]) currentPP++;
                    else break;
                }
                let ppLength = currentPP + 1;

                let maxBiet = chunks.length > 1 ? Math.max(...chunks.slice(0, -1).map(c => c.length)) : 0;
                let avgBiPerCol = chunks.length > 0 ? (pureStr.length / chunks.length) : 0;
                let isThienBiet = avgBiPerCol >= 1.8;
                let isThienNhay = avgBiPerCol <= 1.4;

                let disagreeCount = [m3, m4, m5].filter(m => m && m.isQualified && m.currentPred && m.currentPred !== currentMasterSignal).length;

                let isDangerousBiet = false;
                let isDangerousPP = false;
                let isDiverging = false;
                
                let predictedToBiet = (currentMasterSignal === lastChar);
                let predictedToPP = (currentMasterSignal !== lastChar);

                if ((currentBiet >= 3 || ppLength >= 4) && disagreeCount >= 2) {
                    isDiverging = true;
                    breakerMsg = `BỘ NGẮT MẠCH (LỚP 3): Rủi ro gãy cầu! Các Cầu Phụ đang phân kỳ.`;
                }
                else if (isThienNhay && predictedToBiet && currentBiet >= 3) {
                    isDangerousBiet = true;
                    breakerMsg = `BỘ NGẮT MẠCH (LỚP 2): Bàn Thiên Nhảy. Chặn rủi ro đu bệt ở tay ${currentBiet + 1}!`;
                }
                else if (predictedToBiet && currentBiet >= maxBiet && maxBiet >= 3 && !isThienBiet) {
                    isDangerousBiet = true;
                    breakerMsg = `BỘ NGẮT MẠCH (LỚP 1): Chạm đỉnh lịch sử! Từ chối đu bệt tay ${currentBiet + 1}.`;
                }
                else if (predictedToPP && ppLength >= 5 && !isThienNhay) {
                    isDangerousPP = true;
                    breakerMsg = `BỘ NGẮT MẠCH (LỚP 1): Ping-Pong tay ${ppLength}. Từ chối đu PP!`;
                }

                if (isDiverging || isDangerousBiet || isDangerousPP) {
                    if (isShieldEnabled) {
                        isCircuitBreaker = true;
                        currentMasterSignal = null;
                        hasValidSignal = false;
                    } else {
                        alertsBox.innerHTML += `<div class="text-[9px] font-bold text-orange-400 bg-orange-900/30 border border-orange-700 px-2 py-1 rounded">⚠️ CẢNH BÁO: ${breakerMsg} (Lá Chắn Đang Tắt)</div>`;
                        alertsBox.classList.remove('hidden');
                        alertsBox.classList.add('flex');
                    }
                }
            }

            const masterBox = document.getElementById('master-box');
            const masterDecision = document.getElementById('master-decision');
            const masterReason = document.getElementById('master-reason');
            const gateStatus = document.getElementById('gate-status');

            // --- VỊ NHỊP TRƯỚC ---
            let viReason = "";
            if (detailedHistory.length > 0) {
                let lastInfo = detailedHistory[detailedHistory.length - 1];
                if (lastInfo && lastInfo.scoreCon !== null && lastInfo.scoreCai !== null) {
                    let key = `${lastInfo.scoreCon}_${lastInfo.scoreCai}_${lastInfo.cardsCon}_${lastInfo.cardsCai}`;
                    let match = VI_MATRIX[key];
                    if (match) {
                        let pStr = match.pred === 'C' ? 'CÁI' : 'CON';
                        let pColor = match.pred === 'C' ? 'text-red-400' : 'text-blue-400';
                        viReason = `<div class="text-[13px] leading-tight mt-1.5 p-2 bg-gray-900 border border-gray-600 rounded">Vị Nhịp Trước <span class="font-black text-gray-300">(${lastInfo.scoreCon}-${lastInfo.scoreCai} | ${lastInfo.cardsCon}L-${lastInfo.cardsCai}L)</span> ➔ Báo <span class="${pColor} font-black text-[14px]">${pStr}</span> Win <span class="text-yellow-400 font-black text-[14px] bg-gray-800 px-1 rounded shadow-sm border border-gray-600">${match.pct}</span> ${match.note || ''}</div>`;
                    } else {
                        viReason = `<div class="text-[11px] leading-tight mt-1.5 p-1.5 bg-gray-900 border border-gray-700 rounded text-gray-400">Vị Nhịp Trước <span class="font-bold">(${lastInfo.scoreCon}-${lastInfo.scoreCai} | ${lastInfo.cardsCon}L-${lastInfo.cardsCai}L)</span> ➔ Không có data</div>`;
                    }
                }
            }

            if (isHardStop) {
                gateStatus.innerHTML = "CHỐT GIAO DỊCH (HARD STOP)";
                gateStatus.className = "text-[10px] font-black uppercase tracking-widest text-white bg-red-600 px-2 py-0.5 rounded border border-red-700 inline-block mb-1";
                masterDecision.innerHTML = "NGƯNG LỆNH";
                masterDecision.className = "text-3xl font-black text-red-500 drop-shadow-sm flex items-center justify-center leading-none";
                masterReason.innerHTML = hardStopReason;
                masterBox.className = "bg-red-900/10 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 border-red-600";
            }
            else if (isCircuitBreaker) {
                gateStatus.innerHTML = "KÍCH HOẠT LÁ CHẮN PHÒNG NGỰ";
                gateStatus.className = "text-[10px] font-black uppercase tracking-widest text-purple-300 bg-purple-900/40 px-2 py-0.5 rounded border border-purple-600 inline-block mb-1";
                masterDecision.innerHTML = "DỪNG LẠI";
                masterDecision.className = "text-3xl font-black text-purple-400 drop-shadow-sm flex items-center justify-center leading-none";
                masterReason.innerHTML = breakerMsg;
                masterBox.className = "bg-gray-800 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 breaker-signal border-purple-600";
            }
            else if (hasValidSignal) {
                let signalColor = currentMasterSignal === 'C' ? 'text-red-500' : 'text-blue-500';

                if (!isLiveMode || isForcedVirtual) {
                    gateStatus.innerHTML = "CHẾ ĐỘ MÔ PHỎNG NGẦM (TÌM vW)";
                    if (isForcedVirtual) {
                        gateStatus.className = "text-[10px] font-black uppercase tracking-widest text-orange-300 bg-orange-900/50 px-2 py-0.5 rounded border border-orange-600 inline-block mb-1";
                        masterReason.innerHTML = `Hệ thống lệch nhịp (Thua ${recentLosses} tay). Ép tìm vW!${viReason}`;
                        masterBox.className = "bg-gray-800 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 cooldown-signal border-orange-600";
                        masterDecision.className = `text-3xl font-black drop-shadow-sm opacity-90 flex items-center justify-center text-orange-500 leading-none`;
                    } else {
                        gateStatus.className = "text-[10px] font-black uppercase tracking-widest text-yellow-400 bg-yellow-900/40 px-2 py-0.5 rounded border border-yellow-600 inline-block mb-1";
                        masterReason.innerHTML = `Chờ Win ngầm (vW) để tìm nhịp vào Live tốt nhất.${viReason}`;
                        masterBox.className = "bg-gray-800 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 cooldown-signal border-yellow-600";
                        masterDecision.className = `text-3xl font-black drop-shadow-sm opacity-80 flex items-center justify-center text-yellow-500 leading-none`;
                    }
                    masterDecision.innerHTML = currentMasterSignal === 'C' ? "(MÔ PHỎNG) CÁI" : "(MÔ PHỎNG) CON";
                } else {
                    gateStatus.innerHTML = "";
                    gateStatus.className = "hidden";
                    
                    masterDecision.innerHTML = `${currentMasterSignal === 'C' ? 'ĐÁNH CÁI' : 'ĐÁNH CON'} <span class="text-[20px] ml-1 text-green-400 font-black tracking-tighter bg-green-900 px-1.5 py-0.5 rounded border border-green-600">Đi 5%</span>`;
                    masterDecision.className = `text-3xl font-black flex items-center justify-center drop-shadow-sm leading-none ${signalColor}`;
                    masterReason.innerHTML = viReason;
                    
                    masterBox.className = "bg-gray-800 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 safe-signal border-green-600";
                }
            } else {
                gateStatus.innerHTML = "LỆNH BỊ KHÓA (KHÔNG ĐẠT ĐIỀU KIỆN)";
                gateStatus.className = `text-[10px] font-black uppercase tracking-widest px-2 py-0.5 rounded border text-red-400 bg-red-900/20 border-red-700 inline-block mb-1`;
                masterDecision.innerHTML = "QUAN SÁT";
                masterDecision.className = "text-3xl font-black text-gray-500 drop-shadow-sm flex items-center justify-center leading-none";
                
                masterReason.innerHTML = `Hệ thống chưa tìm thấy lệnh an toàn để vào tiền.${viReason}`;
                masterBox.className = `bg-gray-800 border-2 rounded-xl p-2 my-2 shadow-md transition-all duration-300 locked-signal border-red-700`;
            }

            const grid = document.getElementById('methods-grid');
            grid.innerHTML = '';
            
            rankedMethods.forEach((m, index) => {
                let isSelected = hasValidSignal && m.isQualified && (m.currentPred === currentMasterSignal);
                let isDimmed = !m.isQualified; 
                let predText = m.currentPred === 'C' ? '<span class="text-red-500 font-black">CÁI</span>' : (m.currentPred === 'O' ? '<span class="text-blue-500 font-black">CON</span>' : '<span class="text-gray-500">CHỜ</span>');
                let deltaText = m.delta > 0 ? `+${m.delta}%` : `${m.delta}%`;
                let trendIcon = '';
                let trendClass = 'text-gray-500';
                
                if (m.total > 0) {
                    if (!m.isQualified) { trendClass = 'text-gray-500'; trendIcon = '🚫'; } 
                    else { trendClass = 'text-green-400'; trendIcon = (m.delta <= 0) ? '🔥' : '📈'; }
                } else { deltaText = 'Chờ Data'; trendClass = 'text-gray-500 text-[8px] italic'; }

                let recent6_html = m.history.slice(-6).map(w => w ? '<span class="text-green-500 font-black tracking-tight">W</span>' : '<span class="text-red-500 font-bold tracking-tight">L</span>').join(' ');
                
                let boxClass = 'bg-gray-800 border border-gray-700';
                if (isSelected && !isHardStop && !isCircuitBreaker) boxClass = 'border-[1.5px] border-green-500 bg-green-900/30 shadow-sm';
                if (isDimmed) boxClass += ' opacity-[0.45] grayscale';

                let html = `
                    <div class="method-card p-1 rounded flex flex-col justify-between ${boxClass}">
                        <div class="flex justify-between items-start mb-0.5">
                            <span class="text-[9px] font-bold text-gray-300 leading-tight w-[65%] truncate">${index+1}. ${m.name}</span>
                            <span class="text-[9px] font-black ${trendClass}">${deltaText} ${trendIcon}</span>
                        </div>
                        <div class="flex justify-between items-center border-t border-gray-700 pt-0.5">
                            <div class="text-[8px] flex gap-[2px]">${recent6_html || '-'}</div>
                            <div class="text-[8px] font-bold bg-gray-900 px-1 rounded shadow-sm border border-gray-700 flex gap-1 items-center">
                                <span class="text-indigo-400 text-[7px] font-black">${m.frameInfo}</span>
                                <span class="text-gray-600">|</span> Báo: ${predText}
                            </div>
                        </div>
                        <div class="text-[7px] text-gray-400 italic truncate w-full text-center mt-1 border-t border-gray-700 pt-0.5">${m.explainText}</div>
                    </div>
                `;
                grid.innerHTML += html;
            });
        }

        function renderMasterHistory() {
            const container = document.getElementById('master-history-container');
            let allBets = masterBets.filter(b => b.type !== 'SKIP'); 
            let realWins = liveBetRecords.filter(b => b.result === 'W').length;
            let totalReal = liveBetRecords.length;
            let wr = totalReal > 0 ? Math.round((realWins / totalReal) * 100) : 0;
            let pnlColor = totalPnL > 0 ? 'text-green-400' : (totalPnL < 0 ? 'text-red-400' : 'text-gray-400');
            let pnlSign = totalPnL > 0 ? '+' : '';

            if (allBets.length === 0) {
                container.innerHTML = `<div class="text-[10px] text-gray-500 italic text-center py-1">Chưa có lệnh nào được khớp.</div>`;
                return;
            }

            // GIAO DIỆN LỊCH SỬ 2 DÒNG MỚI THEO YÊU CẦU
            let historyHtml = allBets.map(b => {
                let sCon = (b.details && b.details.sCon !== null) ? b.details.sCon : '-';
                let sCai = (b.details && b.details.sCai !== null) ? b.details.sCai : '-';
                
                let bgCon = "bg-gray-800 text-gray-400 border-gray-700";
                let bgCai = "bg-gray-800 text-gray-400 border-gray-700";

                // Tô màu theo kết quả dự đoán tay đó
                if (b.pred === 'O') { // Báo Con
                    if (b.type === 'REAL_WIN') bgCon = "bg-green-600 text-white font-black border-green-400";
                    else if (b.type === 'REAL_LOSS') bgCon = "bg-red-600 text-white font-black border-red-400";
                    else if (b.type === 'VIRTUAL_WIN') bgCon = "bg-green-900/50 text-green-400 font-bold border-green-500 border-dashed";
                    else if (b.type === 'VIRTUAL_LOSS') bgCon = "bg-red-900/50 text-red-400 font-bold border-red-500 border-dashed";
                } else if (b.pred === 'C') { // Báo Cái
                    if (b.type === 'REAL_WIN') bgCai = "bg-green-600 text-white font-black border-green-400";
                    else if (b.type === 'REAL_LOSS') bgCai = "bg-red-600 text-white font-black border-red-400";
                    else if (b.type === 'VIRTUAL_WIN') bgCai = "bg-green-900/50 text-green-400 font-bold border-green-500 border-dashed";
                    else if (b.type === 'VIRTUAL_LOSS') bgCai = "bg-red-900/50 text-red-400 font-bold border-red-500 border-dashed";
                } else if (b.type === 'TIE') {
                    bgCon = "bg-gray-800 text-green-500 border-green-600/50 font-bold";
                    bgCai = "bg-gray-800 text-green-500 border-green-600/50 font-bold";
                }

                return `
                    <div class="flex flex-col gap-0.5 flex-shrink-0 w-6">
                        <div class="flex items-center justify-center h-[22px] text-[11px] rounded border ${bgCon}">${sCon}</div>
                        <div class="flex items-center justify-center h-[22px] text-[11px] rounded border ${bgCai}">${sCai}</div>
                    </div>
                `;
            }).join('');

            container.innerHTML = `
                <div class="flex justify-between items-center bg-gray-900 border border-gray-700 rounded px-2 py-1 mb-1.5">
                    <div class="text-[9px] font-black text-gray-300 uppercase flex gap-1 items-center">
                        <span>Lãi/Lỗ LIVE:</span>
                        <span class="text-[12px] ${pnlColor}">${pnlSign}${totalPnL}%</span>
                    </div>
                    <div class="text-[9px] font-bold text-gray-500">
                        (Live Winrate: ${wr}%)
                    </div>
                </div>
                <div class="flex gap-1 overflow-x-auto no-scrollbar w-full pb-1 pt-0.5 items-start relative" id="history-scroll-box">
                    <div class="flex flex-col gap-0.5 flex-shrink-0 sticky left-0 bg-[#1f2937] z-10 pr-1 border-r border-gray-700">
                        <div class="flex items-center justify-center h-[22px] text-[8px] font-bold text-blue-400">CON</div>
                        <div class="flex items-center justify-center h-[22px] text-[8px] font-bold text-red-400">CÁI</div>
                    </div>
                    ${historyHtml}
                </div>
            `;

            // Auto cuộn lịch sử mới
            setTimeout(() => {
                let hBox = document.getElementById('history-scroll-box');
                if(hBox) hBox.scrollLeft = hBox.scrollWidth;
            }, 10);
        }

        function renderDefault() {
            let pureStr = cauHistory.filter(x => x !== 'T').join('');
            let cols = pureStr.match(/(C+|O+)/g) || [];
            
            document.getElementById('gate-status').innerHTML = `HỆ THỐNG ĐANG GOM DATA... (${cols.length}/2 CỘT)`;
            document.getElementById('gate-status').className = "text-[10px] font-black uppercase tracking-widest text-gray-400 bg-gray-700 px-2 py-0.5 rounded border border-gray-600 inline-block mb-1";
            document.getElementById('master-decision').innerHTML = "CHỜ DATA";
            document.getElementById('master-decision').className = "text-3xl font-black text-gray-500 drop-shadow-sm flex items-center justify-center leading-none";
            document.getElementById('master-reason').innerHTML = `Cần tối thiểu 2 Cột (Không tính Hòa) để kích hoạt Ma Trận`;
            document.getElementById('master-box').className = "bg-gray-800 border-2 border-gray-600 rounded-xl p-2 my-2 shadow-md transition-all duration-300";
            document.getElementById('core-alerts').className = "hidden";

            const grid = document.getElementById('methods-grid');
            grid.innerHTML = methodsState.map((m, i) => {
                let predText = m.currentPred === 'C' ? '<span class="text-red-500 font-black">CÁI</span>' : (m.currentPred === 'O' ? '<span class="text-blue-500 font-black">CON</span>' : '<span class="text-gray-500">CHỜ</span>');
                return `
                <div class="p-1 border border-gray-700 rounded bg-gray-800 opacity-60">
                    <div class="flex justify-between items-start mb-0.5">
                        <span class="text-[9px] font-bold text-gray-400 leading-tight w-[65%] truncate">${i+1}. ${m.name}</span>
                        <span class="text-[10px] font-black text-gray-500">-%</span>
                    </div>
                    <div class="flex justify-between items-center border-t border-gray-700 pt-0.5">
                        <div class="text-[8px] text-gray-500">-</div>
                        <div class="text-[8px] font-bold text-gray-500">Báo: ${predText}</div>
                    </div>
                    <div class="text-[7px] text-gray-500 italic truncate w-full text-center mt-1 border-t border-gray-700 pt-0.5">Đang chờ nạp liệu...</div>
                </div>
            `}).join('');
            renderMasterHistory();
        }

        function confirmReset() { if(confirm("Xóa trắng Bàn này và làm lại từ đầu?")) resetCa(); }
        function resetCa() {
            cauHistory = []; detailedHistory = []; masterBets = []; currentMasterSignal = null; isForcedVirtual = false;
            methodsState.forEach(m => { m.history = []; m.currentPred = null; m.explainText = '-'; });
            vConScore = null; vCaiScore = null; vConCards = 3; vCaiCards = 3;
            renderViUI();
            recalculateState();
            updateUI();
        }

        renderViUI(); 
        renderDefault();
        drawMainDots(); 
        drawDerivedRoad('draw-m3', 1, 'hollow');
        drawDerivedRoad('draw-m4', 2, 'solid');
        drawDerivedRoad('draw-m5', 3, 'slash');
    </script>
</body>
</html>
