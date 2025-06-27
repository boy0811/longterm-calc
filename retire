<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>退休金試算工具</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", sans-serif;
      background: #f5f7fa;
      color: #333;
      max-width: 720px;
      margin: 40px auto;
      padding: 20px;
      background-color: #fff;
      border-radius: 12px;
      box-shadow: 0 0 12px rgba(0,0,0,0.1);
    }
    h2 {
      text-align: center;
      margin-bottom: 24px;
    }
    label {
      display: block;
      margin-top: 16px;
      font-weight: bold;
    }
    input[type="text"], input[type="number"] {
      width: 100%;
      padding: 8px;
      margin-top: 6px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }
    button {
      margin-top: 24px;
      padding: 10px 20px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
    }
    #result {
      margin-top: 20px;
      font-size: 16px;
    }
    .contact {
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px solid #ccc;
      font-size: 14px;
      color: #555;
    }
    .contact img {
      margin-top: 8px;
    }
  </style>
</head>
<body>
  <h2>退休金試算工具</h2>
  <form id="retireForm">
    <label>姓名：<input type="text" id="name" required></label>
    <label>電話：<input type="text" id="phone" required></label>
    <label>你現在幾歲？<input type="number" id="age" required></label>
    <label>預估退休年齡：<input type="number" id="retireAge" required></label>
    <label>預估會活到幾歲？（我國平均餘命約為 85 歲）<input type="number" id="lifeExpect" required></label>
    <label>退休後每月預估支出（元）：<input type="number" id="monthlyExpense" required></label>
    <label>每月可存退休金（元）：<input type="number" id="monthlySaving" required></label>
    <label>預估年報酬率（%）：<input type="number" id="annualReturn" required></label>

    <button type="submit">試算退休金</button>
  </form>

  <div id="result"></div>

  <div class="contact">
    📞 南山人壽 張又仁<br>
    📱 電話：0927-779-159<br>
    💬 LINE ID：@731bpjaj<br>
    👉 <a href="https://lin.ee/qQa1RAV" target="_blank">點我加入 LINE 好友</a><br>
    <a href="https://lin.ee/qQa1RAV" target="_blank">
      <img src="https://scdn.line-apps.com/n/line_add_friends/btn/zh-Hant.png" alt="加入好友" height="36" border="0">
    </a>
  </div>

  <script>
    document.getElementById("retireForm").addEventListener("submit", function(e) {
      e.preventDefault();

      const name = document.getElementById("name").value;
      const phone = document.getElementById("phone").value;
      const age = +document.getElementById("age").value;
      const retireAge = +document.getElementById("retireAge").value;
      const lifeExpect = +document.getElementById("lifeExpect").value;
      const monthlySaving = +document.getElementById("monthlySaving").value;
      const annualReturn = +document.getElementById("annualReturn").value;
      const monthlyExpense = +document.getElementById("monthlyExpense").value;

      const yearsToSave = retireAge - age;
      const monthsToSave = yearsToSave * 12;
      const r = annualReturn / 100 / 12;

      let futureAmount = 0;
      if (r > 0) {
        futureAmount = monthlySaving * ((Math.pow(1 + r, monthsToSave) - 1) / r) * (1 + r);
      } else {
        futureAmount = monthlySaving * monthsToSave;
      }

      const retirementYears = lifeExpect - retireAge;
      const totalNeeded = retirementYears * 12 * monthlyExpense;

      let grade = "C";
      const ratio = futureAmount / totalNeeded;
      if (ratio >= 1.2) grade = "A";
      else if (ratio >= 0.8) grade = "B";

      const resultDiv = document.getElementById("result");
      resultDiv.innerHTML = `
        📊 預估需準備金額：約 ${Math.round(totalNeeded).toLocaleString()} 元<br>
        💰 預估可準備金額：約 ${Math.round(futureAmount).toLocaleString()} 元<br>
        💡 評級：${grade} 級（${grade === "A" ? "非常充足" : grade === "B" ? "尚可，建議強化" : "不足，需加強準備"}）<br>
        📤 資料送出中...
      `;

      fetch("https://script.google.com/macros/s/AKfycbxve6DfYiwtKo9fCtPPKOFml5JCtfKBm67kH8ljPFS4thW3XCRE8bZP2U_wBXIjJN42/exec", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          type: "retire",
          name,
          phone,
          age,
          retireAge,
          lifeExpect,
          monthlySaving,
          annualReturn,
          monthlyExpense,
          futureNeeded: Math.round(totalNeeded),
          futureAmount: Math.round(futureAmount),
          grade
        })
      }).then(res => res.text())
        .then(txt => {
          resultDiv.innerHTML += `<br>${txt === "success" ? "✅ 已成功儲存" : "❌ 儲存失敗：" + txt}`;
        });
    });
  </script>
</body>
</html>
