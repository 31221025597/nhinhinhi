# File tính GPA cho NhiNHi
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tính GPA và Dự đoán</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            line-height: 1.6;
            padding: 20px;
            background-color: #f5f5f5;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        h1, h2, h3 {
            color: #333;
            margin-bottom: 20px;
        }

        h1 {
            text-align: center;
        }

        .previous-info {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: #666;
        }

        input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }

        .courses {
            margin-bottom: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 15px;
        }

        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #f8f9fa;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        button {
            padding: 8px 16px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin: 5px;
        }

        button:hover {
            opacity: 0.9;
        }

        .add-course { background-color: #28a745; }
        .remove-course { background-color: #dc3545; }
        .auto-generate { background-color: #6c757d; }
        .predict-button { background-color: #17a2b8; }

        .result, .prediction {
            margin-top: 20px;
            padding: 20px;
            background-color: #f8f9fa;
            border-radius: 4px;
            display: none;
        }

        .result.show, .prediction.show {
            display: block;
        }

        .error {
            color: #dc3545;
            margin-bottom: 10px;
            display: none;
            padding: 10px;
            background-color: #ffe6e6;
            border-radius: 4px;
        }

        .error.show {
            display: block;
        }

        .success {
            color: #28a745;
            background-color: #e8f5e9;
            padding: 10px;
            border-radius: 4px;
            margin-top: 10px;
        }

        .warning {
            color: #856404;
            background-color: #fff3cd;
            padding: 10px;
            border-radius: 4px;
            margin-top: 10px;
        }

        .course-name { min-width: 300px; }
        .course-credits, .course-grade { width: 100px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tính điểm tích lũy GPA</h1>
        
        <div class="error" id="error"></div>

        <div class="previous-info">
            <div class="form-group">
                <label for="previousGPA">GPA tích lũy trước đó (thang 4)</label>
                <input type="number" id="previousGPA" min="0" max="4" step="0.01" placeholder="Ví dụ: 3.2">
            </div>
            <div class="form-group">
                <label for="previousCredits">Tổng số tín chỉ đã tích lũy</label>
                <input type="number" id="previousCredits" min="0" step="1" placeholder="Ví dụ: 60">
            </div>
            <div class="form-group">
                <label for="targetGPA">GPA mục tiêu (thang 4)</label>
                <input type="number" id="targetGPA" min="0" max="4" step="0.01" value="3.2">
            </div>
        </div>

        <div class="courses">
            <div class="button-group">
                <button class="add-course" onclick="addCourse()">Thêm môn học</button>
                <button class="auto-generate" onclick="autoGenerateGrades()">Tự động điền điểm ngẫu nhiên</button>
                <button class="predict-button" onclick="predictRequiredGrades()">Dự đoán điểm cần đạt</button>
            </div>
            <table id="coursesTable">
                <thead>
                    <tr>
                        <th>Tên môn học</th>
                        <th>Số tín chỉ</th>
                        <th>Điểm GPA (thang 4)</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody id="coursesList"></tbody>
            </table>
        </div>

        <button onclick="calculateGPA()">Tính điểm GPA</button>

        <div class="result" id="result">
            <h3>Kết quả tính GPA</h3>
            <p>GPA học kỳ này: <strong id="currentGPA">0.00</strong></p>
            <p>Số tín chỉ học kỳ này: <strong id="currentCredits">0</strong></p>
            <p>GPA tích lũy: <strong id="cumulativeGPA">0.00</strong></p>
            <p>Tổng số tín chỉ tích lũy: <strong id="totalCredits">0</strong></p>
        </div>

        <div class="prediction" id="prediction">
            <h3>Phân tích GPA mục tiêu</h3>
            <div id="predictionResult"></div>
        </div>
    </div>

    <script>
        // Danh sách môn học mẫu
        const sampleCourses = [
            { name: "Lập trình hướng đối tượng", credits: 3 },
            { name: "Cấu trúc dữ liệu và giải thuật", credits: 4 },
            { name: "Toán rời rạc", credits: 3 },
            { name: "Cơ sở dữ liệu", credits: 3 },
            { name: "Mạng máy tính", credits: 3 },
            { name: "Kiến trúc máy tính", credits: 3 },
            { name: "Tiếng Anh chuyên ngành", credits: 2 }
        ];

        // Khởi tạo môn học mẫu khi trang load
        window.onload = function() {
            sampleCourses.forEach(course => {
                addCourse(course.name, course.credits);
            });
        };

        function addCourse(name = "", credits = "") {
            const coursesList = document.getElementById('coursesList');
            const newRow = document.createElement('tr');
            newRow.innerHTML = `
                <td><input type="text" class="course-name" placeholder="Tên môn học" value="${name}"></td>
                <td><input type="number" class="course-credits" min="1" max="15" step="1" placeholder="Số tín chỉ" value="${credits}"></td>
                <td><input type="number" class="course-grade" min="0" max="4" step="0.1" placeholder="Điểm GPA"></td>
                <td><button class="remove-course" onclick="removeCourse(this)">Xóa</button></td>
            `;
            coursesList.appendChild(newRow);
        }

        function removeCourse(button) {
            const coursesList = document.getElementById('coursesList');
            if (coursesList.children.length > 1) {
                button.parentElement.parentElement.remove();
            }
        }

        function autoGenerateGrades() {
            const gradeInputs = document.getElementsByClassName('course-grade');
            for (let input of gradeInputs) {
                // Tạo điểm ngẫu nhiên từ 2.0 đến 4.0
                const randomGrade = (Math.random() * 2 + 2).toFixed(1);
                input.value = randomGrade;
            }
        }

        function showError(message) {
            const error = document.getElementById('error');
            error.textContent = message;
            error.classList.add('show');
            setTimeout(() => {
                error.classList.remove('show');
            }, 5000);
        }

        function validateInput(value, min, max) {
            const num = parseFloat(value);
            return !isNaN(num) && num >= min && num <= max;
        }

        function calculateGPA() {
            // Lấy thông tin tích lũy trước
            const previousGPA = parseFloat(document.getElementById('previousGPA').value) || 0;
            const previousCredits = parseInt(document.getElementById('previousCredits').value) || 0;

            // Validate thông tin tích lũy trước
            if (previousGPA && !validateInput(previousGPA, 0, 4)) {
                showError('GPA tích lũy trước phải từ 0 đến 4');
                return;
            }
            if (previousCredits && !validateInput(previousCredits, 0, 300)) {
                showError('Số tín chỉ tích lũy trước phải từ 0 đến 300');
                return;
            }

            // Tính toán cho học kỳ hiện tại
            let totalCredits = 0;
            let totalPoints = 0;
            let hasError = false;

            const courses = document.getElementsByTagName('tr');
            for (let i = 1; i < courses.length; i++) {
                const credits = parseFloat(courses[i].querySelector('.course-credits').value);
                const grade = parseFloat(courses[i].querySelector('.course-grade').value);

                if (!credits || !grade) continue;

                if (!validateInput(credits, 1, 15)) {
                    showError('Số tín chỉ phải từ 1 đến 15');
                    hasError = true;
                    break;
                }

                if (!validateInput(grade, 0, 4)) {
                    showError('Điểm GPA phải từ 0 đến 4');
                    hasError = true;
                    break;
                }

                totalCredits += credits;
                totalPoints += credits * grade;
            }

            if (hasError) return;

            // Tính GPA học kỳ này
            const currentGPA = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : 0;

            // Tính GPA tích lũy
            const totalCumulativeCredits = previousCredits + totalCredits;
            const cumulativeGPA = totalCumulativeCredits > 0 
                ? ((previousGPA * previousCredits + totalPoints) / totalCumulativeCredits).toFixed(2)
                : 0;

            // Hiển thị kết quả
            document.getElementById('currentGPA').textContent = currentGPA;
            document.getElementById('currentCredits').textContent = totalCredits;
            document.getElementById('cumulativeGPA').textContent = cumulativeGPA;
            document.getElementById('totalCredits').textContent = totalCumulativeCredits;
            document.getElementById('result').classList.add('show');
        }

        function predictRequiredGrades() {
            const targetGPA = parseFloat(document.getElementById('targetGPA').value);
            const previousGPA = parseFloat(document.getElementById('previousGPA').value) || 0;
            const previousCredits = parseInt(document.getElementById('previousCredits').value) || 0;

            if (!validateInput(targetGPA, 0, 4)) {
                showError('GPA mục tiêu phải từ 0 đến 4');
                return;
            }

            let currentSemesterCredits = 0;
            const courses = document.getElementsByTagName('tr');
            for (let i = 1; i < courses.length; i++) {
                const credits = parseFloat(courses[i].querySelector('.course-credits').value);
                if (credits) currentSemesterCredits += credits;
            }

            if (currentSemesterCredits === 0) {
                showError('Vui lòng nhập số tín chỉ cho các môn học');
                return;
            }

            // Tính điểm cần đạt
            const totalCumulativeCredits = previousCredits + currentSemesterCredits;
            const requiredPoints = targetGPA * totalCumulativeCredits - (previousGPA * previousCredits);
            const requiredGPA = requiredPoints / currentSemesterCredits;

            const predictionDiv = document.getElementById('predictionResult');
            const prediction = document.getElementById('prediction');

            if (requiredGPA > 4) {
                predictionDiv.innerHTML = `
                    <div class="warning">
                        <p>Không thể đạt được GPA mục tiêu ${targetGPA} trong học kỳ này.</p>
                        <p>Điểm trung bình cần thiết (${requiredGPA.toFixed(2)}) vượt quá thang điểm tối đa (4.0).</p>
                        <p>Bạn nên điều chỉnh mục tiêu GPA hoặc cải thiện dần qua nhiều học kỳ.</p>
                    </div>
                `;
            } else if (requiredGPA < 0) {
                predictionDiv.innerHTML = `
                    <div class="success">
                        <p>Bạn đã đạt GPA mục tiêu ${targetGPA}!</p>
                        <p>Có thể duy trì kết quả học tập hiện tại.</p>
                    </div>
                `;
            } else {
                predictionDiv.innerHTML = `
                    <div class="success">
                        <p>Để đạt GPA mục tiêu ${targetGPA}, học kỳ này bạn cần:</p>
                        <p>- Đạt điểm trung bình tối thiểu: <strong>${requiredGPA.toFixed(2)}</strong></p>
                        <p>- Với ${currentSemesterCredits} tín chỉ đăng ký</p>
                    </div>
                `;
            }
            
            prediction.classList.add('show');
        }
    </script>
</body>
</html>
