<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Household Expense Tracker</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('background.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
    }

    body::before {
      content: "";
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      z-index: -1;
    }

    header {
      display: flex;
      align-items: center;
      background: rgba(0, 0, 0, 0.7);
      padding: 10px 20px;
    }

    header img {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      margin-right: 15px;
      object-fit: cover;
    }

    h1 {
      margin: 0;
      font-size: 24px;
    }

    main {
      padding: 20px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      background: rgba(255, 255, 255, 0.9);
      color: #000;
    }

    table, th, td {
      border: 1px solid #333;
    }

    th, td {
      padding: 10px;
      text-align: center;
    }

    .balance {
      font-weight: bold;
      color: green;
    }

    .negative {
      color: red;
    }

    .btn {
      background: #4CAF50;
      color: white;
      padding: 10px 20px;
      border: none;
      margin-top: 15px;
      cursor: pointer;
      border-radius: 5px;
    }
    .btn:hover {
      background: #45a049;
    }
  </style>
</head>
<body>
  <header>
    <img src="logo.jpg" alt="Logo">
    <h1>Household Expense Tracker</h1>
  </header>

  <main>
    <label>Salary: <input type="number" id="salary" placeholder="Enter Salary"></label>
    <button class="btn" onclick="addDay()">Add Day</button>

    <table id="expenseTable">
      <tr>
        <th>Date</th>
        <th>Expense</th>
        <th>Remaining Balance</th>
      </tr>
    </table>

    <h2>Monthly Summary</h2>
    <table id="monthlySummary">
      <tr>
        <th>Month</th>
        <th>Total Expense</th>
      </tr>
    </table>

    <h2>Yearly Summary</h2>
    <table id="yearlySummary">
      <tr>
        <th>Year</th>
        <th>Total Expense</th>
      </tr>
    </table>
  </main>

  <script>
    let balance = 0;
    let monthlyData = {};
    let yearlyData = {};

    function addDay() {
      let salaryInput = document.getElementById('salary');
      if (balance === 0) {
        balance = parseFloat(salaryInput.value) || 0;
      }
      let expense = prompt("Enter today's expense:");
      expense = parseFloat(expense) || 0;
      balance -= expense;

      let today = new Date();
      let dateStr = today.toLocaleDateString();
      let monthKey = today.toLocaleString('default', { month: 'long', year: 'numeric' });
      let yearKey = today.getFullYear();

      // Add to daily table
      let table = document.getElementById('expenseTable');
      let row = table.insertRow(-1);
      let dateCell = row.insertCell(0);
      let expenseCell = row.insertCell(1);
      let balanceCell = row.insertCell(2);

      dateCell.innerHTML = dateStr;
      expenseCell.innerHTML = expense.toFixed(2);
      balanceCell.innerHTML = balance.toFixed(2);
      balanceCell.className = balance < 0 ? 'negative' : 'balance';

      // Update monthly data
      monthlyData[monthKey] = (monthlyData[monthKey] || 0) + expense;
      updateSummary("monthlySummary", monthlyData);

      // Update yearly data
      yearlyData[yearKey] = (yearlyData[yearKey] || 0) + expense;
      updateSummary("yearlySummary", yearlyData);
    }

    function updateSummary(tableId, data) {
      let table = document.getElementById(tableId);
      table.innerHTML = `<tr><th>${tableId === "monthlySummary" ? "Month" : "Year"}</th><th>Total Expense</th></tr>`;
      for (let key in data) {
        let row = table.insertRow(-1);
        row.insertCell(0).innerHTML = key;
        row.insertCell(1).innerHTML = data[key].toFixed(2);
      }
    }
  </script>
</body>
</html>
