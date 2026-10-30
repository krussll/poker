# River Pairs (www.riverpairs.com)

**River Pairs** is a large-scale programmatic SEO (pSEO) and poker strategy reference website. Built on Jekyll and deployed via GitHub Pages, it catalogs pre-flop odds, equities, hand ratings, and showdown frequencies for every heads-up starting hand combination in Texas Hold'em.

## Overview
- **Live Site**: [www.riverpairs.com](https://www.riverpairs.com)
- **Engine**: Jekyll 4+ with Liquid templating
- **Combinatorial Scope**: 169 distinct canonical starting hands yielding 14,365 unique heads-up pre-flop match-up articles.
- **AI Machine Index**: [`/llms.txt`](/llms.txt) provides structured context for AI engines (SearchGPT, Claude, Perplexity).
- **IndexNow Integration**: Verified via IndexNow API key for instant multi-engine indexation (Bing, Yandex, Seznam).

## Architecture
- `_posts/`: Markdown files for each of the 14,365 matchups plus pillar strategy guides.
- `_layouts/`: Liquid templates (`default.html`, `post.html`, `page.html`).
- `_includes/`: Modular UI partials (cards, sidebar, navigation, search).
- `assets/`: Front-end styles (SASS/Bootstrap 4), icons, and card image assets.
- `sitemap.xml`: Canonical XML sitemap with daily/weekly crawl priorities.
- `robots.txt`: Search crawler configuration with explicit AI crawler access.

## Strategy Pillars
- [Sklansky Hand Groups Explained](/sklansky-rating-explained)
- [The Chen Formula Explained](/chen-formula-explained)
- [Phil Hellmuth Starting Tiers](/Hellmuth-tier-explained)
- [Expected Value (EV) in Poker](/expected-value-explained)

## License & Attribution
Content & Simulation Models &copy; Reece Williams. All rights reserved.
