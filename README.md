# lookmaxxingweb
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lookmaxxing - AI Facial Analysis & Appeal Scale</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        gold: {
                            DEFAULT: '#FFD700',
                            dark: '#D4AF37',
                            light: '#FFF8DC'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #050505;
            color: #E5E7EB;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        }
        .gold-gradient-text {
            background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .gold-border {
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        .gold-glow {
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.15);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-gold selection:text-black">

    <!-- Header / Navbar -->
    <header class="border-b border-neutral-800 py-4 px-6 sticky top-0 bg-[#050505]/90 backdrop-blur-md z-50">
        <div class="max-w-4xl mx-auto flex justify-between items-center">
            <h1 class="text-xl font-black tracking-widest gold-gradient-text uppercase">LOOKMAXXING</h1>
            <div id="usage-badge" class="text-xs bg-neutral-900 border border-gold/40 px-3 py-1 rounded-full text-gold">
                Lượt miễn phí còn lại: <span id="free-count">1</span>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-xl mx-auto p-4 w-full flex-grow my-auto">
        
        <!-- Intro Section -->
        <div class="text-center mb-8">
            <h2 class="text-3xl font-extrabold text-white mb-2">VN APPEAL SCALE AI</h2>
            <p class="text-sm text-neutral-400">Hệ thống phân tích tỷ lệ khuôn mặt chuẩn Sub 3 đến True Adam</p>
            <p class="text-xs text-gold/80 mt-2 font-medium">lookmaxxing founder tạo ra</p>
        </div>

        <!-- Upload Card -->
        <div id="upload-card" class="bg-neutral-900/80 rounded-2xl p-6 gold-border gold-glow">
            <div class="border-2 border-dashed border-neutral-700 hover:border-gold transition-colors rounded-xl p-8 text-center cursor-pointer relative" id="drop-zone">
                <input type="file" id="image-input" accept="image/*" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
                <div id="upload-placeholder">
                    <svg class="w-12 h-12 mx-auto text-gold mb-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                    </svg>
                    <p class="text-sm font-semibold text-white">Tải ảnh khuôn mặt lên hoặc chụp ảnh</p>
                    <p class="text-xs text-neutral-400 mt-1">Hỗ trợ định dạng JPG, PNG</p>
                </div>
                <div id="preview-container" class="hidden">
                    <img id="image-preview" class="max-h-64 mx-auto rounded-lg object-contain mb-3 border border-gold/30">
                    <p class="text-xs text-gold">Nhấn để thay đổi ảnh khác</p>
                </div>
            </div>

            <button id="analyze-btn" onclick="startAnalysis()" class="w-full mt-6 bg-gradient-to-r from-gold to-yellow-600 text-black font-bold py-3.5 px-6 rounded-xl shadow-lg hover:opacity-90 transition-opacity uppercase tracking-wider text-sm">
                Bắt đầu chấm điểm ngay
            </button>
        </div>

        <!-- Scanning Loader State -->
        <div id="loader" class="hidden text-center py-12 bg-neutral-900/80 rounded-2xl gold-border">
            <div class="inline-block w-12 h-12 border-4 border-gold border-t-transparent rounded-full animate-spin mb-4"></div>
            <p class="text-gold font-semibold tracking-wide animate-pulse" id="loader-text">AI đang quét tỷ lệ mắt, mũi, miệng...</p>
        </div>

        <!-- Result Card -->
        <div id="result-card" class="hidden bg-neutral-900/90 rounded-2xl p-6 gold-border space-y-6">
            <div class="text-center border-b border-neutral-800 pb-4">
                <span class="text-xs uppercase tracking-widest text-neutral-400">KẾT QUẢ PHÂN TÍCH LOOKMAXXING</span>
                <div class="text-5xl font-black text-gold mt-1" id="res-score">0 / 80</div>
                <div class="inline-block bg-gold/10 border border-gold text-gold px-4 py-1 rounded-full text-sm font-bold mt-2" id="res-tier">MTN</div>
            </div>

            <!-- Scores Breakdown -->
            <div class="grid grid-cols-2 gap-3 text-sm">
                <div class="bg-neutral-950 p-3 rounded-xl border border-neutral-800">
                    <span class="text-neutral-400 text-xs block">Mắt (Eyes)</span>
                    <span class="font-bold text-white text-base" id="score-eyes">--/80</span>
                </div>
                <div class="bg-neutral-950 p-3 rounded-xl border border-neutral-800">
                    <span class="text-neutral-400 text-xs block">Mũi (Nose)</span>
                    <span class="font-bold text-white text-base" id="score-nose">--/80</span>
                </div>
                <div class="bg-neutral-950 p-3 rounded-xl border border-neutral-800">
                    <span class="text-neutral-400 text-xs block">Miệng (Mouth)</span>
                    <span class="font-bold text-white text-base" id="score-mouth">--/80</span>
                </div>
                <div class="bg-neutral-950 p-3 rounded-xl border border-neutral-800">
                    <span class="text-neutral-400 text-xs block">Tóc & Da (Hair & Skin)</span>
                    <span class="font-bold text-white text-base" id="score-hairskin">--/80</span>
                </div>
            </div>

            <!-- Potential Box -->
            <div class="bg-gradient-to-r from-yellow-950/40 to-neutral-950 p-4 rounded-xl border border-gold/40">
                <div class="flex items-center gap-2 mb-1">
                    <svg class="w-5 h-5 text-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                    <span class="text-gold font-bold text-sm uppercase">Ô Tiềm Năng (Max Potential)</span>
                </div>
                <p class="text-xs text-neutral-300 leading-relaxed" id="potential-text">Đang tính toán tiềm năng...</p>
            </div>

            <button onclick="resetApp()" class="w-full bg-neutral-800 hover:bg-neutral-700 text-gold border border-gold/30 font-semibold py-3 rounded-xl transition-colors text-sm">
                Chấm ảnh khác
            </button>
        </div>

        <!-- Payment Modal / Prompt Overlay -->
        <div id="payment-modal" class="hidden fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
            <div class="bg-neutral-900 border border-gold p-6 rounded-2xl max-w-sm w-full text-center space-y-4 shadow-2xl">
                <div class="w-12 h-12 bg-gold/10 rounded-full flex items-center justify-center mx-auto text-gold font-bold text-xl">đ</div>
                <h3 class="text-lg font-bold text-white">Yêu Cầu Thanh Toán</h3>
                <p class="text-xs text-neutral-300">Bạn đã dùng hết lượt miễn phí đầu tiên. Vui lòng thanh toán <span class="text-gold font-bold">5.000 VNĐ</span> để tiếp tục thực hiện lượt quét tiếp theo.</p>
                <div class="bg-neutral-950 p-3 rounded-xl border border-neutral-800 text-xs text-neutral-400">
                    Mã QR Momo/VietQR giả lập:<br><span class="text-gold font-semibold">STK: 0123456789 - Lookmaxxing</span>
                </div>
                <div class="flex gap-2 pt-2">
                    <button onclick="closePaymentModal()" class="flex-1 bg-neutral-800 py-2.5 rounded-xl text-xs font-semibold text-neutral-300">Hủy</button>
                    <button onclick="simulatePayment()" class="flex-1 bg-gold text-black py-2.5 rounded-xl text-xs font-bold hover:bg-gold-dark transition-colors">Đã thanh toán 5k</button>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="text-center py-6 border-t border-neutral-900 text-xs text-neutral-500">
        <p>lookmaxxing founder tạo ra &copy; 2026</p>
    </footer>

    <!-- Script Logic -->
    <script>
        const imageInput = document.getElementById('image-input');
        const uploadPlaceholder = document.getElementById('upload-placeholder');
        const previewContainer = document.getElementById('preview-container');
        const imagePreview = document.getElementById('image-preview');
        const uploadCard = document.getElementById('upload-card');
        const loader = document.getElementById('loader');
        const resultCard = document.getElementById('result-card');
        const paymentModal = document.getElementById('payment-modal');
        const freeCountEl = document.getElementById('free-count');

        let selectedFile = null;

        // Quản lý số lần dùng bằng LocalStorage
        function getUsageCount() {
            let count = localStorage.getItem('lookmaxxing_uses');
            if (count === null) {
                localStorage.setItem('lookmaxxing_uses', '0');
                return 0;
            }
            return parseInt(count);
        }

        function updateUsageUI() {
            let uses = getUsageCount();
            if (uses === 0) {
                freeCountEl.innerText = "1 (Miễn phí)";
            } else {
                freeCountEl.innerText = "0 (Phí 5.000đ)";
            }
        }
        updateUsageUI();

        imageInput.addEventListener('change', function(e) {
            if (e.target.files && e.target.files[0]) {
                selectedFile = e.target.files[0];
                const reader = new FileReader();
                reader.onload = function(e) {
                    imagePreview.src = e.target.result;
                    uploadPlaceholder.classList.add('hidden');
                    previewContainer.classList.remove('hidden');
                }
                reader.readAsDataURL(selectedFile);
            }
        });

        function startAnalysis() {
            if (!selectedFile) {
                alert('Vui lòng tải lên ảnh khuôn mặt của bạn trước!');
                return;
            }

            let uses = getUsageCount();
            if (uses >= 1) {
                // Hiện bảng yêu cầu trả phí 5000đ cho lượt thứ 2 trở đi
                paymentModal.classList.remove('hidden');
                return;
            }

            executeScanProcess();
        }

        function simulatePayment() {
            paymentModal.classList.add('hidden');
            // Cập nhật tăng số lần đã dùng giả lập thanh toán thành công
            localStorage.setItem('lookmaxxing_uses', '2');
            updateUsageUI();
            executeScanProcess();
        }

        function closePaymentModal() {
            paymentModal.classList.add('hidden');
        }

        function executeScanProcess() {
            uploadCard.classList.add('hidden');
            loader.classList.remove('hidden');

            const loaderTexts = [
                "Đang quét cấu trúc xương hàm và gò má...",
                "Đang đo khoảng cách tỷ lệ mắt, mũi, miệng...",
                "Đang đối chiếu thang VN Appeal Scale (LTN, MTN, HTN, Chad)...",
                "Đang tổng hợp điểm số và tiềm năng..."
            ];

            let textIdx = 0;
            const textInterval = setInterval(() => {
                textIdx++;
                if (textIdx < loaderTexts.length) {
                    document.getElementById('loader-text').innerText = loaderTexts[textIdx];
                }
            }, 800);

            setTimeout(() => {
                clearInterval(textInterval);
                loader.classList.add('hidden');
                showResults();
            }, 3500);
        }

        function showResults() {
            resultCard.classList.remove('hidden');

            // Random điểm từ 44 đến 80 theo đúng yêu cầu
            const totalScore = Math.floor(Math.random() * (80 - 44 + 1)) + 44;
            const eyesScore = Math.floor(Math.random() * (80 - totalScore + 5)) + Math.max(40, totalScore - 10);
            const noseScore = Math.floor(Math.random() * (80 - 44 + 1)) + 44;
            const mouthScore = Math.floor(Math.random() * (80 - 44 + 1)) + 44;
            const hairSkinScore = Math.floor(Math.random() * (80 - 44 + 1)) + 44;

            document.getElementById('res-score').innerText = totalScore + " / 80";

            // Xác định phân cấp dựa theo thang VN Appeal Scale
            let tier = "Sub 5";
            let potential = "Cần cải thiện skincare, tỉa lại đường chân mày và tập Mewing để cải thiện góc nghiêng.";
            
            if (totalScore >= 44 && totalScore < 52) {
                tier = "Sub 3 / Sub 5";
                potential = "Tiềm năng cải thiện lớn qua việc giảm mỡ mặt (lean maxxxing), thay đổi kiểu tóc phù hợp với khuôn mặt và cải thiện chất lượng da.";
            } else if (totalScore >= 52 && totalScore < 60) {
                tier = "LTN (Low Tier Normal)";
                potential = "Gương mặt cân đối ở mức trung bình. Có thể tối ưu hóa ngoại hình bằng cách tập thể hình, chăm sóc tóc và cải thiện thần thái.";
            } else if (totalScore >= 60 && totalScore < 68) {
                tier = "MTN (Mid Tier Normal)";
                potential = "Nhan sắc sáng sủa, hài hòa. Tiềm năng đạt mức HTN nếu duy trì độ nét cơ mặt, tối ưu phong cách thời trang và kiểu tóc.";
            } else if (totalScore >= 68 && totalScore < 75) {
                tier = "HTN (High Tier Normal)";
                potential = "Cấu trúc xương mặt rất tốt, tỷ lệ chuẩn xác. Thuộc nhóm thu hút cao, có tiềm năng chạm ngưỡng Chad nếu tối ưu khối cơ hàm.";
            } else {
                tier = "Chad / True Adam";
                potential = "Tỷ lệ khuôn mặt cực kỳ hoàn hảo, khung xương sắc nét chuẩn người mẫu. Duy trì phong độ hiện tại!";
            }

            document.getElementById('res-tier').innerText = tier;
            document.getElementById('score-eyes').innerText = Math.min(80, eyesScore) + " / 80";
            document.getElementById('score-nose').innerText = Math.min(80, noseScore) + " / 80";
            document.getElementById('score-mouth').innerText = Math.min(80, mouthScore) + " / 80";
            document.getElementById('score-hairskin').innerText = Math.min(80, hairSkinScore) + " / 80";
            document.getElementById('potential-text').innerText = potential;

            // Đánh dấu đã dùng 1 lượt miễn phí
            if (getUsageCount() === 0) {
                localStorage.setItem('lookmaxxing_uses', '1');
            }
            updateUsageUI();
        }

        function resetApp() {
            resultCard.classList.add('hidden');
            uploadCard.classList.remove('hidden');
            imageInput.value = '';
            uploadPlaceholder.classList.remove('hidden');
            previewContainer.classList.add('hidden');
            selectedFile = null;
        }
    </script>
</body>
</html>
