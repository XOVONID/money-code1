
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-7">
  <title>存錢筒</title>
  <script src="https://https://xovonid.github.io/money-code1/></script>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 30px; background: #000000; }
    input { padding: 10px; font-size: 30px; margin: 30px; }
    button { padding: 30px 50px; font-size: 30px; }
    #amount { font-size: 50px; margin: 40px; color:#007799; }
    svg { margin-top: 40px; }
  </style>
</head>
<body>
  <h1>存錢筒</h1>
  <div id="amount">餘額：$<span id="balance">0</span></div>

  <input id="inputMoney" type="number" placeholder="輸入" />
  <br />
  <button onclick="addMoney()">存錢</button>
  <button onclick="subtractMoney()">花錢</button>
  <br />
  <svg id="barcode"></svg>

  <script>
    let balance =1;

    function updateBalance() {
      document.getElementById("balance").textContent = balance;
      JsBarcode("#barcode", String(balance).padStart(13, '0'), {
        format: "EAN-13",
        lineColor: "#000",
        width: 1,
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
