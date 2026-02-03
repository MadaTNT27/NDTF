# NDTF
<html lang="ro">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NOI DOI TREI FRAȚI 🍻</title>

<style>
* { box-sizing: border-box; font-family: Arial Black, Arial; }

body {
    margin: 0;
    height: 100vh;
    overflow: hidden;
    background: linear-gradient(120deg, #000, #111);
    color: white;
}

/* CLUB LIGHTS */
body.club {
    animation: club 6s infinite alternate;
}
@keyframes club {
    from { filter: hue-rotate(0deg); }
    to { filter: hue-rotate(120deg); }
}

/* DRUNK EFFECT */
body.drunk {
    animation: drunk 1.5s infinite;
}
@keyframes drunk {
    0% { transform: rotate(0deg); }
    25% { transform: rotate(0.4deg); }
    50% { transform: rotate(-0.4deg); }
    75% { transform: rotate(0.4deg); }
    100% { transform: rotate(0deg); }
}

/* POPUP */
.overlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 15px;
}

.popup {
    background: #111;
    padding: 25px;
    width: 100%;
    max-width: 360px;
    text-align: center;
    border-radius: 18px;
    border: 3px solid cyan;
    box-shadow: 0 0 30px cyan;
    animation: pop 0.4s ease;
}

@keyframes pop {
    from { transform: scale(0.6); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
}

.buttons {
    display: flex;
    justify-content: center;
    gap: 15px;
    position: relative;
}

button {
    padding: 12px 22px;
    font-size: 16px;
    border-radius: 12px;
    border: none;
    cursor: pointer;
}

.yes { background: #27ae60; color: white; }
.no {
    background: #e74c3c;
    color: white;
    position: relative;
    animation: shake 0.3s infinite;
}

@keyframes shake {
    0% { transform: translateX(0); }
    50% { transform: translateX(3px); }
    100% { transform: translateX(0); }
}

/* COUNTER */
#counter {
    position: fixed;
    top: 12px;
    left: 12px;
    background: black;
    color: #0f0;
    padding: 8px 12px;
    border-radius: 12px;
    font-size: 14px;
    box-shadow: 0 0 10px #0f0;
}

/* LEADERBOARD */
#leaderboard {
    position: fixed;
    top: 12px;
    right: 12px;
    background: black;
    padding: 8px 12px;
    border-radius: 12px;
    font-size: 14px;
    box-shadow: 0 0 10px magenta;
}

/* PLAY BUTTON */
#musicBtn {
    position: fixed;
    bottom: 15px;
    left: 15px;
    background: black;
    color: white;
    padding: 12px 16px;
    border-radius: 50px;
    font-size: 14px;
    box-shadow: 0 0 15px cyan;
    z-index: 10;
}

/* FINAL */
#final {
    display: none;
    position: fixed;
    inset: 0;
    background: radial-gradient(circle, #111, #000);
    font-size: 42px;
    text-align: center;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    text-shadow: 0 0 25px magenta;
}

/* CONFETTI */
.confetti {
    position: absolute;
    top: -20px;
    font-size: 24px;
    animation: fall linear;
}
@keyframes fall {
    from { transform: translateY(-30px) rotate(0); }
    to { transform: translateY(110vh) rotate(360deg); }
}

/* YOUTUBE PLAYER (HIDDEN) */
#player {
    position: fixed;
    width: 1px;
    height: 1px;
    opacity: 0;
}
</style>
</head>

<body class="club">

<div id="counter">🍺 Beri: <span id="count">0</span></div>

<div id="leaderboard">
🏆 NOI: <span id="lb">0</span>
</div>

<button id="musicBtn">▶ Play</button>

<!-- YOUTUBE PLAYER -->
<div id="player"></div>

<!-- POPUPS -->
<div class="overlay" id="overlay">
  <div class="popup">
    <h2 id="text">Salut copii 👋<br>NOI DOI TREI FRAȚI</h2>
    <div class="buttons" id="buttons">
      <button class="yes" onclick="next()">Da</button>
    </div>
  </div>
</div>

<!-- FINAL -->
<div id="final">
  AȘA VĂ VREAU 🍻🔥<br>
  NOI DOI TREI FRAȚI
</div>

<script src="https://www.youtube.com/iframe_api"></script>
<script>
let step = 0;
let beers = 0;
let player;
let playing = false;

/* YOUTUBE INIT */
function onYouTubeIframeAPIReady() {
    player = new YT.Player('player', {
        videoId: 'VIDEO_ID_AICI',
        playerVars: { autoplay: 0 }
    });
}

/* PLAY / PAUSE */
document.getElementById("musicBtn").onclick = () => {
    if (!playing) {
        player.playVideo();
        playing = true;
        document.getElementById("musicBtn").innerText = "⏸ Pause";
    } else {
        player.pauseVideo();
        playing = false;
        document.getElementById("musicBtn").innerText = "▶ Play";
    }
};

/* BEEP */
const ctx = new (window.AudioContext || window.webkitAudioContext)();
function beep() {
    const o = ctx.createOscillator();
    const g = ctx.createGain();
    o.frequency.value = 700;
    g.gain.value = 0.15;
    o.connect(g).connect(ctx.destination);
    o.start();
    o.stop(ctx.currentTime + 0.1);
}

function addBeer() {
    beers++;
    document.getElementById("count").innerText = beers;
    document.getElementById("lb").innerText = beers;
    if (beers >= 3) document.body.classList.add("drunk");
}

function next() {
    addBeer();
    step++;

    if (step === 1) {
        document.getElementById("text").innerHTML =
          "Sunteți pregătiți? 😎<br>NOI DOI TREI FRAȚI";
        document.getElementById("buttons").innerHTML = `
          <button class="yes" onclick="next()">Da</button>
          <button class="no" onmouseover="run(this)">Nu</button>
        `;
    }

    if (step === 2) {
        document.getElementById("text").innerHTML =
          "Bem și noi o bere? 🍺";
        document.getElementById("buttons").innerHTML = `
          <button class="yes" onclick="finish()">Da</button>
          <button class="no" onmouseover="run(this)">Nu</button>
        `;
    }
}

function run(btn) {
    beep();
    btn.style.position = "absolute";
    btn.style.left = Math.random() * 70 + "%";
    btn.style.top = Math.random() * 70 + "%";
}

function finish() {
    addBeer();
    document.getElementById("overlay").style.display = "none";
    document.getElementById("final").style.display = "flex";
    startConfetti();
}

/* CONFETTI */
function startConfetti() {
    setInterval(() => {
        const c = document.createElement("div");
        c.className = "confetti";
        c.innerText = Math.random() > 0.5 ? "🍺" : "🎉";
        c.style.left = Math.random() * 100 + "vw";
        c.style.animationDuration = (2 + Math.random() * 3) + "s";
        document.body.appendChild(c);
        setTimeout(() => c.remove(), 6000);
    }, 200);
}
</script>

</body>
</html>
