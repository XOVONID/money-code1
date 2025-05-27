<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>條碼存錢筒</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body {
      font-family: sans-serif;
      background: #f9f9f9;
      text-align: center;
      padding: 40px;
    }
    h1 {
      font-size: 4em;
      color: #708069;
    }
    input, button {
      font-size: 20px;
      padding: 15px;
      margin: 10px;
      border: 8px solid #000000;
      border-radius: 8px;
    }
    button {
      cursor: pointer;
      background-color: #808A87;
    
    
    button:hover {
      background-color: #708069;
    }
    #amount {
      font-size: 28px;
      margin: 20px;
      color: green;
    }
    svg {
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>條碼存錢筒</h1>

  <div id="amount">目前餘額：$<span id="balance">0</span></div>

  <input id="inputMoney" type="number" placeholder="輸入金額" />
  <br />
  <button onclick="addMoney()">存錢</button>
  <button onclick="subtractMoney()">花錢</button>
  <br />

  <svg id="barcode"></svg>

  <script>
    let balance = 0;

    function updateBarcode() {
      const numberStr = String(balance).padStart(12, '0');
      JsBarcode("#barcode", numberStr, {
        format: "EAN13",
        displayValue: true,
        lineColor: "#000",
        width: 2,
        height: 100
      });
      document.getElementById("balance").textContent = balance;
    }

    function addMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance += val;
        updateBarcode();
      }
    }

    function subtractMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance = Math.max(0, balance - val);
        updateBarcode();
      }
    }

    updateBarcode(); // 初始化
  </script>
</body>
</html>