---
layout: page
title: "Poker Odds Calculation Methodology & Simulation Whitepaper"
permalink: /methodology
description: "Understand the combinatorial mathematics, Monte Carlo enumeration, and simulation engine behind River Pairs' 14,365 Texas Hold'em matchup database."
---

## 1. Introduction & Theoretical Foundation

**River Pairs** was created to serve as a mathematically exact, transparent, and authoritative reference for Texas Hold'em heads-up probabilities. 

In modern game theory and computer poker research, evaluating the strength of two starting hands requires determining the probability of each hand making the highest-ranking 5-card poker hand at the river across all possible community board run-outs.

This document describes the computational model, combinatorial algorithms, and mathematical definitions used to generate every equity, win rate, and showdown frequency displayed across our **14,365 matchup articles**.

---

## 2. Combinatorial Deck Mathematics

Standard Texas Hold'em is played with a standard 52-card French deck:
- 4 suits: Spades ($\spadesuit$), Hearts ($\varpi$), Diamonds ($\diamondsuit$), Clubs ($\clubsuit$)
- 13 ranks: $2, 3, 4, 5, 6, 7, 8, 9, 10, \text{Jack}, \text{Queen}, \text{King}, \text{Ace}$

### 2.1 Starting Hand Categories (169 Hands)
Ignoring specific suits, any starting hand falls into one of three structural forms:
1. **Pocket Pairs**: $\binom{13}{1} = 13$ types (e.g., $AA, KK, \dots, 22$). Each has $\binom{4}{2} = 6$ unique suit combinations ($13 \times 6 = 78$ combos).
2. **Suited Cards**: $\binom{13}{2} = 78$ types (e.g., $AKs, 76s$). Each has 4 suit combinations ($78 \times 4 = 312$ combos).
3. **Offsuit Cards**: $\binom{13}{2} = 78$ types (e.g., $AKo, 76o$). Each has $4 \times 3 = 12$ suit combinations ($78 \times 12 = 936$ combos).

$$\text{Total Pre-Flop Combinations} = 78 + 312 + 936 = 1,326\text{ combos}$$

Compressing isomorphic suit permutations leaves exactly **169 canonical starting hand categories**.

### 2.2 Heads-Up Matchup Universe (14,365 Matchups)
Comparing every canonical starting hand against another produces:

$$\frac{169 \times (169 + 1)}{2} = 14,365\text{ unique heads-up confrontations}$$

River Pairs catalogs complete data for all 14,365 matchups.

---

## 3. Board Enumeration & Card Removal Engine

### 3.1 Community Board Possibilities
Once four hole cards (2 for Hand 1, 2 for Hand 2) are dealt from a 52-card deck, **48 cards remain** in the stub.

A complete Texas Hold'em board consists of 5 community cards (Flop: 3 cards, Turn: 1 card, River: 1 card). When evaluating non-overlapping hands, the total number of distinct 5-card boards that can be dealt is:

$$\binom{48}{5} = \frac{48 \times 47 \times 46 \times 45 \times 44}{5 \times 4 \times 3 \times 2 \times 1} = 1,712,304\text{ unique board run-outs}$$

For overlapping hands (e.g., $AA$ vs $AA$ where 4 Aces are removed), the simulation computes over all remaining cards in the deck, adjusting the denominator accordingly.

### 3.2 7-Card Hand Evaluation
For each of the $1,712,304$ possible boards, each player has a 7-card pool (2 hole cards + 5 community cards). The evaluator extracts the best 5-card subset:

$$\binom{7}{5} = 21\text{ combinations per player}$$

Each 5-card combination is classified into the standard 9 hand tiers:
1. **Straight Flush** (including Royal Flush)
2. **Four of a Kind** (Quads)
3. **Full House** (Boat)
4. **Flush**
5. **Straight**
6. **Three of a Kind** (Trips/Set)
7. **Two Pair**
8. **One Pair**
9. **High Card**

The absolute integer rank of Hand 1's best 5-card hand is compared against Hand 2's best 5-card hand:
- If $\text{Rank}_1 > \text{Rank}_2 \implies \text{Hand 1 Win Counter} \gets \text{Win}_1 + 1$
- If $\text{Rank}_2 > \text{Rank}_1 \implies \text{Hand 2 Win Counter} \gets \text{Win}_2 + 1$
- If $\text{Rank}_1 = \text{Rank}_2 \implies \text{Tie Counter} \gets \text{Tie} + 1$

---

## 4. Equity and Probability Formulations

### 4.1 Win and Tie Probabilities
$$\text{Win}_1\% = \left( \frac{\text{Wins}_1}{\text{Total Boards Evaluated}} \right) \times 100$$
$$\text{Win}_2\% = \left( \frac{\text{Wins}_2}{\text{Total Boards Evaluated}} \right) \times 100$$
$$\text{Tie}\% = \left( \frac{\text{Ties}}{\text{Total Boards Evaluated}} \right) \times 100$$

### 4.2 Hand Equity (Pot Share)
In poker game theory, **Equity** represents a hand's mathematical share of the pot at showdown. Because tied pots are split equally between both contestants:

$$\text{Equity}_1 = \text{Win}_1\% + \left( \frac{\text{Tie}\%}{2} \right)$$
$$\text{Equity}_2 = \text{Win}_2\% + \left( \frac{\text{Tie}\%}{2} \right)$$

$$\text{Equity}_1 + \text{Equity}_2 = 100.00\%$$

---

## 5. Limit Rating Models & Modern GTO Context

In addition to pure combinatorial equity, River Pairs incorporates classical and modern hand valuation frameworks:

### 5.1 David Sklansky's Hand Groups
Classifies starting hands into Groups 1 through 8 based on expected win rate in Limit Texas Hold'em. Read our [Sklansky Hand Groups Guide](/sklansky-rating-explained).

### 5.2 Bill Chen's Formula
A numerical point system developed by Dr. Bill Chen and Jerrod Ankenman in *The Mathematics of Poker*. Evaluates high cards, pair multipliers, suitedness, and connector gaps. Read our [Chen Formula Guide](/chen-formula-explained).

### 5.3 Phil Hellmuth's 7 Tiers
Codified in *Play Poker Like the Pros*, Hellmuth's tiers focus on tournament survival and minimizing post-flop reverse-implied odds. Read our [Hellmuth Tiers Guide](/Hellmuth-tier-explained).

### 5.4 Expected Value (EV)
Quantifies the net expected profit or loss per hand:

$$EV = (P_{\text{win}} \times \text{Pot Won}) - (P_{\text{lose}} \times \text{Bet Risked})$$

Read our [Expected Value Guide](/expected-value-explained).

---

## 6. Verification & Data Integrity Standards

1. **Deterministic Exhaustion**: For all heads-up matchups, odds are determined by exhaustive combinatorial enumeration rather than random sample approximations, guaranteeing mathematical reproducibility.
2. **Card Removal Fidelity**: Blockers and suit conflicts (e.g. holding an Ace that prevents an opponent from making a nut flush) are fully accounted for on every iteration.
3. **Open Access**: All data is freely available for educational, strategic, and machine learning research.

For mathematical questions, verification inquiries, or data collaboration, contact **Reece Williams** at [contact@riverpairs.com](mailto:contact@riverpairs.com).
