<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>斗六顧德補習班 - 成績登記表 v2.0</title>
    <style>
        body { 
            font-family: '微軟正黑體', sans-serif; 
            background-color: #f8f9fa; 
            margin: 0; 
            padding: 20px; 
            color: #333;
        }
        .container { 
            max-width: 800px; 
            margin: 0 auto; 
            background: white; 
            padding: 30px; 
            border-radius: 12px; 
            box-shadow: 0 5px 15px rgba(0,0,0,0.05); 
            border-top: 5px solid #0056b3;
        }
        h1 { 
            text-align: center; 
            color: #0056b3; 
            margin-bottom: 5px;
        }
        .subtitle {
            text-align: center;
            color: #6c757d;
            margin-bottom: 25px;
            font-size: 14px;
        }
        .form-panel {
            background: #f1f8ff; 
            padding: 20px; 
            border-radius: 8px; 
            margin-bottom: 25px;
            border: 1px solid #cce5ff;
        }
        .input-row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        .input-group {
            flex: 1;
        }
        label { 
            display: block; 
            margin-bottom: 8px; 
            font-weight: bold; 
            font-size: 14px;
        }
        input { 
            width: 100%; 
            padding: 10px; 
            box-sizing: border-box; 
            border: 1px solid #ced4da; 
            border-radius: 6px; 
        }
        button.add-btn { 
            background-color: #28a745; 
            color: white; 
            border: none; 
            padding: 12px 15px; 
            cursor: pointer; 
            border-radius: 6px; 
            font-size: 16px; 
            width: 100%; 
            font-weight: bold;
            transition: 0.3s;
        }
        button.add-btn:hover { 
            background-color: #218838; 
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
        }
        th, td { 
            border-bottom: 1px solid #dee2e6; 
            padding: 12px; 
            text-align: center; 
        }
        th { 
            background-color: #0056b3; 
            color: white; 
        }
        tr:hover {
            background-color: #f8f9fa;
        }
        .delete-btn { 
            background-color: #dc3545; 
            color: white;
            border: none;
            padding: 6px 12px; 
            border-radius: 4px;
            cursor: pointer;
            font-size: 13px;
        }
        .delete-btn:hover { 
            background-color: #c82333; 
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🎓 斗六顧德補習班</h1>
    <div class="subtitle">內部成績登記系統 v2.0</div>

    <div class="form-panel">
        <div class="input-row">
            <div class="input-group">
                <label for="studentName">學生姓名</label>
                <input type="text" id="studentName" placeholder="例：王小明">
            </div>
            <div class="input-group">
                <label for="examSubject">測驗項目</label>
                <input type="text" id="examSubject" placeholder="例：國一數學">
            </div>
            <div class="input-group">
                <label for="examScore">分數</label>
                <input type="number" id="examScore" placeholder="例：95">
            </div>
        </div>
        <button class="add-btn" onclick="addScore()">➕ 新增紀錄</button>
    </div>

    <table>
        <thead>
            <tr>
                <th>學生姓名</th>
                <th>測驗項目</th>
                <th>分數</th>
                <th>操作</th>
            </tr>
        </thead>
        <tbody id="scoreTableBody">
            </tbody>
    </table>
</div>

<script>
    document.addEventListener('DOMContentLoaded', loadScores);

    function addScore() {
        const name = document.getElementById('studentName').value;
        const subject = document.getElementById('examSubject').value;
        const score = document.getElementById('examScore').value;

        if (!name || !subject || !score) {
            alert('⚠️ 請完整填寫資料！');
            return;
        }

        const record = { name, subject, score, id: Date.now() };
        let scores = JSON.parse(localStorage.getItem('gudeScores_v2')) || [];
        scores.push(record);
        localStorage.setItem('gudeScores_v2', JSON.stringify(scores));

        document.getElementById('studentName').value = '';
        document.getElementById('examSubject').value = '';
        document.getElementById('examScore').value = '';

        loadScores();
    }

    function loadScores() {
        const scores = JSON.parse(localStorage.getItem('gudeScores_v2')) || [];
        const tbody = document.getElementById('scoreTableBody');
        tbody.innerHTML = '';

        if (scores.length === 0) {
            tbody.innerHTML = '<tr><td colspan="4" style="color:#6c757d; padding:20px;">尚無資料，請從上方新增。</td></tr>';
            return;
        }

        scores.forEach(record => {
            const tr = document.createElement('tr');
            tr.innerHTML = `
                <td>${record.name}</td>
                <td>${record.subject}</td>
                <td style="font-weight:bold; color:${record.score < 60 ? '#dc3545' : '#28a745'}">${record.score}</td>
                <td><button class="delete-btn" onclick="deleteScore(${record.id})">刪除</button></td>
            `;
            tbody.appendChild(tr);
        });
    }

    function deleteScore(id) {
        if(confirm('確定刪除？')) {
            let scores = JSON.parse(localStorage.getItem('gudeScores_v2')) || [];
            scores = scores.filter(r => r.id !== id);
            localStorage.setItem('gudeScores_v2', JSON.stringify(scores));
            loadScores();
        }
    }
</script>

</body>
</html>
