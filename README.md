<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>EAN-13金額記錄系統</title>
  
  <style>
    body {
      font-family: sans-serif;
      background: #708069;
      text-align: center;
      padding: 20px;
    }
    h1 {
      color: #000000;
    }
    input, button {
      padding: 10px;
      font-size: 15px;
      margin: 2px;
      border-radius: 10px;
      border: 1px solid #000000;
    }
    button {
      background-color: #808A87;
      color: #000000;
      cursor: pointer;
    }
    button:hover {
      background-color: #F5F5F5;
    }
    #amount {
      font-size: 20px;
      margin: 10px;
      color: #A39480;
    }
    svg {
      margin-top: 40px;
    }
    #app, #barcode {
      display: none;
    }
  </style>
</head>
<body>

  <h1>EAN-13金錢記錄系統</h1>

  <div id="login">
    <p><strong>請輸入資訊（本系統全免費，每個用戶初次使用都會自動註冊）</strong></p>
    <input id="username" placeholder="請輸入帳號" />
    <input id="password" type="password" placeholder="請輸入密碼" />
    <br>
    <label><input type="checkbox" id="remember"> 記住帳號密碼（可以方便登出登入本系統）</label>
    <br>
    <button onclick="login()">登入</button>
  </div>

  <div id="app">
    <div id="amount">目前餘額：$<span id="balance">0</span></div>
    <input id="inputMoney" type="number" placeholder="輸入金額" />
    <br>
    <button onclick="addMoney()">存錢</button>
    <button onclick="subtractMoney()">花錢</button>
    <br><br>
    <svg id="barcode"></svg>
    <br><br>
    <button onclick="logout()">登出</button>
  </div>

  <script>
    let currentUser = null;
    let balance = 0;

    // 自動填入帳密
    window.onload = () => {
      const savedUser = localStorage.getItem("saved_username");
      const savedPass = localStorage.getItem("saved_password");
      if (savedUser) document.getElementById("username").value = savedUser;
      if (savedPass) document.getElementById("password").value = savedPass;
      if (savedUser && savedPass) {
        document.getElementById("remember").checked = true;
      }
    }

    function login() {
      const username = document.getElementById("username").value.trim();
      const password = document.getElementById("password").value;
      const remember = document.getElementById("remember").checked;

      if (!username || !password) {
        alert("請輸入帳號與密碼");
        return;
      }

      const pwKey = "user_" + username + "_password";
      const savedPw = localStorage.getItem(pwKey);

      if (savedPw && savedPw !== password) {
        alert("密碼錯誤！");
        return;
      }

      if (!savedPw) {
        localStorage.setItem(pwKey, password); // 註冊帳號
      }

      if (remember) {
        localStorage.setItem("saved_username", username);
        localStorage.setItem("saved_password", password);
      } else {
        localStorage.removeItem("saved_username");
        localStorage.removeItem("saved_password");
      }

      currentUser = username;
      document.getElementById("login").style.display = "none";
      document.getElementById("app").style.display = "block";
      document.getElementById("barcode").style.display = "block";
      loadBalance();
    }

    function logout() {
      currentUser = null;
      balance = 0;
      document.getElementById("app").style.display = "none";
      document.getElementById("barcode").style.display = "none";
      document.getElementById("login").style.display = "block";
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
      const numberStr = String(balance).padStart(12, '0');
      JsBarcode("#barcode", numberStr, {
        format: "EAN13",
        lineColor: "#000",
        width: 2,
        height: 100,
        displayValue: true
      });
    }

    function addMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance += val;
        updateDisplay();
        saveBalance();
        speak(`你已經存入 ${val} 元，目前餘額 ${balance} 元`);
      }
    }

    function subtractMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance = Math.max(0, balance - val);
        updateDisplay();
        saveBalance();
        speak(`你已經花費 ${val} 元，目前餘額 ${balance} 元`);
      }
    }

    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = "zh-TW"; // 中文語音
      speechSynthesis.speak(utterance);
    }
  </script>

</body>
</html>  