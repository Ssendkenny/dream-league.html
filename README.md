<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Kenhazel Advanced Calculator 2026</title>
<style>
body{font-family:sans-serif;background:#0f172a;color:white;display:flex;justify-content:center;padding:10px}
.calc{background:#1e293b;border-radius:20px;padding:15px;width:100%;max-width:360px;box-shadow:0 10px 30px #000}
#display{width:100%;height:70px;background:#0f172a;color:#38bdf8;font-size:32px;text-align:right;padding:10px;border-radius:12px;border:none;outline:none;margin-bottom:8px}
#history{height:20px;font-size:12px;color:#94a3b8;text-align:right;overflow:hidden}
.buttons{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
button{height:60px;border-radius:12px;border:none;font-size:20px;font-weight:bold}
.num{background:#334155;color:white}
.op{background:#f59e0b;color:white}
.func{background:#475569;color:#38bdf8;font-size:16px}
.eq{background:#38bdf8;color:#0f172a;grid-column:span 2}
.zero{grid-column:span 2}
</style>
</head>
<body>
<div class="calc">
<div id="history"></div>
<input id="display" readonly>
<div class="buttons">
<button class="func" onclick="clearAll()">AC</button>
<button class="func" onclick="del()">DEL</button>
<button class="func" onclick="add('%')">%</button>
<button class="op" onclick="add('/')">÷</button>

<button class="func" onclick="add('Math.sin(')">sin</button>
<button class="func" onclick="add('Math.cos(')">cos</button>
<button class="func" onclick="add('Math.log10(')">log</button>
<button class="op" onclick="add('*')">×</button>

<button class="func" onclick="add('Math.sqrt(')">√</button>
<button class="func" onclick="add('Math.pow(')">x^y</button>
<button class="func" onclick="add('Math.PI')">π</button>
<button class="op" onclick="add('-')">-</button>

<button class="num" onclick="add('7')">7</button>
<button class="num" onclick="add('8')">8</button>
<button class="num" onclick="add('9')">9</button>
<button class="op" onclick="add('+')">+</button>

<button class="num" onclick="add('4')">4</button>
<button class="num" onclick="add('5')">5</button>
<button class="num" onclick="add('6')">6</button>
<button class="func" onclick="add('(')">(</button>

<button class="num" onclick="add('1')">1</button>
<button class="num" onclick="add('2')">2</button>
<button class="num" onclick="add('3')">3</button>
<button class="func" onclick="add(')')">)</button>

<button class="num zero" onclick="add('0')">0</button>
<button class="num" onclick="add('.')">.</button>
<button class="eq" onclick="calculate()">=</button>
</div>
</div>

<script>
let d=document.getElementById('display');
let h=document.getElementById('history');
function add(v){d.value+=v}
function clearAll(){d.value='';h.innerText=''}
function del(){d.value=d.value.slice(0,-1)}
function calculate(){
 try{
  let exp=d.value.replace(/,/g,',');
  h.innerText=d.value+' =';
  let res=eval(exp);
  d.value=Number(res.toFixed(8));
  localStorage.setItem('last',d.value);
 }catch{ d.value='Error' }
}
</script>
</body>
</html>