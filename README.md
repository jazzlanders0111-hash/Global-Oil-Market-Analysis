<div align="center">

# Global Oil Market Analysis
### 60 Years of Oil: Supply, Demand, Power, and Price

*Built on data through December 2025. In February 2026, a military strike on Iran triggered the largest oil supply disruption since 1973, making every finding in this project immediately relevant.*

**Python · pandas · Plotly · SciPy · EIA · World Bank · FetchSeries**

</div>

---

## What This Is About

Oil is the world's most traded commodity. Its price shapes what you pay at the pump, what airlines charge for tickets, what governments can afford to do. Six datasets, six notebooks, one question: how does the global oil market actually work, and who controls it?

Three themes run through everything:

**The US Shale Revolution changed who has power.** America went from importing most of its oil to becoming the world's largest producer. That shift permanently reduced OPEC's ability to control prices alone, which is why they had to invite Russia into OPEC+ in 2016.

**The market mechanism works, but it is weak and fast.** Supply surpluses push prices down, but the signal gets priced in within weeks, not months. Geopolitics, OPEC decisions, and speculation all hit prices at the same time, so the supply-demand balance alone only weakly predicts direction.

**OPEC cuts only work in the right conditions.** They succeed when demand is stable. They fail when demand is collapsing. And they are becoming less effective as US shale keeps growing outside OPEC's reach.

---

## The Data

All six datasets are available through [FetchSeries](https://www.fetchseries.com/oil/).

| Dataset | Original Source | What It Covers | Frequency | Period |
|---|---|---|---|---|
| Oil prices | World Bank Pink Sheets | World avg, Europe, Asia, North America (USD/bbl) | Monthly | 1960-2026 |
| Production by country | EIA STEO | 43 countries, world total, OPEC (Mb/d) | Monthly | 1993-2027* |
| Consumption by region | EIA STEO | 7 regions, 8 major countries (Mb/d) | Monthly | 1990-2027* |
| Global crude stocks | FetchSeries Research | World total, 39 countries (million barrels) | Monthly | 2012-2025 |
| US crude stocks | EIA | Commercial, SPR, total (million barrels) | Weekly | 1982-2026 |
| US tight oil | EIA STEO | Total shale, 8 formations: Permian, Bakken, Eagle Ford etc. (Mb/d) | Monthly | 2000-2026 |

*Includes EIA forward projections through 2027. Forecast and actual data are kept separate throughout.

**Mb/d** = million barrels per day. The world produces and consumes roughly 100 to 108 million barrels every single day.

---

## The Notebooks

### NB01: Data Engineering

Six datasets, three sources, different formats, different units, different time coverage. This notebook loads and cleans all of them, then merges them into three analysis-ready datasets that every other notebook reads from.

The main challenge: US stocks are published weekly (2,267 rows) while everything else is monthly. The weekly data was resampled by averaging each month's weekly readings, bringing it into alignment without losing the underlying signal.

The global stocks dataset only starts in 2012, so the full six-source merge is limited to 2012-2025. For analysis that does not need stocks data, like the shale revolution and OPEC power shift, a broader dataset going back to 1993 was used instead.

EIA production and consumption data extends to 2027 because it includes STEO forward projections. These are kept completely separate from observed actuals throughout the analysis.

One data quality note worth documenting: thirteen OPEC member countries had a uniform 22-month gap at the start of the production series (January 1993 to October 1994). This reflects EIA coverage at the time, not corrupted data. Since the analysis focuses on post-2000 dynamics, these gaps have no impact on any finding.

**Market baseline at end-2025:**

| Metric | Value |
|---|---|
| World oil price | $60.88/bbl |
| World production | 108.0 Mb/d |
| World consumption | 106.0 Mb/d |
| Supply-demand balance | +2.06 Mb/d (surplus) |
| US shale production | 9.30 Mb/d |
| US shale as share of world | 8.6% |
| OPEC share of world | 32.0% |
| Global crude stocks | 1,848 million barrels |

Production is running 2 Mb/d above consumption. That modest surplus is directly consistent with price sitting at the lower end of the post-COVID range.

---

### NB02: Price History and Major Shocks

Sixty-six years of monthly oil prices with every major spike and crash annotated. The point is to show that oil price movements are not random: every major swing has an identifiable cause.

**The 10 historical price eras:**

| Era | Period | Avg Price | Volatility | What drove it |
|---|---|---|---|---|
| Pre-OPEC | before 1973 | $1.47 | 3.1%/mo | Oil practically given away. Western companies controlled everything |
| OPEC Shock | 1973-1985 | $21.75 | **18.7%/mo** | Two OPEC supply shocks permanently repriced the commodity |
| Price Collapse | 1986-1998 | $17.61 | 9.5%/mo | Saudi Arabia flooded the market to punish quota cheaters |
| China Demand Boom | 1999-2007 | $39.00 | 8.2%/mo | China's economic rise drove a decade-long demand surge |
| Financial Crisis | 2008-2009 | $79.37 | 12.8%/mo | $132 peak then crashed to $41 in five months |
| High Price Era | 2010-2014 | **$97.67** | 5.8%/mo | Highest sustained price in history |
| Shale Crash | 2015-2019 | $55.22 | 8.8%/mo | US shale supply glut; OPEC tried to kill it with low prices |
| COVID | 2020-2021 | $55.16 | 17.2%/mo | Demand collapsed. WTI briefly went negative in April 2020 |
| Energy Crisis | 2022-2023 | $88.93 | 8.8%/mo | Ukraine war spike, then gradual unwind |
| Current | 2024 onward | $72.52 | 5.0%/mo | Modest surplus, moderate prices |

The OPEC Shock era at 18.7% monthly volatility is the most chaotic in the entire dataset, nearly double the COVID era. When people talk about today's market being volatile, the 1970s were in a completely different category.

**Major crash severity, peak to trough:**

| Event | Peak | Trough | Drop |
|---|---|---|---|
| 1985-86 Saudi Price War | $28.60 | $9.62 | -66% |
| 1990-1998 Post-Gulf decline | $34.50 | $10.41 | -70% |
| 2008 Financial Crisis | $132.83 | $41.34 | -69% |
| 2014-2016 Shale Crash | $108.37 | $29.78 | -72% |
| 2020 COVID Crash | $61.63 | $21.04 | -66% |
| 2022-2023 Energy unwind | $116.80 | $73.26 | **-37%** |

Four of the five major crashes fell 66 to 72% from peak to trough. The 2022-2023 unwind was only -37%, which is what a managed decline looks like. OPEC+ actively cut production on the way down and provided a floor. Every other crash was either unmanaged or deliberately accelerated.

**EIA forecast through 2027:**

The EIA projects a growing supply surplus averaging +2.40 Mb/d through 2026-2027. Based on the historical relationship between surplus and price, that implies continued downward pressure from the $60-70 range, unless OPEC+ responds with coordinated cuts.

**Key charts:** Full 60-year annotated price history (13 event markers, 12-month rolling average); price volatility bar chart by era coloured by average price level.

---

### NB03: The Shale Revolution

In 2008 the United States produced 7.1 Mb/d and had been declining for decades. By 2026 it was producing 23.6 Mb/d total liquids. That growth came almost entirely from one technology: hydraulic fracturing of tight rock formations. This notebook traces how it happened and what it did to the global balance of power.

**US production milestones:**

| Year | Production | Context |
|---|---|---|
| 2008 | 7.12 Mb/d | The low point, declining for decades |
| 2019 | 20.48 Mb/d | All-time peak before COVID |
| 2020 | 16.26 Mb/d | COVID crash, wells shut in as prices collapsed |
| 2026 | 23.58 Mb/d | New all-time high |

Growth from 2008 to 2019: **+187%**

*These are EIA total liquids figures, including crude oil, natural gas liquids, biofuels, and refinery gains. Crude oil only comparisons put the 2019 peak at roughly 13 Mb/d.*

**The Permian Basin takeover:**

US shale comes from eight geological basins across Texas, North Dakota, Wyoming, and surrounding states. The mix has shifted dramatically:

| Year | Permian share of all US shale |
|---|---|
| 2000 | 13.9% |
| 2010 | 22.8% |
| 2015 | 23.3% |
| 2020 | **53.2%** |
| 2025 | **63.8%** |

The Permian jumped from 23% to 53% between 2015 and 2020. Every other formation plateaued or declined while the Permian kept going. The reason is geology: the Permian sits on multiple stacked rock layers, each separately productive. Drillers can extract from a new layer without moving to a new location, which dramatically reduces costs.

Permian production from 2010 to today: 0.15 Mb/d to 5.95 Mb/d. That is **+3,777%.**

**The 2014 OPEC price war:**

When US shale became impossible to ignore, OPEC chose to flood the market rather than cut. The goal was to make prices so low that shale producers would go bankrupt.

| Month | OPEC Production | Oil Price |
|---|---|---|
| Jun 2014 | 32.65 Mb/d | $108.37 |
| Nov 2014 | 33.20 Mb/d | $76.99 |
| Jun 2015 | 34.54 Mb/d | $61.31 |
| Dec 2015 | 34.77 Mb/d | $36.57 |
| Jun 2016 | 35.26 Mb/d | $47.69 |
| Dec 2016 | 36.01 Mb/d | $52.62 |

Every six months prices fell and OPEC produced more, not less. They added 3.4 Mb/d to an already oversupplied market. Hundreds of smaller shale companies went bankrupt. But Permian operators cut their break-even costs below $40/bbl and survived. By late 2016 OPEC admitted the strategy had failed and formed OPEC+ with Russia.

**Key charts:** US vs OPEC vs Russia vs Saudi Arabia production lines (1993-2026); stacked area chart of US shale production by formation (2000-2026).

---

### NB04: Supply, Demand, and the Market Mechanism

Does the oil market follow basic economics? When supply exceeds demand, do prices fall? And how quickly?

**Does surplus predict falling prices?**

```
Balance vs next-month price change:
  Pearson r = -0.142
  p-value   = 0.0047
  Significant: YES
```

Yes, but weakly. Surplus and price changes move in opposite directions as expected, but the correlation is modest at -0.14. Geopolitics, OPEC decisions, speculation, and currency movements all hit prices simultaneously, so supply-demand balance is one input among many.

**How fast does the market respond?**

```
Lag  0 months: r = -0.260  p < 0.001  strongest
Lag  1 months: r = -0.142  p = 0.005
Lag  2 months: r = -0.046  p = 0.365  gone
Lag  3+ months: not significant
```

The signal peaks at lag zero and disappears by lag two. Supply-demand information is already priced in within weeks. Traders watch this data in real time and adjust immediately, so there is no delayed signal to exploit.

**Where demand growth is coming from:**

Every region increased oil consumption since 1990, except one:

| Region | Start | Latest | Change |
|---|---|---|---|
| Asia and Oceania | 14.22 Mb/d | 41.88 Mb/d | **+195%** |
| Middle East | 3.50 Mb/d | 9.45 Mb/d | +170% |
| Africa | 2.07 Mb/d | 5.27 Mb/d | +155% |
| C&S America | 3.88 Mb/d | 7.39 Mb/d | +90% |
| North America | 20.24 Mb/d | 24.98 Mb/d | +23% |
| **Europe** | **15.17 Mb/d** | **13.97 Mb/d** | **-8%** |

Europe is the only declining region, driven by electrification, efficiency improvements, and a deliberate post-Ukraine policy shift.

**China and India are the 21st century demand story:**

| Country | 1990 | Latest | Growth |
|---|---|---|---|
| China | 2.33 Mb/d | 17.04 Mb/d | **+632%** |
| India | 1.17 Mb/d | 6.27 Mb/d | **+437%** |
| United States | 16.99 Mb/d | 20.73 Mb/d | +22% |

China went from using roughly the same oil as France in 1990 to using more than the entire European continent today. India is following the same path, about 15 to 20 years behind.

**Key charts:** Supply-demand balance bar chart coloured green for surplus and red for deficit, with price panel below; major economies consumption lines (US, China, India, Russia, Japan) from 1990 to 2027.

---

### NB05: OPEC and the Power Shift

OPEC controlled nearly 40% of world production at its peak. Today it controls 32% and needed Russia's help through OPEC+ just to maintain pricing influence. This notebook traces that decline and tests whether production cuts actually move prices.

**OPEC market share over time:**

| Period | OPEC share | US share |
|---|---|---|
| 1993-2000 | 37.8% | 12.9% |
| 2001-2008 | 37.4% | 10.5% |
| 2009-2014 | 36.3% | 12.3% |
| 2015-2019 | 35.3% | 16.8% |
| 2020-2025 | **32.0%** | **20.9%** |

As OPEC's share fell from 37.8% to 32.0%, the US share nearly doubled from 10.5% to 20.9%. Roughly 5 to 6 million barrels per day of production influence shifted from OPEC-controlled to non-OPEC sources over 30 years.

**Do OPEC production cuts actually work?**

| Event | Cut Size | Price at Cut | Price 3 Months Later | Change | Result |
|---|---|---|---|---|---|
| 1998 Asian crisis | -2.5 Mb/d | $13.50 | $12.70 | -5.9% | NO |
| Post-9/11 2001 | -1.5 Mb/d | $25.21 | $18.52 | -26.5% | NO |
| Financial crisis Oct 2008 | -2.0 Mb/d | $72.69 | $43.86 | -39.7% | NO |
| GFC deepens Jan 2009 | -4.2 Mb/d | $43.86 | $50.28 | +14.6% | **YES** |
| OPEC+ founding Nov 2016 | -1.8 Mb/d | $45.26 | $54.35 | +20.1% | **YES** |
| COVID record cut May 2020 | -9.7 Mb/d | $30.38 | $43.44 | +43.0% | **YES** |
| Post-Ukraine Nov 2022 | -2.0 Mb/d | $87.38 | $80.25 | -8.2% | NO |
| Surprise cuts Apr 2023 | -1.7 Mb/d | $82.46 | $78.98 | -4.2% | NO |

3 out of 8. The pattern explains everything. The three failures in 1998, 2001, and October 2008 all happened while demand was actively collapsing. No supply cut stops a price crash when nobody is buying. The three successes in January 2009, 2016, and 2020 all happened when demand was stable or recovering: the floor was already there, and removing supply tightened the balance.

The 2022 and 2023 failures represent a newer problem: cuts too small to overcome structural surplus in a market where US shale keeps adding supply outside OPEC's control.

**Key charts:** OPEC vs US share of world production over time with OPEC+ formation marked; Saudi Arabia production vs price dual panel, showing the swing producer role.

---

### NB06: Stocks as a Market Signal

Oil inventories are the market's physical buffer. When production runs ahead of consumption, the surplus goes into storage. The EIA publishes US crude stock data every Wednesday and markets react within minutes. This notebook tests whether inventory levels actually predict where prices are going next.

**US commercial crude stocks:**

| Metric | Value |
|---|---|
| Historical minimum | 247 million barrels |
| Historical maximum | 541 million barrels |
| Latest reading | 443 million barrels |

At 443 million barrels, US commercial stocks sit near the midpoint of their historical range.

**Does high inventory predict falling prices?**

```
US stocks z-score vs next 3-month price change:
  Pearson r = +0.182   p = 0.0001   Significant: YES
  Direction is positive, not the negative you would expect.
```

This counterintuitive result has a real explanation. During demand-led boom periods like 2010-2014 and post-COVID recovery, both inventories and prices rose at the same time. Refineries were buying and storing oil because demand was strong and expected to stay that way. That demand-driven co-movement overwhelms the supply-glut signal in the data.

```
Global stocks vs current price:          r = -0.327  significant
Global stocks vs price 3 months later:   r = +0.081  NOT significant
```

Global stocks are negatively correlated with current price. But they have no predictive power for where prices will be in three months. Inventory data is published and priced in immediately. By the time you observe a high stock reading, its price implication has already been traded.

**The Strategic Petroleum Reserve:**

| Metric | Value |
|---|---|
| Created | 1975, response to the 1973 OPEC embargo |
| Peak level | 727 million barrels (January 2010) |
| Current level | 415 million barrels |
| Total drawdown | 311 million barrels, down **42.8%** from peak |

The SPR has been deployed four times: the 1990 Gulf War, Hurricane Katrina in 2005, the 2011 Libya disruption, and the 2022 Ukraine war, which drew 180 million barrels in six months. The March 2026 IEA coordinated release of 400 million barrels, triggered by the Strait of Hormuz disruption, draws directly on this resource.

**Key charts:** SPR history 1982-2026, annotated with all four major releases; US commercial stocks vs price dual panel with 5-year normal range band.

---

## How Everything Connects

The six notebooks tell one coherent story where each finding supports the next.

**NB01** sets the baseline: +2.06 Mb/d surplus, $60.88, US shale at 8.6% of world supply.

**NB02** shows where $60.88 sits historically: well below the $97.67 average of 2010-2014, consistent with a market in surplus. The current conditions are not unusual; they follow directly from the supply-demand picture.

**NB03** explains where the surplus came from. US shale grew from near zero to 9.23 Mb/d, with the Permian alone up 3,777%. That supply sits outside OPEC's control. It is why prices cannot stay above $80 for long before American drillers respond by adding more.

**NB04** confirms the surplus is already reflected in prices. The market adjusts within weeks. The EIA projects the surplus widening to 3.66 Mb/d by early 2027, implying continued downward pressure. Asia's +195% demand growth is the only structural force large enough to absorb it.

**NB05** shows why OPEC cannot simply cut its way out. Cuts work in stable demand but fail during demand destruction, and they are progressively less effective as non-OPEC supply keeps growing. OPEC+ formed in 2016 because OPEC alone no longer had enough market share to move the needle.

**NB06** closes the loop. US commercial stocks at 443 million barrels confirm a market in modest surplus. The SPR's depletion from 727 to 415 million barrels leaves a thinner buffer for future crises, which turned out to matter almost immediately.

**Then real life interrupted the analysis.**

On February 28, 2026, while this project was being built on data through December 2025, military strikes on Iran triggered the largest supply disruption since the 1973 OPEC embargo. Gulf production fell 8 to 10 Mb/d. The Strait of Hormuz, through which roughly 20% of the world's oil normally flows, became effectively impassable for commercial tankers. Price jumped from $71 to over $100 in three weeks.

Every mechanism this project analyzes is playing out right now: the supply shock driving prices up (NB02), OPEC+ spare capacity as the only available buffer (NB05), strategic reserves released as an emergency response (NB06), and EIA forecasts immediately overtaken by events (NB04). The project built to analyze 60 years of history became a live framework for understanding what is happening today.

---

## Key Findings at a Glance

| Finding | Number | Notebook |
|---|---|---|
| All-time price low | $1.21/bbl (January 1970) | NB02 |
| All-time price high | $132.83/bbl (July 2008) | NB02 |
| Biggest single-month crash | -39.6% (March 2020, COVID) | NB02 |
| Most volatile era | OPEC Shock 1973-1985, 18.7%/month std dev | NB02 |
| US shale growth 2000-2026 | 0.25 to 9.23 Mb/d | NB03 |
| Permian Basin growth since 2010 | +3,777% | NB03 |
| Supply surplus predicts falling price | r = -0.14, p = 0.005, within 1 month | NB04 |
| Asia demand growth since 1990 | +195% | NB04 |
| China demand growth since 1990 | +632% | NB04 |
| Europe demand since 1990 | -8%, only declining region | NB04 |
| OPEC market share 1993 vs today | 37.8% to 32.0% | NB05 |
| US market share 2001 vs today | 10.5% to 20.9% | NB05 |
| OPEC cut success rate | 3 of 8 events | NB05 |
| US commercial stocks range | 247 to 541 million barrels | NB06 |
| SPR drawdown from peak | 727 to 415 million barrels, -42.8% | NB06 |
| Stocks as price predictor | Not reliable beyond 1 month | NB06 |

---

## Stack

```
Python 3.11
pandas        time series, resampling, joins
numpy         numerical operations
openpyxl      reading multi-sheet Excel files
plotly        all interactive charts
scipy         Pearson correlation and lag analysis
pyarrow       parquet file I/O for processed datasets
```

---

## How to Run

```
global-oil-market-analysis/
├── data/
│   ├── oil-prices-world-bank-pink-sheets.xlsx
│   ├── oil-production-supply-by-country-eia-steo.xlsx
│   ├── oil-consumption-demand-by-country-eia-steo.xlsx
│   ├── crude-oil-stocks-in-the-world-fsr.xlsx
│   ├── oil-stocks-in-the-us-eia.xlsx
│   └── tight-oil-production-in-the-us-eia-steo.xlsx
├── NB01_data_engineering.ipynb     ← run this first
├── NB02_price_history.ipynb
├── NB03_shale_revolution.ipynb
├── NB04_supply_demand.ipynb
├── NB05_opec_power.ipynb
└── NB06_stocks_signal.ipynb
```

```bash
pip install pandas numpy openpyxl plotly scipy pyarrow
```

Run NB01 first. It creates `data/processed/` and writes all the parquet files the other notebooks read from. After that, the remaining notebooks can run in any order.

---

*Datasets sourced from [FetchSeries](https://www.fetchseries.com/oil/). Original data providers: World Bank Pink Sheets (prices), EIA Short-Term Energy Outlook (production, consumption, shale), EIA weekly reports (US stocks), FetchSeries Research (global stocks). EIA production and consumption forecasts for 2026-2027 are revised monthly and reflect the dataset download date.*
