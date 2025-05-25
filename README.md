
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>存錢筒</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 30px; background: #000000; }
    input { padding: 15px; font-size: 20px; margin: 20px; }
    button { padding: 20px 40px; font-size: 20px; }
    #amount { font-size: 40px; margin: 30px; color:#007799; }
    svg { margin-top: 30px; }
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
    let balance =000;

    function updateBalance() {
      document.getElementById("balance").textContent = balance;
      JsBarcode("#barcode", String(balance).padStart(13, '0'), {
        format: "code11",
        lineColor: "#000",
        width: 2,
        height: 1000,
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
