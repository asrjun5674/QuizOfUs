<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Arjun vs Eric - Quiz Battle</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #101027, #302060);
  color: white;
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 34px;
  color: #ffdd57;
}

.subtitle {
  color: #ddd;
}

.scoreboard {
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
  margin: 25px auto;
}

.score {
  width: 160px;
  padding: 20px;
  border-radius: 18px;
  background: #222244;
  border: 2px solid #555;
}

.score h2 {
  margin: 0 0 10px;
}

.points {
  font-size: 38px;
  font-weight: bold;
}

.arjun {
  color: #ffcc33;
}

.eric {
  color: #53c7ff;
}

.progress {
  max-width: 600px;
  margin: 20px auto;
  background: #444;
  height: 12px;
  border-radius: 20px;
  overflow: hidden;
}

#progressBar {
  width: 0%;
  height: 100%;
  background: #35e08b;
  transition: width 0.3s;
}

.questions {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 15px;
  max-width: 1100px;
  margin: 25px auto;
}

.question {
  background: #222244;
  padding: 20px;
  border-radius: 15px;
  border: 1px solid #555;
}

.question h3 {
  margin-top: 0;
}

button {
  border: none;
  padding: 12px 16px;
  margin: 5px;
  border-radius: 10px;
  font-weight: bold;
  cursor: pointer;
  font-size: 15px;
  transition: 0.2s;
}

button:hover:not(:disabled) {
  transform: scale(1.04);
}

.arjun-btn {
  background: #ffcc33;
  color: black;
}

.eric-btn {
  background: #53c7ff;
  color: black;
}

.both-btn {
  background: #35e08b;
  color: black;
}

button:disabled {
  opacity: 0.35;
  cursor: not-allowed;
  transform: none;
}

.winner {
  background: #ffdd57;
  color: #111;
  max-width: 600px;
  margin: 30px auto;
  padding: 25px;
  border-radius: 20px;
  display: none;
}

.winner h1 {
  color: #111;
}

.reset {
  background: #ff5252;
  color: white;
  font-size: 18px;
  padding: 15px 35px;
  margin: 30px;
}

footer {
  color: #aaa;
  padding: 20px;
}
</style>
</head>

<body>

<h1>🏆 ARJUN VS ERIC</h1>
<p class="subtitle">100 Questions • 2 Players • 1 Winner</p>

<div class="scoreboard">

  <div class="score">
    <h2 class="arjun">ARJUN</h2>
    <div class="points" id="arjunScore">0</div>
    <p>Points</p>
  </div>

  <div class="score">
    <h2 class="eric">ERIC</h2>
    <div class="points" id="ericScore">0</div>
    <p>Points</p>
  </div>

</div>

<h3 id="progressText">0 / 100 Questions Completed</h3>

<div class="progress">
  <div id="progressBar"></div>
</div>

<div class="questions" id="questions"></div>

<div class="winner" id="winner">
  <h1 id="winnerTitle"></h1>
  <h2 id="finalScore"></h2>
  <p id="winnerMessage"></p>
</div>

<button class="reset" onclick="resetGame()">
  🔄 Restart Game
</button>

<footer>
  Made for Arjun & Eric ❤️
</footer>

<script>
let arjun = 0;
let eric = 0;
let completed = 0;

const totalQuestions = 100;

let answers = Array(totalQuestions).fill(null);

const container = document.getElementById("questions");

// Create all 100 questions
function createQuestions() {
  container.innerHTML = "";

  for (let i = 1; i <= totalQuestions; i++) {

    const card = document.createElement("div");
    card.className = "question";
    card.id = "question-" + i;

    card.innerHTML = `
      <h3>Question ${i}</h3>
      <p>Who got the correct answer?</p>

      <button class="arjun-btn"
        id="arjun-${i}"
        onclick="givePoint(${i}, 'arjun')">
        Arjun
      </button>

      <button class="eric-btn"
        id="eric-${i}"
        onclick="givePoint(${i}, 'eric')">
        Eric
      </button>

      <button class="both-btn"
        id="both-${i}"
        onclick="givePoint(${i}, 'both')">
        Both (=)
      </button>

      <p id="status-${i}">Not answered</p>
    `;

    container.appendChild(card);
  }
}

// Award points
function givePoint(question, player) {

  // Prevent duplicate scoring
  if (answers[question - 1] !== null) {
    return;
  }

  answers[question - 1] = player;
  completed++;

  if (player === "arjun") {
    arjun++;
  }

  else if (player === "eric") {
    eric++;
  }

  else if (player === "both") {
    arjun++;
    eric++;
  }

  // Disable all three buttons for this question
  document.querySelectorAll(
    `#question-${question} button`
  ).forEach(button => {
    button.disabled = true;
  });

  // Display result
  let message;

  if (player === "both") {
    message = "🤝 Both got 1 point!";
  } else {
    message = "✅ Point for " + player.toUpperCase();
  }

  document.getElementById(
    "status-" + question
  ).innerText = message;

  updateScore();
}

// Update scores and progress
function updateScore() {

  document.getElementById("arjunScore").innerText = arjun;
  document.getElementById("ericScore").innerText = eric;

  document.getElementById("progressText").innerText =
    completed + " / " + totalQuestions + " Questions Completed";

  document.getElementById("progressBar").style.width =
    (completed / totalQuestions * 100) + "%";

  if (completed === totalQuestions) {
    showWinner();
  }
}

// Calculate the winner
function showWinner() {

  const box = document.getElementById("winner");

  box.style.display = "block";

  if (arjun > eric) {

    document.getElementById("winnerTitle").innerText =
      "🏆 ARJUN WINS!";

    document.getElementById("winnerMessage").innerText =
      "Congratulations Arjun! You scored more points!";

  } else if (eric > arjun) {

    document.getElementById("winnerTitle").innerText =
      "🏆 ERIC WINS!";

    document.getElementById("winnerMessage").innerText =
      "Congratulations Eric! You scored more points!";

  } else {

    document.getElementById("winnerTitle").innerText =
      "🤝 IT'S A TIE!";

    document.getElementById("winnerMessage").innerText =
      "Both players scored the same number of points!";

  }

  document.getElementById("finalScore").innerText =
    "Arjun: " + arjun + " | Eric: " + eric;

  box.scrollIntoView({
    behavior: "smooth"
  });
}

// Restart the game
function resetGame() {

  arjun = 0;
  eric = 0;
  completed = 0;

  answers = Array(totalQuestions).fill(null);

  document.getElementById("winner").style.display = "none";

  updateScore();
  createQuestions();

  window.scrollTo({
    top: 0,
    behavior: "smooth"
  });
}

// Start game
createQuestions();
</script>

</body>
</html>
