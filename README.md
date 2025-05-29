<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <title>EAN-13條碼存錢系統</title>
  <script src="https://XOVONID Github io/money code-1></script>
  <style>
    body {
      font-family: 日系中文;
      background: #808069;
      text-align: center;
      padding: 30px;
    }
    h1 {
      font-size: 2em;
      color: #000000;
    }
    input, button {
      font-size: 18px;
      padding: 10px;
      margin: 2px;
      border: 1px solid #000000;
      border-radius: 10px;
    }
    button {
      cursor: pointer;
      background-color: #FAF0E6;
      color: white;
    }
    button:hover {
      background-color: #0056b3;
    }
    #amount {
      font-size: 28px;
      margin: 20px;
      color: 708069;
    }
    svg {
      margin-top: 20px;
    }
    #app, #barcode {
      display: none;
    }
  </style>
</head>
<body>
  <h1></h1>

  <div id="login">
    <p><strong>請輸入帳號與密碼</strong>EAN-13條碼存錢系統</p>
    <input id="username" placeholder="帳號" />
    <input id="password" type="password" placeholder="密碼" />
    <br />
    <label><input type="checkbox" id="remember" /> 記住帳號</label>
    <br />
    <button onclick="login()">登入</button>
  </div>

  <div id="app">
    <div id="amount">目前餘額：$<span id="balance">0</span></div>
    <input id="inputMoney" type="number" placeholder="輸入金額" />
    <br />
    <button onclick="addMoney()">存錢</button>
    <button onclick="subtractMoney()">花錢</button>
    <br />
    <svg id="barcode"></svg>
    <br />
    <button onclick="logout()">登出</button>
  </div>

  <script>
    let currentUser = null;
    let balance = 0;

    function login() {
      const username = document.getElementById("username").value.trim();
      const password = document.getElementById("password").value;
      const remember = document.getElementById("remember").checked;

      if (!username || !password) {
        alert("請輸入帳號與密碼");
        return;
      }

      const pwKey = "user_" + username + "_password";
      const storedPassword = localStorage.getItem(pwKey);

      if (storedPassword && storedPassword !== password) {
        alert("密碼錯誤！");
        return;
      }

      if (!storedPassword) {
        localStorage.setItem(pwKey, password); // 自動註冊帳號
      }

      if (remember) {
        localStorage.setItem("saved_username", username);
      } else {
        localStorage.removeItem("saved_username");
      }

      currentUser = username;
      document.getElementById("login").style.display = "none";
      document.getElementById("app").style.display = "block";
      document.getElementById("barcode").style.display = "block";
      loadBalance();
    }

    function loadBalance() {
      const key = "user_" + currentUser + "_balance";
      const saved = localStorage.getItem(key);
      balance = saved ? parseInt(saved) : 0;
      updateDisplay();
    }

    function saveBalance() {
      const key = "user_" + currentUser + "_balance";
      localStorage.setItem(key, balance);
    }

    function updateDisplay() {
      document.getElementById("balance").textContent = balance;
      const numberStr = String(balance).padStart(12, '0'); // EAN13 需要 12 碼，會自動補最後一碼檢查碼
      JsBarcode("#barcode", numberStr, {
        format: "EAN13",
        displayValue: true,
        lineColor: "#000",
        width: 2,
        height: 100
      });
    }

    function addMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance += val;
        updateDisplay();
        saveBalance();
      }
    }

    function subtractMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance = Math.max(0, balance - val);
        updateDisplay();
        saveBalance();
      }
    }

    function logout() {
      currentUser = null;
      balance = 0;
      document.getElementById("app").style.display = "none";
      document.getElementById("barcode").style.display = "none";
      document.getElementById("login").style.display = "block";
    }

    // 自動填入記住的帳號
    const remembered = localStorage.getItem("saved_username");
    if (remembered) {
      document.getElementById("username").value = remembered;
      document.getElementById("remember").checked = true;
    }
  </script>
</body>
</html>
    
      