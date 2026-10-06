# Frank Crilley

Computer Science at Clemson, class of 2029. I build systems designed to prove themselves wrong.

Most of my work is quantitative research infrastructure — not strategies, but the machinery that decides whether a strategy is real. I got there backwards. I set out to search a huge space of trading strategies looking for a freak winner, and discovered that the search is the easy half. A search run through a filter that lies doesn't find alpha; it manufactures an overfit winner with a beautiful equity curve, every single time, and the harder you search the more certain you are to be fooled.

So I stopped building the search and built the filter. Then I pointed it at my own work, and it cost me most of my results.

---

### Start here

**[the-gauntlet](https://github.com/fraxkie/the-gauntlet)** — a research engine built to kill trading strategies, not to find them.

Four adversarial firewalls, each catching a different way a backtest lies: Monte Carlo permutation testing, a walk-forward consistency gate, regime segmentation, and a look-ahead audit. Above them sits a persistent trial ledger applying a multiple-comparison correction across every test I have ever run — the firewall against myself.

Nothing is trusted on real markets until it's proven on synthetic data where the answer is known in advance: the suite confirms the engine catches a deliberately planted look-ahead cheater, rejects noise, and finds a deliberately planted edge. The engine is vectorized and Numba-compiled — 9,000 strategy simulations in 19 seconds, a 50x+ speedup over the original.

What it found, over 21 years of daily bars: eight strategy families went in and all eight came out dead, most as market beta in a costume. Sixteen intraday Nasdaq-futures strategies, pre-registered before testing, were all rejected at the corrected significance bar. A widely cited leveraged-ETF rebalancing effect turned out to have decayed as those flows grew. The overnight effect was real and worthless at the same time — +432% raw, −79% after transaction costs.

**[core-bot](https://github.com/fraxkie/core-bot)** — the one strategy that survived, running live on Alpaca.

The part worth reading is what the engine did to my own numbers. It caught three errors in my own backtest — unrealistic stop fills, a Sharpe annualization bug, and outlier-year skew — which cut the reported Sharpe from 2.1 to an honest ~0.9. I'd rather publish the 0.9.

The system around it: strategy logic with zero I/O so the decision function is unit-testable without mocking a broker, limit-then-market execution, restart-safe persisted state, a catch-up exit for missed runs, crash logging with retries, and a regression suite. A read-only monitoring dashboard pulls fills straight from the broker and tracks implementation shortfall per trade — live fill versus backtest price — which is the metric that gates moving to real capital.

**[martin](https://github.com/fraxkie/martin)** — a local-first assistant that reads my screen and runs commands on my machine, built on the premise that everything it reads is hostile.

A web page sets its own window title, so a window title is attacker-controlled input — and so is anything read off the screen. Any turn whose context includes that text is tainted and can never reach the executor, whatever it says. The executor then enforces the same rule independently from the other end, so there's no single point of failure.

Hybrid model routing: a local LLM handles everyday turns on-device, and only hard questions escalate to a hosted model, so ordinary data never leaves the machine and the thing costs nearly nothing to run. Persistent memory in SQLite with full-text search, and a self-built index of where I go assembled from browser history.

---

### Currently

Working in C++ on memory layout and performance, heading toward a limit order book — matching engine, price-time priority, benchmarked and then optimized. Same domain I already know, and a continuation of the profiling work that got the gauntlet to 19 seconds.

Targeting quant developer internships.

---

### How these were built

the-gauntlet, martin and core-bot were implemented with AI assistance, against specifications I wrote. What's mine is the framing, the architecture, and the decisions about what to trust, what to kill, and what to defer. Each repo says so in its own README, and I can defend every design choice in them.

The order book is the exception. That one gets written by hand, because the whole point of it is the thing it's meant to demonstrate.
