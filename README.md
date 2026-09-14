# Slow Cooked - the world trades calories, not nutrition

An interactive companion to our submission for the **Women in Data Datathon 2026**, theme *What's Cooking?*

**Track:** Trade × Eat
**Team:** Mansi Modi · Amia Nagpal · Gargee Nimdeo · Alexandra Harlan

---

## The question

> Which countries have traded their way into a starch-rich, micronutrient-poor food supply, and does it show up in women's anemia?

The reasoning: starch, sugar and oil ship well and don't spoil. Vegetables, eggs and fish don't. So the global food trade selects, without anyone deciding it, for calories over nutrition. Cereals, oil and sugar are 57% of the average diet. If trade pushes countries toward starch, and starch is low in absorbable iron, women in trade-dependent countries should be more anemic.

We tested that four separate ways. It failed all four.

## The finding

**Trade is not the cause. Diet composition is, and it holds even after income.**

| | |
|---|---|
| Import dependence vs anemia | **−0.184** — and the sign points the wrong way |
| Import share vs starch share | **−0.448** — starchy countries trade *least* |
| Diet change within a country, 13 years | not significant |
| Diet composition as a 3-year lead signal | not significant |
| Commodity prices of iron-rich food | no relationship |
| **Starch share of diet vs anemia** | **+0.315** — survives everything below |

The starch effect holds after malaria (+0.271), water and sanitation (+0.256), fertility and stunting (+0.235) and iron supplementation (+0.241). Five rival explanations, and it never reaches zero.

**The paradox that frames it:** Niger supplies 24.2 mg of iron per person per day against a requirement of 18, and 47% of its women are anemic - because 81% of that iron comes from cereals and beans, where the body absorbs very little of it. Quantity is not the problem. Form is.

---

## What's in the site

Three tabs in one page.

**Model** — the cross-sectional regression running live in the browser. Load any of 161 countries, then move three sliders and watch the predicted anemia rate change. The import slider is deliberately greyed out: sweeping it across its entire range moves the estimate by under one point, while starch moves it by twenty-three. That contrast is the finding, discovered by hand rather than asserted.

Below it, a scatter of all 161 countries - click any dot to load it into the model, search by name, colour by income band, toggle names and the trend line.

**Write-up** — eight sections: the question, *Pick your starting point*, what we got wrong, what held, the method, the sources, the limits, and the team.

**Findings** - ten sections carrying the full analysis: the premise tested, the twelve starch-trap countries, what the trap costs women, the nine explanations that were tested, movement over time, the 2030 outlook, the damped projections, the recommendations, sources and limits, and the correlation matrix. Eight charts and thirteen tables.

## The model

```
anemia% = 40.02
        + 5.07  × import share
        + 31.98 × starch share
        + 0.49  × micronutrient adequacy
        − 3.15  × log(GDP per capita, PPP)
```

Ordinary least squares, 167 countries, **R² = 0.463**. Starch coefficient z = 5.435, p < 0.001, CI 20.45 to 43.52.

## Data

| Source | What it gave us |
|---|---|
| FAOSTAT Food Balance Sheets | starch share of calories, import dependence |
| FAOSTAT Food Security Indicators | anemia in women 15–49, water and sanitation |
| FAOSTAT Supply-Utilization Accounts | iron supply in mg/capita/day by food group |
| World Bank | GDP per capita PPP, anemia prevalence, commodity prices |
| WHO · DHS | malaria incidence, iron supplementation coverage |

178 countries, 2010 to 2023. All open and public. `country_data.json` holds the 161 countries with complete records that the site runs on.

## What this cannot tell you

- National food supply, **not** what any individual woman ate. Food is not shared equally inside a household and we have no data that reaches the table.
- **Twenty countries are missing** from the FAO data entirely, including Mali, Chad and Somalia, which carry some of the world's worst anemia. Six more have no World Bank anemia figure.
- **Structural, not a lever.** Across countries, starchier diets go with more anemia. Track one country over thirteen years and its own diet changes do not move its own anemia. This identifies who is at risk today; it does not prove changing diet tomorrow would fix it.
- The model explains **46%** of the variation. The rest is healthcare, genetics, fortification and geography.
- **Nothing here is causal.** Every relationship is an association, reported with its controls.

---

## Running it

No build step, no dependencies. Two files.

```bash
# any static server — the page fetches country_data.json, so file:// won't work
python -m http.server 8000
# then open http://localhost:8000
```

## Deploying to Vercel

It's a static site, so there is nothing to configure.

1. Go to [vercel.com/new](https://vercel.com/new) and import this repository
2. Framework preset: **Other**
3. Leave build command and output directory empty
4. Deploy

Vercel serves `index.html` at the root and `country_data.json` alongside it.

## Files

```
index.html          the entire site: markup, styles, logic, and both datasets
                    embedded, so it opens straight from disk with no server
country_data.json   161 countries · starch share, import dependence, anemia,
                    iron supply, GDP, malaria, 2030 projections
findings.json       import-share trend, food groups, density gaps, the trap
                    classification and the damped projections
```

## Notes

Built as a single self-contained file so it survives being zipped, emailed, or opened from a Drive folder on an unknown machine. Motion runs through one token that switches off under `prefers-reduced-motion`. The palette was checked for colourblind separation before use.

There's an easter egg. Click the wordmark.
