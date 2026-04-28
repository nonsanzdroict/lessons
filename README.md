[sorting-exercise.html](https://github.com/user-attachments/files/27178992/sorting-exercise.html)
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Распредели слова по категориям</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Nunito', sans-serif;
    background: #f0f4ff;
    min-height: 100vh;
    padding: 24px 16px;
    color: #1a1a2e;
  }

  h1 {
    text-align: center;
    font-size: 1.5rem;
    font-weight: 800;
    color: #3a3a6e;
    margin-bottom: 6px;
  }
  .subtitle {
    text-align: center;
    font-size: 0.9rem;
    color: #888;
    margin-bottom: 24px;
  }

  /* Word bank */
  .word-bank {
    background: white;
    border-radius: 16px;
    padding: 16px;
    margin-bottom: 20px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.07);
  }
  .word-bank-title {
    font-size: 0.8rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #aaa;
    margin-bottom: 12px;
  }
  .words-container {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    min-height: 40px;
  }

  .word-btn {
    background: #eef0ff;
    border: 2px solid #c5caf5;
    color: #3a3a8c;
    font-family: 'Nunito', sans-serif;
    font-size: 0.95rem;
    font-weight: 700;
    padding: 7px 16px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.15s ease;
    user-select: none;
  }
  .word-btn:hover {
    background: #d8dbff;
    transform: translateY(-2px);
    box-shadow: 0 4px 10px rgba(100,100,200,0.2);
  }
  .word-btn:active { transform: translateY(0); }
  .word-btn.placed { opacity: 0; pointer-events: none; }

  /* Categories */
  .categories {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
    margin-bottom: 20px;
  }
  @media(max-width: 480px) {
    .categories { grid-template-columns: 1fr; }
  }

  .category {
    background: white;
    border-radius: 16px;
    padding: 14px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.07);
    min-height: 140px;
    transition: box-shadow 0.2s;
  }
  .category.highlight {
    box-shadow: 0 0 0 3px rgba(100,149,237,0.4);
  }

  .category-header {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 10px;
    padding-bottom: 8px;
    border-bottom: 2px solid #f0f0f0;
  }
  .category-emoji { font-size: 1.4rem; }
  .category-name {
    font-size: 0.95rem;
    font-weight: 800;
    color: #333;
  }

  .drop-zone {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    min-height: 60px;
    border-radius: 10px;
    padding: 6px;
    transition: background 0.15s;
  }

  .placed-word {
    font-family: 'Nunito', sans-serif;
    font-size: 0.88rem;
    font-weight: 700;
    padding: 5px 12px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.15s;
    border: none;
  }
  .placed-word:hover { opacity: 0.75; }

  /* Colors per category */
  .cat-0 .placed-word { background: #ffd6d6; color: #c0392b; }
  .cat-1 .placed-word { background: #d6ffe8; color: #1e8449; }
  .cat-2 .placed-word { background: #fff3cd; color: #b7770d; }
  .cat-3 .placed-word { background: #d6eeff; color: #1a5276; }

  .cat-0 .category-header { border-color: #ffb3b3; }
  .cat-1 .category-header { border-color: #a3f0c0; }
  .cat-2 .category-header { border-color: #ffe89a; }
  .cat-3 .category-header { border-color: #a3d4ff; }

  /* Buttons */
  .btn-row {
    display: flex;
    gap: 10px;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }

  .btn-check {
    background: #4a6cf7;
    color: white;
    border: none;
    font-family: 'Nunito', sans-serif;
    font-size: 1rem;
    font-weight: 800;
    padding: 12px 28px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.15s;
    box-shadow: 0 4px 14px rgba(74,108,247,0.35);
  }
  .btn-check:hover { background: #3a5ce0; transform: translateY(-2px); }

  .btn-reset {
    background: white;
    color: #888;
    border: 2px solid #ddd;
    font-family: 'Nunito', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    padding: 12px 24px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .btn-reset:hover { border-color: #aaa; color: #555; }

  /* Feedback */
  .feedback {
    text-align: center;
    font-size: 1rem;
    font-weight: 700;
    padding: 12px 20px;
    border-radius: 12px;
    margin-bottom: 12px;
    display: none;
  }
  .feedback.show { display: block; }
  .feedback.correct { background: #d4edda; color: #1e7e34; }
  .feedback.wrong   { background: #fdecea; color: #c0392b; }

  /* Correct/wrong marks on placed words */
  .placed-word.correct-mark { outline: 2px solid #27ae60; }
  .placed-word.wrong-mark   { outline: 2px solid #e74c3c; text-decoration: line-through; }

  .score-badge {
    display: inline-block;
    background: #4a6cf7;
    color: white;
    border-radius: 50px;
    padding: 2px 12px;
    font-size: 1rem;
    font-weight: 800;
    margin-left: 6px;
  }
</style>
</head>
<body>

<h1>🗂 Распредели слова</h1>
<p class="subtitle">Нажми на слово — оно попадёт в нужную категорию. Можно убрать обратно.</p>

<div class="word-bank">
  <div class="word-bank-title">Слова для сортировки</div>
  <div class="words-container" id="wordBank"></div>
</div>

<div class="categories" id="categoriesGrid"></div>

<div class="btn-row">
  <button class="btn-check" onclick="checkAnswers()">✅ Проверить</button>
  <button class="btn-reset" onclick="resetAll()">🔄 Сначала</button>
</div>

<div class="feedback" id="feedback"></div>

<script>
// ============================================================
// ДАННЫЕ ЗАДАНИЯ — меняй здесь!
// ============================================================
const TASK = {
  categories: [
    { name: "Животные",   emoji: "🐾", words: ["кот","собака","слон","лиса","волк","медведь"] },
    { name: "Фрукты",     emoji: "🍎", words: ["яблоко","банан","груша","манго","слива","лимон"] },
    { name: "Цвета",      emoji: "🎨", words: ["красный","синий","жёлтый","зелёный","белый","чёрный"] },
    { name: "Транспорт",  emoji: "🚗", words: ["машина","автобус","самолёт","корабль","велосипед","поезд"] },
  ]
};
// ============================================================

let allWords = [];
let placed = {}; // word -> categoryIndex

function init() {
  // flatten & shuffle
  allWords = TASK.categories.flatMap(c => c.words);
  allWords = allWords.sort(() => Math.random() - 0.5);
  placed = {};
  render();
}

function render() {
  // Word bank
  const bank = document.getElementById('wordBank');
  bank.innerHTML = '';
  allWords.forEach(w => {
    const btn = document.createElement('button');
    btn.className = 'word-btn' + (placed[w] !== undefined ? ' placed' : '');
    btn.textContent = w;
    btn.onclick = () => handleWordClick(w, btn);
    bank.appendChild(btn);
  });

  // Categories
  const grid = document.getElementById('categoriesGrid');
  grid.innerHTML = '';
  TASK.categories.forEach((cat, ci) => {
    const div = document.createElement('div');
    div.className = `category cat-${ci}`;
    div.innerHTML = `
      <div class="category-header">
        <span class="category-emoji">${cat.emoji}</span>
        <span class="category-name">${cat.name}</span>
      </div>
      <div class="drop-zone" id="zone-${ci}"></div>
    `;
    grid.appendChild(div);
  });

  // Place words already placed
  Object.entries(placed).forEach(([w, ci]) => addToZone(w, ci, false));

  // Hide feedback
  const fb = document.getElementById('feedback');
  fb.className = 'feedback';
}

let pendingWord = null;
let pendingBtn = null;

function handleWordClick(word, btn) {
  if (placed[word] !== undefined) return; // already placed, do nothing (remove via zone)

  // highlight zones
  document.querySelectorAll('.category').forEach((el, i) => {
    el.style.cursor = 'pointer';
    el.classList.add('highlight');
    el.onclick = () => placeWord(word, i, btn);
  });

  pendingWord = word;
  pendingBtn = btn;
  btn.style.background = '#c5caf5';

  // click outside = cancel
  setTimeout(() => {
    document.addEventListener('click', cancelPending, { once: true });
  }, 10);
}

function cancelPending(e) {
  if (!e.target.closest('.category') && !e.target.closest('.word-btn')) {
    clearHighlights();
    if (pendingBtn) pendingBtn.style.background = '';
    pendingWord = null; pendingBtn = null;
  }
}

function placeWord(word, ci, btn) {
  placed[word] = ci;
  clearHighlights();
  btn.classList.add('placed');
  btn.style.background = '';
  addToZone(word, ci, true);
  pendingWord = null; pendingBtn = null;
  // reset feedback
  document.getElementById('feedback').className = 'feedback';
}

function addToZone(word, ci, animate) {
  const zone = document.getElementById(`zone-${ci}`);
  if (!zone) return;
  const span = document.createElement('button');
  span.className = 'placed-word';
  span.textContent = word;
  span.title = 'Нажми, чтобы вернуть';
  if (animate) span.style.animation = 'none';
  span.onclick = () => removeFromZone(word, ci, span);
  zone.appendChild(span);
}

function removeFromZone(word, ci, el) {
  delete placed[word];
  el.remove();
  // unhide in bank
  const bankBtns = document.querySelectorAll('.word-btn');
  bankBtns.forEach(b => { if (b.textContent === word) b.classList.remove('placed'); });
  document.getElementById('feedback').className = 'feedback';
}

function clearHighlights() {
  document.querySelectorAll('.category').forEach(el => {
    el.classList.remove('highlight');
    el.style.cursor = '';
    el.onclick = null;
  });
}

function checkAnswers() {
  let correct = 0, total = 0;

  // remove old marks
  document.querySelectorAll('.placed-word').forEach(el => {
    el.classList.remove('correct-mark','wrong-mark');
  });

  Object.entries(placed).forEach(([word, ci]) => {
    total++;
    const isCorrect = TASK.categories[ci].words.includes(word);
    // find the element
    const zone = document.getElementById(`zone-${ci}`);
    zone.querySelectorAll('.placed-word').forEach(el => {
      if (el.textContent === word) {
        el.classList.add(isCorrect ? 'correct-mark' : 'wrong-mark');
      }
    });
    if (isCorrect) correct++;
  });

  const fb = document.getElementById('feedback');
  const totalWords = allWords.length;

  if (total === 0) {
    fb.className = 'feedback wrong show';
    fb.innerHTML = 'Сначала распредели хотя бы несколько слов!';
    return;
  }

  if (correct === totalWords && total === totalWords) {
    fb.className = 'feedback correct show';
    fb.innerHTML = `🎉 Отлично! Все ${totalWords} слов на своих местах!`;
  } else {
    fb.className = 'feedback wrong show';
    fb.innerHTML = `Правильно: <span class="score-badge">${correct} / ${total}</span> — зачёркнутые слова стоят не там, попробуй ещё раз!`;
  }
}

function resetAll() {
  init();
}

init();
</script>
</body>
</html>
