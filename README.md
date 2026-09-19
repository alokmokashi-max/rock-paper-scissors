# rock-paper-scissors
A modern, responsive Rock-Paper-Scissors game built with vanilla HTML, CSS &amp; JavaScript. Features a dark glassmorphism UI, animated 3-2-1 countdown, live score tracking, adjustable win target, and confetti celebration on victory. No frameworks, no backend, no dependencies — just open and play in any browser.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Paper Scissors</title>
<style>
  :root {
    --bg1: #0f0c29;
    --bg2: #302b63;
    --bg3: #24243e;
    --accent: #7f5af0;
    --accent2: #2cb67d;
    --danger: #ef4565;
    --text: #fffffe;
    --muted: #a7a9be;
    --glass: rgba(255, 255, 255, 0.07);
    --glass-border: rgba(255, 255, 255, 0.15);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    min-height: 100vh;
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    background: linear-gradient(135deg, var(--bg1), var(--bg2), var(--bg3));
    background-size: 400% 400%;
    animation: gradientShift 15s ease infinite;
    color: var(--text);
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 24px 16px 60px;
  }

  @keyframes gradientShift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  .app {
    width: 100%;
    max-width: 620px;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  header {
    text-align: center;
  }

  header h1 {
    font-size: clamp(1.8rem, 5vw, 2.6rem);
    font-weight: 800;
    background: linear-gradient(90deg, #a78bfa, #34d399, #60a5fa);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    letter-spacing: 0.5px;
  }

  header p {
    color: var(--muted);
    margin-top: 6px;
    font-size: 0.95rem;
  }

  .glass {
    background: var(--glass);
    border: 1px solid var(--glass-border);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border-radius: 20px;
    box-shadow: 0 8px 32px rgba(0,0,0,0.35);
  }

  /* Score board */
  .scoreboard {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    padding: 18px;
  }

  .score-card {
    text-align: center;
    padding: 14px 8px;
    border-radius: 14px;
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--glass-border);
    transition: transform 0.25s ease;
  }

  .score-card.pulse { animation: pop 0.4s ease; }

  @keyframes pop {
    0% { transform: scale(1); }
    40% { transform: scale(1.12); }
    100% { transform: scale(1); }
  }

  .score-card .label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: var(--muted);
    margin-bottom: 6px;
  }

  .score-card .value {
    font-size: 1.8rem;
    font-weight: 800;
  }

  .score-card.player .value { color: var(--accent2); }
  .score-card.computer .value { color: var(--danger); }
  .score-card.draw .value { color: #f5c26b; }

  .round-info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 20px;
    font-size: 0.85rem;
    color: var(--muted);
  }

  .round-info span b { color: var(--text); }

  #targetInput {
    background: rgba(255,255,255,0.08);
    border: 1px solid var(--glass-border);
    color: var(--text);
    border-radius: 8px;
    padding: 4px 8px;
    width: 54px;
    text-align: center;
    font-size: 0.85rem;
  }

  /* Arena */
  .arena {
    padding: 28px 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    min-height: 220px;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .vs-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 18px;
    width: 100%;
  }

  .choice-display {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: rgba(255,255,255,0.06);
    border: 2px solid var(--glass-border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.6rem;
    flex-shrink: 0;
    position: relative;
  }

  .choice-display.win-glow {
    border-color: var(--accent2);
    box-shadow: 0 0 22px rgba(44,182,125,0.6);
  }

  .choice-display.lose-glow {
    border-color: var(--danger);
    box-shadow: 0 0 22px rgba(239,69,101,0.5);
  }

  .choice-display.shake {
    animation: shake 0.5s ease;
  }

  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    20% { transform: translateX(-6px) rotate(-4deg); }
    40% { transform: translateX(6px) rotate(4deg); }
    60% { transform: translateX(-4px) rotate(-2deg); }
    80% { transform: translateX(4px) rotate(2deg); }
  }

  .vs-label {
    font-weight: 800;
    color: var(--muted);
    font-size: 0.9rem;
  }

  .side-label {
    text-align: center;
    font-size: 0.8rem;
    color: var(--muted);
    margin-top: 6px;
  }

  .countdown {
    font-size: 3.5rem;
    font-weight: 900;
    color: var(--accent);
    animation: countdownPop 0.9s ease;
  }

  @keyframes countdownPop {
    0% { transform: scale(0.4); opacity: 0; }
    50% { transform: scale(1.15); opacity: 1; }
    100% { transform: scale(1); opacity: 1; }
  }

  .result-msg {
    font-size: 1.3rem;
    font-weight: 800;
    text-align: center;
    min-height: 34px;
    animation: fadeInUp 0.4s ease;
  }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .result-msg.win { color: var(--accent2); }
  .result-msg.lose { color: var(--danger); }
  .result-msg.draw { color: #f5c26b; }

  .hint {
    color: var(--muted);
    font-size: 0.9rem;
    text-align: center;
  }

  /* Choice buttons */
  .choices {
    display: flex;
    justify-content: center;
    gap: 16px;
    padding: 6px 20px 26px;
    flex-wrap: wrap;
  }

  .choice-btn {
    background: linear-gradient(145deg, rgba(255,255,255,0.09), rgba(255,255,255,0.03));
    border: 1px solid var(--glass-border);
    color: var(--text);
    border-radius: 18px;
    width: 96px;
    height: 96px;
    font-size: 2.4rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s ease;
    position: relative;
  }

  .choice-btn span.name {
    position: absolute;
    bottom: -22px;
    font-size: 0.7rem;
    color: var(--muted);
    letter-spacing: 0.5px;
    text-transform: uppercase;
  }

  .choice-btn:hover:not(:disabled) {
    transform: translateY(-6px) scale(1.05);
    border-color: var(--accent);
    box-shadow: 0 10px 24px rgba(127,90,240,0.35);
  }

  .choice-btn:active:not(:disabled) {
    transform: translateY(-2px) scale(0.97);
  }

  .choice-btn:disabled {
    opacity: 0.35;
    cursor: not-allowed;
  }

  .choice-btn.selected {
    border-color: var(--accent2);
    box-shadow: 0 0 20px rgba(44,182,125,0.5);
  }

  /* Action buttons */
  .actions {
    display: flex;
    justify-content: center;
    gap: 14px;
    padding: 4px 20px 22px;
    flex-wrap: wrap;
  }

  .btn {
    border: none;
    border-radius: 12px;
    padding: 12px 26px;
    font-size: 0.95rem;
    font-weight: 700;
    cursor: pointer;
    transition: transform 0.15s ease, box-shadow 0.15s ease, opacity 0.15s ease;
    color: white;
  }

  .btn:active { transform: scale(0.96); }

  .btn-primary {
    background: linear-gradient(135deg, var(--accent), #5b3fd6);
    box-shadow: 0 8px 20px rgba(127,90,240,0.4);
  }

  .btn-secondary {
    background: rgba(255,255,255,0.08);
    border: 1px solid var(--glass-border);
  }

  .btn-danger {
    background: linear-gradient(135deg, var(--danger), #b32a45);
  }

  .btn:disabled {
    opacity: 0.35;
    cursor: not-allowed;
  }

  /* Winner banner */
  .winner-banner {
    display: none;
    text-align: center;
    padding: 26px 20px;
    animation: fadeInUp 0.5s ease;
  }

  .winner-banner.show { display: block; }

  .winner-banner .trophy {
    font-size: 3.5rem;
    margin-bottom: 8px;
    display: inline-block;
    animation: bounce 1s ease infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
  }

  .winner-banner h2 {
    font-size: 1.6rem;
    margin-bottom: 6px;
  }

  .winner-banner p {
    color: var(--muted);
    margin-bottom: 18px;
  }

  /* Instructions */
  .instructions {
    padding: 18px 22px;
  }

  .instructions summary {
    cursor: pointer;
    font-weight: 700;
    font-size: 0.95rem;
    color: var(--text);
    outline: none;
  }

  .instructions ul {
    margin: 12px 0 0 18px;
    color: var(--muted);
    font-size: 0.88rem;
    line-height: 1.6;
  }

  footer {
    text-align: center;
    color: var(--muted);
    font-size: 0.75rem;
    margin-top: 4px;
  }

  /* Confetti */
  .confetti-piece {
    position: absolute;
    top: -10px;
    width: 8px;
    height: 14px;
    opacity: 0.9;
    animation: fall linear forwards;
  }

  @keyframes fall {
    to { transform: translateY(420px) rotate(360deg); opacity: 0; }
  }

  @media (max-width: 480px) {
    .choice-btn { width: 82px; height: 82px; font-size: 2rem; }
    .choice-display { width: 78px; height: 78px; font-size: 2rem; }
    .score-card .value { font-size: 1.4rem; }
  }
</style>
</head>
<body>

<div class="app">
  <header>
    <h1>🎮 Rock Paper Scissors</h1>
    <p>Play against the computer. First to the target score wins!</p>
  </header>

  <div class="glass scoreboard">
    <div class="score-card player" id="playerCard">
      <div class="label">You</div>
      <div class="value" id="playerScore">0</div>
    </div>
    <div class="score-card draw" id="drawCard">
      <div class="label">Draws</div>
      <div class="value" id="drawScore">0</div>
    </div>
    <div class="score-card computer" id="computerCard">
      <div class="label">Computer</div>
      <div class="value" id="computerScore">0</div>
    </div>
  </div>

  <div class="glass round-info">
    <span>Round <b id="roundNumber">1</b></span>
    <span>Playing to <input type="number" id="targetInput" min="1" max="99" value="5"> wins</span>
  </div>

  <div class="glass arena" id="arena">
    <div class="hint" id="hintText">Choose Rock, Paper, or Scissors to start the round!</div>
  </div>

  <div class="glass" style="padding: 6px 0 0;">
    <div class="choices" id="choiceButtons">
      <button class="choice-btn" data-choice="rock" title="Rock">🪨<span class="name">Rock</span></button>
      <button class="choice-btn" data-choice="paper" title="Paper">📄<span class="name">Paper</span></button>
      <button class="choice-btn" data-choice="scissors" title="Scissors">✂️<span class="name">Scissors</span></button>
    </div>
    <div class="actions">
      <button class="btn btn-primary" id="nextRoundBtn" disabled>Next Round</button>
      <button class="btn btn-secondary" id="resetBtn">Reset Game</button>
    </div>
  </div>

  <div class="glass winner-banner" id="winnerBanner">
    <div class="trophy" id="winnerTrophy">🏆</div>
    <h2 id="winnerTitle">You Win!</h2>
    <p id="winnerSub">You reached the target score first.</p>
    <button class="btn btn-danger" id="playAgainBtn">Play Again</button>
  </div>

  <details class="glass instructions">
    <summary>📖 How to Play</summary>
    <ul>
      <li>Pick Rock, Paper, or Scissors — the computer picks at random.</li>
      <li>🪨 Rock beats ✂️ Scissors, ✂️ Scissors beats 📄 Paper, 📄 Paper beats 🪨 Rock.</li>
      <li>Each round updates the score automatically. Click <b>Next Round</b> to continue.</li>
      <li>Set your target score above — first to reach it wins the match.</li>
      <li>Use <b>Reset Game</b> anytime to start over from scratch.</li>
    </ul>
  </details>

  <footer>Built with HTML, CSS &amp; JavaScript — runs entirely in your browser.</footer>
</div>

<script>
(function () {
  const EMOJI = { rock: '🪨', paper: '📄', scissors: '✂️' };
  const BEATS = { rock: 'scissors', paper: 'rock', scissors: 'paper' };

  const state = {
    playerScore: 0,
    computerScore: 0,
    draws: 0,
    round: 1,
    target: 5,
    locked: false,
    gameOver: false
  };

  const els = {
    playerScore: document.getElementById('playerScore'),
    computerScore: document.getElementById('computerScore'),
    drawScore: document.getElementById('drawScore'),
    playerCard: document.getElementById('playerCard'),
    computerCard: document.getElementById('computerCard'),
    drawCard: document.getElementById('drawCard'),
    roundNumber: document.getElementById('roundNumber'),
    targetInput: document.getElementById('targetInput'),
    arena: document.getElementById('arena'),
    hintText: document.getElementById('hintText'),
    choiceButtons: document.getElementById('choiceButtons'),
    nextRoundBtn: document.getElementById('nextRoundBtn'),
    resetBtn: document.getElementById('resetBtn'),
    winnerBanner: document.getElementById('winnerBanner'),
    winnerTrophy: document.getElementById('winnerTrophy'),
    winnerTitle: document.getElementById('winnerTitle'),
    winnerSub: document.getElementById('winnerSub'),
    playAgainBtn: document.getElementById('playAgainBtn')
  };

  function pulse(el) {
    el.classList.remove('pulse');
    void el.offsetWidth; // restart animation
    el.classList.add('pulse');
  }

  function randomChoice() {
    const options = ['rock', 'paper', 'scissors'];
    return options[Math.floor(Math.random() * options.length)];
  }

  function decideWinner(player, computer) {
    if (player === computer) return 'draw';
    return BEATS[player] === computer ? 'player' : 'computer';
  }

  function setChoiceButtonsEnabled(enabled) {
    document.querySelectorAll('.choice-btn').forEach(btn => btn.disabled = !enabled);
  }

  function buildArenaIdle(message) {
    els.arena.innerHTML = `<div class="hint" id="hintText">${message}</div>`;
  }

  function renderCountdown(callback) {
    let count = 3;
    els.arena.innerHTML = `<div class="countdown" id="cd">${count}</div>`;
    const cdEl = document.getElementById('cd');
    const interval = setInterval(() => {
      count -= 1;
      if (count > 0) {
        cdEl.textContent = count;
        cdEl.style.animation = 'none';
        void cdEl.offsetWidth;
        cdEl.style.animation = 'countdownPop 0.9s ease';
      } else {
        cdEl.textContent = 'GO!';
        cdEl.style.animation = 'none';
        void cdEl.offsetWidth;
        cdEl.style.animation = 'countdownPop 0.6s ease';
        clearInterval(interval);
        setTimeout(callback, 550);
      }
    }, 700);
  }

  function renderRoundResult(playerChoice, computerChoice, outcome) {
    const playerGlow = outcome === 'player' ? 'win-glow' : outcome === 'computer' ? 'lose-glow' : '';
    const computerGlow = outcome === 'computer' ? 'win-glow' : outcome === 'player' ? 'lose-glow' : '';

    let resultText, resultClass;
    if (outcome === 'player') {
      resultText = `🎉 ${cap(playerChoice)} beats ${cap(computerChoice)} — You win this round!`;
      resultClass = 'win';
    } else if (outcome === 'computer') {
      resultText = `💥 ${cap(computerChoice)} beats ${cap(playerChoice)} — Computer wins this round!`;
      resultClass = 'lose';
    } else {
      resultText = `🤝 Both chose ${cap(playerChoice)} — It's a draw!`;
      resultClass = 'draw';
    }

    els.arena.innerHTML = `
      <div class="vs-row">
        <div style="text-align:center;">
          <div class="choice-display shake ${playerGlow}">${EMOJI[playerChoice]}</div>
          <div class="side-label">You</div>
        </div>
        <div class="vs-label">VS</div>
        <div style="text-align:center;">
          <div class="choice-display shake ${computerGlow}">${EMOJI[computerChoice]}</div>
          <div class="side-label">Computer</div>
        </div>
      </div>
      <div class="result-msg ${resultClass}">${resultText}</div>
    `;
  }

  function cap(word) {
    return word.charAt(0).toUpperCase() + word.slice(1);
  }

  function updateScoreboard(outcome) {
    if (outcome === 'player') {
      state.playerScore++;
      els.playerScore.textContent = state.playerScore;
      pulse(els.playerCard);
    } else if (outcome === 'computer') {
      state.computerScore++;
      els.computerScore.textContent = state.computerScore;
      pulse(els.computerCard);
    } else {
      state.draws++;
      els.drawScore.textContent = state.draws;
      pulse(els.drawCard);
    }
  }

  function checkMatchOver() {
    if (state.playerScore >= state.target || state.computerScore >= state.target) {
      state.gameOver = true;
      showWinnerBanner();
      return true;
    }
    return false;
  }

  function showWinnerBanner() {
    setChoiceButtonsEnabled(false);
    els.nextRoundBtn.disabled = true;
    els.winnerBanner.classList.add('show');

    if (state.playerScore > state.computerScore) {
      els.winnerTrophy.textContent = '🏆';
      els.winnerTitle.textContent = '🏆 You Win!';
      els.winnerSub.textContent = `You beat the computer ${state.playerScore} - ${state.computerScore}.`;
      launchConfetti();
    } else if (state.computerScore > state.playerScore) {
      els.winnerTrophy.textContent = '🤖';
      els.winnerTitle.textContent = '🤖 Computer Wins!';
      els.winnerSub.textContent = `The computer beat you ${state.computerScore} - ${state.playerScore}.`;
    } else {
      els.winnerTrophy.textContent = '🤝';
      els.winnerTitle.textContent = "🤝 It's a Draw!";
      els.winnerSub.textContent = `Final score: ${state.playerScore} - ${state.computerScore}.`;
    }
  }

  function launchConfetti() {
    const colors = ['#7f5af0', '#2cb67d', '#f5c26b', '#ef4565', '#60a5fa'];
    for (let i = 0; i < 40; i++) {
      const piece = document.createElement('div');
      piece.className = 'confetti-piece';
      piece.style.left = Math.random() * 100 + '%';
      piece.style.background = colors[Math.floor(Math.random() * colors.length)];
      piece.style.animationDuration = (1.5 + Math.random() * 1.5) + 's';
      piece.style.animationDelay = (Math.random() * 0.4) + 's';
      els.winnerBanner.appendChild(piece);
      setTimeout(() => piece.remove(), 3500);
    }
  }

  function playRound(playerChoice) {
    if (state.locked || state.gameOver) return;
    state.locked = true;
    setChoiceButtonsEnabled(false);
    els.nextRoundBtn.disabled = true;

    document.querySelectorAll('.choice-btn').forEach(btn => {
      btn.classList.toggle('selected', btn.dataset.choice === playerChoice);
    });

    renderCountdown(() => {
      const computerChoice = randomChoice();
      const outcome = decideWinner(playerChoice, computerChoice);
      renderRoundResult(playerChoice, computerChoice, outcome);
      updateScoreboard(outcome);

      const isOver = checkMatchOver();
      if (!isOver) {
        els.nextRoundBtn.disabled = false;
      }
      state.locked = false;
    });
  }

  function nextRound() {
    if (state.gameOver) return;
    state.round++;
    els.roundNumber.textContent = state.round;
    document.querySelectorAll('.choice-btn').forEach(btn => btn.classList.remove('selected'));
    setChoiceButtonsEnabled(true);
    els.nextRoundBtn.disabled = true;
    buildArenaIdle('Choose Rock, Paper, or Scissors to start the round!');
  }

  function resetGame() {
    state.playerScore = 0;
    state.computerScore = 0;
    state.draws = 0;
    state.round = 1;
    state.locked = false;
    state.gameOver = false;

    els.playerScore.textContent = '0';
    els.computerScore.textContent = '0';
    els.drawScore.textContent = '0';
    els.roundNumber.textContent = '1';

    els.winnerBanner.classList.remove('show');
    els.winnerBanner.querySelectorAll('.confetti-piece').forEach(p => p.remove());

    document.querySelectorAll('.choice-btn').forEach(btn => btn.classList.remove('selected'));
    setChoiceButtonsEnabled(true);
    els.nextRoundBtn.disabled = true;

    buildArenaIdle('Choose Rock, Paper, or Scissors to start the round!');
  }

  // Event listeners
  document.querySelectorAll('.choice-btn').forEach(btn => {
    btn.addEventListener('click', () => playRound(btn.dataset.choice));
  });

  els.nextRoundBtn.addEventListener('click', nextRound);
  els.resetBtn.addEventListener('click', resetGame);
  els.play
