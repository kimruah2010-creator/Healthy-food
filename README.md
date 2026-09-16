<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>편의점 영양 페어링 - NutriMatch</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    }
    body {
      background-color: #f1f3f5;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 16px;
    }
    .app-container {
      width: 100%;
      max-width: 380px;
      height: 640px;
      background: #ffffff;
      border-radius: 28px;
      box-shadow: 0 12px 32px rgba(0, 0, 0, 0.1);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      position: relative;
    }
    header {
      padding: 20px 20px 12px;
      text-align: center;
      border-bottom: 1px solid #f1f3f5;
    }
    header h1 {
      font-size: 18px;
      color: #198754;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
    }
    .screen {
      flex: 1;
      padding: 20px;
      display: none;
      flex-direction: column;
      justify-content: space-between;
    }
    .screen.active {
      display: flex;
    }

    /* 화면 1: 메인 선택 */
    .menu-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin-top: 16px;
    }
    .menu-btn {
      background: #f8f9fa;
      border: 2px solid #e9ecef;
      border-radius: 16px;
      padding: 20px 12px;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      transition: all 0.2s;
    }
    .menu-btn:hover {
      border-color: #198754;
      background: #e8f5e9;
    }
    .menu-btn .icon {
      font-size: 32px;
    }

    /* 화면 2: 카드 스와이프 */
    .card-area {
      flex: 1;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 16px 0;
    }
    .card {
      width: 100%;
      height: 320px;
      background: #ffffff;
      border-radius: 20px;
      border: 1px solid #e9ecef;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
      padding: 24px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      text-align: center;
      transition: transform 0.3s ease, opacity 0.3s ease;
    }
    .card .food-icon {
      font-size: 54px;
      margin: 12px 0;
    }
    .card h3 {
      font-size: 20px;
      color: #212529;
    }
    .tag {
      display: inline-block;
      align-self: center;
      background: #e8f5e9;
      color: #2e7d32;
      font-size: 13px;
      font-weight: 600;
      padding: 4px 10px;
      border-radius: 20px;
      margin-bottom: 8px;
    }
    .card-desc {
      font-size: 14px;
      color: #495057;
      line-height: 1.4;
    }
    .action-controls {
      display: flex;
      justify-content: center;
      gap: 32px;
      margin-bottom: 12px;
    }
    .action-btn {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      border: none;
      font-size: 24px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.1s;
    }
    .action-btn:active {
      transform: scale(0.92);
    }
    .btn-pass {
      background: #fff;
      color: #dc3545;
      border: 2px solid #ffc9c9;
    }
    .btn-pick {
      background: #198754;
      color: #fff;
    }

    /* 화면 3: 최종 결과 */
    .result-box {
      text-align: center;
      margin-top: 20px;
    }
    .score-circle {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      background: #e8f5e9;
      color: #198754;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      margin: 0 auto 16px;
    }
    .score-num {
      font-size: 32px;
      font-weight: 800;
    }
    .summary-card {
      background: #f8f9fa;
      border-radius: 14px;
      padding: 16px;
      text-align: left;
      font-size: 14px;
      line-height: 1.6;
      margin: 16px 0;
    }
    .summary-card strong {
      color: #212529;
    }
    .restart-btn {
      width: 100%;
      padding: 14px;
      background: #212529;
      color: white;
      border: none;
      border-radius: 12px;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>🥗 NutriMatch</h1>
    </header>

    <!-- STEP 1: 메인 메뉴 선택 -->
    <div id="step-select" class="screen active">
      <div>
        <h2 style="font-size: 18px; margin-bottom: 6px;">오늘 뭐 드시나요?</h2>
        <p style="font-size: 13px; color: #6c757d;">부족한 영양을 채워줄 꿀조합을 매칭해 드려요.</p>
        <div class="menu-grid">
          <button class="menu-btn" onclick="selectMain('ramen')">
            <span class="icon">🍜</span>
            국물 컵라면
          </button>
          <button class="menu-btn" onclick="selectMain('buldak')">
            <span class="icon">🌶️</span>
            매운 볶음면
          </button>
          <button class="menu-btn" onclick="selectMain('triangle')">
            <span class="icon">🍙</span>
            삼각김밥
          </button>
          <button class="menu-btn" onclick="selectMain('tteok')">
            <span class="icon">🍢</span>
            편의점 떡볶이
          </button>
        </div>
      </div>
      <p style="text-align: center; font-size: 12px; color: #adb5bd;">청소년 식생활 개선 프로젝트</p>
    </div>

    <!-- STEP 2: 페어링 카드 스와이프 매칭 -->
    <div id="step-swipe" class="screen">
      <div>
        <p id="main-notice" style="font-size: 13px; color: #6c757d; text-align: center;"></p>
      </div>

      <div class="card-area">
        <div id="swipe-card" class="card">
          <span id="card-tag" class="tag">#태그</span>
          <div id="card-icon" class="food-icon">🥚</div>
          <h3 id="card-name">음식 이름</h3>
          <p id="card-desc" class="card-desc">설명 텍스트</p>
        </div>
      </div>

      <div>
        <div class="action-controls">
          <button class="action-btn btn-pass" onclick="handleChoice(false)">✕</button>
          <button class="action-btn btn-pick" onclick="handleChoice(true)">♥</button>
        </div>
        <p style="text-align: center; font-size: 12px; color: #adb5bd;">패스는 ✕ | 마음에 들면 ♥</p>
      </div>
    </div>

    <!-- STEP 3: 최종 결과 화면 -->
    <div id="step-result" class="screen">
      <div class="result-box">
        <div class="score-circle">
          <span class="score-num" id="final-score">88</span>
          <span style="font-size: 11px;">밸런스 점수</span>
        </div>
        <h2 id="result-title" style="font-size: 18px; margin-bottom: 8px;">완벽한 한 끼 매칭!</h2>
        
        <div class="summary-card">
          <p id="result-items" style="margin-bottom: 6px;"></p>
          <hr style="border: 0; border-top: 1px dashed #dee2e6; margin: 8px 0;" />
          <p id="result-comment" style="color: #495057;"></p>
        </div>
      </div>

      <button class="restart-btn" onclick="resetApp()">다시 조합해보기</button>
    </div>
  </div>

  <script>
    // 영양 페어링 데이터베이스
    const pairingDB = {
      ramen: {
        name: "국물 컵라면",
        baseScore: 40,
        candidates: [
          { name: "바나나", icon: "🍌", tag: "#나트륨배출", desc: "풍부한 칼륨이 라면의 나트륨 배출을 유도해 붓기를 막아줍니다.", score: 25 },
          { name: "감동란(삶은달걀)", icon: "🥚", tag: "#단백질보충", desc: "면 탄수화물에 부족한 필수 단백질을 채워 집중력을 올려줍니다.", score: 25 },
          { name: "흰 우유(200ml)", icon: "🥛", tag: "#칼슘충전", desc: "라면의 짠 기운을 가라앉히고 뼈 성장에 필요한 칼슘을 공급합니다.", score: 20 }
        ]
      },
      buldak: {
        name: "매운 볶음면",
        baseScore: 35,
        candidates: [
          { name: "스트링 치즈", icon: "🧀", tag: "#위벽보호", desc: "지방과 단백질이 캡사이신을 감싸 위장 자극을 줄여줍니다.", score: 25 },
          { name: "플레인 두유", icon: "🧃", tag: "#혈당스파이크방지", desc: "식물성 단백질이 매운맛을 중화하고 포만감을 오래 지속시킵니다.", score: 25 },
          { name: "컵 샐러드", icon: "🥗", tag: "#식이섬유보충", desc: "장내 유해물질 흡착을 막고 부족한 비타민을 채워줍니다.", score: 25 }
        ]
      },
      triangle: {
        name: "삼각김밥",
        baseScore: 50,
        candidates: [
          { name: "닭가슴살 핫바", icon: "🍗", tag: "#고단백페어링", desc: "밥만으로 부족한 동물성 단백질을 깔끔하게 채워줍니다.", score: 25 },
          { name: "하루견과", icon: "🥜", tag: "#뇌에너지공급", desc: "불포화지방산이 집중력 저하를 막고 균형 잡힌 포만감을 줍니다.", score: 20 }
        ]
      },
      tteok: {
        name: "편의점 떡볶이",
        baseScore: 40,
        candidates: [
          { name: "훈제란 2구", icon: "🥚", tag: "#단백질밸런스", desc: "초고탄수화물 식단의 영양 흡수 속도를 늦춰줍니다.", score: 25 },
          { name: "보리차/옥수수차", icon: "🍵", tag: "#수분&나트륨배출", desc: "달고 짠 양념의 부담을 덜어주는 무가당 수분 보충.", score: 20 }
        ]
      }
    };

    let selectedMainKey = "";
    let currentCards = [];
    let currentIndex = 0;
    let selectedPartners = [];

    function selectMain(key) {
      selectedMainKey = key;
      const data = pairingDB[key];
      currentCards = data.candidates;
      currentIndex = 0;
      selectedPartners = [];

      document.getElementById("main-notice").innerText = `선택: [${data.name}]의 짝꿍 찾기`;
      showScreen("step-swipe");
      renderCard();
    }

    function renderCard() {
      if (currentIndex >= currentCards.length) {
        showResult();
        return;
      }
      const item = currentCards[currentIndex];
      const card = document.getElementById("swipe-card");
      
      card.style.transform = "none";
      card.style.opacity = "1";

      document.getElementById("card-tag").innerText = item.tag;
      document.getElementById("card-icon").innerText = item.icon;
      document.getElementById("card-name").innerText = item.name;
      document.getElementById("card-desc").innerText = item.desc;
    }

    function handleChoice(isPick) {
      const card = document.getElementById("swipe-card");
      
      // 넘기는 애니메이션 효과
      card.style.transform = isPick ? "translateX(120px) rotate(15deg)" : "translateX(-120px) rotate(-15deg)";
      card.style.opacity = "0";

      if (isPick) {
        selectedPartners.push(currentCards[currentIndex]);
      }

      setTimeout(() => {
        currentIndex++;
        renderCard();
      }, 250);
    }

    function showResult() {
      const main = pairingDB[selectedMainKey];
      let score = main.baseScore;
      selectedPartners.forEach(p => score += p.score);
      score = Math.min(score, 100);

      document.getElementById("final-score").innerText = score;
      
      const partnerNames = selectedPartners.length > 0 
        ? selectedPartners.map(p => p.name).join(", ") 
        : "선택 안 함 (영양 불균형 위험!)";

      document.getElementById("result-items").innerHTML = `<strong>최종 구성:</strong> ${main.name} + ${partnerNames}`;
      
      let comment = "";
      if (score >= 80) {
        comment = "💡 단백질과 미네랄이 이상적으로 채워진 훌륭한 밸런스 식단입니다!";
      } else if (score >= 60) {
        comment = "💡 무난한 식단이지만 과일이나 채소류를 하나 더 더하면 완벽해요.";
      } else {
        comment = "⚠️ 자극적인 탄수화물/나트륨 위주 식단입니다. 물을 많이 마시고 단백질을 챙겨주세요!";
      }
      document.getElementById("result-comment").innerText = comment;

      showScreen("step-result");
    }

    function showScreen(id) {
      document.querySelectorAll(".screen").forEach(s => s.classList.remove("active"));
      document.getElementById(id).classList.add("active");
    }

    function resetApp() {
      showScreen("step-select");
    }
  </script>
</body>
</html>
