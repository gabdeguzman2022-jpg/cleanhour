# CleanHour

**Grid carbon scheduling.** CleanHour pulls live grid carbon-intensity data from real grid operators and helps you shift power-hungry tasks (a dishwasher, laundry, EV charging) into the cleanest window in the next 48 hours — instead of running them whenever, on whatever mix of coal, gas, or renewables happens to be on the grid at that moment.

Built for [NextStep Hacks 2026](https://nextstep2026.devpost.com/) — Earth Forward track.

![CleanHour screenshot](.thumbnail)

## What it does

- **Now**: live carbon intensity for your zone, current generation mix, 24h trend.
- **Schedule**: add a task (duration, power draw, deadline), and CleanHour finds the lowest-carbon window that still meets your deadline, respecting a household power cap. Shows the real CO₂ saved vs. running it right now.
- **Forecast Lab**: an honest accuracy scorecard — CleanHour's own forecast vs. the grid operator's own forecast (where one is published) vs. what actually happened. It's a real comparison, not a rigged one: the model doesn't always win.

## 17 zones, all live, all real

| Region | Source |
|---|---|
| Great Britain | National Grid ESO (free, keyless, official real-time + 48h forecast) |
| France | RTE / eco2mix (free, keyless, official real-time CO₂) |
| Germany | SMARD / Bundesnetzagentur (free, keyless, per-fuel generation) |
| California, Texas, Mid-Atlantic, Midwest, New York, New England, Georgia, Florida, Great Plains, Pacific Northwest, Carolinas, Tennessee Valley, Arizona, Colorado | U.S. EIA Hourly Electric Grid Monitor (free API key) |

No zone in the picker is fake or a placeholder — every one of them pulls a real number from a real grid operator when you load the page.

## Why "honest" matters here

Most carbon-intensity demos either fabricate their forecast accuracy or quietly skip showing it at all. CleanHour computes a genuine backtest: for Great Britain, a per-time-of-day bias correction fit on the grid operator's own historical forecast-vs-actual data; for zones with no published forecast, two real baselines (naive persistence vs. a 3-day smoothed average) computed from measured generation. The Forecast Lab tab shows the real result — including the zones where the simple baseline beats CleanHour's own model, because that's what actually happened.

## Running it

This is a single self-contained HTML file (`CleanHour.dc.html`) with no build step and no backend server — every API call happens client-side, directly from your browser to the grid operators' public endpoints. Open `index.html` (or `CleanHour.dc.html` directly) in a browser, or visit the GitHub Pages link.

To add your own EIA API key (free, instant, at [eia.gov/opendata](https://www.eia.gov/opendata/register.php)), edit the `API_KEYS` constant near the top of the `<script>` block in `CleanHour.dc.html`.

## Tech notes

- Zero dependencies beyond React (via CDN, for the rendering runtime) — no build tooling, no npm install.
- Real-world data quirks handled honestly rather than papered over: EIA's ~27h publish lag means near-term accuracy stats correctly show "n/a" instead of a fabricated number; SMARD's per-fuel reporting lag is detected and filtered so a partially-reported hour never skews the mix.
