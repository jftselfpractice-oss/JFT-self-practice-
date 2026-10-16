
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>JFT SELF PRACTICE</title>
<style>
body{font-family:Arial,sans-serif;background:#f1f5f9;margin:0;color:#172033}
header{background:#123d2b;color:white;text-align:center;padding:22px 10px}
.container{max-width:700px;margin:20px auto;padding:15px}
.card{background:white;padding:20px;border-radius:12px;margin:15px 0;box-shadow:0 2px 8px #ddd}
button{background:#087443;color:white;border:0;border-radius:7px;padding:11px 16px;margin:5px;cursor:pointer}
input,textarea,select{box-sizing:border-box;width:100%;padding:10px;margin:7px 0;border:1px solid #bbb;border-radius:6px}
.option{display:block;background:#eef6f1;padding:12px;margin:8px 0;border-radius:7px}
.hidden{display:none}
</style>
</head>
<body>
<header>
<h1>🇯🇵 JFT SELF PRACTICE</h1>
<p>JFT-BASIC A2 MOCK TEST</p>
</header>
<div class="container">
<div class="card" id="home">
<h2>স্বাগতম!</h2>
<p>আপনার JFT-Basic পরীক্ষার প্রস্তুতি শুরু করুন।</p>
<button onclick="startTest()">মক টেস্ট শুরু করুন</button>
<button onclick="showAdmin()">Admin Panel</button>
</div>

<div class="card hidden" id="test">
<h3 id="progress"></h3>
<p id="timer">সময়: 30:00</p>
<h3 id="question"></h3>
<div id="options"></div>
<button onclick="nextQuestion()">পরবর্তী প্রশ্ন ➜</button>
<button onclick="finishTest()">পরীক্ষা শেষ করুন</button>
</div>

<div class="card hidden" id="result"></div>

<div class="card hidden" id="admin">
<h2>🔐 Admin Panel</h2>
<p>এই প্রাথমিক সংস্করণে প্রশ্ন যোগ করতে নিচের ফর্ম ব্যবহার করুন।</p>
<input id="adminPassword" type="password" placeholder="Admin password">
<button onclick="loginAdmin()">প্রবেশ করুন</button>
<div id="adminForm" class="hidden">
<input id="newQuestion" placeholder="প্রশ্ন লিখুন">
<input id="optionA" placeholder="Option A">
<input id="optionB" placeholder="Option B">
<input id="optionC" placeholder="Option C">
<input id="optionD" placeholder="Option D">
<select id="correct">
<option value="0">সঠিক উত্তর: A</option>
<option value="1">সঠিক উত্তর: B</option>
<option value="2">সঠিক উত্তর: C</option>
<option value="3">সঠিক উত্তর: D</option>
</select>
<button onclick="addQuestion()">প্রশ্ন যোগ করুন</button>
<p id="adminMessage"></p>
</div>
<button onclick="goHome()">হোমে ফিরুন</button>
</div>
</div>

<script>
const starterQuestions = [
{q:"「学生」の読み方はどれですか。",o:["がくせい","せんせい","がっこう","せいと"],a:0},
{q:"「ありがとう」の意味は何ですか。",o:["দুঃখিত","ধন্যবাদ","সুপ্রভাত","বিদায়"],a:1},
{q:"「学校」は何ですか。",o:["বই","বাড়ি","স্কুল","গাড়ি"],a:2}
];

let questions = JSON.parse(localStorage.getItem("jftQuestions") || "null") || starterQuestions;
let index=0, score=0, answers=[], seconds=1800, clock;

function startTest(){
 if(!questions.length){alert("প্রথমে প্রশ্ন যোগ করুন");return}
 index=0;score=0;answers=[];
 document.getElementById("home").classList.add("hidden");
 document.getElementById("result").classList.add("hidden");
 document.getElementById("test").classList.remove("hidden");
 showQuestion();
 clearInterval(clock);
 seconds=1800;
 clock=setInterval(()=>{
 seconds--;
 document.getElementById("timer").textContent=
 "সময়: "+Math.floor(seconds/60)+":"+String(seconds%60).padStart(2,"0");
 if(seconds<=0)finishTest();
 },1000);
}
function showQuestion(){
 document.getElementById("progress").textContent=
 "প্রশ্ন "+(index+1)+" / "+questions.length;
 document.getElementById("question").textContent=questions[index].q;
 document.getElementById("options").innerHTML="";
 questions[index].o.forEach((op,i)=>{
 let label=document.createElement("label");
 label.className="option";
 label.innerHTML='<input type="radio" name="answer" value="'+i+'" style="width:auto"> '+String.fromCharCode(65+i)+". "+op;
 document.getElementById("options").appendChild(label);
 });
}
function nextQuestion(){
 let selected=document.querySelector('input[name="answer"]:checked');
 answers[index]=selected?Number(selected.value):-1;
 if(index<questions.length-1){index++;showQuestion()}
 else finishTest();
}
function finishTest(){
 if(document.getElementById("test").classList.contains("hidden"))return;
 clearInterval(clock);
 let selected=document.querySelector('input[name="answer"]:checked');
 if(selected)answers[index]=Number(selected.value);
 score=questions.reduce((sum,q,i)=>sum+(answers[i]===q.a?1:0),0);
 document.getElementById("test").classList.add("hidden");
 let result=document.getElementById("result");
 result.classList.remove("hidden");
 result.innerHTML="<h2>পরীক্ষার ফলাফল</h2><p>সঠিক উত্তর: "+score+
 " / "+questions.length+"</p><p
