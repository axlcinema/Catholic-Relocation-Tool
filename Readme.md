# ✝️ Catholic Family Relocation Assessment Tool

A weighted, interactive decision tool for freedom-loving Catholic families (or individuals) evaluating countries for permanent relocation. Built as a single standalone HTML file that runs in any browser, optimized for iPhone.

![Data](https://img.shields.io/badge/Countries-48-c9a96e)
![Criteria](https://img.shields.io/badge/Criteria-17-c9a96e)
![Climates](https://img.shields.io/badge/Climate_Types-6-c9a96e)
![License](https://img.shields.io/badge/License-MIT-green)

## What It Does

You rate what matters to you on a 1-5 star scale (or skip factors entirely), pick your preferred climate(s), select your family situation, and the tool ranks 48 countries against your personal priorities using a weighted scoring algorithm.

Every family has different non-negotiables. This tool respects that instead of giving you a generic "top 10 countries" listicle.

## Features

### 🌍 48 Countries Across 7 Regions

- **EU (27):** All member states from Portugal to Cyprus
- **Non-EU Europe (3):** Andorra, Switzerland, Montenegro
- **Eurasia (3):** Georgia, Turkey, Russia
- **Middle East (2):** UAE, Qatar
- **Asia (3):** Philippines, Thailand, South Korea
- **Americas (13):** USA, Mexico, Argentina, Chile, Uruguay, Paraguay, Costa Rica, Ecuador, Panama, Colombia, Brazil, Peru, Guatemala, Dominican Republic, Belize

### 📊 17 Weighted Criteria

Each country is scored 1-10 on:

Catholic Community & Mass, Homeschooling Freedom, Parental Vaccine Choice, Mountains & Nature, Cost of Living, Low Taxation, Safety & Low Crime, English Accessibility, Infrastructure Quality, Gun Ownership Rights, Water Quality, Travel Accessibility, Food Quality, Residency Pathway, Property Ownership, Political Trajectory, Traditional Latin Mass Access

### 🌤️ Multi-Select Climate Preference

Six climate profiles, select one or more:

- ☀️ Mediterranean
- 🍂 Four Seasons
- 🌴 Tropical
- 🌧️ Cool & Green
- 🏔️ Alpine
- 🏜️ Dry & Arid

Countries are scored against each climate type independently (1-10), so a country like Italy that offers both Mediterranean coastline and Alpine mountains gets properly rewarded when you select both.

### 👤 Family Scenario Mode

Choose your situation and the tool pre-adjusts priority weights:

- **Single:** Boosts cost of living, language, taxation, travel
- **Couple:** Boosts safety, property rights, taxation, residency
- **Family with Kids:** Boosts safety (+2), homeschooling (+2), Catholic community, vaccine choice, water quality, food quality

All pre-set weights are fully customizable on the next screen.

### ⏭️ Skip (Ignore) Toggle

Each priority factor has a "SKIP" button. Tap it to completely remove that factor from scoring. A single person who doesn't care about homeschooling freedom can skip it entirely rather than giving it 1 star (which still adds weight). Skipped factors also disappear from expanded country views and comparison tables.

### 🏦 Double Taxation Treaty (DTA) Checker

Select your home country from the dropdown and instantly see which destination countries have active double taxation treaties with it. Supported home countries:

USA, UK, Ireland, Canada, Australia, South Africa, Germany

DTA status appears as a green/red badge on each country card and in comparison view.

### 📊 Side-by-Side Comparison

Tap the "+" button on up to 4 countries, then hit "Compare" to see a factor-by-factor table including climate match scores and DTA status.

### 📱 Mobile-First Design

- iPhone-optimized touch targets and viewport
- Safe area insets for notch/dynamic island
- Add to Home Screen support (standalone web app)
- No scrolling issues, no pinch-zoom jank
- Works offline once loaded (no server required)

## How to Use

### Option 1: Open directly

Download `catholic-relocation-tool.html` and open it in any browser. That's it. No server, no dependencies, no build step.

### Option 2: Host it

Upload the single HTML file to any web host, S3 bucket, GitHub Pages, or Netlify. Send the URL to anyone.

### Option 3: iPhone Home Screen App

1. Open the file in Safari (either locally or via URL)
2. Tap the Share button (square with arrow)
3. Tap "Add to Home Screen"
4. It now appears as a standalone app with its own icon

## Scoring Algorithm

```
For each country:
  weighted_sum = 0
  total_weight = 0

  For each non-skipped factor:
    weighted_sum += user_star_rating × country_factor_score
    total_weight += user_star_rating

  climate_score = average of country scores for all selected climate types
  weighted_sum += 3 × climate_score
  total_weight += 3

  final_score = weighted_sum / total_weight  (out of 10)
```

Countries are then sorted by final score descending. Top 3 get medal indicators.

## Data Sources

All country scores are based on data from:

- Heritage Foundation Economic Freedom Index 2025
- Fraser Institute Human Freedom Index 2025
- Global Peace Index
- HSLDA International (homeschooling laws)
- Small Arms Survey (civilian firearm data)
- Numbeo (cost of living, safety indices)
- Our World in Data (vaccination policies)
- IRS, HMRC, Revenue.ie (double taxation treaty lists)
- Latin Mass Directory, Una Voce (TLM availability)

Data last updated: **February 2026**

## Tech Stack

- React 18 (CDN, no build step)
- Babel standalone (JSX transpilation in-browser)
- Google Fonts (Playfair Display + Source Sans 3)
- Pure CSS (no framework, CSS custom properties for theming)
- Zero dependencies, zero build tools, single file

## Customization

### Adding a Country

In the `C` array, add an object following this pattern:

```javascript
{
  id: "countryid",       // unique lowercase identifier
  n: "Country Name",     // display name
  f: "🇽🇽",              // flag emoji
  r: "Region",           // one of: EU, Europe, Americas, Asia, Middle East, Eurasia
  s: [7,5,7,7,6,5,9,7,8,2,7,9,8,8,9,5,5],  // 17 scores (1-10) matching F array order
  cl: {med:10,four:4,trop:3,cool:5,alp:3,dry:5},  // climate profile scores
  nt: "Notes about this country."  // displayed when expanded
}
```

Also add a DTA entry in the `DT` object:

```javascript
countryid: [1,1,0,1,0,0,1]  // [USA,UK,IE,CA,AU,ZA,DE] - 1=treaty exists, 0=no treaty
```

If your region is new, add it to the `REGS` array.

### Adding a Home Country for DTA

Add the country name to the `HC` array, then add a new position (index) to every entry in the `DT` object.

### Adding a Criterion

Add to the `F` array, then add a score at the corresponding index position in every country's `s` array.

## Contributing

PRs welcome, especially for:

- Country score corrections with sources
- New countries with complete data
- Additional home countries for DTA data
- UX improvements
- Accessibility improvements

## License

MIT

## Disclaimer

This tool is for informational and decision-support purposes only. Country scores reflect a combination of publicly available indices and editorial judgment relevant to traditionally-minded Catholic families. Scores are not legal, tax, or immigration advice. Always consult qualified professionals before making relocation decisions. Verify all data independently, as laws and conditions change.