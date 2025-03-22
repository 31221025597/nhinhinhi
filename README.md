<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản lí điểm cho Nhi Nhi Nhi</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #2563eb;
            --primary-dark: #1d4ed8;
            --success: #059669;
            --danger: #dc2626;
            --warning: #d97706;
            --info: #0891b2;
            --text-primary: #1f2937;
            --text-secondary: #4b5563;
            --bg-primary: #ffffff;
            --bg-secondary: #f3f4f6;
            --border: #e5e7eb;
            --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
            --shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }

        @keyframes gradient {
            0% {
                background-position: 0% 50%;
            }
            50% {
                background-position: 100% 50%;
            }
            100% {
                background-position: 0% 50%;
            }
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
            background-size: 400% 400%;
            animation: gradient 15s ease infinite;
            padding: 2rem;
            color: var(--text-primary);
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
        }

        .header {
            text-align: center;
            margin-bottom: 1.5rem;
            animation: fadeDown 0.6s ease-out;
            background: linear-gradient(120deg, #2563eb, #7c3aed);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            padding: 1rem;
        }

        .header h1 {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 1rem;
            letter-spacing: -0.025em;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        .header p {
            font-size: 1.2rem;
            color: var(--text-secondary);
        }

        .current-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
            animation: fadeIn 0.6s ease-out;
        }

        .input-group {
            background: rgba(255, 255, 255, 0.7);
            padding: 1rem;
            border-radius: 12px;
            box-shadow: var(--shadow-sm);
        }

        .input-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--text-secondary);
            font-size: 0.95rem;
        }

        .semester-card {
            background: rgba(255, 255, 255, 0.9);
            border-radius: 20px;
            padding: 2rem;
            margin-bottom: 2rem;
            box-shadow: var(--shadow);
            animation: slideUp 0.5s ease-out;
            border: 1px solid rgba(255, 255, 255, 0.18);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .semester-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.1);
        }

        .controls {
            display: flex;
            gap: 1rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
            justify-content: center;
            animation: fadeIn 0.6s ease-out;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 12px;
            font-weight: 500;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            color: white;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-primary {
            background: linear-gradient(45deg, #2563eb, #7c3aed);
            box-shadow: 0 4px 15px rgba(37, 99, 235, 0.2);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(37, 99, 235, 0.3);
        }

        .btn-success {
            background: linear-gradient(45deg, #059669, #10b981);
            box-shadow: 0 4px 15px rgba(5, 150, 105, 0.2);
        }

        .btn-success:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(5, 150, 105, 0.3);
        }

        .btn-remove {
            background: linear-gradient(45deg, #dc2626, #ef4444);
            box-shadow: 0 4px 15px rgba(220, 38, 38, 0.2);
        }

        .btn-remove:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(220, 38, 38, 0.3);
        }

        .semester-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .semester-title {
            font-size: 1.5rem;
            font-weight: 600;
            color: var(--text-primary);
        }

        .semester-actions {
            display: flex;
            gap: 0.5rem;
        }

        .stat-card {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.7));
            padding: 1.5rem;
            border-radius: 15px;
            text-align: center;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.18);
            transition: transform 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-3px);
        }

        .stat-value {
            font-size: 2rem;
            font-weight: 700;
            background: linear-gradient(45deg, #2563eb, #7c3aed);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .result-card {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.7));
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.18);
            border-radius: 20px;
            padding: 2rem;
            margin-top: 2rem;
        }

        .result-title {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            color: var(--text-primary);
            text-align: center;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
        }

        input, select {
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid rgba(200, 200, 200, 0.5);
            border-radius: 10px;
            padding: 0.75rem 1rem;
            font-size: 0.95rem;
            width: 100%;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #2563eb;
        }

        /* Chỉ cho phép nhập số trong input type number */
        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        
        input[type="number"] {
            -moz-appearance: textfield;
        }

        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0 0.5rem;
            margin-bottom: 1.5rem;
        }

        th {
            background: linear-gradient(45deg, rgba(37, 99, 235, 0.1), rgba(124, 58, 237, 0.1));
            color: var(--text-secondary);
            font-weight: 600;
            padding: 1rem;
            text-align: left;
            border-radius: 10px;
        }

        td {
            padding: 0.75rem;
            background: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(4px);
        }

        tr:hover td {
            background: rgba(255, 255, 255, 0.8);
        }

        .forecast-section {
            margin-top: 3rem;
            padding: 2rem;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.7), rgba(255, 255, 255, 0.5));
            border-radius: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.18);
            animation: fadeIn 0.6s ease-out;
        }

        .forecast-title {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            text-align: center;
            color: var(--text-primary);
        }

        .forecast-inputs {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .forecast-result {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.6));
            padding: 1.5rem;
            border-radius: 15px;
            margin-top: 1.5rem;
            text-align: center;
            font-size: 1.1rem;
        }
        
        .forecast-result.not-feasible {
            background: linear-gradient(135deg, rgba(254, 226, 226, 0.8), rgba(254, 202, 202, 0.6));
            color: #b91c1c;
        }

        .chart-container {
            background: white;
            border-radius: 15px;
            padding: 1.5rem;
            margin-top: 1.5rem;
            box-shadow: var(--shadow);
        }

        @keyframes fadeDown {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 768px) {
            .container {
                padding: 1rem;
            }

            .header h1 {
                font-size: 2rem;
            }

            .semester-header {
                flex-direction: column;
                align-items: stretch;
                gap: 1rem;
            }

            .btn {
                width: 100%;
                justify-content: center;
            }

            th, td {
                padding: 0.75rem;
            }
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
            color: var(--text-secondary);
        }

        .course-row {
            display: flex;
            gap: 1rem;
            margin-bottom: 0.5rem;
            align-items: center;
        }

        .course-row button {
            flex-shrink: 0;
        }

        .remove-btn {
            background: linear-gradient(45deg, #dc2626, #ef4444);
            color: white;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 2px 5px rgba(220, 38, 38, 0.2);
        }

        .remove-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 3px 8px rgba(220, 38, 38, 0.3);
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Quản lí điểm cho Nhi Nhi Nhi</h1>
            <p>Theo dõi và dự báo điểm số một cách thông minh</p>
        </div>

        <div class="current-stats">
            <div class="input-group">
                <label>GPA Hiện Tại</label>
                <input type="number" id="initialGPA" step="0.01" min="0" max="4" placeholder="Nhập GPA hiện tại" oninput="validateGPA(this); updateInitialValues()">
            </div>
            <div class="input-group">
                <label>Tín Chỉ Hiện Tại</label>
                <input type="number" id="initialCredits" min="0" placeholder="Nhập số tín chỉ hiện tại" oninput="updateInitialValues()">
            </div>
        </div>

        <div class="controls">
            <button class="btn btn-primary" onclick="addSemester()">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <line x1="12" y1="5" x2="12" y2="19"></line>
                    <line x1="5" y1="12" x2="19" y2="12"></line>
                </svg>
                Thêm kỳ học mới
            </button>
            <button class="btn btn-success" onclick="calculateOverallGPA()">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                    <polyline points="22 4 12 14.01 9 11.01"></polyline>
                </svg>
                Tính GPA tích lũy
            </button>
        </div>

        <div id="semesters"></div>

        <div id="overallResult" class="result-card" style="display: none;">
            <div class="result-title">Kết quả tích lũy</div>
            <div class="stats-grid">
                <div class="stat-card">
                    <div class="stat-value" id="overallGPA">0.00</div>
                    <div class="stat-label">GPA tích lũy</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value" id="overallCredits">0</div>
                    <div class="stat-label">Tổng số tín chỉ</div>
                </div>
            </div>
        </div>

        <div class="forecast-section">
            <div class="forecast-title">Dự báo GPA</div>
            <div class="forecast-inputs">
                <div class="form-group">
                    <label>GPA mục tiêu</label>
                    <input type="number" id="targetGPA" step="0.01" min="0" max="4" placeholder="Nhập GPA mục tiêu" oninput="validateGPA(this); calculateForecast()">
                </div>
                <div class="form-group">
                    <label>Số tín chỉ còn lại</label>
                    <input type="number" id="remainingCredits" min="0" placeholder="Số tín chỉ còn lại" oninput="calculateForecast()">
                </div>
            </div>
            <button class="btn btn-primary" onclick="calculateForecast()">Dự báo GPA</button>
            <div class="forecast-result" id="forecastResult" style="display: none;">
                <p>Để đạt GPA mục tiêu <strong id="targetGPAValue">0.00</strong>, bạn cần đạt GPA các kỳ tới là: <strong id="requiredGPA">0.00</strong></p>
            </div>
            <div class="forecast-result not-feasible" id="notFeasibleResult" style="display: none;">
                <p>Mục tiêu GPA <strong id="targetGPANotFeasible">0.00</strong> là không khả thi với số tín chỉ còn lại.</p>
                <p>GPA cần đạt (<strong id="requiredGPANotFeasible">0.00</strong>) vượt quá thang điểm 4.0</p>
            </div>
            <div class="chart-container">
                <canvas id="forecastChart"></canvas>
            </div>
        </div>
    </div>

    <script>
        let semesterCount = 0;
        let forecastChart = null;
        
        // Giới hạn giá trị GPA không quá 4
        function validateGPA(input) {
            if (input.value > 4) {
                input.value = 4;
            }
        }
        
        // Cập nhật giá trị ban đầu
        function updateInitialValues() {
            calculateOverallGPA();
        }
        
        // Thêm kỳ học mới
        function addSemester() {
            semesterCount++;
            const semestersDiv = document.getElementById('semesters');
            const semesterId = `semester-${semesterCount}`;
            
            const semesterDiv = document.createElement('div');
            semesterDiv.className = 'semester-card';
            semesterDiv.id = semesterId;
            
            semesterDiv.innerHTML = `
                <div class="semester-header">
                    <div class="semester-title">Kỳ học ${semesterCount}</div>
                    <div class="semester-actions">
                        <button class="btn btn-primary" onclick="addCourse('${semesterId}')">Thêm môn học</button>
                    </div>
                </div>
                <div id="${semesterId}-courses">
                    <!-- Courses will be added here -->
                </div>
                <div class="stats-grid" style="margin-top: 1.5rem;">
                    <div class="stat-card">
                        <div class="stat-value" id="${semesterId}-gpa">0.00</div>
                        <div class="stat-label">GPA kỳ này</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="${semesterId}-credits">0</div>
                        <div class="stat-label">Số tín chỉ</div>
                    </div>
                </div>
            `;
            
            semestersDiv.appendChild(semesterDiv);
            addCourse(semesterId);
        }
        
        // Thêm môn học mới vào kỳ học
        function addCourse(semesterId) {
            const coursesDiv = document.getElementById(`${semesterId}-courses`);
            const courseRow = document.createElement('div');
            courseRow.className = 'course-row';
            
            courseRow.innerHTML = `
                <div style="flex: 2;">
                    <input type="text" placeholder="Tên môn học" class="course-name">
                </div>
                <div style="flex: 1;">
                    <input type="number" min="1" max="15" placeholder="Số tín chỉ" class="course-credits" oninput="calculateSemesterGPA('${semesterId}')">
                </div>
                <div style="flex: 1;">
                    <input type="number" min="0" max="4" step="0.1" placeholder="Điểm GPA" class="course-grade" oninput="validateGPA(this); calculateSemesterGPA('${semesterId}')">
                </div>
                <button class="remove-btn" onclick="removeCourse(this, '${semesterId}')">×</button>
            `;
            
            coursesDiv.appendChild(courseRow);
        }
        
        // Xóa môn học
        function removeCourse(button, semesterId) {
            const courseRow = button.parentNode;
            courseRow.parentNode.removeChild(courseRow);
            calculateSemesterGPA(semesterId);
        }
        
        // Tính GPA cho một kỳ học
        function calculateSemesterGPA(semesterId) {
            const coursesDiv = document.getElementById(`${semesterId}-courses`);
            const courseRows = coursesDiv.getElementsByClassName('course-row');
            
            let totalCredits = 0;
            let totalPoints = 0;
            
            for (let row of courseRows) {
                const creditsInput = row.querySelector('.course-credits');
                const gradeInput = row.querySelector('.course-grade');
                
                const credits = parseFloat(creditsInput.value);
                const grade = parseFloat(gradeInput.value);
                
                if (!isNaN(credits) && !isNaN(grade)) {
                    totalCredits += credits;
                    totalPoints += credits * grade;
                }
            }
            
            const gpa = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : '0.00';
            
            document.getElementById(`${semesterId}-gpa`).textContent = gpa;
            document.getElementById(`${semesterId}-credits`).textContent = totalCredits;
            
            // Cập nhật dự báo GPA sau khi tính GPA kỳ học
            calculateOverallGPA();
        }
        
        // Tính GPA tích lũy
        function calculateOverallGPA() {
            // Lấy giá trị ban đầu từ input
            const initialGPA = parseFloat(document.getElementById('initialGPA').value) || 0;
            const initialCredits = parseFloat(document.getElementById('initialCredits').value) || 0;
            
            let totalCredits = initialCredits;
            let totalPoints = initialCredits * initialGPA;
            
            for (let i = 1; i <= semesterCount; i++) {
                const semesterId = `semester-${i}`;
                const semesterGPA = parseFloat(document.getElementById(`${semesterId}-gpa`).textContent);
                const semesterCredits = parseFloat(document.getElementById(`${semesterId}-credits`).textContent);
                
                if (!isNaN(semesterGPA) && !isNaN(semesterCredits)) {
                    totalCredits += semesterCredits;
                    totalPoints += semesterCredits * semesterGPA;
                }
            }
            
            const overallGPA = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : '0.00';
            
            document.getElementById('overallGPA').textContent = overallGPA;
            document.getElementById('overallCredits').textContent = totalCredits;
            document.getElementById('overallResult').style.display = 'block';
            
            // Tự động cập nhật dự báo
            calculateForecast();
        }
        
        // Tính toán dự báo GPA
        function calculateForecast() {
            const currentGPA = parseFloat(document.getElementById('overallGPA').textContent);
            const currentCredits = parseFloat(document.getElementById('overallCredits').textContent);
            const targetGPA = parseFloat(document.getElementById('targetGPA').value);
            const remainingCredits = parseInt(document.getElementById('remainingCredits').value);
            
            if (isNaN(targetGPA) || isNaN(remainingCredits)) {
                // Chỉ hiển thị kết quả khi đã nhập đầy đủ thông tin
                document.getElementById('forecastResult').style.display = 'none';
                document.getElementById('notFeasibleResult').style.display = 'none';
                return;
            }
            
            // Công thức tính GPA cần đạt
            const totalTargetPoints = targetGPA * (currentCredits + remainingCredits);
            const currentPoints = currentGPA * currentCredits;
            const requiredPoints = totalTargetPoints - currentPoints;
            const requiredGPA = (requiredPoints / remainingCredits).toFixed(2);
            
            // Kiểm tra xem mục tiêu có khả thi không (GPA cần đạt <= 4.0)
            if (parseFloat(requiredGPA) > 4) {
                // Hiển thị thông báo không khả thi
                document.getElementById('targetGPANotFeasible').textContent = targetGPA.toFixed(2);
                document.getElementById('requiredGPANotFeasible').textContent = requiredGPA;
                document.getElementById('forecastResult').style.display = 'none';
                document.getElementById('notFeasibleResult').style.display = 'block';
            } else {
                // Hiển thị kết quả bình thường
                document.getElementById('targetGPAValue').textContent = targetGPA.toFixed(2);
                document.getElementById('requiredGPA').textContent = requiredGPA;
                document.getElementById('forecastResult').style.display = 'block';
                document.getElementById('notFeasibleResult').style.display = 'none';
            }
            
            // Vẽ biểu đồ dự báo
            updateForecastChart(currentGPA, targetGPA, requiredGPA);
        }
        
        // Cập nhật biểu đồ dự báo
        function updateForecastChart(currentGPA, targetGPA, requiredGPA) {
            if (forecastChart) {
                forecastChart.destroy();
            }
            
            const ctx = document.getElementById('forecastChart').getContext('2d');
            
            const parsedRequiredGPA = parseFloat(requiredGPA);
            const chartData = [currentGPA, targetGPA];
            const chartLabels = ['GPA Hiện Tại', 'GPA Mục Tiêu'];
            const chartColors = [
                'rgba(37, 99, 235, 0.6)',
                'rgba(5, 150, 105, 0.6)'
            ];
            const chartBorders = [
                'rgba(37, 99, 235, 1)',
                'rgba(5, 150, 105, 1)'
            ];
            
                          // Chỉ hiển thị GPA cần đạt nếu khả thi
              if (parsedRequiredGPA <= 4) {
                  chartData.push(parsedRequiredGPA);
                  chartLabels.push('GPA Cần Đạt');
                  chartColors.push('rgba(220, 38, 38, 0.6)');
                  chartBorders.push('rgba(220, 38, 38, 1)');
              }
              
              forecastChart = new Chart(ctx, {
                  type: 'bar',
                  data: {
                      labels: chartLabels,
                      datasets: [{
                          label: 'GPA',
                          data: chartData,
                          backgroundColor: chartColors,
                          borderColor: chartBorders,
                          borderWidth: 1
                      }]
                  },
                  options: {
                      responsive: true,
                      scales: {
                          y: {
                              beginAtZero: true,
                              max: 4.5,
                              title: {
                                  display: true,
                                  text: 'GPA'
                              }
                          }
                      },
                      plugins: {
                          title: {
                              display: true,
                              text: 'So sánh GPA',
                              font: {
                                  size: 16
                              }
                          },
                          legend: {
                              display: false
                          }
                      }
                  }
              });
          }
          
          // Khởi tạo một kỳ học mặc định khi trang được tải
          window.onload = function() {
              addSemester();
          };
          // Sửa lại hàm addSemester(), thêm nút xóa vào semester-actions
function addSemester() {
    semesterCount++;
    const semestersDiv = document.getElementById('semesters');
    const semesterId = `semester-${semesterCount}`;
    
    const semesterDiv = document.createElement('div');
    semesterDiv.className = 'semester-card';
    semesterDiv.id = semesterId;
    
    semesterDiv.innerHTML = `
        <div class="semester-header">
            <div class="semester-title">Kỳ học ${semesterCount}</div>
            <div class="semester-actions">
                <button class="btn btn-primary" onclick="addCourse('${semesterId}')">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <line x1="12" y1="5" x2="12" y2="19"></line>
                        <line x1="5" y1="12" x2="19" y2="12"></line>
                    </svg>
                    Thêm môn học
                </button>
                <button class="btn btn-remove" onclick="removeSemester('${semesterId}')">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                    </svg>
                    Xóa kỳ học
                </button>
            </div>
        </div>
        <div id="${semesterId}-courses">
            <!-- Courses will be added here -->
        </div>
        <div class="stats-grid" style="margin-top: 1.5rem;">
            <div class="stat-card">
                <div class="stat-value" id="${semesterId}-gpa">0.00</div>
                <div class="stat-label">GPA kỳ này</div>
            </div>
            <div class="stat-card">
                <div class="stat-value" id="${semesterId}-credits">0</div>
                <div class="stat-label">Số tín chỉ</div>
            </div>
        </div>
    `;
    
    semestersDiv.appendChild(semesterDiv);
    addCourse(semesterId);
}

// Thêm hàm xóa học kì
function removeSemester(semesterId) {
    const semesterDiv = document.getElementById(semesterId);
    if (semesterDiv) {
        // Hiển thị hộp thoại xác nhận
        if (confirm('Bạn có chắc chắn muốn xóa kỳ học này?')) {
            semesterDiv.remove();
            // Cập nhật lại GPA tích lũy
            calculateOverallGPA();
        }
    }
}

// Cập nhật lại hàm calculateOverallGPA() để xử lý đúng khi có học kì bị xóa
function calculateOverallGPA() {
    // Lấy giá trị ban đầu từ input
    const initialGPA = parseFloat(document.getElementById('initialGPA').value) || 0;
    const initialCredits = parseFloat(document.getElementById('initialCredits').value) || 0;
    
    let totalCredits = initialCredits;
    let totalPoints = initialCredits * initialGPA;
    
    // Thay đổi cách lấy danh sách học kì
    const semesterDivs = document.getElementsByClassName('semester-card');
    
    for (let semesterDiv of semesterDivs) {
        const semesterId = semesterDiv.id;
        const semesterGPA = parseFloat(document.getElementById(`${semesterId}-gpa`).textContent);
        const semesterCredits = parseFloat(document.getElementById(`${semesterId}-credits`).textContent);
        
        if (!isNaN(semesterGPA) && !isNaN(semesterCredits)) {
            totalCredits += semesterCredits;
            totalPoints += semesterCredits * semesterGPA;
        }
    }
    
    const overallGPA = totalCredits > 0 ? (totalPoints / totalCredits).toFixed(2) : '0.00';
    
    document.getElementById('overallGPA').textContent = overallGPA;
    document.getElementById('overallCredits').textContent = totalCredits;
    document.getElementById('overallResult').style.display = 'block';
    
    // Tự động cập nhật dự báo
    calculateForecast();
}

      </script>
  </body>
  </html>
