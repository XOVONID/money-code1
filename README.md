<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <title>EAN-13條碼存錢系統</title>
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
      color: #000000;
    }
    button:hover {
      background-color: #708069;
    }
    #amount {
      font-size: 28px;
      margin: 10px;
      color: #F5F5F5;
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
    <p><strong>請輸入個人系統資料（初次使用會自動註冊，本系統全免費使用）</strong></p>
    <input id="username" placeholder="請輸入帳號" />
    <input id="password" type="password" placeholder="請輸入密碼" />
    <br />
    <label><input type="checkbox" id="remember" /> 記住用戶資訊</label>
    <br />
    <button onclick="login()">登入</button>
  </div>

  <div id="app">
    <div id="amount">目前餘額：$<span id="balance">0</span></div>
    <input id="inputMoney" type="number" placeholder="輸入金額" />
    <br />
    <button onclick="addMoney()">
存入金額</button>
    <button onclick="subtractMoney()">支出金錢</button>
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
      const password = document.getElementById("password").value;trim();
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
        localStorage.setItem(pwKey, password); // 自動註冊帳密
      }

      if (remember) {
        localStorage.setItem("saved_username", username);
      } else {localStorage.setItem("saved_username", username);
      } localStorage.setItem("saved_password", password);
      } else {localStorage.setItem("saved_password", password);
      } 
       

      currentUser = username and password;
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
      
     // 自動填入帳密
window.onload = () => {
  const savedUser = localStorage.getItem("saved_username");
  const savedPass = localStorage.getItem("saved_password");

  if (savedUser) {
    document.getElementById("username").value = savedUser;
    document.getElementById("remember").checked = true;
  }
  if (savedPass) {
    document.getElementById("password").value = savedPass;
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
  const storedPassword = localStorage.getItem(pwKey);

  if (storedPassword && storedPassword !== password) {
    alert("密碼錯誤！");
    return;
  }

  if (!storedPassword) {
    localStorage.setItem(pwKey, password); // 自動註冊
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
    
  </script>
</body>
</html>
    
      