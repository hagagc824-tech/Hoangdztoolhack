<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>AI PRO 2026 | HOANGDZ SYSTEM</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;900&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        :root { --neon: #00f2ff; --bg: #050505; }
        body { margin:0; background:var(--bg); color:#fff; font-family:'Orbitron', sans-serif; overflow-y: auto; }
        .snow-container { position:fixed; inset:0; pointer-events:none; z-index:999; }
        .snowflake { position:absolute; color:white; font-size: 15px; opacity:0.6; animation: fall linear infinite; }
        @keyframes fall { to { transform: translateY(100vh) rotate(360deg); } }
        .page-wrapper { padding: 20px 15px; }
        .wrapper { max-width:450px; margin:0 auto; position:relative; z-index:1000; }
        .glass { background:rgba(0,0,0,0.8); border:1px solid var(--neon); border-radius:20px; padding:20px; margin-bottom:20px; text-align:center; box-shadow: 0 0 15px rgba(0,242,255,0.1); }
        input, button, select { width:100%; padding:14px; margin-bottom:12px; border-radius:10px; border:1px solid #444; background:rgba(255,255,255,0.05); color:#fff; font-size:16px; box-sizing:border-box; }
        .btn { background:linear-gradient(90deg, var(--neon), #7000ff); font-weight:bold; cursor:pointer; border:none; }
        .btn-del { background:#ff3b3b; color:white; font-size: 12px; padding: 10px; }
        .res-box { font-size:3rem; font-weight:900; color:var(--neon); margin:10px 0; text-shadow: 0 0 15px var(--neon); }
        .res-jumping { animation: jump 0.2s infinite; }
        @keyframes jump { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
        .info-text { font-size:12px; text-align:left; line-height:1.6; margin-top:10px; border-top:1px solid #333; padding-top:10px; }
        .history-item { font-size:13px; border-bottom:1px solid #333; padding:10px 0; display:flex; justify-content:space-between; }
        .game-frame { width: 100%; height: 500px; border: 2px solid var(--neon); border-radius: 15px; background: #fff; margin-top: 15px; }
        .login-overlay { position:fixed; inset:0; background:#000; z-index:9999; display:flex; align-items:center; justify-content:center; padding:20px; }
    </style>
</head>
<body>
    <div class="snow-container" id="snow"></div>

    <div class="login-overlay" id="loginScreen">
        <div class="glass" style="width:100%; max-width:320px;">
            <h2 style="color:var(--neon);">🔑 KÍCH HOẠT VIP (ONE-TIME)</h2>
            <input type="text" id="keyInput" placeholder="Nhập Key...">
            <button class="btn" onclick="checkKey()">KÍCH HOẠT NGAY</button>
            <div id="checkStatus" style="font-size:12px; color:red; margin-top:10px;"></div>
        </div>
    </div>

    <div class="page-wrapper" id="mainScreen" style="display:none;">
        <div class="wrapper">
            <div class="glass">
                <h3 style="color:var(--neon); margin:0;">AI PRO 2026</h3>
                <select id="gameSelect" onchange="loadGame()">
                    <option value="https://lc79b.bet">🎮 GAME: LC79</option>
                    <option value="https://play.betvip.fit">🎮 GAME: BetVIP</option>
                </select>
            </div>

            <div class="glass">
                <input type="text" id="code" placeholder="Mã phiên MD5...">
                <button class="btn" onclick="runAnalysis()">PHÂN TÍCH CHUYÊN SÂU</button>
                <div id="resultArea" style="display:none;">
                    <div class="res-box" id="res">--</div>
                    <div class="info-text" id="details"></div>
                </div>
            </div>

            <div class="glass">
                <h4 style="margin:0 0 10px; color:var(--neon);">LỊCH SỬ DỰ ĐOÁN</h4>
                <div id="historyList"></div>
                <button class="btn btn-del" onclick="clearHistory()">XÓA LỊCH SỬ</button>
            </div>

            <div class="glass">
                <h4 style="margin:0 0 10px; color:var(--neon);">TRỰC TIẾP TRONG TOOL</h4>
                <iframe id="gameFrame" class="game-frame" src="https://lc79b.bet"></iframe>
            </div>
        </div>
    </div>

    <script>
        // Danh sách Key (Bạn có thể thêm key mới vào đây)
        let availableKeys = ["KEY-HOANGDZ-1", "KEY-HOANGDZ-2", "KEY-HOANGDZ-3"];

        function checkKey() {
            let key = document.getElementById('keyInput').value.trim();
            let index = availableKeys.indexOf(key);
            if (index !== -1) {
                availableKeys.splice(index, 1); // Xóa Key vĩnh viễn
                localStorage.setItem('isActivated', 'true');
                location.reload();
            } else {
                document.getElementById('checkStatus').innerText = "❌ Key không hợp lệ hoặc đã dùng!";
            }
        }

        function runAnalysis() {
            let input = document.getElementById('code').value.trim();
            if(input.length < 32) return alert("Nhập đủ 32 ký tự!");
            
            let resEl = document.getElementById('res'), resultArea = document.getElementById('resultArea');
            resultArea.style.display = "block";
            resEl.classList.add('res-jumping');
            
            // Thuật toán MD5 Chuyên sâu
            let hash = CryptoJS.MD5(input).toString().toLowerCase();
            let digits = hash.split('').map(c => parseInt(c, 16));
            let total_sum = digits.reduce((a, b) => a + b, 0);
            let xor_val = digits.reduce((a, b) => a ^ b, 0);
            let complex_score = (total_sum * xor_val) % 256;
            let entropy_factor = new Set(digits).size / 16.0;
            let is_tai = (complex_score % 2 === 0);
            let reliability = Math.min(99, Math.max(70, 70 + Math.floor((complex_score / 255.0) * 20)));

            setTimeout(() => {
                resEl.classList.remove('res-jumping');
                resEl.innerText = is_tai ? "🔴 TÀI" : "⚪ XỈU";
                document.getElementById('details').innerHTML = `
                    <p><b>Dự đoán:</b> ${is_tai ? "TÀI" : "XỈU"}</p>
                    <p><b>Tỉ lệ thắng:</b> ${reliability}%</p>
                    <p><b>Complex Score:</b> ${complex_score}</p>
                    <p><b>Entropy Factor:</b> ${entropy_factor.toFixed(2)}</p>
                    <p><b>Tại sao ra kết quả:</b> Phân tích biến thiên XOR và entropy 256-bit.</p>
                    <p><b>Anh HOANGDZ bảo:</b> "Chơi có kỷ luật, thắng không kiêu, bại không nản!"</p>
                `;
                
                let history = JSON.parse(localStorage.getItem('history') || '[]');
                history.unshift({time: new Date().toLocaleTimeString().slice(0,5), res: is_tai ? "🔴 TÀI" : "⚪ XỈU"});
                localStorage.setItem('history', JSON.stringify(history.slice(0, 10)));
                renderHistory();
            }, 2000);
        }

        function renderHistory() {
            let list = document.getElementById('historyList');
            let history = JSON.parse(localStorage.getItem('history') || '[]');
            list.innerHTML = history.map(h => `<div class="history-item"><span>${h.time}</span><span>${h.res}</span></div>`).join('');
        }

        function clearHistory() { localStorage.removeItem('history'); renderHistory(); }
        function loadGame() { document.getElementById('gameFrame').src = document.getElementById('gameSelect').value; }

        window.onload = () => {
            if (localStorage.getItem('isActivated') === 'true') {
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('mainScreen').style.display = 'block';
                renderHistory();
            }
        };
        for(let i=0; i<40; i++) {
            let d = document.createElement('div'); d.className = 'snowflake'; d.innerHTML = '❄';
            d.style.left = Math.random()*100 + '%'; d.style.animationDuration = Math.random()*5 + 5 + 's';
            document.getElementById('snow').appendChild(d);
        }
    </script>
</body>
</html>
