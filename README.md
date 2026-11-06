# trac-nghiem-dia-ly12
HTML
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ôn Tập Địa Lý Việt Nam</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .selected-option {
            background-color: #dbeafe !important;
            border-color: #2563eb !important;
            font-weight: 600;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen pb-12 font-sans antialiased text-gray-800">

    <!-- Header -->
    <header class="bg-blue-700 text-white sticky top-0 z-50 shadow-md">
        <div class="max-w-4xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-2">
            <div>
                <h1 class="text-xl sm:text-2xl font-bold">ÔN TẬP ĐỊA LÝ VIỆT NAM</h1>
                <p class="text-xs sm:text-sm text-blue-100">Phần I: Trắc nghiệm (18 câu) & Phần II: Tính toán (6 câu)</p>
            </div>
            <div id="timer" class="bg-blue-800 px-4 py-2 rounded-lg font-mono font-bold text-lg border border-blue-600">
                00:00
            </div>
        </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 pt-6 space-y-8">
        
        <!-- Instruction Banner -->
        <div class="bg-white p-4 sm:p-6 rounded-xl shadow-sm border border-gray-200">
            <h2 class="text-lg font-bold text-gray-900 mb-2">Hướng dẫn làm bài:</h2>
            <ul class="list-disc list-inside text-base text-gray-700 space-y-1">
                <li>Phần I: Chọn 1 đáp án đúng nhất cho từng câu hỏi.</li>
                <li>Phần II: Nhập kết quả tính toán số học vào ô trống (làm tròn theo đúng yêu cầu đề bài).</li>
                <li>Mỗi lần nhấn <b>"Làm lại"</b>, thứ tự các câu hỏi và các lựa chọn đáp án sẽ được <b>xáo trộn ngẫu nhiên</b>.</li>
            </ul>
        </div>

        <!-- Section 1 -->
        <section class="bg-white p-4 sm:p-6 rounded-xl shadow-sm border border-gray-200">
            <h2 class="text-xl font-bold text-blue-800 border-b pb-3 mb-6">PHẦN I. CÂU HỎI TRẮC NGHIỆM (18 CÂU)</h2>
            <div id="part1-container" class="space-y-8"></div>
        </section>

        <!-- Section 2 -->
        <section class="bg-white p-4 sm:p-6 rounded-xl shadow-sm border border-gray-200">
            <h2 class="text-xl font-bold text-blue-800 border-b pb-3 mb-6">PHẦN II. BÀI TẬP TÍNH TOÁN & THÔNG KÊ (6 CÂU)</h2>
            <div id="part2-container" class="space-y-8"></div>
        </section>

        <!-- Action Buttons & Score Result -->
        <div class="bg-white p-6 rounded-xl shadow-sm border border-gray-200 text-center space-y-4">
            <div id="result-box" class="hidden p-4 rounded-lg bg-blue-50 border border-blue-200 text-left">
                <h3 class="text-xl font-bold text-blue-900 mb-1" id="score-text">Kết quả của bạn: 0 / 24 câu</h3>
                <p class="text-base text-blue-800" id="score-details"></p>
            </div>
            
            <div class="flex flex-col sm:flex-row justify-center gap-4">
                <button id="btn-submit" onclick="submitQuiz()" class="w-full sm:w-auto px-8 py-3.5 bg-blue-600 hover:bg-blue-700 text-white font-bold text-lg rounded-xl shadow-md transition-all active:scale-95">
                    Nộp Bài & Chấm Điểm
                </button>
                <button id="btn-reset" onclick="resetQuiz()" class="w-full sm:w-auto px-8 py-3.5 bg-gray-600 hover:bg-gray-700 text-white font-bold text-lg rounded-xl shadow-md transition-all active:scale-95">
                    Làm Lai (Xáo Trộn Đề)
                </button>
            </div>
        </div>

    </main>

    <script>
        const originalMCQs = [
            {
                id: 1,
                q: "Căn cứ vào Atlat Địa lí Việt Nam trang Hành chính, cho biết tỉnh nào sau đây giáp với Campuchia?",
                options: ["An Giang.", "Quảng Ninh.", "Sơn La.", "Hà Tĩnh."],
                ans: 0
            },
            {
                id: 2,
                q: "Vị trí địa lí của nước ta không tạo thuận lợi nào sau đây?",
                options: ["Phát triển giao thông đường bộ quốc tế dễ dàng.", "Giao lưu kinh tế với các nước trong khu vực.", "Phát triển kinh tế biển đa dạng.", "Tạo điều kiện thuận lợi cho việc giữ vững an ninh quốc phòng."],
                ans: 3
            },
            {
                id: 3,
                q: "Tính chất nhiệt đới của khí hậu nước ta được quy định bởi",
                options: ["vị trí nằm hoàn toàn trong vùng nội nhiệt đới.", "ảnh hưởng sâu sắc của Biển Đông.", "hoạt động của gió mùa Châu Á.", "địa hình phần lớn là đồi núi thấp."],
                ans: 0
            },
            {
                id: 4,
                q: "Gió mùa Đông Bắc khi di chuyển xuống phía Nam nước ta bị suy yếu dần do",
                options: ["gặp bức chắn địa hình và sự biến tính của khối khí.", "lực Côriôlit giảm dần về phía Nam.", "sự thống trị của khối khí xích đạo.", "khoảng cách xa nguồn phát sinh gió."],
                ans: 0
            },
            {
                id: 5,
                q: "Đặc điểm nào sau đây đúng với địa hình vùng núi Đông Bắc nước ta?",
                options: ["Có các dãy núi hướng cánh cung.", "Độ cao địa hình cao nhất nước ta.", "Địa hình gồm các dãy núi song song.", "Địa hình thấp ở hai đầu, cao ở giữa."],
                ans: 0
            },
            {
                id: 6,
                q: "Đặc điểm nào sau đây của Biển Đông có ảnh hưởng lớn nhất đến thiên nhiên nước ta?",
                options: ["Biển rộng, tương đối kín, mang tính chất nhiệt đới ẩm gió mùa.", "Có các dòng hải lưu chạy dọc bờ biển.", "Đáy biển có nhiều địa hình lòng chảo sâu.", "Bờ biển kéo dài với nhiều vũng vịnh sâu."],
                ans: 0
            },
            {
                id: 7,
                q: "Nước ta có lượng mưa lớn, trung bình từ 1500 - 2000 mm/năm chủ yếu do",
                options: ["các khối khí di chuyển qua biển mang lại lượng ẩm lớn.", "nước ta nằm hoàn toàn trong vùng nội nhiệt đới.", "địa hình nhiều đồi núi chắn gió gây mưa lớn.", "ảnh hưởng của dải hội tụ nhiệt đới và bão."],
                ans: 0
            },
            {
                id: 8,
                q: "Suy giảm tài nguyên rừng ở nước ta dẫn đến hậu quả nghiêm trọng nào sau đây?",
                options: ["Gia tăng nguy cơ sạt lở đất, lũ quét và biến đổi khí hậu.", "Giảm diện tích đất nông nghiệp ở vùng đồng bằng.", "Làm thay đổi hoàn toàn khí hậu của vùng nội nhiệt đới.", "Làm cho mực nước biển dâng cao nhanh chóng."],
                ans: 0
            },
            {
                id: 9,
                q: "Địa hình vùng núi Tây Bắc nước ta có đặc điểm nào sau đây?",
                options: ["Hướng núi chính là hướng tây bắc - đông nam.", "Gồm các khối núi cổ vươn cao về phía biển.", "Địa hình đồi núi thấp chiếm diện tích lớn nhất.", "Có nhiều cánh cung núi lớn chụm lại ở Ngân Sơn."],
                ans: 0
            },
            {
                id: 10,
                q: "Hệ thống sông ngòi nước ta có đặc điểm dày đặc, nhiều nước do",
                options: ["lượng mưa lớn và địa hình nhiều đồi núi bị cắt xẻ.", "nằm hoàn toàn trong vùng nội nhiệt đới gió mùa.", "có nhiều hồ chứa nước nhân tạo trên các lưu vực.", "ảnh hưởng của các dòng hải lưu từ đại dương."],
                ans: 0
            },
            {
                id: 11,
                q: "Đồng bằng sông Hồng có đặc điểm địa hình nổi bật nào sau đây?",
                options: ["Có hệ thống đê chống lũ ngăn chia đồng bằng thành nhiều ô.", "Địa hình cao ở phía nam, thấp dần về phía bắc.", "Bị chia cắt mạnh bởi các dãy núi đâm ngang ra biển.", "Nhiều vùng giồng cát ven biển và vùng đầm lầy lớn."],
                ans: 0
            },
            {
                id: 12,
                q: "Thiên nhiên phân hóa theo chiều Bắc - Nam ở nước ta thể hiện rõ nhất qua yếu tố nào?",
                options: ["Khí hậu (nhiệt độ trung bình năm và biên độ nhiệt).", "Số lượng các loài sinh vật tự nhiên.", "Đặc điểm độ cao và hướng núi của địa hình.", "Cấu trúc địa chất và các loại khoáng sản."],
                ans: 0
            },
            {
                id: 13,
                q: "Đồng bằng sông Cửu Long có đặc điểm tự nhiên nổi bật nào sau đây?",
                options: ["Diện tích rộng lớn, địa hình thấp và bằng phẳng.", "Có hệ thống đê sông ven biển kiên cố.", "Nhiều vùng đất phù sa cổ tập trung ở phía bắc.", "Độ cao trung bình cao hơn đồng bằng sông Hồng."],
                ans: 0
            },
            {
                id: 14,
                q: "Nguyên nhân chủ yếu làm cho đất đai ở đồi núi nước ta dễ bị rửa trôi, xói mòn là",
                options: ["mưa lớn tập trung theo mùa trên địa hình dốc.", "sử dụng nhiều phân bón hóa học trong trồng trọt.", "khí hậu khô hạn kéo dài trong nhiều tháng.", "biến đổi khí hậu toàn cầu diễn ra gay gắt."],
                ans: 0
            },
            {
                id: 15,
                q: "Tài nguyên khoáng sản nổi bật nhất ở vùng biển nước ta là",
                options: ["dầu mỏ và khí đốt.", "quặng sắt và titan.", "than đá và bôxit.", "đá vôi và cát thủy tinh."],
                ans: 0
            },
            {
                id: 16,
                q: "Nhiệt độ trung bình năm ở miền Bắc thấp hơn miền Nam chủ yếu do miền Bắc",
                options: ["chịu ảnh hưởng trực tiếp của gió mùa Đông Bắc.", "có địa hình cao hơn miền Nam rất nhiều.", "nằm xa vĩ tuyến Xích đạo hơn miền Nam.", "có lượng mưa lớn hơn kéo dài trong năm."],
                ans: 0
            },
            {
                id: 17,
                q: "Biện pháp quan trọng nhất để bảo vệ đa dạng sinh học ở nước ta là",
                options: ["xây dựng và mở rộng hệ thống vườn quốc gia, khu bảo tồn.", "tăng cường khai thác rừng trồng để thay thế rừng tự nhiên.", "nghiêm cấm hoàn toàn việc chặt phá cây xanh đô thị.", "đẩy mạnh xuất khẩu các loài sinh vật quý hiếm."],
                ans: 0
            },
            {
                id: 18,
                q: "Sự phân hóa thiên nhiên theo độ cao ở nước ta gồm mấy đai cao?",
                options: ["3 đai.", "2 đai.", "4 đai.", "5 đai."],
                ans: 0
            }
        ];

        const originalShortAnswers = [
            {
                id: 101,
                q: "<b>Câu 1:</b> Cho bảng số liệu:<br><div class='text-center font-bold my-2'>Tổng số dân của nước ta, giai đoạn 1990 - 2021</div><div class='overflow-x-auto my-2'><table class='min-w-full border-collapse border border-gray-400 text-sm sm:text-base text-center'><thead><tr class='bg-gray-100'><th class='border border-gray-400 p-2'>Năm</th><th class='border border-gray-400 p-2'>1990</th><th class='border border-gray-400 p-2'>2000</th><th class='border border-gray-400 p-2'>2015</th><th class='border border-gray-400 p-2'>2021</th></tr></thead><tbody><tr><td class='border border-gray-400 p-2 font-semibold'>Tổng số dân (Triệu người)</td><td class='border border-gray-400 p-2'>66.9</td><td class='border border-gray-400 p-2'>77.6</td><td class='border border-gray-400 p-2'>92.2</td><td class='border border-gray-400 p-2'>98.5</td></tr></tbody></table></div>Căn cứ vào bảng số liệu trên, hãy cho biết dân số năm 2021 tăng bao nhiêu triệu người so với năm 1990? <i>(Làm tròn kết quả đến một chữ số thập phân)</i>",
                ans: [31.6],
                explain: "Số dân tăng = 98.5 - 66.9 = 31.6 (triệu người)."
            },
            {
                id: 102,
                q: "<b>Câu 2:</b> Cho bảng số liệu:<br><div class='text-center font-bold my-2'>Diện tích rừng của nước ta giai đoạn 2010 - 2023</div><div class='overflow-x-auto my-2'><table class='min-w-full border-collapse border border-gray-400 text-sm sm:text-base text-center'><thead><tr class='bg-gray-100'><th class='border border-gray-400 p-2'>Năm</th><th class='border border-gray-400 p-2'>2010</th><th class='border border-gray-400 p-2'>2023</th></tr></thead><tbody><tr><td class='border border-gray-400 p-2 font-semibold text-left'>Tổng diện tích rừng (Triệu ha)</td><td class='border border-gray-400 p-2'>13.4</td><td class='border border-gray-400 p-2'>14.8</td></tr><tr><td class='border border-gray-400 p-2 font-semibold text-left'>- Diện tích rừng tự nhiên</td><td class='border border-gray-400 p-2'>10.3</td><td class='border border-gray-400 p-2'>10.1</td></tr><tr><td class='border border-gray-400 p-2 font-semibold text-left'>- Diện tích rừng trồng</td><td class='border border-gray-400 p-2'>3.1</td><td class='border border-gray-400 p-2'>4.7</td></tr></tbody></table></div>Căn cứ vào bảng số liệu, cho biết tỷ trọng diện tích rừng trồng ở nước ta năm 2023 tăng bao nhiêu phần trăm so với năm 2010? <i>(Làm tròn kết quả đến một chữ số thập phân)</i>",
                ans: [8.6],
                explain: "Tỷ trọng 2010 = (3.1 / 13.4) * 100% = 23.13%. Tỷ trọng 2023 = (4.7 / 14.8) * 100% = 31.76%. Lượng tăng = 31.76% - 23.13% = 8.63% -> Làm tròn: 8.6%."
            },
            {
                id: 103,
                q: "<b>Câu 3:</b> Cho bảng số liệu:<br><div class='text-center font-bold my-2'>Nhiệt độ không khí trung bình các tháng trong năm tại Hà Nội năm 2021 (Đơn vị: °C)</div><div class='overflow-x-auto my-2'><table class='min-w-full border-collapse border border-gray-400 text-xs sm:text-sm text-center'><thead><tr class='bg-gray-100'><th class='border border-gray-400 p-1.5'>Tháng</th><th class='border border-gray-400 p-1.5'>1</th><th class='border border-gray-400 p-1.5'>2</th><th class='border border-gray-400 p-1.5'>3</th><th class='border border-gray-400 p-1.5'>4</th><th class='border border-gray-400 p-1.5'>5</th><th class='border border-gray-400 p-1.5'>6</th><th class='border border-gray-400 p-1.5'>7</th><th class='border border-gray-400 p-1.5'>8</th><th class='border border-gray-400 p-1.5'>9</th><th class='border border-gray-400 p-1.5'>10</th><th class='border border-gray-400 p-1.5'>11</th><th class='border border-gray-400 p-1.5'>12</th></tr></thead><tbody><tr><td class='border border-gray-400 p-1.5 font-semibold'>Nhiệt độ</td><td class='border border-gray-400 p-1.5'>16.9</td><td class='border border-gray-400 p-1.5'>20.9</td><td class='border border-gray-400 p-1.5'>22.5</td><td class='border border-gray-400 p-1.5'>25.6</td><td class='border border-gray-400 p-1.5'>29.7</td><td class='border border-gray-400 p-1.5'>31.6</td><td class='border border-gray-400 p-1.5'>30.8</td><td class='border border-gray-400 p-1.5'>30.5</td><td class='border border-gray-400 p-1.5'>28.7</td><td class='border border-gray-400 p-1.5'>24.6</td><td class='border border-gray-400 p-1.5'>22.5</td><td class='border border-gray-400 p-1.5'>19.4</td></tr></tbody></table></div>Căn cứ vào bảng số liệu trên, cho biết nhiệt độ trung bình năm tại Hà Nội năm 2021 là bao nhiêu °C? <i>(Làm tròn kết quả đến một chữ số thập phân)</i>",
                ans: [25.3],
                explain: "Nhiệt độ trung bình năm = Tổng 12 tháng / 12 = 303.7 / 12 = 25.308°C -> Làm tròn: 25.3°C."
            },
            {
                id: 104,
                q: "<b>Câu 4:</b> Cho bảng số liệu:<br><div class='text-center font-bold my-2'>Lượng mưa các tháng tại Huế năm 2022 (Đơn vị: mm)</div><div class='overflow-x-auto my-2'><table class='min-w-full border-collapse border border-gray-400 text-xs sm:text-sm text-center'><thead><tr class='bg-gray-100'><th class='border border-gray-400 p-1.5'>Tháng</th><th class='border border-gray-400 p-1.5'>1</th><th class='border border-gray-400 p-1.5'>2</th><th class='border border-gray-400 p-1.5'>3</th><th class='border border-gray-400 p-1.5'>4</th><th class='border border-gray-400 p-1.5'>5</th><th class='border border-gray-400 p-1.5'>6</th><th class='border border-gray-400 p-1.5'>7</th><th class='border border-gray-400 p-1.5'>8</th><th class='border border-gray-400 p-1.5'>9</th><th class='border border-gray-400 p-1.5'>10</th><th class='border border-gray-400 p-1.5'>11</th><th class='border border-gray-400 p-1.5'>12</th></tr></thead><tbody><tr><td class='border border-gray-400 p-1.5 font-semibold'>Lượng mưa</td><td class='border border-gray-400 p-1.5'>95.6</td><td class='border border-gray-400 p-1.5'>70.8</td><td class='border border-gray-400 p-1.5'>128.3</td><td class='border border-gray-400 p-1.5'>38.1</td><td class='border border-gray-400 p-1.5'>157.3</td><td class='border border-gray-400 p-1.5'>33.8</td><td class='border border-gray-400 p-1.5'>61.3</td><td class='border border-gray-400 p-1.5'>157.5</td><td class='border border-gray-400 p-1.5'>448.8</td><td class='border border-gray-400 p-1.5'>1366.5</td><td class='border border-gray-400 p-1.5'>226.4</td><td class='border border-gray-400 p-1.5'>786.6</td></tr></tbody></table></div>Căn cứ vào bảng số liệu trên, tính tổng lượng mưa của Huế năm 2022? <i>(Làm tròn kết quả đến hàng đơn vị)</i>",
                ans: [3571],
                explain: "Tổng lượng mưa = Tổng 12 tháng = 3571.1 mm -> Làm tròn đến hàng đơn vị: 3571 mm."
            },
            {
                id: 105,
                q: "<b>Câu 5:</b> Biết diện tích tự nhiên của Việt Nam là 331 212 km², số dân năm 2022 là 99,47 triệu người. Vậy mật độ dân số nước ta năm 2022 là bao nhiêu người/km²? <i>(Làm tròn kết quả đến hàng đơn vị)</i>",
                ans: [300],
                explain: "Mật độ dân số = 99 470 000 / 331 212 = 300.32 người/km² -> Làm tròn: 300 người/km²."
            },
            {
                id: 106,
                q: "<b>Câu 6:</b> Biết đỉnh núi Phan-xi-păng cao 3147 mét, nếu nhiệt độ tại đỉnh núi này là 2,0°C thì trong cùng thời điểm, cùng sườn núi đón gió, nhiệt độ ở chân núi này sẽ là bao nhiêu °C? <i>(Làm tròn kết quả đến một chữ số thập phân)</i>",
                ans: [20.9],
                explain: "Độ chênh lệch nhiệt độ = (3147 / 100) * 0.6 = 18.882°C. Nhiệt độ chân núi = 2.0 + 18.882 = 20.882°C -> Làm tròn: 20.9°C."
            }
        ];

        let currentMCQs = [];
        let currentShortAnswers = [];
        let timerInterval;
        let secondsPassed = 0;

        function shuffle(array) {
            let currentIndex = array.length, randomIndex;
            while (currentIndex != 0) {
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex--;
                [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
            }
            return array;
        }

        function startTimer() {
            clearInterval(timerInterval);
            secondsPassed = 0;
            timerInterval = setInterval(() => {
                secondsPassed++;
                let m = Math.floor(secondsPassed / 60).toString().padStart(2, '0');
                let s = (secondsPassed % 60).toString().padStart(2, '0');
                document.getElementById('timer').innerText = `${m}:${s}`;
            }, 1000);
        }

        function renderQuiz() {
            // Shuffle MCQs
            currentMCQs = JSON.parse(JSON.stringify(originalMCQs));
            shuffle(currentMCQs);
            
            const p1Container = document.getElementById('part1-container');
            p1Container.innerHTML = '';

            currentMCQs.forEach((item, qIdx) => {
                // Shuffle options for each question
                let optionsObj = item.options.map((opt, originalIndex) => ({ text: opt, isCorrect: originalIndex === item.ans }));
                shuffle(optionsObj);
                item.shuffledOptions = optionsObj;

                let qEl = document.createElement('div');
                qEl.className = 'p-4 rounded-xl border border-gray-200 bg-gray-50/50 space-y-3';
                
                let optHtml = optionsObj.map((opt, oIdx) => {
                    let label = String.fromCharCode(65 + oIdx);
                    return `
                        <button type="button" 
                            onclick="selectOption(${qIdx}, ${oIdx})"
                            id="btn-opt-${qIdx}-${oIdx}"
                            class="opt-btn-${qIdx} w-full text-left p-3.5 sm:p-4 rounded-xl border-2 border-gray-300 bg-white hover:bg-blue-50/50 transition-all text-base sm:text-lg flex items-center gap-3">
                            <span class="inline-flex items-center justify-center w-8 h-8 rounded-full bg-gray-200 text-gray-800 font-bold shrink-0">${label}</span>
                            <span class="text-gray-800">${opt.text}</span>
                        </button>
                    `;
                }).join('');

                qEl.innerHTML = `
                    <div class="font-bold text-base sm:text-lg text-gray-900 leading-snug">Câu ${qIdx + 1}: ${item.q}</div>
                    <div class="grid grid-cols-1 gap-2.5 pt-2">${optHtml}</div>
                    <div id="explain-mcq-${qIdx}" class="hidden p-3 rounded-lg text-sm bg-blue-100 text-blue-900 mt-2 font-medium"></div>
                `;
                p1Container.appendChild(qEl);
            });

            // Shuffle Short Answers
            currentShortAnswers = JSON.parse(JSON.stringify(originalShortAnswers));
            shuffle(currentShortAnswers);

            const p2Container = document.getElementById('part2-container');
            p2Container.innerHTML = '';

            currentShortAnswers.forEach((item, qIdx) => {
                let qEl = document.createElement('div');
                qEl.className = 'p-4 rounded-xl border border-gray-200 bg-gray-50/50 space-y-3';
                
                qEl.innerHTML = `
                    <div class="text-base sm:text-lg text-gray-900 leading-snug">${item.q.replace(/Câu \d+:/, `Câu ${qIdx + 1}:`)}</div>
                    <div class="pt-2 flex flex-col sm:flex-row items-start sm:items-center gap-3">
                        <label class="font-bold text-base text-gray-700">Đáp số của bạn:</label>
                        <input type="number" step="any" id="input-sa-${qIdx}" 
                            class="w-full sm:w-48 p-3 rounded-xl border-2 border-gray-300 text-lg font-bold text-gray-900 focus:border-blue-600 focus:outline-none" 
                            placeholder="Nhập số...">
                    </div>
                    <div id="explain-sa-${qIdx}" class="hidden p-3 rounded-lg text-sm bg-blue-100 text-blue-900 mt-2 font-medium"></div>
                `;
                p2Container.appendChild(qEl);
            });

            document.getElementById('result-box').classList.add('hidden');
            startTimer();
        }

        function selectOption(qIdx, oIdx) {
            const btns = document.querySelectorAll(`.opt-btn-${qIdx}`);
            btns.forEach((btn, idx) => {
                if (idx === oIdx) {
                    btn.classList.add('selected-option');
                    btn.querySelector('span').classList.remove('bg-gray-200', 'text-gray-800');
                    btn.querySelector('span').classList.add('bg-blue-600', 'text-white');
                } else {
                    btn.classList.remove('selected-option');
                    btn.querySelector('span').classList.remove('bg-blue-600', 'text-white');
                    btn.querySelector('span').classList.add('bg-gray-200', 'text-gray-800');
                }
            });
            currentMCQs[qIdx].userSelected = oIdx;
        }

        function submitQuiz() {
            clearInterval(timerInterval);
            let p1Correct = 0;
            let p2Correct = 0;

            // Check Part I
            currentMCQs.forEach((item, qIdx) => {
                const btns = document.querySelectorAll(`.opt-btn-${qIdx}`);
                const explainEl = document.getElementById(`explain-mcq-${qIdx}`);
                
                let correctOptIdx = item.shuffledOptions.findIndex(o => o.isCorrect);

                btns.forEach((btn, oIdx) => {
                    btn.disabled = true;
                    if (oIdx === correctOptIdx) {
                        btn.classList.add('bg-green-100', 'border-green-600', 'font-bold');
                        btn.querySelector('span').classList.add('bg-green-600', 'text-white');
                    }
                });

                if (item.userSelected !== undefined) {
                    if (item.shuffledOptions[item.userSelected].isCorrect) {
                        p1Correct++;
                    } else {
                        btns[item.userSelected].classList.add('bg-red-100', 'border-red-600');
                        btns[item.userSelected].querySelector('span').classList.add('bg-red-600', 'text-white');
                    }
                }

                explainEl.innerText = `Đáp án đúng: ${String.fromCharCode(65 + correctOptIdx)}. ${item.shuffledOptions[correctOptIdx].text}`;
                explainEl.classList.remove('hidden');
            });

            // Check Part II
            currentShortAnswers.forEach((item, qIdx) => {
                const inputEl = document.getElementById(`input-sa-${qIdx}`);
                const explainEl = document.getElementById(`explain-sa-${qIdx}`);
                inputEl.disabled = true;

                let val = parseFloat(inputEl.value);
                let isCorrect = item.ans.some(a => Math.abs(a - val) < 0.11);

                if (isCorrect) {
                    p2Correct++;
                    inputEl.classList.add('bg-green-100', 'border-green-600', 'text-green-900');
                } else {
                    inputEl.classList.add('bg-red-100', 'border-red-600', 'text-red-900');
                }

                explainEl.innerHTML = `<b>Đáp số chuẩn:</b> ${item.ans[0]} <br> <b>Lời giải:</b> ${item.explain}`;
                explainEl.classList.remove('hidden');
            });

            let totalScore = p1Correct + p2Correct;
            let totalQ = currentMCQs.length + currentShortAnswers.length;

            const resBox = document.getElementById('result-box');
            document.getElementById('score-text').innerText = `Kết quả của bạn: ${totalScore} / ${totalQ} câu (${((totalScore/totalQ)*100).toFixed(1)}%)`;
            document.getElementById('score-details').innerText = `Phần I (Trắc nghiệm): ${p1Correct}/18 câu | Phần II (Tính toán): ${p2Correct}/6 câu`;
            resBox.classList.remove('hidden');

            window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
        }

        function resetQuiz() {
            renderQuiz();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Initialize on load
        window.onload = renderQuiz;
    </script>
</body>
</html>
