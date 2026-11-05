# Awesome LLM Trading [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

Large language models applied to trading and factor research, with the evidence behind each result.

Last updated: 2026.11. ⭐ marks the maintainer's own paper. Nothing here is investment advice.

## Contents

- [Key Findings](#key-findings)
- [Reading the Entries](#reading-the-entries)
- [Benchmarks and Honest Evaluation](#benchmarks-and-honest-evaluation)
- [Factor Mining](#factor-mining)
- [Trading Agents](#trading-agents)
- [Text and Cross-Stock Signals](#text-and-cross-stock-signals)
- [Surveys](#surveys)
- [Related Lists](#related-lists)

## Key Findings

- **Careful evaluation is hard to pass.** With point-in-time US stocks and full costs, [no active strategy in the suite survives](https://arxiv.org/abs/2608.27734), and a planted leaky oracle with a Sharpe of 35 still passes Deflated Sharpe and PBO.
- **Paper versions matter.** [QuantaAlpha v1](https://arxiv.org/abs/2602.07085v1) reported IC 0.1501 and ARR 27.75%. [v3](https://arxiv.org/abs/2602.07085v3) reports IC 0.0472 and ARR 4.68% after moving its search gate to a 2021 validation period. An [Orient Securities note](https://quantaalpha.com/documents/orient-20260407-quantaalpha-factor-mining.pdf) had flagged test-segment RankIC driving search in the original code.
- **Style exposure can look like skill.** In [FactorBench](https://arxiv.org/abs/2610.06947), the residual IC of R&D-Agent falls from 0.0253 to 0.0011 after controlling for measured exposures.
- **Narrow samples flatter agents.** Over two decades and 100+ symbols, [FINSABER](https://arxiv.org/abs/2505.07078) finds that reported LLM advantages deteriorate significantly.
- **The filter can matter more than the generator.** Removing the statistical gate in [FaVOR](https://arxiv.org/abs/2608.30192) takes excess IR from 1.53 to 0.15 on CSI 500 and from 1.13 to -0.88 on S&P 500.

## Reading the Entries

Each entry is tagged in the same order: market, test window, costs. Entries are newest first.

- `5/15 bp` means 5 bp to buy and 15 bp to sell.
- `costs not stated` means we could not find it in the current arXiv version. We may have missed it, so corrections are welcome.
- A `Code` link appears only when we found an official repository.
- Performance numbers are author-reported backtests unless the entry says otherwise. Protocols differ across papers, so a rerun that disagrees with a paper is a reason to look closer. It does not by itself show the original is wrong.

## Benchmarks and Honest Evaluation

- [FactorBench](https://arxiv.org/abs/2610.06947) - `S&P 100, HSI, CSI 300, FTSE 100, Nikkei 225` `2024-01 to 2025-10` `10 bp` - Reruns 9 methods across 5 markets with 3 repetitions and finds that no paradigm consistently dominates. Its static universes may add survivorship bias. [Code](https://github.com/ZhuoHan1998/FactorBench-Release)
- [Agentic Empirical Asset Pricing](https://arxiv.org/abs/2609.00731) - `US equity panels (JKP, CRSP/Compustat)` `2020 to 2025-12` `50 bp per side, diagnostic only` - Introduces SEADS and reruns the discovery process itself on a rolling basis. No single metric ranks the systems consistently.
- [What Survives Honest Evaluation?](https://arxiv.org/abs/2608.27734) - `453 point-in-time US stocks, 39 ETFs` `2017 to 2025` `commission, spread, impact, borrow` - Excludes look-ahead by construction and counts every trial. No active strategy survives, while passive risk premia are certified.
- [KTD-Fin](https://arxiv.org/abs/2605.28359) - `A-shares, long-only` `2024-01 to 2026-04, plus 10 short windows` `5/15 bp` - Masks firm identity to control memory leakage. Barra attribution shows returns are largely market and style exposure. [Code](https://github.com/MaYiding/OracleProto)
- [Look-Ahead-Bench](https://arxiv.org/abs/2601.13770) - `5 US tech stocks` `2021-04 to 2021-09 vs 2024-07 to 2024-12` `costs not stated` - Compares periods before and after model cutoffs. Standard LLMs lose more than 15 percentage points of alpha while point-in-time models hold up. [Code](https://github.com/benstaf/lookaheadbench)
- [FINSABER](https://arxiv.org/abs/2505.07078) - `US, 100+ symbols` `two decades` `$0.0049 per share` - Tests LLM timing strategies over long horizons and finds them too conservative in bull markets and too aggressive in bear markets. Includes an audit of how short earlier evaluations were. [Code](https://github.com/waylonli/FINSABER)

## Factor Mining

- [AlphaLedger](https://arxiv.org/abs/2610.23744) - `A-shares, dynamic pool and point-in-time CSI 500` `2025-01 to 2026-07` `5/15 bp` - Distills portfolio-grounded feedback into a generator. Earns 55.4% annualized on its dynamic universe and 31.6% on CSI 500, where the strongest baseline earns 22.0%.
- [AlphaRLM](https://arxiv.org/abs/2610.14959) - `CSI 300, CSI 1000, S&P 500` `2021 to 2024` `5/15 bp` - Trains a language model on financial literature for symbolic factors, using five seeds and the same generation budget for LLM baselines.
- [AlphaDiverse](https://arxiv.org/abs/2609.29014) - `4 A-share universes` `2026-01 to 2026-08, evaluated once` `gross, long-only` - Keeps research feedback in an inner period and freezes the model before a single outer evaluation. Its trained local model reaches RankIC 0.0407 against 0.0398 for GPT-5.5 in the same workflow.
- [FaVOR](https://arxiv.org/abs/2608.30192) - `CSI 500, S&P 500` `2025` `CSI 500 5/15 bp, S&P 500 sell 5 bp` - States a hypothesis first and then applies a statistical gate, which turns out to carry most of the result. [Code](https://github.com/damilab/FaVOR)
- [AlphaSchema](https://arxiv.org/abs/2607.26642) - `CSI 300, 5-day target` `2023-01 to 2025-12` `5/15 bp` - Searches over semantic plans and includes reruns of RD-Agent and QuantaAlpha. [Code](https://github.com/JingyangYi/AlphaSchema)
- [AlphaMemo](https://arxiv.org/abs/2606.20625) - `CSI 500, S&P 500, 20-day target` `2022 to 2025-12` `costs not stated` - Stores search-process memory for self-evolving agents and reruns several baselines. Table values changed between v1 and the current version, so cite the version. [Code](https://github.com/jarrettyu/alphamemo)
- [AlphaCrafter](https://arxiv.org/abs/2605.05580) - `CSI 300, S&P 500` `paper trading 2026-03 to 2026-06` `CSI 300 2 bp, S&P 500 1 bp` - Paper-trades after all backbone cutoffs. On S&P 500 with Claude Opus 4.6, it reaches Sharpe 1.60 against 1.58 for AlphaAgent on the same backbone.
- [AlphaLogics](https://arxiv.org/abs/2603.20247) - `CSI 500, S&P 500` `2021-01 to 2024-12` `CSI 500 5/15 bp, S&P 500 sell 5 bp` - Derives market logics before generating factors and aligns budgets across LLM baselines. Its S&P 500 IC of 0.0093 with AR 13.75% is a gap worth inspecting.
- [FactorEngine](https://arxiv.org/abs/2603.16365) - `CSI 300, CSI 500` `2017-01 to 2024-12` `1.5 bp commission, 5 bp stamp duty, 8 bp slippage` - Has the longest test window here and itemized costs. With an equal information budget it ties AlphaAgent (IC 0.0315 vs 0.0314), and the larger gain (0.0474) comes from research-report seeds.
- [FactorMiner](https://arxiv.org/abs/2602.14670) - `A-shares at 10 minutes, 64 Binance assets` `2025` `no costs, left to future work` - Combines skills with experience memory and evaluates IC and related diagnostics.
- [QuantaAlpha](https://arxiv.org/abs/2602.07085) - `CSI 300, transfer to CSI 500 and S&P 500` `2022-01 to 2025-12` `5/15 bp` - Evolves whole research trajectories. Reruns under other protocols give IC 0.0208 in AlphaSchema and S&P 500 AR -8.57% in AlphaRLM. We have not audited the current code. [Code](https://github.com/QuantaAlpha/QuantaAlpha)
- [Alpha-R1](https://arxiv.org/abs/2512.23515) - `S&P 500, CSI 300, point-in-time membership` `2025` `10 bp two-sided` - Selects factors by market context and reports a rare multiple-testing check, with Deflated Sharpe of 0.39 to 0.85 under N = 101 to 158 trials. [Code](https://github.com/FinStep-AI/Alpha-R1)
- [CogAlpha](https://arxiv.org/abs/2511.18850) - `CSI 300, also CSI 500, S&P 500, HSI, HSCI` `2021-01 to 2024-12` `5/15 bp` - Evolves factors at the code level. The public repository is a prompt release. [Code](https://github.com/OpenFinMind/CogAlpha_Prompt)
- [EFS](https://arxiv.org/abs/2507.17211) - `US50, HSI45, CSI 300` `US50 2019 to 2025, others from 2022` `gross, plus a 10 bp block` - Evolves factors for sparse portfolios and adds a post-2025 check to isolate the period after LLM cutoffs.
- [Navigating the Alpha Jungle](https://arxiv.org/abs/2505.11122) - `CSI 300, CSI 1000, appendix S&P 500` `2021-01 to 2024-11` `15 bp per trade` - Runs Monte Carlo tree search over formulas with LLM refinement.
- [R&D-Agent(Q)](https://arxiv.org/abs/2505.15155) - `CSI 300, appendix CSI 500 and NASDAQ 100` `2017-01 to 2020-08, appendix 2024 to 2025` `5/15 bp` - Sets the common closed loop of hypothesis, code, backtest and feedback. FactorBench reruns it at raw IC 0.0253 and residual IC 0.0011. [Code](https://github.com/microsoft/RD-Agent)
- [AlphaAgent](https://arxiv.org/abs/2502.16789) - `CSI 500, S&P 500` `2021-01 to 2025-01` `CSI 500 5/15 bp, S&P 500 sell 5 bp` - Regularizes exploration to counter alpha decay. FactorBench reruns it at raw IC 0.0030 and long-short Sharpe -0.087 ± 0.432. [Code](https://github.com/RndmVariableQ/AlphaAgent)
- [Alpha-GPT](https://arxiv.org/abs/2308.00016) - `WorldQuant competition` `IQC 2024` `not applicable` - Introduces human-AI interactive alpha mining. Its main evidence is a competition result comparable to top human participants, which cannot be reproduced offline.

## Trading Agents

These papers test on a handful of symbols over short windows, so read [FINSABER](https://arxiv.org/abs/2505.07078) before their Sharpe ratios.

- [AI-Trader](https://arxiv.org/abs/2512.10971) - `US stocks, A-shares, crypto` `2025-10 to 2025-11, live` `costs not stated` - Benchmarks autonomous agents in live markets, which avoids backtest leakage over a window of about two months. [Code](https://github.com/HKUDS/AI-Trader)
- [Trading-R1](https://arxiv.org/abs/2509.11420) - `large-cap US stocks and ETFs` `2024-06 to 2024-08` `costs not stated` - Trains a reasoning model for trading and reports Sharpe 1.88 on NVDA over three months. [Code](https://github.com/TauricResearch/Trading-R1)
- [TradingAgents](https://arxiv.org/abs/2412.20138) - `AAPL, NVDA, MSFT, META, GOOGL` `2024-01 to 2024-03` `costs not stated` - Mirrors a trading firm with specialized agents and is a common reference system in this field. [Code](https://github.com/TauricResearch/TradingAgents)
- [FinCon](https://arxiv.org/abs/2407.06567) - `8 stocks, two 3-stock portfolios` `2022-10 to 2023-06` `costs not stated` - Organizes agents in a manager-analyst hierarchy. The linked repository holds a README and a license. [Code](https://github.com/The-FinAI/FinCon)
- [FinAgent](https://arxiv.org/abs/2402.18485) - `AAPL, AMZN, GOOGL, MSFT, TSLA, ETHUSD` `2023-06 to 2024-01` `commission rate not found` - Adds chart and other multimodal inputs to a tool-augmented agent.
- [FinMem](https://arxiv.org/abs/2311.13743) - `5 US stocks, per the FINSABER audit` `2022-10 to 2023-04` `costs not stated` - Gives a trading agent layered memory and a configurable character. [Code](https://github.com/pipiku915/FinMem-LLM-StockTrading)

## Text and Cross-Stock Signals

- ⭐ [CrossAlpha](https://github.com/Persdre/CrossAlpha) - `US, Japan, Taiwan, South Korea, Hong Kong` `2015 to 2026 price panel` `2 bp one-way` - Provides an annual-report benchmark for cross-market factor research, published in Findings of EMNLP 2026. No independent rerun that we know of.
- [Cross-Stock Predictability via LLM-Augmented Semantic Networks](https://arxiv.org/abs/2604.19476) - `S&P 500 constituent list` `2011 to 2019` `costs not stated` - Filters 10-K similarity edges with an LLM, lifting long-short Sharpe from 0.742 to 0.820.
- [Can ChatGPT Forecast Stock Price Movements?](https://arxiv.org/abs/2304.07619) - `US stocks` `2021-10 to 2024-05` `34 bp per day before costs` - Links headline scores to next-day returns. The authors say exploiting this is only feasible for participants with very low costs, such as market makers.

## Surveys

- [Agentic Quantitative Trading](https://arxiv.org/abs/2608.31041) - Surveys workflows, systems and evaluation for agentic trading.
- [Towards Autonomous Formulaic Alpha Discovery](https://arxiv.org/abs/2608.01789) - Takes an evolutionary computation view and warns that repeated optimization against the same setup can create collective overfitting.
- [Large Language Models for Stock Price Forecasting from a Hedge-Fund Perspective](https://arxiv.org/abs/2605.05211) - Reviews LLM forecasting from a practitioner's angle.
- [From Deep Learning to LLMs](https://arxiv.org/abs/2503.21422) - Surveys AI in quantitative investment across both eras.

## Related Lists

- [Awesome LLM Quantitative Trading Papers](https://github.com/Tom-roujiang/Awesome-LLM-Quantitative-Trading-Papers) - Broad and well-organized recent papers, including financial benchmarks.
- [Awesome Trading Agents](https://github.com/LLMQuant/awesome-trading-agents) - Open-source agents, MCP servers and skills.
- [Awesome AI in Finance](https://github.com/georgezouq/awesome-ai-in-finance) - AI in finance more broadly.
- [Awesome LLM Trading Agents](https://github.com/bettyguo/awesome-llm-trading-agents) - Frameworks, datasets and common methodology pitfalls.
- [Awesome Quant](https://github.com/wilsonfreitas/awesome-quant) - Libraries and resources for quantitative finance.

## Contributing

Contributions are welcome, including corrections from authors. Please read the [contribution guidelines](contributing.md).

## Footnotes

Papers awaiting annotation are in the [backlog](backlog.md).

To cite this list:

```bibtex
@misc{wang2024awesomellmtrading,
  title        = {Awesome LLM Trading},
  author       = {Wang, Qian},
  year         = {2024},
  howpublished = {\url{https://github.com/Persdre/Awesome-LLM-Trading-Papers}}
}
```
