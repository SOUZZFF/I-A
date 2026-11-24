
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PUMPKIN HUB 🎃</title>
<style>
* {
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  margin: 0;
  min-height: 100vh;
  background:
    radial-gradient(circle at center, #ff7518, #431000 75%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}

.hub {
  width: 340px;
  background: #111;
  border: 2px solid #ff3c00;
  border-radius: 18px;
  box-shadow: 0 0 35px #ff4500;
  overflow: hidden;
  animation: appear .6s ease;
}

@keyframes appear {
  from { opacity: 0; transform: scale(.7) translateY(30px); }
  to { opacity: 1; transform: scale(1) translateY(0); }
}

.header {
  padding: 20px;
  text-align: center;
  background: linear-gradient(120deg,#ff2400,#ff7900);
}

.header h1 {
  margin: 0;
  font-size: 25px;
  text-shadow: 0 0 12px black;
}

.header p {
  margin: 6px 0 0;
  font-size: 11px;
}

.content { padding: 18px; }

.section {
  color: #ff6500;
  font-size: 12px;
  margin: 10px 0;
  font-weight: bold;
}

.option {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #202020;
  border: 1px solid #3a2018;
  border-radius: 10px;
  padding: 13px;
  margin-bottom: 10px;
  transition: .25s;
}

.option:hover {
  border-color: #ff4500;
  transform: translateX(4px);
}

button {
  background: #b92308;
  color: white;
  border: none;
  border-radius: 7px;
  padding: 8px 13px;
  cursor: pointer;
  transition: .2s;
}

button:hover {
  background: #ff6500;
  box-shadow: 0 0 12px #ff4500;
}

button.active {
  background: #ff7900;
}

.footer {
  text-align: center;
  color: #999;
  font-size: 10px;
  padding: 12px;
}

.close {
  float: right;
  background: #171717;
  padding: 5px 10px;
}
</style>
</head>

<body>

<div class="hub" id="hub">

  <div class="header">
    <button class="close" onclick="closeHub()">✕</button>
    <h1>🎃 PUMPKIN HUB</h1>
    <p>HALLOWEEN EDITION</p>
  </div>

  <div class="content">
    <div class="section">🔥 PRINCIPAL</div>

    <div class="option">
      <span>🚀 Fly</span>
      <button onclick="toggle(this)">OFF</button>
    </div>

    <div class="option">
      <span>⚡ Speed</span>
      <button onclick="toggle(this)">OFF</button>
    </div>

    <div class="option">
      <span>👻 Noclip</span>
      <button onclick="toggle(this)">OFF</button>
    </div>

    <div class="option">
      <span>👁️ ESP</span>
      <button onclick="toggle(this)">OFF</button>
    </div>

    <div class="section">🎃 CONFIGURAÇÕES</div>

    <div class="option">
      <span>✨ Halloween FX</span>
      <button onclick="toggle(this)">OFF</button>
    </div>

    <div class="option">
      <span>🌙 Dark Mode</span>
      <button onclick="toggle(this)">OFF</button>
    </div>
  </div>

  <div class="footer">
    PUMPKIN HUB • HALLOWEEN 2026
  </div>

</div>

<script>
function toggle(btn) {
  const enabled = btn.textContent === "OFF";
  btn.textContent = enabled ? "ON" : "OFF";
  btn.classList.toggle("active", enabled);
}

function closeHub() {
  document.getElementById("hub").style.display = "none";
}
</script>

</body>
</html>
