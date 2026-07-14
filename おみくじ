<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>おみくじ</title>
<style>
  :root {
    --washi: #F6EFE0;
    --washi-deep: #EDE2C9;
    --shrine-red: #A62639;
    --shrine-red-deep: #7E1C2B;
    --gold: #B8925A;
    --ink: #2B2620;
    --shadow: rgba(43, 38, 32, 0.25);
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background:
      radial-gradient(circle at 20% 15%, rgba(184,146,90,0.10), transparent 40%),
      radial-gradient(circle at 80% 85%, rgba(166,38,57,0.08), transparent 45%),
      var(--washi);
    font-family: "Hiragino Mincho ProN", "Yu Mincho", "Noto Serif JP", serif;
    color: var(--ink);
    padding: 24px;
    overflow: hidden;
  }

  .stage {
    position: relative;
    width: min(420px, 92vw);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 28px;
  }

  h1 {
    margin: 0;
    font-size: 1.5rem;
    letter-spacing: 0.35em;
    font-weight: 600;
    color: var(--shrine-red-deep);
    text-align: center;
  }

  h1::after {
    content: "";
    display: block;
    width: 48px;
    height: 2px;
    background: var(--gold);
    margin: 14px auto 0;
  }

  /* --- 筒 (Omikuji cylinder) --- */
  .cylinder-wrap {
    width: 120px;
    height: 220px;
    position: relative;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
  }

  .cylinder {
    position: absolute;
    inset: 0;
    border-radius: 16px 16px 10px 10px;
    background: linear-gradient(180deg, #3a3230 0%, #2b2620 100%);
    box-shadow: 0 18px 30px var(--shadow), inset 0 0 0 2px rgba(184,146,90,0.5);
    display: flex;
    align-items: flex-start;
    justify-content: center;
    overflow: visible;
    transition: transform 0.08s ease;
  }

  .cylinder::before {
    content: "御神籤";
    writing-mode: vertical-rl;
    color: var(--gold);
    font-size: 1.1rem;
    letter-spacing: 0.3em;
    margin-top: 34px;
    opacity: 0.9;
  }

  .cylinder.shaking {
    animation: shake 0.5s ease-in-out;
  }

  @keyframes shake {
    0%, 100% { transform: rotate(0deg) translateX(0); }
    15% { transform: rotate(-6deg) translateX(-4px); }
    30% { transform: rotate(5deg) translateX(4px); }
    45% { transform: rotate(-5deg) translateX(-3px); }
    60% { transform: rotate(4deg) translateX(3px); }
    75% { transform: rotate(-3deg) translateX(-2px); }
    90% { transform: rotate(2deg) translateX(1px); }
  }

  .stick {
    position: absolute;
    top: -70px;
    left: 50%;
    width: 16px;
    height: 90px;
    background: linear-gradient(180deg, #EFE6CF, #E3D6B4);
    border-radius: 3px;
    box-shadow: 0 6px 10px var(--shadow);
    transform: translateX(-50%) translateY(70px);
    opacity: 0;
    transition: transform 0.5s cubic-bezier(.2,.9,.3,1.3), opacity 0.3s ease;
  }

  .stick.out {
    transform: translateX(-50%) translateY(-30px);
    opacity: 1;
  }

  .hint {
    font-family: "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif;
    font-size: 0.8rem;
    letter-spacing: 0.1em;
    color: var(--ink);
    opacity: 0.55;
  }

  /* --- 結果の紙 --- */
  .paper {
    width: 100%;
    padding: 28px 20px 24px;
    background: var(--washi-deep);
    border: 1px solid rgba(184,146,90,0.4);
    border-radius: 4px;
    box-shadow: 0 20px 40px var(--shadow);
    text-align: center;
    opacity: 0;
    transform: translateY(14px) scale(0.98);
    transition: opacity 0.5s ease, transform 0.5s ease;
    position: relative;
  }

  .paper::before, .paper::after {
    content: "";
    position: absolute;
    left: 12px; right: 12px;
    height: 1px;
    background: rgba(184,146,90,0.4);
  }
  .paper::before { top: 10px; }
  .paper::after { bottom: 10px; }

  .paper.show {
    opacity: 1;
    transform: translateY(0) scale(1);
  }

  .result {
    font-size: 2.4rem;
    font-weight: 700;
    letter-spacing: 0.15em;
    margin: 6px 0 14px;
  }

  .result.great { color: var(--shrine-red-deep); }
  .result.good { color: var(--gold); }
  .result.bad { color: #55606b; }

  .message {
    font-family: "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif;
    font-size: 0.95rem;
    line-height: 1.8;
    color: var(--ink);
    opacity: 0.85;
  }

  .again {
    margin-top: 18px;
    font-family: "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif;
    font-size: 0.8rem;
    letter-spacing: 0.1em;
    color: var(--shrine-red-deep);
    background: none;
    border: 1px solid var(--shrine-red-deep);
    border-radius: 999px;
    padding: 8px 20px;
    cursor: pointer;
    transition: background 0.2s ease, color 0.2s ease;
  }

  .again:hover {
    background: var(--shrine-red-deep);
    color: var(--washi);
  }
</style>
</head>
<body>

<div class="stage">
  <h1>おみくじ</h1>

  <div class="cylinder-wrap" id="cylinderWrap">
    <div class="stick" id="stick"></div>
    <div class="cylinder" id="cylinder"></div>
  </div>
  <div class="hint" id="hint">筒をタップして運勢を引く</div>

  <div class="paper" id="paper">
    <div class="result" id="result"></div>
    <div class="message" id="message"></div>
    <button class="again" id="again">もう一度引く</button>
  </div>
</div>

<script>
  const fortunes = [
    { label: "大吉", cls: "great", weight: 1, msg: "何をやってもうまくいく最高の日。思い切って動いてみて。" },
    { label: "中吉", cls: "great", weight: 2, msg: "良い流れが来ています。焦らず一歩ずつ進みましょう。" },
    { label: "小吉", cls: "good", weight: 3, msg: "小さな幸運が積み重なる日。足元の準備を大切に。" },
    { label: "吉", cls: "good", weight: 3, msg: "穏やかで安定した運気。いつも通りが一番うまくいきます。" },
    { label: "末吉", cls: "good", weight: 2, msg: "これから運気が上向きに。今は種まきの時期です。" },
    { label: "凶", cls: "bad", weight: 1, msg: "少し慎重に。無理をせず、休むことも大事な選択です。" },
  ];

  function pickFortune() {
    const total = fortunes.reduce((s, f) => s + f.weight, 0);
    let r = Math.random() * total;
    for (const f of fortunes) {
      if (r < f.weight) return f;
      r -= f.weight;
    }
    return fortunes[0];
  }

  const cylinderWrap = document.getElementById('cylinderWrap');
  const cylinder = document.getElementById('cylinder');
  const stick = document.getElementById('stick');
  const paper = document.getElementById('paper');
  const result = document.getElementById('result');
  const message = document.getElementById('message');
  const hint = document.getElementById('hint');
  const again = document.getElementById('again');

  let busy = false;

  function draw() {
    if (busy) return;
    busy = true;
    hint.style.visibility = 'hidden';
    paper.classList.remove('show');
    stick.classList.remove('out');
    cylinder.classList.remove('shaking');
    void cylinder.offsetWidth; // reflow to restart animation
    cylinder.classList.add('shaking');

    setTimeout(() => {
      stick.classList.add('out');
    }, 300);

    setTimeout(() => {
      const f = pickFortune();
      result.textContent = f.label;
      result.className = 'result ' + f.cls;
      message.textContent = f.msg;
      paper.classList.add('show');
      busy = false;
    }, 900);
  }

  cylinderWrap.addEventListener('click', draw);
  again.addEventListener('click', (e) => {
    e.stopPropagation();
    draw();
  });
</script>

</body>
</html>
# -[omikuji.html.html](https://github.com/user-attachments/files/30008663/omikuji.html.html)
