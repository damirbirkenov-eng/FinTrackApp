# FinTrackApp
Регулируем финансы.
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>FinTrack - Финансовый трекер</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
/* --- Базовые стили --- */
* { box-sizing: border-box; margin:0; padding:0; font-family: 'Roboto', sans-serif; }
body { background: #f5f7fa; color:#333; }
header { background:#4a90e2; color:white; padding:20px; text-align:center; font-size:26px; font-weight:700; }
nav { display:flex; justify-content:center; background:#356ac3; }
nav button { flex:1; padding:12px; border:none; cursor:pointer; background:#356ac3; color:white; font-weight:700; transition:0.3s; }
nav button:hover { background:#2a4f8c; }
nav button.active { background:#4a90e2; }
.container { max-width:900px; margin:20px auto; padding:20px; background:white; border-radius:15px; box-shadow:0 5px 15px rgba(0,0,0,0.1); }
h2 { text-align:center; margin-bottom:20px; color:#4a90e2; }
input, select { width:100%; padding:10px; margin:10px 0; border-radius:8px; border:1px solid #ccc; }
button.action { padding:12px 20px; border:none; border-radius:8px; background:#4a90e2; color:white; font-weight:700; cursor:pointer; transition:0.3s; }
button.action:hover{ background:#356ac3; }
ul { list-style:none; padding:0; margin:0; }
li { padding:12px; margin:6px 0; display:flex; justify-content:space-between; align-items:center; background:#f1f5f9; border-radius:8px; }
li.income { border-left:5px solid #2ecc71; }
li.expense { border-left:5px solid #e74c3c; }
li button { background:none; border:none; cursor:pointer; font-size:18px; color:#888; transition:0.3s; }
li button:hover { color:#333; }
.summary { display:flex; justify-content:space-around; margin:20px 0; text-align:center; }
.summary div { background:#f1f5f9; padding:20px; border-radius:12px; width:30%; font-weight:700; font-size:18px; }
.summary .income { color:#2ecc71; }
.summary .expense { color:#e74c3c; }
.summary .balance { color:#4a90e2; }
/* Мобильный адаптив */
@media(max-width:600px){
  .summary { flex-direction:column; }
  .summary div { margin-bottom:10px; width:100%; }
}
.hidden { display:none; }
</style>
</head>
<body>

<header>FinTrack - Финансовый трекер</header>
<nav>
  <button id="transactionsTab" class="active">Транзакции</button>
  <button id="addTab">Добавить</button>
</nav>

<div class="container">
  <!-- Транзакции -->
  <div id="transactionsSection">
    <h2>Мои финансы</h2>
    <div class="summary">
      <div class="income">Доход: <span id="totalIncome">0</span> ₸</div>
      <div class="expense">Расход: <span id="totalExpense">0</span> ₸</div>
      <div class="balance">Баланс: <span id="balance">0</span> ₸</div>
    </div>
    <ul id="transactionsList"></ul>
  </div>

  <!-- Добавить транзакцию -->
  <div id="addSection" class="hidden">
    <h2>Добавить транзакцию</h2>
    <input type="text" id="descInput" placeholder="Описание">
    <input type="number" id="amountInput" placeholder="Сумма (₸)">
    <select id="typeSelect">
      <option value="income">Доход</option>
      <option value="expense">Расход</option>
    </select>
    <button class="action" onclick="addTransaction()">Добавить</button>
  </div>
</div>

<script>
let transactions = [];

// --- Навигация ---
const tabs = ["transactionsTab","addTab"];
const sections = ["transactionsSection","addSection"];
tabs.forEach((tab,i)=>{
  document.getElementById(tab).addEventListener('click', ()=>{
    tabs.forEach(t=>document.getElementById(t).classList.remove('active'));
    document.getElementById(tab).classList.add('active');
    sections.forEach(s=>document.getElementById(s).classList.add('hidden'));
    document.getElementById(sections[i]).classList.remove('hidden');
  });
});

// --- Функции ---
function updateSummary(){
  const income = transactions.filter(t=>t.type==="income").reduce((sum,t)=>sum+t.amount,0);
  const expense = transactions.filter(t=>t.type==="expense").reduce((sum,t)=>sum+t.amount,0);
  document.getElementById("totalIncome").textContent = income;
  document.getElementById("totalExpense").textContent = expense;
  document.getElementById("balance").textContent = income - expense;
}

function renderTransactions(){
  const list = document.getElementById("transactionsList");
  list.innerHTML = "";
  transactions.forEach((t,i)=>{
    const li = document.createElement('li');
    li.className = t.type;
    li.innerHTML = `<span>${t.desc} - ${t.amount} ₸</span> <button onclick="deleteTransaction(${i})">❌</button>`;
    list.appendChild(li);
  });
}

function addTransaction(){
  const desc = document.getElementById("descInput").value.trim();
  const amount = parseFloat(document.getElementById("amountInput").value);
  const type = document.getElementById("typeSelect").value;
  if(!desc || !amount || amount<=0) return alert("Введите корректные данные!");
  transactions.push({desc, amount, type});
  document.getElementById("descInput").value="";
  document.getElementById("amountInput").value="";
  renderTransactions();
  updateSummary();
}

function deleteTransaction(index){
  transactions.splice(index,1);
  renderTransactions();
  updateSummary();
}

// --- Инициализация ---
updateSummary();
</script>

</body>
</html>
