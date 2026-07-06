# ✝️ Catholic Family Relocation Assessment Tool

A weighted, interactive decision tool for freedom-loving Catholic families (or individuals) evaluating countries for permanent relocation. Built as a single standalone HTML file that runs in any browser, optimized for iPhone.

![Data](https://img.shields.io/badge/Countries-59-c9a96e)
![Criteria](https://img.shields.io/badge/Criteria-17-c9a96e)
![Climates](https://img.shields.io/badge/Climate_Types-6-c9a96e)
![License](https://img.shields.io/badge/License-MIT-green)

## What It Does

You rate what matters to you on a 1-5 star scale within a fixed 51-star budget (or skip factors entirely, or mark them as MUST-have dealbreakers), pick your preferred climate(s), select your family situation and citizenship, and the tool ranks 59 countries against your personal priorities using a weighted scoring algorithm.

Every family has different non-negotiables. This tool respects that instead of giving you a generic "top 10 countries" listicle — and countries that fail a non-negotiable are flagged as failing rather than quietly compensated by strong scores elsewhere.

## Features

### 🌍 59 Countries Across 9 Regions

- **EU (27):** All member states from Portugal to Cyprus
- **Non-EU Europe (4):** Andorra, Switzerland, Montenegro, United Kingdom
- **Eurasia (3):** Georgia, Turkey, Russia
- **Middle East (2):** UAE, Qatar
- **Asia (3):** Philippines, Thailand, South Korea
- **Oceania (2):** Australia, New Zealand
- **Africa (1):** South Africa
- **Americas (17):** USA, Canada, Mexico, El Salvador, Argentina, Chile, Uruguay, Paraguay, Costa Rica, Ecuador, Panama, Colombia, Brazil, Peru, Guatemala, Dominican Republic, Belize

### 📊 17 Weighted Criteria

Each country is scored 1-10 on:

Catholic Community & Mass, Homeschooling Freedom, Parental Vaccine Choice, Mountains & Nature, Cost of Living, Low Taxation, Safety & Low Crime, English Accessibility, Infrastructure Quality, Gun Ownership Rights, Water Quality, Travel Accessibility, Food Quality, Residency Pathway, Property Ownership, Political Trajectory, Traditional Latin Mass Access

### ⭐ Star Budget

You get **51 stars** (17 factors × 3) to distribute. Raising one factor above 3★ means lowering another below it — you can't rate everything 5★. A sticky budget bar shows stars spent/remaining and shakes red when you try to overspend; skipping a factor refunds its stars to the pool. This forces the question the tool exists to answer: *what actually matters most to you?*

(Math note: in a weighted average, uniform ratings cancel out — all-5s produces identical rankings to all-3s. The budget doesn't change the algorithm; it forces your input to contain real information.)

The Single / Couple / Family presets are budget-exact 51-star distributions you can fine-tune.

### 🚫 MUST (Dealbreaker) Mode

Every factor has a **MUST** button next to SKIP. Mark a factor as MUST and any country scoring **under 5/10** on it is flagged as failing: greyed out, sorted below all passing countries, and labelled with exactly which dealbreakers it fails and by how much (e.g. *"Fails: Parental Vaccine Choice (2/10)"*). Countries scoring **5–6/10** on a MUST pass, but carry an amber *"Borderline — verify closely"* flag so a threshold-skimming score can never look like a clean pass.

This exists because weighted averages hide dealbreakers — a country can score terribly on your one non-negotiable and still rank #1 on the strength of everything else. MUST mode makes that impossible.

### ⚠️ Interaction Warnings

Criteria interact in ways independent scores can't express. The classic trap: a country where homeschooling is legal *and* vaccines are "not mandatory for school" — but the vaccine mandate applies to all children regardless of schooling (Italy), or homeschooling is illegal so school vaccine rules bind everyone (Germany). Countries with known traps carry a ⚠️ marker in the rankings and a prominent amber warning banner when expanded.

### 🛂 Citizenship-Aware Residency

Select your citizenship and Residency Pathway scores adjust to your passport: EU citizens get free movement across all 27 EU states (and easier Swiss access), UK citizens get the Common Travel Area right to live in Ireland, US citizens get the USA. A badge shows when a score was adjusted for your passport.

### 🌤️ Multi-Select Climate Preference

Six climate profiles, select one or more (or none, to ignore climate entirely):

- ☀️ Mediterranean
- 🍂 Four Seasons
- 🌴 Tropical
- 🌧️ Cool & Green
- 🏔️ Alpine
- 🏜️ Dry & Arid

Countries are scored against each climate type independently (1-10), so a country like Italy that offers both Mediterranean coastline and Alpine mountains gets properly rewarded when you select both. A star rating controls how much climate matters relative to your other priorities.

### 💾 Persistence

Your ratings, dealbreakers, climate picks, scenario, and citizenship are saved in the browser (localStorage) and survive closing the app. "Start Over" wipes them.

### 👤 Family Scenario Mode

Choose your situation and the tool pre-adjusts priority weights:

- **Single:** Boosts cost of living, language, taxation, travel
- **Couple:** Boosts safety, property rights, taxation, residency
- **Family with Kids:** Boosts safety (+2), homeschooling (+2), Catholic community, vaccine choice, water quality, food quality

All pre-set weights are fully customizable on the next screen.

### ⏭️ Skip (Ignore) Toggle

Each priority factor has a "SKIP" button. Tap it to completely remove that factor from scoring. A single person who doesn't care about homeschooling freedom can skip it entirely rather than giving it 1 star (which still adds weight). Skipped factors also disappear from expanded country views and comparison tables.

### 🏦 Double Taxation Treaty (DTA) Checker

The citizenship dropdown doubles as the DTA selector — see which destination countries have active double taxation treaties with your home country. Supported home countries:

USA, UK, Ireland, Canada, Australia, South Africa, Germany

DTA status appears as a green/red badge on each country card and in comparison view.

### 📊 Side-by-Side Comparison

Tap the "+" button on up to 4 countries, then hit "Compare" to see a factor-by-factor table including climate match scores and DTA status.

### 📱 Mobile-First Design

- iPhone-optimized touch targets and viewport
- Safe area insets for notch/dynamic island
- Add to Home Screen support (standalone web app)
- Pinch-zoom enabled (accessibility)
- No server or build step — note that React and fonts load from CDNs, so the first load (and reloads with a cleared cache) need an internet connection

## How to Use

### Option 1: Open directly

Download `index.html` and open it in any browser. That's it. No server, no dependencies, no build step.

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
  scores = country_factor_scores
  if citizenship grants an easier residency pathway (EU free movement,
     UK↔Ireland CTA, home country): scores.residency = max(base, override)

  weighted_sum = 0
  total_weight = 0

  For each non-skipped factor:
    weighted_sum += user_star_rating × scores[factor]
    total_weight += user_star_rating

  If any climates selected:
    climate_score = average of country scores for selected climate types
    weighted_sum += climate_importance_stars × climate_score
    total_weight += climate_importance_stars

  final_score = weighted_sum / total_weight  (out of 10)

  fails = [each MUST factor where scores[factor] < 5]
```

Countries with no failed MUSTs are ranked by final score descending (top 3 get medals). Countries failing any MUST sort below all passing countries, greyed out, with their failures listed.

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

Data compiled: **February 2026** · Targeted corrections (vaccine mandates and homeschool regulations for Italy, Portugal, Germany, Poland, Czechia, Bulgaria, Greece, Switzerland, Malta; USA residency perspective) and five new countries — Canada, Australia (No Jab No Pay/Play), New Zealand, South Africa (BELA Act), El Salvador — researched against current law: **July 2026**

⚠️ **De-jure legality can be undone by de-facto administrative rules.** A country where homeschooling is "legal" and vaccines are "not mandatory" may still require school registration that enforces the vaccine schedule. The warning banners cover known cases, but always verify the interaction of rules — not just each rule in isolation — before committing.

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
  w: "Optional interaction warning shown as an amber banner.",  // omit if none
  nt: "Notes about this country."  // displayed when expanded
}
```

Score residency (index 13) from the perspective of a foreigner with none of the supported citizenships; citizenship-based overrides live in the `CIT` object.

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
