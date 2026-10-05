# Mushroom Yield Model

Predict mushroom yield from grow-room conditions, using a simple model fitted on 122 cultivation trials across six species.

**Live site:** [Open the calculator](https://eggy0eggymicster-sudo.github.io/Mushroom-Growth-Predictor/)

Enter species, substrate, temperature, humidity and CO2, and get:

- predicted **biological efficiency (BE)** and fresh yield in grams
- a grade: Excellent, Good, Fair or Poor
- a red / yellow / green light for each condition
- a short "fix this first" list

> **BE** = fresh yield ÷ substrate dry weight × 100. A BE of 90% means 900 g of mushrooms from 1,000 g of dry substrate.

## Data source and scope

The project owner confirms that the 122 embedded records are their own measured cultivation trials. The source files do not specify collection location, exact trial dates, measurement protocol, or whether fresh yield covers one or multiple flushes. These details are not inferred. Statistical checks confirm internal consistency, not independent replication.

## The model

```
BE (%) = a[species | substrate] + 1.4905 × Temp(°F) − 0.18789 × CO2(ppm)
```

The result is clipped to 0 to 100 as a software constraint, not a biological maximum for BE. Predicted yield is `BE / 100 × substrate dry weight`. Each species and substrate pair has its own starting value `a`, listed on the site.

| Measure | Value |
|---|---|
| Trials | 122 |
| R² | 0.895 |
| Typical error | 5.5 BE points (residual standard deviation) |
| Leave-one-out error | 5.95 BE points |

Worked example: Blue Oyster on hardwood sawdust at 51°F and 470 ppm gives 105.09 + 1.4905 × 51 − 0.18789 × 470 ≈ 92.8% BE. The real trial came in at 100%.

## Using the calculator

Choose a measured species/substrate combination and enter temperature in °F, relative humidity in %, and CO2 in ppm. Humidity and optional substrate moisture affect diagnostic flags only; they do not change the yield equation. Moisture must be between 0 and 100%. Enter a positive substrate **dry** weight in grams to see fresh yield; leave it blank to see BE alone. A default weight is supplied initially and your entered weight is preserved when changing species or substrate.

The displayed error band is the prediction plus/minus one residual standard deviation, clipped to the model bounds. It is descriptive, not a validated confidence or prediction interval. Colonization days are a group average, not a forecast for your entered conditions. Conditions outside the observed ranges are marked as unsupported instead of receiving a favorable green flag.

## Key findings

- **CO2 is the strongest signal.** The fitted equation associates each extra 100 ppm with about 19 fewer BE points when its other inputs are held fixed; this is not an isolated causal effect. Trials at 750 ppm or more averaged about 50% BE, against 80% or better below that.
- **Summer records had lower yields.** Summer trials averaged about 53% BE against about 81% in other seasons.
- **Substrate groups differed.** Blue Oyster on hardwood sawdust reached about 100% BE in winter, against about 65% on supplemented sawdust.
- **Pink Oyster on wheat straw had lower winter yields:** 35 to 37% BE in winter against about 76% in fall.

## Scoring matrix

Points show how much better or worse trials did than the average for the same species and substrate.

| Factor | Band | Points | Light |
|---|---|---|---|
| CO2 (ppm) | under 750 | +5 to +18 | Green |
| | 750 and up | −16 to −17 | Red |
| Temperature (°F) | under 55 | +2 | Green |
| | 55 to 59.9 | −1 | Yellow |
| | 60 to 69.9 | +6 to +15 | Green |
| | 70 and up | −11 to −17 | Red |
| Humidity (%) | under 80 | −14 to −16 | Red |
| | 80 to 84 | +5 | Yellow |
| | 85 and up | +3 to +5 | Green |
| Moisture (%) | under 60 | −19 | Red |
| | 60 to 62 | −7 | Yellow |
| | 63 and up | 0 to +10 | Green |

Grades: **Excellent** 85+ · **Good** 75 to below 85 · **Fair** 60 to below 75 · **Poor** under 60.

Do not add all the points together. Summer conditions trigger every penalty at once, so the sum overstates the real drop. The predicted number comes from the equation; the lights are a diagnosis.

## Limitations

- **Temperature and CO2 move together** in this data (correlation 0.93), so their separate effects can't be pulled apart. The positive temperature term is a correction on top of CO2, not evidence that warmer is better.
- **Unusual combinations.** Cool rooms with high CO2, or warm rooms with low CO2, never occurred in the trials. The calculator warns when you enter one, because the estimate is less reliable there.
- **Small groups.** Several species and substrate pairs have only 4 to 6 trials.
- **Observational data.** The trials were not randomized, so the model shows association, not proven cause.
- **Tested range only:** 50.5 to 87.5°F, 65 to 96% humidity, 470 to 930 ppm CO2.
- **Known misses.** Blue Oyster on supplemented sawdust in winter is over-predicted by 10 to 14 points (about 64% observed, about 77% predicted). Pink Oyster on wheat straw in winter is over-predicted by about 6. Both point to a species-specific temperature effect the simple model leaves out.

## Validation and interpretation

Recalculation from the embedded records gives R² 0.895027, training RMSE 5.1972 BE points, residual standard deviation 5.4984, and leave-one-out RMSE 5.9525. All 122 IDs are unique; recorded BE agrees with yield/dry-weight calculations to the displayed precision. Leave-one-out validation holds out individual rows within this dataset and does not establish performance on another farm or independent batch.

Observed group averages are descriptive summaries, distinct from the regression intercepts. Seasonal averages pool substrates and are not isolated estimates of seasonal effects. Diagnostic bands are shared across species and should be treated as factors to investigate, not universal cultivation targets.

## Publish on GitHub Pages

This repo is a single self-contained page: `index.html` holds the calculator, charts, data and model.

1. Upload `index.html` and `README.md` to a new GitHub repository.
2. Open **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
3. After a minute the site is live at `https://eggy0eggymicster-sudo.github.io/Mushroom-Growth-Predictor/`.

## Roadmap

- Run trials that vary CO2 independently of temperature.
- Run each substrate in every season.
- Add a species-specific temperature term for cold-sensitive cases.
- Add spawn rate and colonization-time prediction.

## License

The supplied draft proposed MIT for code and CC BY 4.0 for data, but did not include a complete license or attribution record. No license grant is asserted in this revision; the owner should choose and supply the intended license before reuse terms are advertised.
