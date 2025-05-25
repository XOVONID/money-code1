# money-code
money code
<!DOCTYPE html>
<html><!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>條碼存錢筒</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 20px; background: #f4f4f4; }
    input { padding: 10px; font-size: 18px; margin: 10px; }
    button { padding: 10px 20px; font-size: 18px; }
    #amount { font-size: 32px; margin: 20px; color: green; }
    svg { margin-top: 20px; }
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

    function updateBalance() {
      document.getElementById("balance").textContent = balance;
      JsBarcode("#barcode", String(balance).padStart(13, '0'), {
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
        updateBalance();
      }
    }

    function subtractMoney() {
      const val = parseInt(document.getElementById("inputMoney").value);
      if (!isNaN(val)) {
        balance -= val;
        updateBalance();
      }
    }

    updateBalance();
  </script>
</body>
</html>
<head>
  <meta charset="UTF-8">
  <title>存錢筒 EAN13 條碼工具</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body { font-family: sans-serif; padding: 20px; }
    input { margin: 5px; padding: 5px; }
    #barcode { margin-top: 20px; }
  </style>
</head>
<body>
  <h2>存錢筒 EAN13 條碼工具</h2>

  <label>掃描/輸入條碼（13碼）：
    <input type="text" id="barcodeInput" maxlength="13" />
  </label><br>

  <label>要存的金額（元）：
    <input type="number" id="depositInput" />
  </label><br>

  <button onclick="updateBalance()">更新餘額並產生新條碼</button>

  <h3>新條碼：</h3>
  <svg id="barcode"></svg>

  <script>
    function updateBalance() {
      const code = document.getElementById("barcodeInput").value;
      const deposit = parseInt(document.getElementById("depositInput").value);

      if (code.length !== 13 || isNaN(deposit)) {
        alert("請輸入正確的條碼與金額");
        return;
      }

      const accountId = code.substring(0, 2);
      const currentBalance = parseInt(code.substring(2, 11));
      const newBalance = currentBalance + deposit;

      const newData = accountId + String(newBalance).padStart(9, "0");

      JsBarcode("#barcode", newData, {
        format: "ean13",
        lineColor: "#000",
        width: 2,
        height: 100,
        displayValue: true
      });
    }
  </script>
</body>
</html>
