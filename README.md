<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>斗六顧德補習班 - 成績登記表</title>
    <style>
        body { 
            font-family: '微軟正黑體', sans-serif; 
            background-color: #f4f7f6; 
            margin: 0; 
            padding: 20px; 
        }
        .container { 
            max-width: 800px; 
            margin: 0 auto; 
            background: white; 
            padding: 30px; 
            border-radius: 10px; 
            box-shadow: 0 4px 10px rgba(0,0,0,0.1); 
        }
        h1 { 
            text-align: center; 
            color: #2c3e50; 
            margin-bottom: 20px;
        }
        .form-panel {
            background: #eaf2f8; 
            padding: 20px; 
            border-radius: 8px; 
            margin-bottom: 20px;
        }
        .form-group { 
            margin-bottom: 15px; 
        }
        label { 
            display: block; 
            margin-bottom: 5px; 
            font-weight: bold; 
            color: #34495e;
        }
        input { 
            width: 100%; 
            padding: 10px; 
            box-sizing: border-box; 
            border: 1px solid #ccc; 
            border-radius: 5px; 
            font-size: 16px;
        }
        button { 
            background-color: #27ae60; 
            color: white; 
            border: none; 
            padding: 12px 15px; 
            cursor: pointer; 
            border-radius: 5px; 
            font-size: 16px; 
            width: 100%; 
            font-weight: bold;
        }
        button:hover { 
            background-color: #219150; 
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            margin-top: 20px; 
        }
        th, td { 
            border: 1px solid #ddd; 
            padding: 12px; 
            text-align: center; 
        }
        th { 
            background-color: #2980b9; 
            color: white; 
        }
        .delete-btn { 
            background-color: #e74c3c; 
            padding: 6px 12px; 
            width: auto; 
            font-size: 14px;
        }
        .delete-btn:hover { 
            background-color: #c0392b; 
        }
        .empty-message {
            text-align: center;
            color: #7f8c8d;
            padding: 20px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🎓 斗六顧德補習班 <br> 專屬成績登記系統</h1>

    <div class="form-panel">
        <div class="form-group">
            <label for="studentName">學生姓名：</label>
            <input type="text" id="studentName" placeholder="例如：王小明">
        </div>
        <div class="form-group">
            <label for="examSubject">考試項目：</label>
            <input type="text" id="examSubject" placeholder="例如：國一數學 平時測驗">
        </div>
        <div class="form-group">
            <label for="examScore">考試分數：</label>
            <input type="number" id="examScore" placeholder="例如：95" min="0" max="100">
        </div>
        <button onclick="addScore()">➕ 新增成績</button>
    </div>

    <table>
        <thead>
            <tr>
                <th>學生姓名</th>
                <th>考試項目</th>
                <th>分數</th>
                <th>操作</th>
            </tr>
        </thead>
        <tbody id="scoreTableBody">
            </tbody>
    </table>
</div>

<script>
    // 當網頁載入完成時，立刻讀取歷史資料
    document.addEventListener('DOMContentLoaded', loadScores);

    // 新增成績的函數
    function addScore() {
        const name = document.getElementById('studentName').value;
        const subject = document.getElementById('examSubject').value;
        const score = document.getElementById('examScore').value;

        // 檢查有沒有漏填
        if (!name || !subject || !score) {
            alert('⚠️ 請填寫完整的學生姓名、科目與分數！');
            return;
        }

        // 建立一筆新資料，用 Date.now() 產生唯一 ID
        const record = { name, subject, score, id: Date.now() };

        // 從瀏覽器的 LocalStorage 取得舊資料
        let scores = JSON.parse(localStorage.getItem('gudeScores')) || [];
        scores.push(record);

        // 儲存回 LocalStorage
        localStorage.setItem('gudeScores', JSON.stringify(scores));

        // 清空輸入框，方便輸入下一個
        document.getElementById('studentName').value = '';
        document.getElementById('examSubject').value = '';
        document.getElementById('examScore').value = '';

        // 重新整理表格
        loadScores();
    }

    // 載入並顯示成績的函數
    function loadScores() {
        const scores = JSON.parse(localStorage.getItem('gudeScores')) || [];
        const tbody = document.getElementById('scoreTableBody');
        tbody.innerHTML = '';

        if (scores.length === 0) {
            tbody.innerHTML = '<tr><td colspan="4" class="empty-message">目前還沒有成績紀錄喔！請從上方新增。</td></tr>';
            return;
        }

        // 將每一筆資料轉成表格的 Row (<tr>)
        scores.forEach(record => {
            const tr = document.createElement('tr');
            tr.innerHTML = `
                <td>${record.name}</td>
                <td>${record.subject}</td>
                <td style="font-weight: bold; color: ${record.score < 60 ? '#e74c3c' : '#27ae60'};">${record.score}</td>
                <td><button class="delete-btn" onclick="deleteScore(${record.id})">刪除</button></td>
            `;
            tbody.appendChild(tr);
        });
    }

    // 刪除特定成績的函數
    function deleteScore(id) {
        if(confirm('確定要刪除這筆成績嗎？')) {
            let scores = JSON.parse(localStorage.getItem('gudeScores')) || [];
            // 過濾掉被點擊刪除的那一筆
            scores = scores.filter(record => record.id !== id);
            localStorage.setItem('gudeScores', JSON.stringify(scores));
            loadScores();
        }
    }
</script>

</body>
</html>
