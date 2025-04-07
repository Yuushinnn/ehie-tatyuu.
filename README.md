# ehie-tatyuu.
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>愛媛県 中高生 卓球募集</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
            line-height: 1.6;
        }
        h1 {
            text-align: center;
            color: #2c3e50;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
        }
        .form-container, .list-container {
            flex: 1;
            min-width: 300px;
            max-width: 600px;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
        }
        .filter-container {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
            margin-bottom: 20px;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
        .button {
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.3s;
        }
        .submit-btn {
            background-color: #28a745;
            color: white;
        }
        .submit-btn:hover {
            background-color: #218838;
        }
        .delete-btn {
            background-color: #dc3545;
            color: white;
            padding: 5px 10px;
            font-size: 12px;
            margin-top: 10px;
        }
        .delete-btn:hover {
            background-color: #c82333;
        }
        label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
        }
        input, select, textarea {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        textarea {
            height: 100px;
            resize: vertical;
        }
        .recruitment-item {
            border: 1px solid #ddd;
            padding: 15px;
            margin-top: 15px;
            border-radius: 5px;
            position: relative;
        }
        .recruitment-meta {
            color: #666;
            font-size: 14px;
            margin-bottom: 10px;
        }
        .no-recruitments {
            text-align: center;
            color: #666;
            padding: 20px;
        }
        .filter-group {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
            flex-wrap: wrap;
        }
        .filter-group select {
            flex: 1;
            min-width: 120px;
        }
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>

    <h1>愛媛県 中高生 卓球募集</h1>

    <div class="filter-container">
        <h2>検索フィルター</h2>
        <div class="filter-group">
            <select id="filter-area">
                <option value="">すべての地域</option>
                <option value="松山">松山</option>
                <option value="新居浜">新居浜</option>
                <option value="今治">今治</option>
                <option value="西条">西条</option>
                <option value="その他">その他</option>
            </select>
            <select id="filter-level">
                <option value="">すべてのレベル</option>
                <option value="初心者">初心者</option>
                <option value="中級者">中級者</option>
                <option value="上級者">上級者</option>
            </select>
            <select id="filter-grade">
                <option value="">すべての学年</option>
                <option value="1年">1年</option>
                <option value="2年">2年</option>
                <option value="3年">3年</option>
            </select>
        </div>
        <button class="button" onclick="filterRecruitments()">検索</button>
        <button class="button" onclick="resetFilters()">リセット</button>
    </div>

    <div class="container">
        <div class="form-container">
            <h2>募集投稿フォーム</h2>
            <form id="recruitment-form">
                <label for="name">名前（任意）:</label>
                <input type="text" id="name" name="name" placeholder="匿名可">
                
                <label for="grade">学年:</label>
                <select id="grade" name="grade" required>
                    <option value="" disabled selected>選択してください</option>
                    <option value="1年">1年</option>
                    <option value="2年">2年</option>
                    <option value="3年">3年</option>
                </select>

                <label for="level">レベル:</label>
                <select id="level" name="level" required>
                    <option value="" disabled selected>選択してください</option>
                    <option value="初心者">初心者</option>
                    <option value="中級者">中級者</option>
                    <option value="上級者">上級者</option>
                </select>

                <label for="area">地域:</label>
                <select id="area" name="area" required>
                    <option value="" disabled selected>選択してください</option>
                    <option value="松山">松山</option>
                    <option value="新居浜">新居浜</option>
                    <option value="今治">今治</option>
                    <option value="西条">西条</option>
                    <option value="その他">その他</option>
                </select>

                <label for="message">募集内容:</label>
                <textarea id="message" name="message" required placeholder="練習したい内容、希望日時、連絡方法などを記入してください"></textarea>

                <button type="button" class="button submit-btn" onclick="submitForm()">投稿する</button>
            </form>
        </div>

        <div class="list-container">
            <h2>募集一覧 <span id="recruitment-count"></span></h2>
            <div id="recruitments">
                <!-- 投稿される募集内容がここに表示されます -->
            </div>
        </div>
    </div>

    <script>
        // ページ読み込み時に保存された投稿を表示
        document.addEventListener('DOMContentLoaded', function() {
            loadRecruitments();
            updateRecruitmentCount();
        });

        // 投稿を保存する関数
        function submitForm() {
            const name = document.getElementById('name').value || '匿名';
            const grade = document.getElementById('grade').value;
            const level = document.getElementById('level').value;
            const area = document.getElementById('area').value;
            const message = document.getElementById('message').value;

            if (!grade || !level || !area || !message) {
                alert('必須項目をすべて入力してください');
                return;
            }

            const recruitment = {
                id: Date.now(), // ユニークIDとしてタイムスタンプを使用
                name: name,
                grade: grade,
                level: level,
                area: area,
                message: message,
                date: new Date().toLocaleString()
            };

            // localStorageから既存の投稿を取得
            let recruitments = JSON.parse(localStorage.getItem('recruitments')) || [];
            
            // 新しい投稿を追加
            recruitments.push(recruitment);
            
            // localStorageに保存
            localStorage.setItem('recruitments', JSON.stringify(recruitments));
            
            // 投稿を表示
            displayRecruitment(recruitment);
            
            // フォームをリセット
            document.getElementById('recruitment-form').reset();
            
            // 投稿数を更新
            updateRecruitmentCount();
            
            alert('投稿が完了しました！');
        }

        // 投稿を表示する関数
        function displayRecruitment(recruitment) {
            const recruitmentsDiv = document.getElementById('recruitments');
            const newRecruitment = document.createElement('div');
            newRecruitment.className = 'recruitment-item';
            newRecruitment.id = 'recruitment-' + recruitment.id;
            
            newRecruitment.innerHTML = `
                <div class="recruitment-meta">
                    <strong>${recruitment.name}</strong> (${recruitment.grade}, ${recruitment.level}) - ${recruitment.area}
                    <br><small>投稿日: ${recruitment.date}</small>
                </div>
                <p>${recruitment.message}</p>
                <button class="button delete-btn" onclick="deleteRecruitment(${recruitment.id})">削除</button>
            `;
            
            recruitmentsDiv.appendChild(newRecruitment);
        }

        // 投稿を削除する関数
        function deleteRecruitment(id) {
            if (!confirm('本当にこの投稿を削除しますか？')) return;
            
            // localStorageから投稿を削除
            let recruitments = JSON.parse(localStorage.getItem('recruitments')) || [];
            recruitments = recruitments.filter(r => r.id !== id);
            localStorage.setItem('recruitments', JSON.stringify(recruitments));
            
            // 画面から投稿を削除
            const element = document.getElementById('recruitment-' + id);
            if (element) element.remove();
            
            // 投稿数を更新
            updateRecruitmentCount();
        }

        // 保存された投稿を読み込む関数
        function loadRecruitments() {
            const recruitments = JSON.parse(localStorage.getItem('recruitments')) || [];
            const recruitmentsDiv = document.getElementById('recruitments');
            recruitmentsDiv.innerHTML = '';
            
            if (recruitments.length === 0) {
                recruitmentsDiv.innerHTML = '<div class="no-recruitments">現在投稿はありません</div>';
                return;
            }
            
            recruitments.forEach(recruitment => {
                displayRecruitment(recruitment);
            });
        }

        // 投稿数を更新する関数
        function updateRecruitmentCount() {
            const recruitments = JSON.parse(localStorage.getItem('recruitments')) || [];
            const countElement = document.getElementById('recruitment-count');
            countElement.textContent = `(${recruitments.length}件)`;
        }

        // フィルタリング関数
        function filterRecruitments() {
            const areaFilter = document.getElementById('filter-area').value;
            const levelFilter = document.getElementById('filter-level').value;
            const gradeFilter = document.getElementById('filter-grade').value;
            
            const recruitments = JSON.parse(localStorage.getItem('recruitments')) || [];
            const recruitmentsDiv = document.getElementById('recruitments');
            recruitmentsDiv.innerHTML = '';
            
            let filtered = recruitments;
            
            if (areaFilter) {
                filtered = filtered.filter(r => r.area === areaFilter);
            }
            
            if (levelFilter) {
                filtered = filtered.filter
