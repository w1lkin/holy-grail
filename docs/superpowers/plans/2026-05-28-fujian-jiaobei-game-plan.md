# 福建圣杯掷筊小游戏 — 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建一款微信 HTML 掷筊小游戏——连续掷筊博连击、见好就收 STOP 结算、都市牛马风打工人自嘲美学。

**Architecture:** 单文件 HTML（内嵌 CSS + JS），零依赖。CSS 变量管理主题，Canvas 生成分享图，localStorage 存历史。触屏手势驱动核心交互。

**Tech Stack:** HTML5 + CSS3 + Vanilla JS，无框架，无构建工具。

---

### 文件结构

```
holy-grail/
└── index.html    # 全部代码
```

---

### Task 1: HTML 骨架 + CSS 主题变量 + 基础布局

**Files:**
- Create: `index.html`

- [ ] **Step 1: 编写 HTML 骨架和 CSS 主题变量**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>掷筊问卜 · 打工人的玄学时刻</title>
<style>
:root {
  --bg: #f5f5f0;
  --card: #ffffff;
  --text: #2c2c2c;
  --text-secondary: #888888;
  --excel-green: #217346;
  --coffee: #6f4e37;
  --approval-red: #c41e3a;
  --overtime-blue: #1d3557;
  --border: #e0dcd0;
  --gold: #d4a017;
  --shadow: 0 2px 8px rgba(0,0,0,0.08);
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: -apple-system, "PingFang SC", "Microsoft YaHei", sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
  -webkit-user-select: none;
}

.app-container {
  width: 100%;
  max-width: 420px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  padding: 16px;
}
</style>
</head>
<body>
<div class="app-container">
  <!-- 后续任务填充 -->
</div>
</body>
</html>
```

- [ ] **Step 2: 在浏览器中打开确认基础布局正常**

打开 `index.html`，确认页面白色背景、无报错。

---

### Task 2: 输入区域 + 筊杯展示区

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 在 `.app-container` 内添加输入区和筊杯展示区 HTML**

```html
<!-- 输入区 -->
<div class="input-section">
  <input type="text" class="question-input" placeholder="写下想问的事（不写也行）" maxlength="50">
</div>

<!-- 连击进度条 -->
<div class="combo-bar-section">
  <div class="combo-label">
    <span>当前连击</span>
    <span class="combo-count">x0</span>
  </div>
  <div class="combo-track">
    <div class="combo-fill" style="width: 0%"></div>
  </div>
  <div class="combo-markers">
    <span>0</span><span>3</span><span>6</span><span class="marker-bonus">⑨暴击</span>
  </div>
</div>

<!-- 筊杯展示区 -->
<div class="blocks-area" id="blocksArea">
  <div class="blocks-container" id="blocksContainer">
    <div class="block block-left" id="blockLeft"></div>
    <div class="block block-right" id="blockRight"></div>
  </div>
  <div class="swipe-hint">⬆️ 向上滑抛掷筊</div>
</div>

<!-- STOP 按钮 -->
<button class="stop-btn" id="stopBtn">⏹ STOP · 见好就收</button>
```

- [ ] **Step 2: 添加对应的 CSS 样式**

```css
.input-section { margin-bottom: 12px; }

.question-input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--border);
  border-radius: 8px;
  font-size: 15px;
  background: var(--card);
  outline: none;
  transition: border-color 0.2s;
}
.question-input:focus { border-color: var(--coffee); }
.question-input::placeholder { color: #ccc; }

.combo-bar-section { margin-bottom: 16px; }

.combo-label {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: var(--text-secondary);
  margin-bottom: 4px;
}
.combo-count { font-weight: bold; color: var(--approval-red); }

.combo-track {
  height: 6px;
  background: var(--border);
  border-radius: 3px;
  overflow: hidden;
}
.combo-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--excel-green), var(--gold), var(--approval-red));
  border-radius: 3px;
  transition: width 0.3s;
  width: 0%;
}

.combo-markers {
  display: flex;
  justify-content: space-between;
  font-size: 10px;
  color: #ccc;
  margin-top: 2px;
}
.marker-bonus { color: var(--approval-red); font-weight: bold; }

.blocks-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  min-height: 300px;
}

.blocks-container {
  display: flex;
  gap: 24px;
  position: relative;
}

.block {
  width: 48px;
  height: 72px;
  border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
  background: linear-gradient(135deg, #e8d5b0, #c4a87c);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  transition: transform 0.1s;
}

.swipe-hint {
  margin-top: 32px;
  color: #ccc;
  font-size: 13px;
  letter-spacing: 1px;
}

.stop-btn {
  width: 100%;
  padding: 14px;
  background: var(--card);
  border: 2px solid var(--approval-red);
  color: var(--approval-red);
  font-size: 16px;
  font-weight: bold;
  border-radius: 12px;
  cursor: pointer;
  margin-top: 8px;
  transition: all 0.2s;
}
.stop-btn:active { background: var(--approval-red); color: #fff; }
.stop-btn:disabled { opacity: 0.4; pointer-events: none; }
```

- [ ] **Step 3: 浏览器中确认布局**

打开 `index.html`，确认输入框、连击进度条、两个筊杯、提示文字、STOP 按钮均正确显示。

---

### Task 3: 上滑手势检测 + 掷杯动画

**Files:**
- Modify: `index.html` — 在 `</body>` 前添加 `<script>` 块

- [ ] **Step 1: 添加手势检测逻辑**

```js
<script>
(function() {
  const blocksArea = document.getElementById('blocksArea');
  const blockLeft = document.getElementById('blockLeft');
  const blockRight = document.getElementById('blockRight');
  const swipeHint = document.querySelector('.swipe-hint');

  let touchStartY = 0;
  let touchStartX = 0;
  let isAnimating = false;
  let comboCount = 0;

  blocksArea.addEventListener('touchstart', function(e) {
    if (isAnimating) return;
    const t = e.touches[0];
    touchStartY = t.clientY;
    touchStartX = t.clientX;
  }, { passive: true });

  blocksArea.addEventListener('touchend', function(e) {
    if (isAnimating) return;
    const t = e.changedTouches[0];
    const dy = touchStartY - t.clientY;
    const dx = Math.abs(touchStartX - t.clientX);

    if (dy > 60 && dy > dx * 1.5) {
      // 有效上滑
      throwBlocks();
    }
  });

  // 鼠标兼容（桌面调试用）
  let mouseDown = false, mouseStartY = 0, mouseStartX = 0;
  blocksArea.addEventListener('mousedown', function(e) {
    if (isAnimating) return;
    mouseDown = true;
    mouseStartY = e.clientY;
    mouseStartX = e.clientX;
  });
  document.addEventListener('mouseup', function(e) {
    if (!mouseDown || isAnimating) { mouseDown = false; return; }
    mouseDown = false;
    const dy = mouseStartY - e.clientY;
    const dx = Math.abs(mouseStartX - e.clientX);
    if (dy > 60 && dy > dx * 1.5) {
      throwBlocks();
    }
  });
})();
</script>
```

- [ ] **Step 2: 添加掷杯动画函数**

```js
function throwBlocks() {
  isAnimating = true;
  swipeHint.style.opacity = '0';

  // 随机决定结果
  const rand = Math.random();
  let result;
  if (rand < 0.68) result = 'shengbei';      // ~68%
  else if (rand < 0.85) result = 'xiaobei';  // ~17%
  else result = 'yinbei';                     // ~15%

  // 筊杯飞出动画
  blockLeft.style.transition = 'transform 0.8s cubic-bezier(0.25, 0.1, 0.25, 1), opacity 0.8s';
  blockRight.style.transition = 'transform 0.8s cubic-bezier(0.25, 0.1, 0.25, 1), opacity 0.8s';

  blockLeft.style.transform = 'translateY(-120px) rotateZ(720deg)';
  blockRight.style.transform = 'translateY(-120px) rotateZ(-720deg)';

  // 落地
  setTimeout(function() {
    blockLeft.style.transition = 'transform 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55)';
    blockRight.style.transition = 'transform 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55)';

    if (result === 'shengbei') {
      // 一正一反
      blockLeft.style.transform = 'translateY(0) rotateZ(0deg)';
      blockRight.style.transform = 'translateY(0) rotateZ(180deg)';
    } else if (result === 'xiaobei') {
      // 两平面朝上（两正）
      blockLeft.style.transform = 'translateY(0) rotateZ(0deg)';
      blockRight.style.transform = 'translateY(0) rotateZ(0deg)';
    } else {
      // 两弧面朝上（两反）
      blockLeft.style.transform = 'translateY(0) rotateZ(180deg)';
      blockRight.style.transform = 'translateY(0) rotateZ(180deg)';
    }

    setTimeout(function() {
      isAnimating = false;
      swipeHint.style.opacity = '1';
      handleResult(result);
    }, 500);
  }, 800);
}
```

- [ ] **Step 3: 浏览器中测试手势和动画**

在移动端/模拟器中打开，上滑测试筊杯能否正确翻转落地。

---

### Task 4: 结果判定 + 概率 + 连击计数

**Files:**
- Modify: `index.html` — 在 `<script>` 内添加

- [ ] **Step 1: 添加结果处理和连击逻辑**

```js
const comboCountEl = document.querySelector('.combo-count');
const comboFill = document.querySelector('.combo-fill');
let sessionResults = [];  // 本次掷杯全部结果

function handleResult(result) {
  sessionResults.push(result);

  if (result === 'shengbei') {
    comboCount++;
    updateComboUI();
  } else {
    comboCount = 0;
    updateComboUI();
  }
}

function updateComboUI() {
  comboCountEl.textContent = 'x' + comboCount;
  comboCountEl.style.color = comboCount >= 3 ? 'var(--approval-red)' : 'var(--text-secondary)';
  comboFill.style.width = Math.min(comboCount / 9 * 100, 100) + '%';

  if (comboCount >= 9) {
    triggerCriticalHit();
  }
}
```

- [ ] **Step 2: 添加九连暴击触发**

```js
function triggerCriticalHit() {
  // 先更新 UI
  comboCountEl.textContent = '⑨暴击!!';
  comboFill.style.width = '100%';
  comboFill.style.background = 'var(--approval-red)';

  // 禁用继续掷杯
  isAnimating = true;
  swipeHint.style.display = 'none';

  // 全屏特效（Task 9 详细实现）
  showCriticalEffect();

  // 强制结算
  setTimeout(function() {
    settleAndSave();
  }, 2000);
}
```

- [ ] **Step 3: 浏览器中测试连击计数**

连续掷杯测试：圣杯是否+1、笑杯/阴杯是否归零、9连是否触发暴击。

---

### Task 5: 连击特效系统

**Files:**
- Modify: `index.html` — 在 `<script>` 和 `<style>` 内添加

- [ ] **Step 1: 添加 CSS 特效关键帧**

```css
@keyframes glowPulse {
  0%, 100% { box-shadow: 0 0 8px rgba(33,115,70,0.4); }
  50% { box-shadow: 0 0 20px rgba(33,115,70,0.8); }
}
@keyframes stampHit {
  0% { transform: translateY(0) scale(2); opacity: 0; }
  30% { transform: translateY(0) scale(1); opacity: 1; }
  100% { transform: translateY(0) scale(1); opacity: 0.8; }
}
@keyframes floatUp {
  0% { transform: translateY(0) rotate(0deg); opacity: 1; }
  100% { transform: translateY(-200px) rotate(360deg); opacity: 0; }
}
@keyframes screenShake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}

.block-combo-1 { animation: glowPulse 1s ease-in-out infinite; }
.block-combo-3 {
  background: linear-gradient(135deg, #217346, #1a5c38) !important;
  box-shadow: 0 0 24px rgba(33,115,70,0.6) !important;
}
.block-combo-6 {
  background: linear-gradient(135deg, #d4a017, #ffd700) !important;
  box-shadow: 0 0 32px rgba(212,160,23,0.7) !important;
}

.shake { animation: screenShake 0.3s ease-in-out 3; }
```

- [ ] **Step 2: 修改 updateComboUI 添加特效切换**

```js
function updateComboUI() {
  comboCountEl.textContent = 'x' + comboCount;
  comboFill.style.width = Math.min(comboCount / 9 * 100, 100) + '%';

  // 重置所有特效 class
  blockLeft.className = 'block block-left';
  blockRight.className = 'block block-right';

  if (comboCount >= 9) {
    triggerCriticalHit();
    return;
  }

  if (comboCount >= 6) {
    blockLeft.classList.add('block-combo-6');
    blockRight.classList.add('block-combo-6');
    document.querySelector('.app-container').classList.add('shake');
    setTimeout(function() {
      document.querySelector('.app-container').classList.remove('shake');
    }, 1000);
  } else if (comboCount >= 3) {
    blockLeft.classList.add('block-combo-3');
    blockRight.classList.add('block-combo-3');
  } else if (comboCount >= 1) {
    blockLeft.classList.add('block-combo-1');
    blockRight.classList.add('block-combo-1');
  }

  // 粒子特效
  if (comboCount >= 3) {
    spawnParticles(comboCount >= 6 ? 16 : 8);
  }
}

function spawnParticles(count) {
  const area = document.getElementById('blocksArea');
  for (let i = 0; i < count; i++) {
    const p = document.createElement('div');
    p.style.cssText = `
      position: absolute;
      width: 8px; height: 8px;
      background: ${i % 2 === 0 ? 'var(--gold)' : 'var(--excel-green)'};
      border-radius: 2px;
      left: ${40 + Math.random() * 20}%;
      top: 50%;
      pointer-events: none;
      animation: floatUp ${0.8 + Math.random() * 0.8}s ease-out forwards;
      animation-delay: ${Math.random() * 0.2}s;
    `;
    area.appendChild(p);
    setTimeout(function() { p.remove(); }, 2000);
  }
}
```

- [ ] **Step 3: 浏览器测试连击特效**

掷出连续圣杯，确认各档特效正确触发、笑杯归零后特效重置。

---

### Task 6: STOP 按钮 + 结算面板 + 解读

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 添加结算面板 HTML（放在筊杯区之后）**

```html
<div class="settle-panel" id="settlePanel" style="display:none;">
  <div class="settle-card">
    <div class="settle-tier" id="settleTier"></div>
    <div class="settle-stats" id="settleStats"></div>
    <div class="settle-interpretation" id="settleInterpretation"></div>
    <div class="settle-date" id="settleDate"></div>
    <div class="settle-actions">
      <button class="btn-share" id="btnShare">📤 分享战绩</button>
      <button class="btn-retry" id="btnRetry">🔄 再来一局</button>
    </div>
  </div>
</div>
```

- [ ] **Step 2: 添加结算面板 CSS**

```css
.settle-panel {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 24px;
}
.settle-card {
  background: var(--card);
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 340px;
  text-align: center;
  box-shadow: 0 8px 32px rgba(0,0,0,0.2);
}
.settle-tier { font-size: 28px; font-weight: bold; margin-bottom: 12px; }
.settle-stats { font-size: 13px; color: var(--text-secondary); margin-bottom: 16px; line-height: 1.6; }
.settle-interpretation {
  font-size: 15px;
  line-height: 1.7;
  padding: 16px;
  background: #fdf8f0;
  border-radius: 10px;
  margin-bottom: 12px;
  border-left: 3px solid var(--coffee);
}
.settle-date { font-size: 11px; color: #ccc; margin-bottom: 16px; }
.settle-actions { display: flex; flex-direction: column; gap: 8px; }
.btn-share, .btn-retry {
  width: 100%;
  padding: 12px;
  border-radius: 10px;
  font-size: 15px;
  font-weight: bold;
  border: none;
  cursor: pointer;
}
.btn-share { background: var(--overtime-blue); color: #fff; }
.btn-retry { background: var(--border); color: var(--text); }
```

- [ ] **Step 3: 添加结算逻辑和解读文本池**

```js
const interpretations = {
  daji: {
    tier: '🏆 带薪年假',
    text: '九天连圣！老板看了都沉默，今日宜提涨薪、宜早退、宜一切。你的运势已经超过了99%的打工人。'
  },
  ji: {
    tier: '☕ 宜摸鱼',
    texts: [
      '运势走高，今日提案过审概率大。建议趁机提需求，领导心情好。',
      '神明比了个OK，今天适合做决定。那个攒了很久的请假理由，可以用了。',
      '圣杯频出，今日顺风顺水。项目推进无障碍，下午茶也该归你了。'
    ]
  },
  ping: {
    tier: '📋 需求评审中',
    texts: [
      '运势平平，需求可能会改。建议原地待命，不要冲动消费或裸辞。',
      '诸神在开会，暂无明确指示。今天适合低调摸鱼，不适合主动出击。',
      '圣杯和阴杯五五开，说明老天也在犹豫。再观望一下吧。'
    ]
  },
  xiong: {
    tier: '⚠️ 加班预警',
    texts: [
      '今日诸事不宜！会议改期、代码回滚、下班早点走。别跟老板对视。',
      '阴杯连连，今天大概率要背锅。建议保持沉默，能躲就躲。',
      '神明摇头了。下班关手机，明天再战。这不是你的问题，是运势的问题。'
    ]
  }
};

function settleAndSave() {
  const totalThrows = sessionResults.length;
  const shengbeiCount = sessionResults.filter(function(r) { return r === 'shengbei'; }).length;
  const ratio = totalThrows > 0 ? shengbeiCount / totalThrows : 0;

  let tier, interpretation, tierKey;
  if (comboCount >= 9) {
    tierKey = 'daji';
    tier = interpretations.daji.tier;
    interpretation = interpretations.daji.text;
  } else if (ratio >= 0.7) {
    tierKey = 'ji';
    tier = interpretations.ji.tier;
    interpretation = interpretations.ji.texts[Math.floor(Math.random() * interpretations.ji.texts.length)];
  } else if (ratio >= 0.4) {
    tierKey = 'ping';
    tier = interpretations.ping.tier;
    interpretation = interpretations.ping.texts[Math.floor(Math.random() * interpretations.ping.texts.length)];
  } else {
    tierKey = 'xiong';
    tier = interpretations.xiong.tier;
    interpretation = interpretations.xiong.texts[Math.floor(Math.random() * interpretations.xiong.texts.length)];
  }

  // 保存到 localStorage
  const record = {
    id: Date.now(),
    date: new Date().toLocaleString('zh-CN'),
    question: document.querySelector('.question-input').value || '(未填写)',
    results: sessionResults.slice(),
    maxCombo: Math.max(comboCount, ...(function() {
      let max = 0, cur = 0;
      sessionResults.forEach(function(r) {
        if (r === 'shengbei') { cur++; max = Math.max(max, cur); }
        else { cur = 0; }
      });
      return [max];
    })()),
    totalThrows: totalThrows,
    shengbeiCount: shengbeiCount,
    tier: tierKey,
    interpretation: interpretation
  };
  saveRecord(record);

  // 展示结算面板
  document.getElementById('settleTier').textContent = tier;
  document.getElementById('settleStats').textContent =
    '总掷杯 ' + totalThrows + ' 次 | 圣杯 ' + shengbeiCount + ' 次 | 最高连击 ' + record.maxCombo;
  document.getElementById('settleInterpretation').textContent = '「' + interpretation + '」';
  document.getElementById('settleDate').textContent = record.date;
  document.getElementById('settlePanel').style.display = 'flex';

  // 隐藏游戏区
  document.getElementById('stopBtn').style.display = 'none';
}

// STOP 按钮
document.getElementById('stopBtn').addEventListener('click', function() {
  if (sessionResults.length === 0) return;
  settleAndSave();
});
```

- [ ] **Step 4: 添加"再来一局"重置逻辑**

```js
document.getElementById('btnRetry').addEventListener('click', function() {
  sessionResults = [];
  comboCount = 0;
  isAnimating = false;
  updateComboUI();
  blockLeft.style.transform = 'translateY(0) rotateZ(0deg)';
  blockRight.style.transform = 'translateY(0) rotateZ(0deg)';
  blockLeft.className = 'block block-left';
  blockRight.className = 'block block-right';
  swipeHint.style.display = '';
  document.getElementById('settlePanel').style.display = 'none';
  document.getElementById('stopBtn').style.display = '';
  document.querySelector('.app-container').classList.remove('shake');
});
```

- [ ] **Step 5: 浏览器测试完整流程**

掷杯若干次 → 点 STOP → 结算面板弹出 → 确认解读正确 → 点再来一局重置。

---

### Task 7: localStorage 保存/读取 + 历史面板

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 添加保存/读取函数**

```js
const STORAGE_KEY = 'jiaobei_records';

function saveRecord(record) {
  let records = [];
  try {
    records = JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
  } catch(e) {
    records = [];
  }
  records.unshift(record);
  if (records.length > 50) { records = records.slice(0, 50); }
  localStorage.setItem(STORAGE_KEY, JSON.stringify(records));
}

function loadRecords() {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
  } catch(e) {
    return [];
  }
}
```

- [ ] **Step 2: 添加历史记录面板 HTML（在结算面板内）**

在结算面板的 `.settle-card` 中，操作按钮前添加：

```html
<div class="history-section" id="historySection">
  <div class="history-title">📋 历史战绩</div>
  <div class="history-list" id="historyList"></div>
</div>
```

- [ ] **Step 3: 添加历史面板 CSS**

```css
.history-section { margin-top: 16px; border-top: 1px solid var(--border); padding-top: 12px; }
.history-title { font-size: 13px; font-weight: bold; margin-bottom: 8px; text-align: left; }
.history-list { max-height: 240px; overflow-y: auto; }
.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  background: #fafaf5;
  border-radius: 8px;
  margin-bottom: 6px;
  font-size: 12px;
  cursor: pointer;
}
.history-item-left { text-align: left; }
.history-item-date { color: #bbb; font-size: 10px; }
.history-item-question { color: var(--text); max-width: 140px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.history-item-tier { font-size: 13px; }
```

- [ ] **Step 4: 渲染历史列表**

```js
function renderHistory() {
  const records = loadRecords();
  const list = document.getElementById('historyList');
  if (records.length === 0) {
    list.innerHTML = '<div style="color:#ccc;text-align:center;padding:16px;">暂无记录，快去掷一局吧</div>';
    return;
  }
  list.innerHTML = records.slice(0, 20).map(function(r) {
    return '<div class="history-item">' +
      '<div class="history-item-left">' +
        '<div class="history-item-question">' + escHtml(r.question) + '</div>' +
        '<div class="history-item-date">' + r.date + '</div>' +
      '</div>' +
      '<div class="history-item-tier">' +
        (r.tier === 'daji' ? '🏆' : r.tier === 'ji' ? '☕' : r.tier === 'ping' ? '📋' : '⚠️') +
      '</div>' +
    '</div>';
  }).join('');
}

function escHtml(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}
```

在 `settleAndSave` 末尾调用 `renderHistory()`。

---

### Task 8: Canvas 分享图生成

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 添加分享按钮逻辑**

```js
document.getElementById('btnShare').addEventListener('click', function() {
  generateShareImage();
});

function generateShareImage() {
  const record = loadRecords()[0]; // 最新一条
  if (!record) return;

  const canvas = document.createElement('canvas');
  canvas.width = 600;
  canvas.height = 800;
  const ctx = canvas.getContext('2d');

  // 背景 — 便签纸风格
  ctx.fillStyle = '#fdf8e8';
  ctx.fillRect(0, 0, 600, 800);

  // 便签纸纹理线
  ctx.strokeStyle = 'rgba(0,0,0,0.05)';
  for (let y = 0; y < 800; y += 30) {
    ctx.beginPath();
    ctx.moveTo(20, y);
    ctx.lineTo(580, y);
    ctx.stroke();
  }

  // 标题
  ctx.fillStyle = '#2c2c2c';
  ctx.font = 'bold 36px "PingFang SC","Microsoft YaHei",sans-serif';
  ctx.textAlign = 'center';
  ctx.fillText('打工人的玄学时刻', 300, 80);

  // 问题
  ctx.fillStyle = '#888';
  ctx.font = '20px "PingFang SC","Microsoft YaHei",sans-serif';
  ctx.fillText('问：' + record.question, 300, 130);

  // 结果图标
  const tierEmoji = record.tier === 'daji' ? '🏆' : record.tier === 'ji' ? '☕' : record.tier === 'ping' ? '📋' : '⚠️';
  ctx.font = '80px sans-serif';
  ctx.fillText(tierEmoji, 300, 240);

  // 档位标签
  ctx.fillStyle = '#2c2c2c';
  ctx.font = 'bold 32px "PingFang SC","Microsoft YaHei",sans-serif';
  const tierLabel = record.tier === 'daji' ? '带薪年假' : record.tier === 'ji' ? '宜摸鱼' : record.tier === 'ping' ? '需求评审中' : '加班预警';
  ctx.fillText(tierLabel, 300, 300);

  // 战绩数据
  ctx.fillStyle = '#666';
  ctx.font = '18px "PingFang SC","Microsoft YaHei",sans-serif';
  ctx.fillText('总掷杯 ' + record.totalThrows + ' 次 | 圣杯 ' + record.shengbeiCount + ' 次 | 最高连击 ' + record.maxCombo, 300, 360);

  // 解读
  ctx.fillStyle = '#2c2c2c';
  ctx.font = '22px "PingFang SC","Microsoft YaHei",sans-serif';
  const lines = wrapText(ctx, '「' + record.interpretation + '」', 500);
  lines.forEach(function(line, i) {
    ctx.fillText(line, 300, 440 + i * 36);
  });

  // 日期
  ctx.fillStyle = '#ccc';
  ctx.font = '14px "PingFang SC","Microsoft YaHei",sans-serif';
  ctx.fillText(record.date, 300, 600);

  // QR 占位区
  ctx.strokeStyle = '#ddd';
  ctx.strokeRect(230, 650, 140, 140);
  ctx.fillStyle = '#ddd';
  ctx.font = '12px "PingFang SC","Microsoft YaHei",sans-serif';
  ctx.fillText('扫码来玩', 300, 730);

  // 展示图片
  const img = document.createElement('img');
  img.src = canvas.toDataURL('image/png');
  img.style.cssText = 'max-width:100%;border-radius:12px;margin-top:12px;';

  // 插入到结算面板
  const card = document.querySelector('.settle-card');
  const existing = card.querySelector('.share-preview');
  if (existing) existing.remove();
  img.className = 'share-preview';
  card.insertBefore(img, card.querySelector('.settle-actions'));
}

function wrapText(ctx, text, maxWidth) {
  const lines = [];
  let current = '';
  for (let i = 0; i < text.length; i++) {
    const test = current + text[i];
    if (ctx.measureText(test).width > maxWidth && current.length > 0) {
      lines.push(current);
      current = text[i];
    } else {
      current = test;
    }
  }
  if (current) lines.push(current);
  return lines;
}
```

- [ ] **Step 2: 浏览器中测试分享图**

完整掷一局 → 结算 → 点分享战绩 → 确认 Canvas 图片正确渲染。

---

### Task 9: 九连暴击全屏特效

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 添加暴击 CSS 动画**

```css
@keyframes criticalBg {
  0% { background: var(--bg); }
  50% { background: #fffbe6; }
  100% { background: #ffe066; }
}
@keyframes coinFall {
  0% { transform: translateY(-100vh) rotate(0deg); opacity: 1; }
  100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
}
@keyframes promoText {
  0% { transform: scale(0); opacity: 0; }
  50% { transform: scale(1.2); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}

.critical-bg { animation: criticalBg 0.5s ease-in-out 4; }

.critical-overlay {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 200;
}
```

- [ ] **Step 2: 添加暴击特效函数**

```js
function showCriticalEffect() {
  // 背景闪烁
  document.body.classList.add('critical-bg');
  setTimeout(function() {
    document.body.classList.remove('critical-bg');
  }, 2000);

  // 金币雨粒子
  const overlay = document.createElement('div');
  overlay.className = 'critical-overlay';
  document.body.appendChild(overlay);

  for (let i = 0; i < 40; i++) {
    const coin = document.createElement('div');
    coin.textContent = ['💰','🪙','✨','🌟','💎'][Math.floor(Math.random() * 5)];
    coin.style.cssText = `
      position: absolute;
      left: ${Math.random() * 100}%;
      font-size: ${20 + Math.random() * 40}px;
      animation: coinFall ${1 + Math.random() * 2}s linear forwards;
      animation-delay: ${Math.random() * 0.5}s;
    `;
    overlay.appendChild(coin);
  }

  // 居中大字
  const promo = document.createElement('div');
  promo.style.cssText = `
    position: fixed;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 36px;
    font-weight: bold;
    color: var(--approval-red);
    text-shadow: 0 0 20px rgba(196,30,58,0.5);
    z-index: 201;
    animation: promoText 0.6s cubic-bezier(0.68, -0.55, 0.27, 1.55) forwards;
  `;
  promo.textContent = '🌟 升职加薪！！🌟';
  document.body.appendChild(promo);

  setTimeout(function() {
    overlay.remove();
    promo.remove();
  }, 3000);
}
```

- [ ] **Step 2: 浏览器中测试 9 连暴击**（可临时调高圣杯概率至 100% 方便测试）

---

### Task 10: 移动端适配 + 微信浏览器兼容 + 收尾

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 添加 viewport 和兼容 meta**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="format-detection" content="telephone=no">
```

- [ ] **Step 2: 添加 iOS 安全区域适配**

```css
.app-container {
  padding-bottom: env(safe-area-inset-bottom, 16px);
}
```

- [ ] **Step 3: 防止微信内下拉回弹**

```js
document.body.addEventListener('touchmove', function(e) {
  if (e.target.closest('.history-list') || e.target.closest('.question-input')) return;
  e.preventDefault();
}, { passive: false });
```

- [ ] **Step 4: 最终全流程测试**

测试清单：
- [ ] 输入问题 → 掷杯 → 圣杯连击特效 → STOP 结算 → 解读展示 → 历史记录 → 分享图 → 再来一局
- [ ] 笑杯/阴杯归零连击
- [ ] 9连暴击强制停止
- [ ] 微信内置浏览器兼容
- [ ] Safari/Chrome 移动端兼容
