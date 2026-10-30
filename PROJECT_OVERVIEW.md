# River Pairs — Codebase & Project Overview

## 1. Executive Summary

**River Pairs** ([riverpairs.com](https://www.riverpairs.com)) is a large-scale programmatic SEO (pSEO) and poker strategy reference website. The site is built with the **Jekyll** static site generator and deployed continuously via **GitHub Pages** (tracking the `gh-pages` branch of `krussll/poker`).

The project's primary value proposition is serving as a definitive mathematical lookup directory for Texas Hold'em poker players. It covers every possible pre-flop heads-up match-up, providing statistical equity, win/tie probabilities, historical rating systems, and full combinatorial showdown outcomes.

---

## 2. Core Purpose & Poker Mathematics Engine

### Combinatorial Coverage (Programmatic SEO)
In Texas Hold'em, there are **169 distinct canonical starting hands**:
- 13 pocket pairs (e.g., `22`, `AA`)
- 78 suited combinations (e.g., `AKs`, `76s`)
- 78 offsuit combinations (e.g., `AKo`, `T8o`)

Comparing every distinct starting hand against another produces:
$$\frac{169 \times (169 + 1)}{2} = 14,365\text{ unique heads-up match-ups}$$

The codebase contains an individual Markdown article in `_posts/` for every one of these 14,365 match-ups (generated under the date `2023-06-30`), plus master index files (such as `2023-09-25-all-hands.md` linking across all combinations).

### Per-Matchup Data Points
Each post contains rich precomputed front matter and structured tables:
1. **Equity & Probabilities:**
   - Pre-flop Equity (`eq_1`, `eq_2`)
   - Direct Win Probabilities (`win_1`, `win_2`) and Tie Probability (`tie`)
   - Dynamic Donut Graphs rendered client-side using Chart.js.
2. **Limit Poker Hand Ratings:**
   - **Sklansky Hand Group Rating**
   - **Chen Formula Score**
   - **Hellmuth Starting Hand Tier**
   - **Expected Value (EV)**
3. **Showdown Hand Distribution:**
   - Precise combinatorial frequencies across all 9 poker hand rankings (High Card, One Pair, Two Pair, Three of a Kind, Straight, Flush, Full House, Four of a Kind, Straight Flush) when evaluated across the entire deck run-out.
4. **Visual Playing Card Graphics:**
   - Cards dynamically assembled using assets in `assets/images/hand1/` and `assets/images/hand2/`.

---

## 3. Technology Stack & Architecture

| Layer | Technology | Details |
| :--- | :--- | :--- |
| **Static Site Generator** | Jekyll | Markdown-driven static compilation with Liquid templating engine |
| **Hosting & CI/CD** | GitHub Pages | Hosted directly from the `gh-pages` branch on custom domain `www.riverpairs.com` |
| **CSS Framework** | Bootstrap 4 (Alpha 6) | Responsive layout grid and utilities |
| **Typography & Icons** | Google Fonts (Rubik), FontAwesome 5.8.1 | Clean readability for numerical and statistical tables |
| **Interactive Graphs** | Chart.js (v4 via CDN) | Client-side doughnut charts rendering equity and win probabilities |
| **Client Scripting** | jQuery, Masonry, Lazysizes | Image lazy-loading and dynamic card grid formatting |
| **Ad / Analytics Stack** | Google Analytics 4, Google AdSense, Ezoic | `gtag.js` (`G-D26Y53Z0KH`), `ads.txt` (`pub-8543572621618524`), and Ezoic placeholders |

---

## 4. Directory Structure

```
.
├── CNAME                         # Custom domain mapping (www.riverpairs.com)
├── _config.yml                   # Jekyll site configuration and author profiles
├── index.html                    # Homepage (Jumbotron, Featured Posts, Latest Articles)
├── all-posts.md                  # Comprehensive post archive page
├── sitemap.xml                   # Dynamic XML sitemap iterating all 14,000+ posts
├── feed.xml                      # RSS 2.0 feed
├── robots.txt                    # Search crawler instructions
├── ads.txt                       # Google AdSense publisher verification
│
├── _layouts/                     # Liquid page templates
│   ├── default.html              # Main HTML shell (head tags, nav, footer, scripts)
│   ├── post.html                 # Match-up article layout with Chart.js initialization
│   ├── page.html                 # Standard static content page layout
│   └── archive.html              # Post list / archive template
│
├── _includes/                    # Reusable template components
│   ├── postbox.html              # Grid card preview component
│   ├── featuredbox.html          # Hero featured article card
│   ├── sidebar.html              # Sidebar wrapper
│   ├── recommended.html          # Popular hand match-up links (AA vs AA, etc.)
│   └── search-form.html          # Search input partial
│
├── _posts/                       # 14,367 Markdown files
│   ├── 2023-06-30-[hand1]-vs-[hand2].md  # 14,365 head-to-head match-up posts
│   └── 2023-09-25-all-hands.md           # Master directory of all matchups
│
├── _sass/                        # SASS partials
│   ├── _syntax.scss              # Code syntax highlighting
│   └── config.scss               # Color and typography theme variables
│
└── assets/                       # Static front-end assets
    ├── css/                      # theme.css & custom.scss
    ├── images/                   # riverpairs.png logo, favicon, and hand1/hand2 card faces
    └── js/                       # jQuery, theme.js, Masonry, Lazysizes
```

---

## 5. Monetization & SEO Strategy

1. **Traffic Acquisition (Programmatic Long-Tail SEO):**
   - Target queries: *"Pocket Twos vs Ace King offsuit odds"*, *"AKs vs QQ equity"*, *"Which is better in poker 22 or 32o"*.
   - Canonical permalinks formatted as `/:title` (e.g., `/22-vs-ako`).
   - Deep internal linking tree via `all-posts.md`, `all-hands.md`, Prev/Next article pagination, and the sidebar's popular matchups.
2. **Structured Data:**
   - Articles implement schema.org markup (`BlogPosting`, `Person` author metadata, and `Organization` publisher metadata) to support Google search rich snippets.
3. **Monetization Channels:**
   - **Display Ads:** Integrated via Google AdSense (`ads.txt`) and Ezoic placeholders (`#ezoic-pub-ad-placeholder-103`).
   - **In-content Breakpoints:** Standard comment tags `[comment]: # (sp0)` through `(sp5)` placed inside post templates for programmatic ad or sponsored insertion.
   - **Affiliate Disclosure:** Includes Amazon Associates disclaimer (`affiliate-disclaimer.md`) with potential for poker affiliate tracking.

---

## 6. Notable Inconsistencies & Technical Debt

During this review, several remnants from previous template projects or unfinished content were identified:

1. **Boilerplate Text from Earlier Projects:**
   - `about-me.md`: Contains biography text describing trips to *"American zoos"* from an earlier project called *Zoo Near Me*.
   - `terms.md` & `privacy.md`: Contain legal agreements referencing `www.zoonear.me` and *Zoo Near Me*.
   - `contact.md`: Contains a mailto link pointing to `reece@jackhighbowls.com`.
   - `README.md`: Contains default GitHub Pages boilerplate referencing `krussll/spencer-bowls`.
2. **Missing Explainer Pages:**
   - Every post links out to informational pages for rating formulas:
     - `/sklansky-rating-explained`
     - `/chen-formula-explained`
     - `/Hellmuth-tier-explained`
     - `/expected-value-explained`
   - These target pages do not currently exist in the repository, resulting in 404s when clicked.
3. **E-E-A-T & YMYL Sensitivity:**
   - Because gambling, odds, and financial betting fall squarely under Google's **YMYL** (Your Money or Your Life) classification, search algorithms strictly scrutinize author credentials, clear editorial disclaimers, and legitimate brand attribution. Aligning `about-me.md`, `privacy.md`, and author metadata directly to poker experience will significantly strengthen search authority.
