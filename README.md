<head>
  <meta charset="UTF-8" />
  <title>Mindfulness Card of the Day</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      padding: 0;
      background: #f6f1ff; /* light lavender background */
      color: #4a2c82; /* deep purple text */
    }

    .page {
      max-width: 900px;
      margin: 0 auto;
      padding: 32px 16px 60px;
      text-align: center;
    }

    h1 {
      font-size: 32px;
      margin-bottom: 8px;
      color: #4a2c82;
    }

    .subtitle {
      font-size: 15px;
      margin-bottom: 28px;
      color: #6a4db3;
    }

    .card-wrapper {
      display: flex;
      justify-content: center;
      margin-bottom: 18px;
    }

    /* Card flip layout */
    .card {
      width: 260px;
      height: 170px;
      perspective: 1000px;
      cursor: pointer;
    }

    .card-inner {
      position: relative;
      width: 100%;
      height: 100%;
      text-align: center;
      transition: transform 0.6s;
      transform-style: preserve-3d;
    }

    .card.flipped .card-inner {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      border-radius: 18px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 16px;
      box-shadow: 0 8px 20px rgba(111, 63, 155, 0.25);
    }

    .card-front {
      background: linear-gradient(145deg, #b79ce8, #9a6edc); /* purple gradient */
      color: white;
      font-size: 18px;
      gap: 4px;
    }

    .card-front span.emoji {
      font-size: 32px;
    }

    .card-back {
      background: #f3eaff; /* soft purple */
      color: #4a2c82;
      transform: rotateY(180deg);
      font-size: 16px;
      line-height: 1.4;
    }

    .hint {
      font-size: 13px;
      color: #7a5bbf;
      margin-bottom: 10px;
    }

    .controls {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      border: none;
      border-radius: 999px;
      padding: 9px 18px;
      font-size: 14px;
      cursor: pointer;
      box-shadow: 0 4px 12px rgba(111, 63, 155, 0.32);
    }

    #new-card-btn {
      background: #7e4cc9; /* purple */
      color: white;
    }

    #flip-back-btn {
      background: white;
      color: #7e4cc9;
      border: 1px solid #d4c4f5;
    }

    button:hover {
      opacity: 0.94;
    }

    .today-label {
      font-size: 14px;
      margin-bottom: 6px;
      color: #6a4db3;
    }
  </style>
</head>
<body>
  <div class="page">
    <h1>Mindfulness Card of the Day</h1>
    <p class="subtitle">
      Tap the purple card to reveal today's mindfulness message.
    </p>

    <p class="today-label" id="date-label"></p>

    <div class="card-wrapper">
      <div class="card" id="card">
        <div class="card-inner">
          <!-- front -->
          <div class="card-face card-front">
            <span class="emoji">💜</span>
            <div>Tap to draw</div>
            <div>your mindfulness card</div>
          </div>

          <!-- back -->
          <div class="card-face card-back">
            <div id="card-text">
              Breathe in… breathe out…<br />
              Tap “New card” when you're ready.
            </div>
          </div>
        </div>
      </div>
    </div>

    <p class="hint">
      Let this card guide your intention for today.
    </p>

    <div class="controls">
      <button id="new-card-btn">New random card</button>
      <button id="flip-back-btn">Close card</button>
    </div>
  </div>

  <script>
    const mindfulnessCards = [
  "หายใจเข้า–ออกช้า ๆ 3 ครั้ง\nTake 3 slow breaths and feel your body soften.",
  "มองรอบตัว แล้วบอก 3 สิ่งที่เห็น 2 สิ่งที่ได้ยิน และ 1 สิ่งที่รู้สึกได้\nName 3 things you see, 2 things you hear, 1 thing you feel.",
  "วางมือบนหัวใจ แล้วบอกตัวเองว่า “ตอนนี้ฉันปลอดภัย”\nPlace your hand on your heart and say: “I am safe in this moment.”",
  "หลับตา 10 วินาที แล้วฟังเสียงรอบตัวอย่างตั้งใจ\nClose your eyes for 10 seconds and listen like a curious explorer.",
  "คิดถึงใครสักคนที่คุณรู้สึกขอบคุณ\nThink of someone you’re grateful for today.",
  "ยืดแขนขึ้นฟ้าแล้วค่อย ๆ ปล่อยลงอย่างอ่อนโยน\nStretch your arms up, then release gently.",
  "วางเท้าบนพื้น แล้วจินตนาการว่ามีรากเติบโตลงดิน\nPlace your feet on the ground and imagine roots holding you steady.",
  "ยิ้มเบา ๆ แล้วสังเกตว่าร่างกายรู้สึกอย่างไร\nSmile softly and notice how your body feels.",
  "วางมือบนท้อง แล้วรู้สึกถึงการหายใจเข้า–ออก 5 ครั้ง\nPlace both hands on your belly and feel 5 breaths.",
  "พูดประโยคให้กำลังใจตัวเอง เช่น “ฉันกำลังทำดีที่สุดแล้ว”\nSay a kind sentence to yourself: “I am doing my best.”",
  "ฟังเสียงลมหายใจของตัวเองอย่างอ่อนโยน\nListen closely to the sound of your own breath.",
  "นั่งเงียบ ๆ แล้วสังเกตความรู้สึกในใจ โดยไม่ตัดสิน\nSit quietly and notice how you feel, without judgment.",
  "มองมือของตัวเอง แล้วขอบคุณที่มันช่วยคุณมามากมาย\nLook at your hands and thank them for all they do.",
  "กอดตัวเองเบา ๆ เพื่อปลอบโยนหัวใจ\nGive yourself a soft self-hug to comfort your heart.",
  "เขียนชื่อหนึ่งสิ่งที่ทำให้คุณยิ้มในวันนี้\nThink of one thing that made you smile today."
    ];

    const cardElement = document.getElementById("card");
    const cardTextElement = document.getElementById("card-text");
    const newCardBtn = document.getElementById("new-card-btn");
    const flipBackBtn = document.getElementById("flip-back-btn");

    let lastIndex = -1;
    let hasShownFirst = false;

    const dateLabel = document.getElementById("date-label");
    const today = new Date();
    dateLabel.textContent = today.toLocaleDateString(undefined, {
      weekday: "long",
      year: "numeric",
      month: "short",
      day: "numeric"
    });

    function drawCard() {
      let index;
      do {
        index = Math.floor(Math.random() * mindfulnessCards.length);
      } while (index === lastIndex);
      lastIndex = index;

      cardTextElement.innerHTML = mindfulnessCards[index].replace(/\n/g, "<br />");

      if (!cardElement.classList.contains("flipped")) {
        cardElement.classList.add("flipped");
      }
      hasShownFirst = true;
    }

    cardElement.addEventListener("click", () => {
      if (!hasShownFirst) drawCard();
      else cardElement.classList.toggle("flipped");
    });

    newCardBtn.addEventListener("click", drawCard);
    flipBackBtn.addEventListener("click", () => {
      cardElement.classList.remove("flipped");
    });
  </script>
</body>
</html>
