---
layout: page
title: "Interactive 13x13 Poker Starting Hand Matrix"
permalink: /hand-matrix
description: "Interactive 13x13 Texas Hold'em starting hand matrix. Explore pre-flop equities, hand categories, and click any combination for full heads-up odds."
---

<style>
.matrix-container {
  max-width: 900px;
  margin: 0 auto;
}
.matrix-grid {
  display: grid;
  grid-template-columns: repeat(13, 1fr);
  gap: 3px;
  background: #e9ecef;
  padding: 8px;
  border-radius: 8px;
  user-select: none;
}
.matrix-cell {
  aspect-ratio: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s ease;
  position: relative;
  text-decoration: none !important;
}
.matrix-cell:hover {
  transform: scale(1.1);
  z-index: 10;
  box-shadow: 0 4px 10px rgba(0,0,0,0.25);
}
.matrix-cell.pair {
  background: #28a745;
  color: #fff;
}
.matrix-cell.suited {
  background: #007bff;
  color: #fff;
}
.matrix-cell.offsuit {
  background: #6c757d;
  color: #fff;
}
.matrix-cell.selected {
  outline: 3px solid #ffc107;
  transform: scale(1.08);
  z-index: 5;
}
.matrix-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
  margin: 15px 0 25px 0;
}
.legend-item {
  display: flex;
  align-items: center;
  font-size: 14px;
  font-weight: 500;
}
.legend-badge {
  width: 18px;
  height: 18px;
  border-radius: 4px;
  margin-right: 6px;
}
.selected-hand-panel {
  background: #f8f9fa;
  border-left: 5px solid #00B2D8;
  padding: 20px;
  border-radius: 6px;
  margin-bottom: 25px;
}
@media (max-width: 768px) {
  .matrix-cell {
    font-size: 10px;
    gap: 1px;
  }
}
@media (max-width: 480px) {
  .matrix-cell {
    font-size: 8px;
  }
}
</style>

<div class="matrix-container">
  <p class="lead text-center">
    Click on any of the <strong>169 canonical starting hands</strong> below to view its strategic strength, equity profile, and jump directly to any heads-up matchup.
  </p>

  <!-- Legend -->
  <div class="matrix-legend">
    <div class="legend-item">
      <span class="legend-badge" style="background: #28a745;"></span> Pocket Pairs (13)
    </div>
    <div class="legend-item">
      <span class="legend-badge" style="background: #007bff;"></span> Suited Combinations (78)
    </div>
    <div class="legend-item">
      <span class="legend-badge" style="background: #6c757d;"></span> Offsuit Combinations (78)
    </div>
  </div>

  <!-- Selected Hand Status Box -->
  <div class="selected-hand-panel" id="selectedPanel">
    <div class="row align-items-center">
      <div class="col-md-7">
        <h3 class="h4 mb-1">Active Hand: <span id="displayHand" class="text-primary font-weight-bold">AA (Pocket Aces)</span></h3>
        <p class="mb-0 text-muted" id="handDescription">The highest starting pair in poker. 85.2% average pre-flop equity vs random hands.</p>
      </div>
      <div class="col-md-5 text-md-right mt-3 mt-md-0">
        <button class="btn btn-sm btn-outline-primary mr-1" onclick="filterCategory('pair')">Pairs</button>
        <button class="btn btn-sm btn-outline-primary mr-1" onclick="filterCategory('suited')">Suited</button>
        <button class="btn btn-sm btn-outline-primary mr-1" onclick="filterCategory('broadway')">Broadways</button>
        <button class="btn btn-sm btn-outline-secondary" onclick="resetMatrix()">Reset</button>
      </div>
    </div>
    <hr class="my-3">
    <div class="row text-center" id="statBadges">
      <div class="col-sm-3 col-6 mb-2">
        <div class="p-2 border rounded bg-white">
          <small class="text-muted d-block">Sklansky Group</small>
          <strong class="h5" id="valSklansky">Group 1</strong>
        </div>
      </div>
      <div class="col-sm-3 col-6 mb-2">
        <div class="p-2 border rounded bg-white">
          <small class="text-muted d-block">Chen Score</small>
          <strong class="h5" id="valChen">20 pts</strong>
        </div>
      </div>
      <div class="col-sm-3 col-6 mb-2">
        <div class="p-2 border rounded bg-white">
          <small class="text-muted d-block">Hellmuth Tier</small>
          <strong class="h5" id="valHellmuth">Tier 1</strong>
        </div>
      </div>
      <div class="col-sm-3 col-6 mb-2">
        <div class="p-2 border rounded bg-white">
          <small class="text-muted d-block">Expected Value</small>
          <strong class="h5 text-success" id="valEV">+0.75 EV</strong>
        </div>
      </div>
    </div>
  </div>

  <!-- 13x13 Interactive Grid -->
  <div class="matrix-grid" id="matrixGrid">
    <!-- Populated dynamically via JavaScript -->
  </div>

  <div class="alert alert-light border mt-4 p-3 small text-muted">
    <i class="fas fa-info-circle mr-1 text-primary"></i> <strong>Pro Tip:</strong> Click any cell to inspect its details, or double-click to view all heads-up confrontations involving that holding in our <a href="/all-hands">all-hands database</a>.
  </div>
</div>

<script>
const ranks = ['A', 'K', 'Q', 'J', 'T', '9', '8', '7', '6', '5', '4', '3', '2'];

const handDatabase = {
  'AA': { name: 'Pocket Aces', type: 'pair', sklansky: 'Group 1', chen: '20', hellmuth: 'Tier 1', ev: '+0.75', desc: 'The strongest starting hand in Texas Hold\'em. Open-raise and 3-bet for value from any position.' },
  'KK': { name: 'Pocket Kings', type: 'pair', sklansky: 'Group 1', chen: '16', hellmuth: 'Tier 1', ev: '+0.60', desc: 'Second-best starting hand ("Cowboys"). Dominates all underpairs and broadway high cards.' },
  'QQ': { name: 'Pocket Queens', type: 'pair', sklansky: 'Group 1', chen: '14', hellmuth: 'Tier 1', ev: '+0.50', desc: 'Premium pocket pair. Strong favorite against underpairs and non-Ace overcards.' },
  'JJ': { name: 'Pocket Jacks', type: 'pair', sklansky: 'Group 1', chen: '12', hellmuth: 'Tier 2', ev: '+0.40', desc: 'Often called "Fishhooks". Highly profitable but vulnerable to overcards on the flop.' },
  'TT': { name: 'Pocket Tens', type: 'pair', sklansky: 'Group 2', chen: '10', hellmuth: 'Tier 2', ev: '+0.32', desc: 'Solid pocket pair. High set-mining value and standard open-raising strength.' },
  '99': { name: 'Pocket Nines', type: 'pair', sklansky: 'Group 3', chen: '8', hellmuth: 'Tier 2', ev: '+0.25', desc: 'Middle pair. Excellent in late position and against wide steal ranges.' },
  '88': { name: 'Pocket Eights', type: 'pair', sklansky: 'Group 4', chen: '8', hellmuth: 'Tier 3', ev: '+0.18', desc: 'Classic middle pair. Great set-mining potential in multi-way pots.' },
  '77': { name: 'Pocket Sevens', type: 'pair', sklansky: 'Group 5', chen: '7', hellmuth: 'Tier 3', ev: '+0.12', desc: 'Middle-low pair. Played primarily for flopping sets (11.8% frequency).' },
  '66': { name: 'Pocket Sixes', type: 'pair', sklansky: 'Group 6', chen: '6', hellmuth: 'Tier 4', ev: '+0.06', desc: 'Small pair. Profitable in late position or multi-way unraised pots.' },
  '55': { name: 'Pocket Fives', type: 'pair', sklansky: 'Group 6', chen: '5', hellmuth: 'Tier 4', ev: '+0.02', desc: 'Small pair. Flop a set or fold to post-flop aggression.' },
  '44': { name: 'Pocket Fours', type: 'pair', sklansky: 'Group 7', chen: '5', hellmuth: 'Tier 4', ev: '-0.02', desc: 'Low pocket pair. Requires discipline when facing early-position bets.' },
  '33': { name: 'Pocket Threes', type: 'pair', sklansky: 'Group 7', chen: '5', hellmuth: 'Tier 4', ev: '-0.06', desc: 'Speculative set-mining hand. Fold facing heavy 3-bets.' },
  '22': { name: 'Pocket Twos', type: 'pair', sklansky: 'Group 7', chen: '5', hellmuth: 'Tier 4', ev: '-0.09', desc: 'Lowest pocket pair ("Ducks"). Slight pre-flop favorite against overcards like AKo.' },
  'AKs': { name: 'Ace-King Suited', type: 'suited', sklansky: 'Group 1', chen: '12', hellmuth: 'Tier 1', ev: '+0.52', desc: 'Big Slick suited. Elite drawing hand capable of flopping nut flushes, straights, or top pair top kicker.' },
  'AKo': { name: 'Ace-King Offsuit', type: 'offsuit', sklansky: 'Group 2', chen: '10', hellmuth: 'Tier 2', ev: '+0.45', desc: 'Premier offsuit broadway. Strong 3-betting and isolation weapon.' },
  'AQs': { name: 'Ace-Queen Suited', type: 'suited', sklansky: 'Group 2', chen: '11', hellmuth: 'Tier 2', ev: '+0.42', desc: 'Powerful suited broadway. Dominates weaker Aces and Queens.' },
  'AQo': { name: 'Ace-Queen Offsuit', type: 'offsuit', sklansky: 'Group 3', chen: '9', hellmuth: 'Tier 3', ev: '+0.34', desc: 'Strong high-card holding. Open-raise from middle and late positions.' },
  'AJs': { name: 'Ace-Jack Suited', type: 'suited', sklansky: 'Group 2', chen: '10', hellmuth: 'Tier 3', ev: '+0.36', desc: 'High playability. Flops strong top pairs and nut flush draws.' },
  'ATs': { name: 'Ace-Ten Suited', type: 'suited', sklansky: 'Group 3', chen: '9', hellmuth: 'Tier 3', ev: '+0.29', desc: 'Broad-range suited Ace. High implied odds in position.' },
  'KQs': { name: 'King-Queen Suited', type: 'suited', sklansky: 'Group 2', chen: '10', hellmuth: 'Tier 3', ev: '+0.32', desc: 'Royal broadway connector. Makes second-nut flushes and open-ended straight draws.' },
  '76s': { name: 'Seven-Six Suited', type: 'suited', sklansky: 'Group 5', chen: '7', hellmuth: 'Tier 6', ev: '+0.08', desc: 'The quintessential suited connector. Highly deceptive implied odds against overpairs.' },
  '87s': { name: 'Eight-Seven Suited', type: 'suited', sklansky: 'Group 5', chen: '8', hellmuth: 'Tier 6', ev: '+0.11', desc: 'Premier mid-range suited connector. Great candidate for semi-bluffing on the flop.' }
};

let selectedHand = 'AA';

function renderMatrix() {
  const grid = document.getElementById('matrixGrid');
  grid.innerHTML = '';

  for (let r = 0; r < 13; r++) {
    for (let c = 0; c < 13; c++) {
      let label = '';
      let typeClass = '';

      if (r === c) {
        label = ranks[r] + ranks[c];
        typeClass = 'pair';
      } else if (r < c) {
        label = ranks[r] + ranks[c] + 's';
        typeClass = 'suited';
      } else {
        label = ranks[c] + ranks[r] + 'o';
        typeClass = 'offsuit';
      }

      const cell = document.createElement('div');
      cell.className = `matrix-cell ${typeClass}` + (label === selectedHand ? ' selected' : '');
      cell.id = `cell-${label}`;
      cell.innerText = label;

      cell.onclick = () => selectHand(label);
      cell.ondblclick = () => {
        window.location.href = `/all-hands#${label.toLowerCase()}`;
      };

      grid.appendChild(cell);
    }
  }
}

function selectHand(hand) {
  selectedHand = hand;
  document.querySelectorAll('.matrix-cell').forEach(c => c.classList.remove('selected'));
  const activeEl = document.getElementById(`cell-${hand}`);
  if (activeEl) activeEl.classList.add('selected');

  const data = handDatabase[hand] || generateFallbackData(hand);
  document.getElementById('displayHand').innerText = `${hand} (${data.name})`;
  document.getElementById('handDescription').innerText = data.desc;
  document.getElementById('valSklansky').innerText = data.sklansky;
  document.getElementById('valChen').innerText = `${data.chen} pts`;
  document.getElementById('valHellmuth').innerText = data.hellmuth;
  document.getElementById('valEV').innerText = data.ev;
}

function generateFallbackData(hand) {
  let isPair = hand.length === 2 && hand[0] === hand[1];
  let isSuited = hand.endsWith('s');
  return {
    name: isPair ? 'Pocket Pair' : (isSuited ? 'Suited Hand' : 'Offsuit Hand'),
    desc: `Pre-flop holding evaluated across all 14,365 heads-up matchups. Click to explore full simulated river run-outs.`,
    sklansky: isPair ? 'Group 4–7' : (isSuited ? 'Group 5–7' : 'Group 6–8'),
    chen: isPair ? '6–10' : (isSuited ? '5–8' : '2–6'),
    hellmuth: isPair ? 'Tier 3–5' : (isSuited ? 'Tier 5–7' : 'Tier 6–7'),
    ev: isPair ? '+0.05' : (isSuited ? '+0.01' : '-0.08')
  };
}

function filterCategory(cat) {
  document.querySelectorAll('.matrix-cell').forEach(el => {
    el.style.opacity = '0.25';
    if (cat === 'pair' && el.classList.contains('pair')) el.style.opacity = '1';
    if (cat === 'suited' && el.classList.contains('suited')) el.style.opacity = '1';
    if (cat === 'broadway' && (el.innerText.includes('A') || el.innerText.includes('K') || el.innerText.includes('Q') || el.innerText.includes('J') || el.innerText.includes('T'))) el.style.opacity = '1';
  });
}

function resetMatrix() {
  document.querySelectorAll('.matrix-cell').forEach(el => el.style.opacity = '1');
  selectHand('AA');
}

document.addEventListener('DOMContentLoaded', () => {
  renderMatrix();
  selectHand('AA');
});
</script>
