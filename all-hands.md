---
layout: page
title: "Texas Hold'em Starting Hands Directory | All 14,365 Heads-Up Matchups"
permalink: /all-hands
description: "Comprehensive Texas Hold'em starting hands directory. Explore all 169 starting hands, browse all 14,365 heads-up matchups, pre-flop equities, and strategy guides."
---

<style>
.directory-container {
  max-width: 1000px;
  margin: 0 auto;
}
.search-box-wrap {
  background: #f8f9fa;
  border: 1px solid #e3e6f0;
  border-radius: 10px;
  padding: 25px;
  margin-bottom: 30px;
}
.hand-search-input {
  font-size: 18px;
  padding: 12px 18px;
  border-radius: 8px;
  border: 2px solid #ced4da;
  width: 100%;
  transition: all 0.2s ease;
}
.hand-search-input:focus {
  border-color: #00B2D8;
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 178, 216, 0.2);
}
.category-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 15px;
}
.cat-pill {
  font-size: 13px;
  font-weight: 600;
  padding: 6px 14px;
  border-radius: 20px;
  border: 1px solid #ced4da;
  background: #fff;
  color: #495057;
  cursor: pointer;
  transition: all 0.15s ease;
}
.cat-pill:hover, .cat-pill.active {
  background: #00B2D8;
  color: #fff;
  border-color: #00B2D8;
}
.hand-card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 10px;
  margin-bottom: 40px;
}
.hand-card {
  background: #fff;
  border: 1px solid #e3e6f0;
  border-radius: 8px;
  padding: 12px 10px;
  text-align: center;
  text-decoration: none !important;
  color: #212529;
  display: block;
  transition: all 0.2s ease;
  position: relative;
}
.hand-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  border-color: #00B2D8;
}
.hand-card .badge-type {
  font-size: 10px;
  font-weight: 700;
  padding: 2px 6px;
  border-radius: 10px;
  display: inline-block;
  margin-bottom: 4px;
}
.hand-card.pair .badge-type { background: #d4edda; color: #155724; }
.hand-card.suited .badge-type { background: #cce5ff; color: #004085; }
.hand-card.offsuit .badge-type { background: #e2e3e5; color: #383d41; }
.hand-card .hand-name {
  font-size: 16px;
  font-weight: 700;
  margin: 0;
  color: #212529;
}
.hand-card .hand-equity {
  font-size: 11px;
  color: #6c757d;
  margin-top: 2px;
}
.matchup-spotlight {
  background: #ffffff;
  border: 1px solid #e3e6f0;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 25px;
}
.spotlight-link {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 14px;
  background: #f8f9fa;
  border-radius: 6px;
  margin-bottom: 8px;
  text-decoration: none !important;
  color: #333;
  font-weight: 600;
  transition: all 0.15s ease;
}
.spotlight-link:hover {
  background: #eef8fb;
  color: #00B2D8;
  padding-left: 18px;
}
</style>

<div class="directory-container">
  <div class="text-center mb-4">
    <p class="lead text-muted">
      Access complete combinatorial probabilities, pre-flop equities, and strategic run-outs for all <strong>169 canonical hands</strong> and <strong>14,365 heads-up matchups</strong>.
    </p>
  </div>

  <!-- Interactive Search & Filter Bar -->
  <div class="search-box-wrap">
    <label for="handSearch" class="font-weight-bold text-dark mb-2">
      <i class="fas fa-search text-primary mr-1"></i> Quick Hand Filter:
    </label>
    <input type="text" id="handSearch" class="hand-search-input" placeholder="Type a hand (e.g. AA, AKs, QQ, JTs, 72o) or rank (Ace, King)..." onkeyup="filterHands()">
    
    <div class="category-pills">
      <button class="cat-pill active" onclick="setCategory('all', this)">All Hands (169)</button>
      <button class="cat-pill" onclick="setCategory('pair', this)">Pocket Pairs (13)</button>
      <button class="cat-pill" onclick="setCategory('broadway', this)">Broadways (32)</button>
      <button class="cat-pill" onclick="setCategory('suited-connector', this)">Suited Connectors (10)</button>
      <button class="cat-pill" onclick="setCategory('suited', this)">All Suited (78)</button>
      <button class="cat-pill" onclick="setCategory('offsuit', this)">All Offsuit (78)</button>
    </div>
  </div>

  <!-- High-Profile Matchup Hubs -->
  <div class="matchup-spotlight mb-5">
    <h2 class="h5 font-weight-bold mb-3 text-dark">
      <i class="fas fa-fire text-danger mr-1"></i> Legendary Texas Hold'em Showdowns
    </h2>
    <div class="row">
      <div class="col-md-6">
        <a href="/kk-vs-aa" class="spotlight-link">
          <span>Pocket Aces vs Pocket Kings</span>
          <span class="badge badge-primary">AA ~82% vs KK ~18%</span>
        </a>
        <a href="/qq-vs-aks" class="spotlight-link">
          <span>Pocket Queens vs Ace-King Suited</span>
          <span class="badge badge-info">Classic Coin Flip (~54% vs 46%)</span>
        </a>
        <a href="/22-vs-ako" class="spotlight-link">
          <span>Pocket Twos vs Ace-King Offsuit</span>
          <span class="badge badge-secondary">Small Pair vs Big Slicks</span>
        </a>
      </div>
      <div class="col-md-6">
        <a href="/tt-vs-jj" class="spotlight-link">
          <span>Pocket Jacks vs Pocket Tens</span>
          <span class="badge badge-primary">JJ ~81% vs TT ~19%</span>
        </a>
        <a href="/87s-vs-aa" class="spotlight-link">
          <span>8-7 Suited vs Pocket Aces</span>
          <span class="badge badge-warning">The Connector Crack (~23% eq)</span>
        </a>
        <a href="/72o-vs-aa" class="spotlight-link">
          <span>Pocket Aces vs Seven-Two Offsuit</span>
          <span class="badge badge-danger">Max Dominance (AA ~88%)</span>
        </a>
      </div>
    </div>
  </div>

  <!-- Hand Matrix CTA -->
  <div class="alert alert-info d-flex justify-content-between align-items-center p-3 mb-4 rounded border">
    <div>
      <div class="h6 font-weight-bold mb-1"><i class="fas fa-th mr-1 text-primary"></i> Prefer a Visual Heatmap?</div>
      <p class="small text-muted mb-0">Use our interactive 13x13 grid with color-coded range categories and instant equity overlays.</p>
    </div>
    <a href="/hand-matrix" class="btn btn-primary btn-sm text-nowrap ml-3">Open 13x13 Matrix &raquo;</a>
  </div>

  <!-- Cards Grid -->
  <h2 class="h4 font-weight-bold text-dark mb-3">All 169 Starting Hands</h2>
  <div class="hand-card-grid" id="handCards"></div>
</div>

<script>
const ranks = ['A', 'K', 'Q', 'J', 'T', '9', '8', '7', '6', '5', '4', '3', '2'];
let allHandsData = [];
let currentCategory = 'all';

function buildHandList() {
  allHandsData = [];
  
  // Pairs
  ranks.forEach(r => {
    allHandsData.push({
      code: r + r,
      type: 'pair',
      category: 'pair',
      isBroadway: ['A', 'K', 'Q', 'J', 'T'].includes(r),
      isConnector: false,
      url: `/hand-matrix#${(r+r).toLowerCase()}`
    });
  });

  // Suited & Offsuit
  for (let i = 0; i < ranks.length; i++) {
    for (let j = i + 1; j < ranks.length; j++) {
      let r1 = ranks[i];
      let r2 = ranks[j];
      let isBroadway = ['A', 'K', 'Q', 'J', 'T'].includes(r1) && ['A', 'K', 'Q', 'J', 'T'].includes(r2);
      let isConnector = (j === i + 1);

      allHandsData.push({
        code: r1 + r2 + 's',
        type: 'suited',
        category: 'suited',
        isBroadway: isBroadway,
        isConnector: isConnector,
        url: `/hand-matrix#${(r1+r2+'s').toLowerCase()}`
      });

      allHandsData.push({
        code: r1 + r2 + 'o',
        type: 'offsuit',
        category: 'offsuit',
        isBroadway: isBroadway,
        isConnector: isConnector,
        url: `/hand-matrix#${(r1+r2+'o').toLowerCase()}`
      });
    }
  }
}

function renderHandCards(hands) {
  const container = document.getElementById('handCards');
  container.innerHTML = '';

  if (hands.length === 0) {
    container.innerHTML = '<div class="col-12 text-center text-muted py-4">No starting hands matched your search query.</div>';
    return;
  }

  hands.forEach(h => {
    const card = document.createElement('a');
    card.className = `hand-card ${h.type}`;
    card.href = h.url;
    
    let typeLabel = h.type === 'pair' ? 'PAIR' : (h.type === 'suited' ? 'SUITED' : 'OFFSUIT');
    card.innerHTML = `
      <span class="badge-type">${typeLabel}</span>
      <div class="hand-name">${h.code}</div>
      <div class="hand-equity">View Matchups &raquo;</div>
    `;
    container.appendChild(card);
  });
}

function filterHands() {
  const query = document.getElementById('handSearch').value.toUpperCase().trim();
  
  let filtered = allHandsData.filter(h => {
    let matchQuery = query === '' || h.code.includes(query);
    let matchCat = true;

    if (currentCategory === 'pair') matchCat = h.type === 'pair';
    else if (currentCategory === 'suited') matchCat = h.type === 'suited';
    else if (currentCategory === 'offsuit') matchCat = h.type === 'offsuit';
    else if (currentCategory === 'broadway') matchCat = h.isBroadway;
    else if (currentCategory === 'suited-connector') matchCat = (h.type === 'suited' && h.isConnector);

    return matchQuery && matchCat;
  });

  renderHandCards(filtered);
}

function setCategory(cat, btn) {
  currentCategory = cat;
  document.querySelectorAll('.cat-pill').forEach(p => p.classList.remove('active'));
  if (btn) btn.classList.add('active');
  filterHands();
}

document.addEventListener('DOMContentLoaded', () => {
  buildHandList();
  renderHandCards(allHandsData);

  // If URL has hash, filter automatically
  if (window.location.hash) {
    let h = window.location.hash.replace('#', '').toUpperCase();
    if (h) {
      document.getElementById('handSearch').value = h;
      filterHands();
    }
  }
});
</script>
