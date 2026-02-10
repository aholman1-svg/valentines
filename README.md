<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valentine Quiz for Maddie 💖</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500;700&display=swap');
body {
  margin: 0;
  font-family: 'Quicksand', sans-serif;
  background: linear-gradient(135deg, #ff758c, #ff7eb3);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  overflow: hidden;
  position: relative;
  color: #333;
}
.heart {
  position: absolute;
  font-size: 1.2rem;
  animation: float 6s linear infinite;
  opacity: 0.8;
}
@keyframes float {
  0% { transform: translateY(100vh) rotate(0deg); opacity:0; }
  10% { opacity: 1; }
  100% { transform: translateY(-10vh) rotate(360deg); opacity:0; }
}
.card {
  background: white;
  padding: 2.5rem;
  border-radius: 20px;
  text-align: center;
  max-width: 400px;
  width: 90%;
  box-shadow: 0 20px 50px rgba(0,0,0,0.2);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
h1 { font-size: 1.8rem; margin-bottom: 0.5rem; }
p { margin-bottom: 1.5rem; color: #555; }
button {
  font-size: 1rem;
  padding: 0.7rem 1.4rem;
  border-radius: 999px;
  border: none;
  cursor: pointer;
  margin: 0.3rem;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
button:hover { transform: translateY(-2px); box-shadow: 0 8px 16px rgba(0,0,0,0.15); }
.yes { background: #ff4d6d; color: white; }
.no { background: #eee; color: #555; }
.hint-btn { background: #ffb6c1; color: white; }
input[type="text"] {
  padding: 0.6rem;
  border-radius: 10px;
  border: 1px solid #ccc;
  width: 80%;
  margin-bottom: 1rem;
  font-size: 1rem;
}
.hidden { display: none; }
.slider-container { display: flex; justify-content: space-around; margin-bottom: 1rem; }
.slider-label { font-weight: 600; margin-bottom: 0.3rem; display:block; }
</style>
</head>
<body>
<script>
for(let i=0;i<30;i++){
  let heart = document.createElement('div');
  heart.className = 'heart';
  heart.style.left = Math.random()*100+'vw';
  heart.style.fontSize = (10+Math.random()*20)+'px';
  heart.innerHTML = '❤️';
  heart.style.animationDuration = (4+Math.random()*4)+'s';
  document.body.appendChild(heart);
}
</script>

<!-- Landing Screen -->
<div class="card" id="landing">
  <h1>Hello Maddie 💕</h1>
  <button class="yes" onclick="startQuiz()">Start ➡️</button>
</div>

<!-- Quiz Screen -->
<div class="card hidden" id="quiz">
  <h1 id="question"></h1>
  <p id="progress"></p>
  <div id="input-container"></div>
  <div id="slider-container" class="slider-container hidden"></div>
  <div id="hint-container" class="hidden">
    <button class="hint-btn" id="hint-btn">Hint 💡</button>
    <span id="hint-emoji" class="hidden" style="font-size:1.5rem;">🌅</span>
  </div>
  <button id="next-btn" class="yes" onclick="checkAnswer()">Submit</button>
</div>

<!-- Marriage Question -->
<div class="card hidden" id="marriage">
  <h1>Will we get married? 💍</h1>
  <button class="yes" onclick="marriageYes()">Yes ❤️</button>
  <button class="no" onclick="marriageNo()">No ❌</button>
</div>

<div class="card hidden" id="wrongMarriage">
  <h1>WRONG ANSWER ❌</h1>
</div>

<!-- Post-Quiz -->
<div class="card hidden" id="postQuiz">
  <h1>You got 100% 🎉💖</h1>
  <button class="yes" onclick="showValentine()">Continue ➡️</button>
</div>

<!-- Valentine Question -->
<div class="card hidden" id="valentine">
  <h1>Be my Valentine? 💘</h1>
  <button class="yes" onclick="valentineYes()">Yes ❤️</button>
  <button class="no" onclick="valentineNo()">No ❌</button>
</div>

<div class="card hidden" id="tryAgain">
  <h1>TRY AGAIN ❌</h1>
</div>

<!-- Final Screen -->
<div class="card hidden" id="final">
  <h1>See you on the 14th my love 💖💌🥰</h1>
</div>

<script>
let questions = [
  {type:'text', q:'What is my name? 📝', answer:'Andrew'},
  {type:'slider', q:'When did we start dating? 📅', month:1, day:17, year:2026},
  {type:'text', q:'Where was our first date (this year)? 🍝🏖️', answer:'Pasta Beach'},
  {type:'hint', q:'What is our favorite activity to do together? 🎬🎮🎵', answer:'sunset'}
];

let current = 0;

function startQuiz(){
  document.getElementById('landing').classList.add('hidden');
  document.getElementById('quiz').classList.remove('hidden');
  loadQuestion();
}

function loadQuestion(){
  let q = questions[current];
  document.getElementById('input-container').innerHTML = '';
  let sliderContainer = document.getElementById('slider-container');
  sliderContainer.classList.add('hidden');
  sliderContainer.innerHTML = '';
  document.getElementById('hint-container').classList.add('hidden');
  document.getElementById('hint-emoji').classList.add('hidden');

  document.getElementById('next-btn').style.display = 'inline-block';
  document.getElementById('progress').textContent = `Question ${current+1} of ${questions.length}`;

  if(q.type==='text'){
    document.getElementById('question').textContent = q.q;
    let input = document.createElement('input');
    input.type='text';
    input.id='text-answer';
    document.getElementById('input-container').appendChild(input);
  }

  if(q.type==='slider'){
    document.getElementById('question').textContent = q.q;
    sliderContainer.classList.remove('hidden');
    sliderContainer.innerHTML = `
      <div>
        <span class="slider-label">Day</span>
        <input type="range" id="slider-day" min="1" max="31" value="1">
        <span id="day-val">1</span>
      </div>
      <div>
        <span class="slider-label">Month</span>
        <input type="range" id="slider-month" min="1" max="12" value="1">
        <span id="month-val">1</span>
      </div>
      <div>
        <span class="slider-label">Year</span>
        <input type="range" id="slider-year" min="2026" max="2026" value="2026" disabled>
        <span id="year-val">2026</span>
      </div>
    `;
    document.getElementById('slider-day').oninput = ()=>document.getElementById('day-val').textContent = document.getElementById('slider-day').value;
    document.getElementById('slider-month').oninput = ()=>document.getElementById('month-val').textContent = document.getElementById('slider-month').value;
  }

  if(q.type==='hint'){
    document.getElementById('question').textContent = q.q;
    let input = document.createElement('input');
    input.type='text';
    input.id='text-answer';
    document.getElementById('input-container').appendChild(input);
    document.getElementById('hint-container').classList.remove('hidden');
    document.getElementById('hint-btn').onclick = ()=>{
      document.getElementById('hint-btn').classList.add('hidden');
      document.getElementById('hint-emoji').classList.remove('hidden');
    };
  }
}

function checkAnswer(){
  let q = questions[current];
  let correct = false;

  if(q.type==='text' || q.type==='hint'){
    let val = document.getElementById('text-answer').value.trim().toLowerCase();
    if(val === q.answer.toLowerCase()) correct = true;
  }

  if(q.type==='slider'){
    let day = parseInt(document.getElementById('slider-day').value);
    let month = parseInt(document.getElementById('slider-month').value);
    let year = parseInt(document.getElementById('slider-year').value);
    if(day===q.day && month===q.month && year===q.year) correct = true;
  }

  if(correct){
    current++;
    if(current<questions.length){
      loadQuestion();
    } else {
      // After last question, show marriage question
      document.getElementById('quiz').classList.add('hidden');
      document.getElementById('marriage').classList.remove('hidden');
    }
  } else {
    alert('Try again! ❌');
  }
}

// Marriage Question
function marriageYes(){
  document.getElementById('marriage').classList.add('hidden');
  document.getElementById('postQuiz').classList.remove('hidden');
}

function marriageNo(){
  document.getElementById('marriage').classList.add('hidden');
  let wrong = document.getElementById('wrongMarriage');
  wrong.classList.remove('hidden');
  setTimeout(()=>{
    wrong.classList.add('hidden');
    document.getElementById('marriage').classList.remove('hidden');
  },3000);
}

// Post-Quiz → Valentine
function showValentine(){
  document.getElementById('postQuiz').classList.add('hidden');
  document.getElementById('valentine').classList.remove('hidden');
}

// Valentine question
function valentineYes(){
  document.getElementById('valentine').classList.add('hidden');
  document.getElementById('final').classList.remove('hidden');
}

function valentineNo(){
  document.getElementById('valentine').classList.add('hidden');
  let tryScreen = document.getElementById('tryAgain');
  tryScreen.classList.remove('hidden');
  setTimeout(()=>{
    tryScreen.classList.add('hidden');
    document.getElementById('valentine').classList.remove('hidden');
  },3000);
}
</script>
</body>
</html>


