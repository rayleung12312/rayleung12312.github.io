# rayleung12312.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>賬號管理系統 - 登錄</title>
    <style>
        /* 全局樣式 */
        * {
            box-sizing: border-box;
        }
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #4CAF50;
            color: white;
            padding: 1rem;
            text-align: center;
            position: relative;
        }
        .logout-button {
            position: absolute;
            top: 50%;
            right: 1rem;
            transform: translateY(-50%);
            background-color: #f44336;
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            cursor: pointer;
            border-radius: 5px;
            font-size: 1rem;
            text-align: center;
            line-height: 30px;
            display: none; /* 初始狀態隱藏，登入後顯示 */
        }
        main {
            padding: 1rem;
        }

        /* 登錄 & 系統容器 */
        .login-container,
        .system-container {
            display: none; /* 只顯示一個容器 */
        }
        .visible {
            display: block;
        }

        /* 登錄表單 */
        .form-container {
            max-width: 400px;
            margin: 2rem auto;
            padding: 1rem;
            border: 1px solid #ccc;
            border-radius: 5px;
            background: #f9f9f9;
        }
        .form-container h2 {
            margin-top: 0;
        }
        input,
        button {
            width: 100%;
            padding: 0.5rem;
            font-size: 1rem;
            margin-bottom: 1rem;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }

        /* 小型按鈕樣式 */
        .btn-sm {
            font-size: 0.8rem;
            padding: 0.25rem 0.5rem;
            width: auto;
            margin-bottom: 0; /* 移除與 input 同行時的 margin */
        }

        /* 賬號列表容器 */
        .gallery {
            display: flex;
            flex-direction: column;
            gap: 1.5rem; /* 與分類標題拉開一些間距 */
            margin-top: 1rem;
        }

        /* 每個分類區塊的容器 (等待中 / 已完成) */
        .account-category {
            border: 1px solid #ccc;
            border-radius: 5px;
            padding: 0.5rem;
            background-color: #fafafa;
        }
        .account-category h3 {
            margin: 0.5rem 0;
            display: flex;         /* 讓按鈕可以在標題旁 */
            justify-content: space-between;
            align-items: center;
        }
        .account-list {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            margin-top: 0.5rem;
        }

        /* 賬號行 (單行顯示所有資訊) */
        .account-row {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 0.5rem;
            border: 1px solid #ccc;
            border-radius: 5px;
            padding: 0.5rem;
            background-color: #fff;
            position: relative;
        }

        /* 區塊：顯示中或編輯中的電郵、密碼、電話、建立時間 */
        .basic-info {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        .basic-info span {
            font-size: 0.9rem;
            color: #333;
        }
        .basic-info .label {
            font-weight: bold;
            color: #555;
        }
        .basic-info input {
            width: auto;
        }

        /* 验證碼圖片樣式：分「縮略」與「放大」兩種狀態+過渡 */
        .verification-image {
            border: 1px solid #ccc;
            border-radius: 3px;
            margin-left: 0.5rem;
            transition: transform 0.3s, max-width 0.3s, max-height 0.3s;
        }
        /* 縮略狀態：較小 */
        .verification-image.small {
            max-width: 60px;
            max-height: 40px;
        }
        /* 放大狀態 (比原本再大 1.5 倍) */
        .verification-image.large {
            position: relative;
            z-index: 10;
            transform-origin: top left;
            transform: scale(1.5);
            max-width: 300px;
            max-height: 200px;
        }

        /* 按鈕容器 */
        .account-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
            margin-left: auto; /* 推到右側 */
        }
        .delete-button {
            background-color: #f44336;
        }

        /* 響應式 */
        @media (max-width: 600px) {
            .form-container {
                width: 90%;
                margin: 1rem auto;
            }
            .account-row {
                flex-direction: column;
                align-items: flex-start;
            }
            .account-actions {
                margin-left: 0;
                width: 100%;
                justify-content: flex-start;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>賬號管理系統</h1>
        <button id="logoutButton" class="logout-button" onclick="logout()">X</button>
    </header>
    <main>
        <!-- 登錄區域 -->
        <div id="loginContainer" class="login-container visible">
            <div class="form-container">
                <h2>登入</h2>
                <p id="errorMessage" style="color: red;"></p>
                <input type="text" id="usernameInput" placeholder="賬號" />
                <input type="password" id="passwordInput" placeholder="密碼" />
                <button onclick="login()">登入</button>
            </div>
        </div>

        <!-- 賬號管理區域 -->
        <div id="systemContainer" class="system-container">
            <!-- 新增賬號表單 -->
            <div class="form-container">
                <h2>新增賬號</h2>
                <input type="email" id="emailInput" placeholder="電郵 (必填)" />
                <input type="text" id="passwordInputAccount" placeholder="密碼 (必填)" />
                <input type="tel" id="phoneInput" placeholder="電話 (必填)" />
                <button onclick="addAccount()">設立賬號</button>
            </div>

            <!-- 賬號列表：等待中 / 已完成 -->
            <div class="gallery">
                <div id="pendingAccounts" class="account-category">
                    <h3 id="pendingTitle">
                        等待中賬號 (0)
                        <button id="pendingToggleBtn" class="btn-sm"></button>
                    </h3>
                    <div class="account-list" id="pendingList"></div>
                </div>

                <div id="completedAccounts" class="account-category">
                    <h3 id="completedTitle">
                        已完成賬號 (0)
                        <button id="completedToggleBtn" class="btn-sm"></button>
                    </h3>
                    <div class="account-list" id="completedList"></div>
                </div>
            </div>
        </div>
    </main>

    <script>
        // 登錄驗證用戶名與密碼
        const validUsername = "uber";
        const validPassword = "qwer1234";

        // DOM 參照
        const loginContainer = document.getElementById("loginContainer");
        const systemContainer = document.getElementById("systemContainer");
        const errorMessage = document.getElementById("errorMessage");
        const logoutButton = document.getElementById("logoutButton");

        const pendingTitle = document.getElementById("pendingTitle");
        const pendingList = document.getElementById("pendingList");
        const pendingToggleBtn = document.getElementById("pendingToggleBtn");

        const completedTitle = document.getElementById("completedTitle");
        const completedList = document.getElementById("completedList");
        const completedToggleBtn = document.getElementById("completedToggleBtn");

        // 兩個布林值，控制等待中帳號 / 已完成帳號的收合狀態
        let isPendingExpanded = true;
        let isCompletedExpanded = true;

        // 賬號資料
        let accountsData = [];

        // 綁定按鈕事件
        pendingToggleBtn.onclick = togglePending;
        completedToggleBtn.onclick = toggleCompleted;

        // 登錄
        function login() {
            const username = document.getElementById("usernameInput").value.trim();
            const password = document.getElementById("passwordInput").value.trim();

            if (username === validUsername && password === validPassword) {
                loginContainer.style.display = "none";
                systemContainer.style.display = "block";
                logoutButton.style.display = "block";
            } else {
                errorMessage.textContent = "賬號或密碼錯誤，請重試！";
            }
        }

        // 登出
        function logout() {
            document.getElementById("usernameInput").value = "";
            document.getElementById("passwordInput").value = "";
            errorMessage.textContent = "";

            loginContainer.style.display = "block";
            systemContainer.style.display = "none";
            logoutButton.style.display = "none";
        }

        // 新增賬號
        function addAccount() {
            const email = document.getElementById("emailInput").value.trim();
            const password = document.getElementById("passwordInputAccount").value.trim();
            const phone = document.getElementById("phoneInput").value.trim();

            if (!email || !password || !phone) {
                alert("請填寫所有必填項目！");
                return;
            }

            const newAccount = {
                email,
                password,
                phone,
                verificationImage: null,
                editing: false,
                verificationVisible: false,
                creationTime: Date.now(),
                status: 'pending'
            };

            accountsData.push(newAccount);
            renderGallery();

            // 清空輸入
            document.getElementById("emailInput").value = "";
            document.getElementById("passwordInputAccount").value = "";
            document.getElementById("phoneInput").value = "";
        }

        // 若已超過 24 小時，將賬號自動轉換為「已完成」
        function checkAndUpdateStatuses() {
            const now = Date.now();
            const ONE_DAY = 24 * 60 * 60 * 1000; // 24小時(毫秒)

            accountsData.forEach(account => {
                if (account.status === 'pending') {
                    const diff = now - account.creationTime;
                    if (diff >= ONE_DAY) {
                        account.status = 'completed';
                    }
                }
            });
        }

        // 上傳圖片 (驗證碼)
        function handleFileUpload(index, event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                accountsData[index].verificationImage = e.target.result;
                renderGallery();
            };
            reader.readAsDataURL(file);
        }

        // 刪除圖片
        function deleteImage(index) {
            accountsData[index].verificationImage = null;
            accountsData[index].verificationVisible = false;
            renderGallery();
        }

        // 顯示 / 收起圖片
        function toggleVerificationCode(index) {
            if (!accountsData[index].verificationImage) return;
            accountsData[index].verificationVisible = !accountsData[index].verificationVisible;
            renderGallery();
        }

        // 編輯
        function editAccount(index) {
            accountsData[index].editing = true;
            renderGallery();
        }

        // 儲存編輯
        function saveAccount(index) {
            const emailInput = document.getElementById(`editEmail-${index}`);
            const passwordInput = document.getElementById(`editPassword-${index}`);
            const phoneInput = document.getElementById(`editPhone-${index}`);

            if (!emailInput.value.trim() || !passwordInput.value.trim() || !phoneInput.value.trim()) {
                alert("請填寫所有必填項目！");
                return;
            }

            accountsData[index].email = emailInput.value.trim();
            accountsData[index].password = passwordInput.value.trim();
            accountsData[index].phone = phoneInput.value.trim();
            accountsData[index].editing = false;

            renderGallery();
        }

        // 取消編輯
        function cancelEditAccount(index) {
            accountsData[index].editing = false;
            renderGallery();
        }

        // 刪除賬號
        function deleteAccount(index) {
            accountsData.splice(index, 1);
            renderGallery();
        }

        // 切換「等待中賬號」收合
        function togglePending() {
            isPendingExpanded = !isPendingExpanded;
            renderGallery();
        }

        // 切換「已完成賬號」收合
        function toggleCompleted() {
            isCompletedExpanded = !isCompletedExpanded;
            renderGallery();
        }

        // 渲染賬號列表
        function renderGallery() {
            checkAndUpdateStatuses();

            // 分類
            const pendingAccounts = accountsData.filter(acc => acc.status === 'pending');
            const completedAccounts = accountsData.filter(acc => acc.status === 'completed');

            // 更新標題
            pendingTitle.firstChild.textContent = `等待中賬號 (${pendingAccounts.length})`;
            completedTitle.firstChild.textContent = `已完成賬號 (${completedAccounts.length})`;

            // 更新收起/展開按鈕
            pendingToggleBtn.textContent = isPendingExpanded ? "收起" : "展開";
            completedToggleBtn.textContent = isCompletedExpanded ? "收起" : "展開";

            // 控制清單顯示/隱藏
            pendingList.style.display = isPendingExpanded ? "block" : "none";
            completedList.style.display = isCompletedExpanded ? "block" : "none";

            // 清空
            pendingList.innerHTML = "";
            completedList.innerHTML = "";

            // 生成「等待中」列表
            pendingAccounts.forEach(account => {
                const globalIndex = accountsData.indexOf(account);

                const accountRow = document.createElement("div");
                accountRow.className = "account-row";

                // 基礎資訊
                const basicInfo = document.createElement("div");
                basicInfo.className = "basic-info";

                if (!account.editing) {
                    // 顯示文字
                    const emailSpan = document.createElement("span");
                    emailSpan.innerHTML = `<span class="label">電郵:</span> ${account.email}`;
                    basicInfo.appendChild(emailSpan);

                    const passwordSpan = document.createElement("span");
                    passwordSpan.innerHTML = `<span class="label">密碼:</span> ${account.password}`;
                    basicInfo.appendChild(passwordSpan);

                    const phoneSpan = document.createElement("span");
                    phoneSpan.innerHTML = `<span class="label">電話:</span> ${account.phone}`;
                    basicInfo.appendChild(phoneSpan);

                    // 建立時間
                    const creationSpan = document.createElement("span");
                    creationSpan.innerHTML = `<span class="label">建立時間:</span> ${new Date(account.creationTime).toLocaleString()}`;
                    basicInfo.appendChild(creationSpan);
                } else {
                    // 編輯模式
                    const emailInput = document.createElement("input");
                    emailInput.type = "email";
                    emailInput.value = account.email;
                    emailInput.id = `editEmail-${globalIndex}`;
                    basicInfo.appendChild(emailInput);

                    const passwordInput = document.createElement("input");
                    passwordInput.type = "text";
                    passwordInput.value = account.password;
                    passwordInput.id = `editPassword-${globalIndex}`;
                    basicInfo.appendChild(passwordInput);

                    const phoneInput = document.createElement("input");
                    phoneInput.type = "tel";
                    phoneInput.value = account.phone;
                    phoneInput.id = `editPhone-${globalIndex}`;
                    basicInfo.appendChild(phoneInput);

                    // 建立時間(只讀)
                    const creationSpan = document.createElement("span");
                    creationSpan.innerHTML = `<span class="label">建立時間:</span> ${new Date(account.creationTime).toLocaleString()}`;
                    basicInfo.appendChild(creationSpan);
                }

                accountRow.appendChild(basicInfo);

                // 按鈕們
                const actions = document.createElement("div");
                actions.className = "account-actions";

                // 上傳
                const uploadInput = document.createElement("input");
                uploadInput.type = "file";
                uploadInput.accept = "image/*;capture=camera";
                uploadInput.className = "btn-sm";
                if (account.verificationImage) uploadInput.disabled = true;
                uploadInput.onchange = (e) => handleFileUpload(globalIndex, e);
                actions.appendChild(uploadInput);

                // 驗證碼
                const verifyBtn = document.createElement("button");
                verifyBtn.textContent = "驗證碼";
                verifyBtn.className = "btn-sm";
                verifyBtn.onclick = () => toggleVerificationCode(globalIndex);
                actions.appendChild(verifyBtn);

                // 若有圖片
                if (account.verificationImage) {
                    const verifyImg = document.createElement("img");
                    verifyImg.src = account.verificationImage;
                    verifyImg.className = "verification-image " + (account.verificationVisible ? "large" : "small");
                    actions.appendChild(verifyImg);

                    // 刪除圖片
                    const deleteImgBtn = document.createElement("button");
                    deleteImgBtn.textContent = "刪除圖片";
                    deleteImgBtn.className = "btn-sm";
                    deleteImgBtn.onclick = () => deleteImage(globalIndex);
                    actions.appendChild(deleteImgBtn);
                }

                // 編輯 / 儲存 / 取消
                if (!account.editing) {
                    const editBtn = document.createElement("button");
                    editBtn.textContent = "編輯";
                    editBtn.className = "btn-sm";
                    editBtn.onclick = () => editAccount(globalIndex);
                    actions.appendChild(editBtn);
                } else {
                    const saveBtn = document.createElement("button");
                    saveBtn.textContent = "儲存";
                    saveBtn.className = "btn-sm";
                    saveBtn.onclick = () => saveAccount(globalIndex);
                    actions.appendChild(saveBtn);

                    const cancelBtn = document.createElement("button");
                    cancelBtn.textContent = "取消";
                    cancelBtn.style.backgroundColor = "#f44336";
                    cancelBtn.className = "btn-sm";
                    cancelBtn.onclick = () => cancelEditAccount(globalIndex);
                    actions.appendChild(cancelBtn);
                }

                // 刪除賬號
                const deleteBtn = document.createElement("button");
                deleteBtn.textContent = "刪除";
                deleteBtn.className = "delete-button btn-sm";
                deleteBtn.onclick = () => deleteAccount(globalIndex);
                actions.appendChild(deleteBtn);

                accountRow.appendChild(actions);
                pendingList.appendChild(accountRow);
            });

            // 生成「已完成」列表
            completedAccounts.forEach(account => {
                const globalIndex = accountsData.indexOf(account);

                const accountRow = document.createElement("div");
                accountRow.className = "account-row";

                // 基礎資訊
                const basicInfo = document.createElement("div");
                basicInfo.className = "basic-info";

                if (!account.editing) {
                    // 顯示文字
                    const emailSpan = document.createElement("span");
                    emailSpan.innerHTML = `<span class="label">電郵:</span> ${account.email}`;
                    basicInfo.appendChild(emailSpan);

                    const passwordSpan = document.createElement("span");
                    passwordSpan.innerHTML = `<span class="label">密碼:</span> ${account.password}`;
                    basicInfo.appendChild(passwordSpan);

                    const phoneSpan = document.createElement("span");
                    phoneSpan.innerHTML = `<span class="label">電話:</span> ${account.phone}`;
                    basicInfo.appendChild(phoneSpan);

                    // 建立時間
                    const creationSpan = document.createElement("span");
                    creationSpan.innerHTML = `<span class="label">建立時間:</span> ${new Date(account.creationTime).toLocaleString()}`;
                    basicInfo.appendChild(creationSpan);
                } else {
                    // 編輯模式
                    const emailInput = document.createElement("input");
                    emailInput.type = "email";
                    emailInput.value = account.email;
                    emailInput.id = `editEmail-${globalIndex}`;
                    basicInfo.appendChild(emailInput);

                    const passwordInput = document.createElement("input");
                    passwordInput.type = "text";
                    passwordInput.value = account.password;
                    passwordInput.id = `editPassword-${globalIndex}`;
                    basicInfo.appendChild(passwordInput);

                    const phoneInput = document.createElement("input");
                    phoneInput.type = "tel";
                    phoneInput.value = account.phone;
                    phoneInput.id = `editPhone-${globalIndex}`;
                    basicInfo.appendChild(phoneInput);

                    // 建立時間(只讀)
                    const creationSpan = document.createElement("span");
                    creationSpan.innerHTML = `<span class="label">建立時間:</span> ${new Date(account.creationTime).toLocaleString()}`;
                    basicInfo.appendChild(creationSpan);
                }

                accountRow.appendChild(basicInfo);

                // 按鈕們
                const actions = document.createElement("div");
                actions.className = "account-actions";

                // 上傳
                const uploadInput = document.createElement("input");
                uploadInput.type = "file";
                uploadInput.accept = "image/*;capture=camera";
                uploadInput.className = "btn-sm";
                if (account.verificationImage) uploadInput.disabled = true;
                uploadInput.onchange = (e) => handleFileUpload(globalIndex, e);
                actions.appendChild(uploadInput);

                // 驗證碼
                const verifyBtn = document.createElement("button");
                verifyBtn.textContent = "驗證碼";
                verifyBtn.className = "btn-sm";
                verifyBtn.onclick = () => toggleVerificationCode(globalIndex);
                actions.appendChild(verifyBtn);

                // 若有圖片
                if (account.verificationImage) {
                    const verifyImg = document.createElement("img");
                    verifyImg.src = account.verificationImage;
                    verifyImg.className = "verification-image " + (account.verificationVisible ? "large" : "small");
                    actions.appendChild(verifyImg);

                    // 刪除圖片
                    const deleteImgBtn = document.createElement("button");
                    deleteImgBtn.textContent = "刪除圖片";
                    deleteImgBtn.className = "btn-sm";
                    deleteImgBtn.onclick = () => deleteImage(globalIndex);
                    actions.appendChild(deleteImgBtn);
                }

                // 編輯 / 儲存 / 取消
                if (!account.editing) {
                    const editBtn = document.createElement("button");
                    editBtn.textContent = "編輯";
                    editBtn.className = "btn-sm";
                    editBtn.onclick = () => editAccount(globalIndex);
                    actions.appendChild(editBtn);
                } else {
                    const saveBtn = document.createElement("button");
                    saveBtn.textContent = "儲存";
                    saveBtn.className = "btn-sm";
                    saveBtn.onclick = () => saveAccount(globalIndex);
                    actions.appendChild(saveBtn);

                    const cancelBtn = document.createElement("button");
                    cancelBtn.textContent = "取消";
                    cancelBtn.style.backgroundColor = "#f44336";
                    cancelBtn.className = "btn-sm";
                    cancelBtn.onclick = () => cancelEditAccount(globalIndex);
                    actions.appendChild(cancelBtn);
                }

                // 刪除賬號
                const deleteBtn = document.createElement("button");
                deleteBtn.textContent = "刪除";
                deleteBtn.className = "delete-button btn-sm";
                deleteBtn.onclick = () => deleteAccount(globalIndex);
                actions.appendChild(deleteBtn);

                accountRow.appendChild(actions);
                completedList.appendChild(accountRow);
            });
        }
    </script>
</body>
</html>
