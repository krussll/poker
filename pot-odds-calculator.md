---
layout: page
title: "Pot Odds & Break-Even Equity Calculator"
permalink: /pot-odds-calculator
description: "Free Texas Hold'em pot odds calculator. Calculate pot odds, required calling equity, and compare with your outs using the Rule of 4 and 2."
---

<style>
.pot-calc-container {
  max-width: 800px;
  margin: 0 auto;
}
.calc-card-modern {
  background: #ffffff;
  border: 1px solid #e3e6f0;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
  padding: 30px;
}
.result-box {
  background: #f0f7fd;
  border-left: 5px solid #00B2D8;
  border-radius: 8px;
  padding: 20px;
}
.form-control-lg {
  font-size: 18px;
  font-weight: 600;
}
</style>

<div class="pot-calc-container py-2">
  <p class="lead text-center mb-4">
    Determine whether calling a bet is mathematically profitable ($+EV$) by calculating your exact <strong>Pot Odds</strong> and <strong>Required Break-Even Equity</strong>.
  </p>

  <div class="calc-card-modern">
    <div class="row">
      <!-- Input Column -->
      <div class="col-md-6 mb-4 mb-md-0">
        <h5 class="font-weight-bold text-dark mb-3"><i class="fas fa-coins text-warning mr-2"></i> Bet &amp; Pot Inputs</h5>
        
        <div class="form-group mb-3">
          <label class="font-weight-bold small text-muted">Current Pot Size ($ or Chips):</label>
          <input type="number" id="inputPot" class="form-control form-control-lg" value="100" min="1" oninput="calculatePotOdds()">
          <small class="form-text text-muted">Total chips in the pot before your opponent's bet.</small>
        </div>

        <div class="form-group mb-3">
          <label class="font-weight-bold small text-muted">Bet Size Faced to Call ($ or Chips):</label>
          <input type="number" id="inputBet" class="form-control form-control-lg" value="50" min="1" oninput="calculatePotOdds()">
          <small class="form-text text-muted">The exact amount you must call.</small>
        </div>

        <div class="form-group mb-3">
          <label class="font-weight-bold small text-muted">Known Outs (Optional):</label>
          <select id="selectOuts" class="form-control" onchange="calculatePotOdds()">
            <option value="0">Custom / Unknown</option>
            <option value="9">9 Outs (Flush Draw - ~36% flop to river)</option>
            <option value="8">8 Outs (Open-Ended Straight - ~32%)</option>
            <option value="4">4 Outs (Gutshot Straight - ~17%)</option>
            <option value="15">15 Outs (Straight + Flush Draw - ~54%)</option>
            <option value="2">2 Outs (Pocket Pair to Set - ~8%)</option>
          </select>
        </div>
      </div>

      <!-- Result Column -->
      <div class="col-md-6">
        <h5 class="font-weight-bold text-dark mb-3"><i class="fas fa-chart-line text-primary mr-2"></i> Mathematical Results</h5>
        
        <div class="result-box mb-3">
          <div class="mb-3">
            <small class="text-muted text-uppercase font-weight-bold">Pot Odds Offered</small>
            <div class="h2 font-weight-bold text-dark mb-0" id="resRatio">3.00 : 1</div>
          </div>
          <div class="mb-3">
            <small class="text-muted text-uppercase font-weight-bold">Required Equity to Call</small>
            <div class="h2 font-weight-bold text-primary mb-0" id="resEquity">25.0%</div>
          </div>
          <div>
            <small class="text-muted text-uppercase font-weight-bold">Total Pot After Call</small>
            <div class="h4 font-weight-bold text-secondary mb-0" id="resTotalPot">$200</div>
          </div>
        </div>

        <div class="alert alert-info py-2 px-3 small mb-0" id="adviceVerdict">
          <i class="fas fa-check-circle text-success mr-1"></i> You need at least <strong>25.0%</strong> equity for calling to be profitable long-term ($+EV$).
        </div>
      </div>
    </div>
  </div>

  <!-- Educational Explainer -->
  <div class="row mt-4">
    <div class="col-md-4 mb-3">
      <div class="p-3 bg-light rounded border h-100">
        <h6 class="font-weight-bold text-dark">The Pot Odds Formula</h6>
        <p class="small text-muted mb-0">
          <code>Required Equity = Call / (Pot + Bet + Call)</code>. If your equity is higher than this percentage, calling makes money over time.
        </p>
      </div>
    </div>
    <div class="col-md-4 mb-3">
      <div class="p-3 bg-light rounded border h-100">
        <h6 class="font-weight-bold text-dark">Rule of 4 and 2</h6>
        <p class="small text-muted mb-0">
          Multiply your outs by <strong>4</strong> on the flop to estimate equity with two cards to come. Multiply by <strong>2</strong> on the turn with one card to come.
        </p>
      </div>
    </div>
    <div class="col-md-4 mb-3">
      <div class="p-3 bg-light rounded border h-100">
        <h6 class="font-weight-bold text-dark">Comparing Hand Equities</h6>
        <p class="small text-muted mb-0">
          Need to know your exact hand equity against an opponent? Check our <a href="/poker-odds-calculator">Odds Calculator</a> or browse <a href="/all-hands">All Matchups</a>.
        </p>
      </div>
    </div>
  </div>
</div>

<script>
function calculatePotOdds() {
  const pot = parseFloat(document.getElementById('inputPot').value) || 0;
  const bet = parseFloat(document.getElementById('inputBet').value) || 0;
  const outs = parseInt(document.getElementById('selectOuts').value) || 0;

  if (bet <= 0 || pot <= 0) return;

  const totalPotAfterCall = pot + bet + bet; // Pot before bet + bet + call
  const potOddsRatio = (pot + bet) / bet;
  const requiredEquity = (bet / totalPotAfterCall) * 100;

  document.getElementById('resRatio').innerText = `${potOddsRatio.toFixed(2)} : 1`;
  document.getElementById('resEquity').innerText = `${requiredEquity.toFixed(1)}%`;
  document.getElementById('resTotalPot').innerText = `$${totalPotAfterCall.toLocaleString()}`;

  const verdict = document.getElementById('adviceVerdict');
  if (outs > 0) {
    const approxEquity = outs * 4; // Rule of 4
    if (approxEquity >= requiredEquity) {
      verdict.className = "alert alert-success py-2 px-3 small mb-0";
      verdict.innerHTML = `<i class="fas fa-check-circle mr-1"></i> <strong>Profitable Call (+EV)!</strong> Your ${outs} outs give approx. <strong>${approxEquity}%</strong> equity, which beats the required <strong>${requiredEquity.toFixed(1)}%</strong>.`;
    } else {
      verdict.className = "alert alert-warning py-2 px-3 small mb-0";
      verdict.innerHTML = `<i class="fas fa-exclamation-triangle mr-1"></i> <strong>Unprofitable Call (-EV)!</strong> Your ${outs} outs yield approx. <strong>${approxEquity}%</strong> equity, falling below the required <strong>${requiredEquity.toFixed(1)}%</strong> without implied odds.`;
    }
  } else {
    verdict.className = "alert alert-info py-2 px-3 small mb-0";
    verdict.innerHTML = `<i class="fas fa-info-circle mr-1"></i> You need at least <strong>${requiredEquity.toFixed(1)}%</strong> equity for calling to be mathematically profitable ($+EV$).`;
  }
}

document.addEventListener('DOMContentLoaded', calculatePotOdds);
</script>
