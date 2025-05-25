# money-code
money code
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>條碼存錢筒</title>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      padding: 20px;
      background: #f0f0f0;
    }
    input, button {
      font-size: 18px;
      padding: 10px;
      margin: 10px;
    }
    svg {
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>條碼存錢筒</h1>
  <p>請輸入 <strong>12 位數字</strong>，按下按鈕產生條碼。</p>

  <input id="code" value="000000000000" maxlength="12"/>
  <br />
  <button onclick="generate()">更新條碼</button>

  <br />
  <svg id="barcode"></svg>

  <script>
    function generate() {
      const code = document.getElementById("code").value;
      if (code.length === 12 && /^\d+$/.test(code)) {
        JsBarcode("#barcode", code, {
          format: "EAN13",
          displayValue: true,
          lineColor: "#000",
          width: 2,
          height: 100,
        });
      } else {
        alert("請輸入正確的 12 位數字（例如：000000001000）");
      }
    }

    generate(); // 預設先產生一次
  </script>
</body>
</html>

  
