<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản lý GPA theo kỳ học của Nhi Nhi Nhi</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            line-height: 1.6;
            padding: 20px;
            background-color: #f5f5f5;
        }

        .container {
            max-width: 1200px;
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

        .semester {
            background: #f8f9fa;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 8px;
            border: 1px solid #dee2e6;
        }

        .semester-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: #666;
        }

        input, select {
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
            background: white;
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

        .add-semester { background-color: #6610f2; }
        .add-course { background-color: #28a745; }
        .remove-course { background-color: #dc3545; }
        .remove-semester { background-color: #dc3545; }
        .auto-generate { background-color: #6c757d; }
        .calculate-all { background-color: #17a2b8; }

        .semester-result {
            background-color: #e9ecef;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }

        .overall-result {
            margin-top: 30px;
            padding: 20px;
            background-color: #f8f9fa;
            border-radius: 4px;
        }

        .error {
            color: #dc3545;
            background-color: #f8d7da;
            padding: 10px;
            border-radius: 4px;
            margin-bottom: 10px;
            display: none;
        }

        .error.show {
            display: block;
        }

        .success {
            color: #155724;
            background-color: #d4edda;
            padding: 10px;
            border-radius: 4px;
            margin-top: 10px;
        }

        .semester-controls {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .course-name { min-width: 300px; }
        .course-credits, .course-grade { width: 100px; }

        @media (max-width: 768px) {
            .semester-header {
                flex-direction: column;
                gap: 10px;
            }
            
            .button-group {
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Quản lý điểm GPA theo kỳ học</h1>
        
        <div class="error" id="error"></div>

        <div class="button-group">
            <button class="add-semester" onclick="addSemester()">Thêm kỳ học mới</button>
            <button class="calculate-all" onclick="calculateOverallGPA()">Tính GPA tổng</button>
        </div>

        <div id="semesters"></div>

        <div class="overall-result" id="overallResult" style="display: none;">
            <h3>Kết quả tổng</h3>
            <p>GPA tích lũy: <strong id="overallGPA">0.00</strong></p>
            <p>Tổng số tín chỉ tích lũy: <strong id="overallCredits">0</strong></p>
        </div>
    </div>

    <script>
        let semesterCount = 0;
        const semesters = [];

        // Danh sách môn học mẫu
        const sampleCourses = [
            { name: "Lập trình hướng đối tượng", credits: 3 },
            { name: "Cấu trúc dữ liệu và giải thuật", credits: 4 },
            { name: "Toán rời rạc", credits: 3 },
            { name: "Cơ sở dữ liệu", credits: 3 },
            { name: "Mạng máy tính", credits: 3 }
        ];

        function addSemester() {
            semesterCount++;
            const semestersDiv = document.getElementById('semesters');
            const semesterId = `semester-${semesterCount}`;
            
            const semesterDiv = document.createElement('div');
            semesterDiv.className = 'semester';
            semesterDiv.id = semesterId;

            const currentYear = new Date().getFullYear();
            const years = Array.from({length: 10}, (_, i) => currentYear - 5 + i);
            const yearOptions = years.map(year => 
                `<option value="${year}">${year}</option>`
            ).join('');

            semesterDiv.innerHTML = `
                <div class="semester-header">
                    <h2>Kỳ học ${semesterCount}</h2>
                    <div class="semester-controls">
                        <select class="semester-year">
                            ${yearOptions}
                        </select>
                        <select class="semester-term">
                            <option value="1">Học kỳ 1</option>
                            <option value="2">Học kỳ 2</option>
                            <option value="3">Học kỳ 3</option>
                        </select>
                        <div class="button-group">
                            <button class="add-course" onclick="addCourseToSemester('${semesterId}')">Thêm môn học</button>
                            <button class="auto-generate" onclick="autoGenerateGrades('${semesterId}')">Điền điểm ngẫu nhiên</button>
                            <button class="remove-semester" onclick="removeSemester('${semesterId}')">Xóa kỳ học</button>
                        </div>
                    </div>
                </div>

                <table>
                    <thead>
                        <tr>
                            <th>Tên môn học</th>
                            <th>Số tín chỉ</th>
                            <th>Điểm GPA (thang 4)</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody id="${semesterId}-courses"></tbody>
                </table>

                <button onclick="calculateSemesterGPA('${semesterId}')">Tính GPA kỳ này</button>

                <div class="semester-result" id="${semesterId}-result" style="display: none;">
                    <h3>Kết quả học kỳ</h3>
                    <p>GPA học kỳ: <strong id="${semesterId}-gpa">0.00</strong></p>
                    <p>Số tín chỉ: <strong id="${semesterId}-credits">0</strong></p>
                </div>
            `;

            semestersDiv.appendChild(semesterDiv);

            // Thêm các môn học mẫu
            sampleCourses.forEach(course => {
                addCourseToSemester(semesterId, course.name, course.credits);
            });

            // Lưu thông tin kỳ học
            semesters.push({
                id: semesterId,
                courses: []
            });
        }

        function removeSemester(semesterId) {
            const semester = document.getElementById(semesterId);
            semester.remove();
            
            // Xóa khỏi mảng semesters
            const index = semesters.findIndex(s => s.id === semesterId);
            if (index > -1) {
                semesters.splice(index, 1);
            }
        }

        function addCourseToSemester(semesterId, name = "", credits = "") {
            const coursesList = document.getElementById(`${semesterId}-courses`);
            const newRow = document.createElement('tr');
            newRow.innerHTML = `
                <td><input type="text" class="course-name" placeholder="Tên môn học" value="${name}"></td>
                <td><input type="number" class="course-credits" min="1" max="15" placeholder="Số tín chỉ" value="${credits}"></td>
                <td><input type="number" class="course-grade" min="0" max="4" step="0.1" placeholder="Điểm GPA"></td>
                <td><button class="remove-course" onclick="removeCourse(this)">Xóa</button></td>
            `;
            coursesList.appendChild(newRow);
        }

        function removeCourse(button) {
            const row = button.parentElement.parentElement;
            row.remove();
        }

        function autoGenerateGrades(semesterId) {
            const coursesList = document.getElementById(`${semesterId}-courses`);
            const gradeInputs = coursesList.getElementsByClassName('course-grade');
            for (let input of gradeInputs) {
                const randomGrade = (Math.random() * 2 + 2).toFixed(1); // 2.0 - 4.0
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

        function calculateSemesterGPA(semesterId) {
            const coursesList = document.getElementById(`${semesterId}-courses`);
            const rows = coursesList.getElementsByTagName('tr');
            
            let totalCredits = 0;
            let totalPoints = 0;
            let hasError = false;

            for (let row of rows) {
                const credits = parseFloat(row.querySelector('.course-credits').value);
                const grade = parseFloat(row.querySelector('.course-grade').value);

                if (!credits || !grade) continue;

                if (credits < 1 || credits > 15) {
                    showError('Số tín chỉ phải từ 1 đến 15');
                    hasError = true;
                    break;
                }

                if (grade < 0 || grade > 4) {
                    showError('Điểm GPA phải từ 0 đến 4');
                    hasError = true;
                    break;
                }

                totalCredits += credits;
                totalPoints += credits * grade;
            }

            if (hasError) return;

            const gpa = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : 0;

            // Hiển thị kết quả học kỳ
            document.getElementById(`${semesterId}-gpa`).textContent = gpa;
            document.getElementById(`${semesterId}-credits`).textContent = totalCredits;
            document.getElementById(`${semesterId}-result`).style.display = 'block';

            // Cập nhật thông tin kỳ học
            const semesterIndex = semesters.findIndex(s => s.id === semesterId);
            if (semesterIndex > -1) {
                semesters[semesterIndex].gpa = parseFloat(gpa);
                semesters[semesterIndex].credits = totalCredits;
            }
        }

        function calculateOverallGPA() {
            let totalCredits = 0;
            let totalPoints = 0;

            semesters.forEach(semester => {
                if (semester.gpa && semester.credits) {
                    totalCredits += semester.credits;
                    totalPoints += semester.credits * semester.gpa;
                }
            });

            const overallGPA = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : 0;

            // Hiển thị kết quả tổng
            document.getElementById('overallGPA').textContent = overallGPA;
            document.getElementById('overallCredits').textContent = totalCredits;
            document.getElementById('overallResult').style.display = 'block';
        }

        // Khởi tạo kỳ học đầu tiên khi trang load
        window.onload = function() {
            addSemester();
        };
    </script>
</body>
</html>
