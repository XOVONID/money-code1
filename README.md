# money-code
money code
<!DOCTYPE html>
<html>
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
