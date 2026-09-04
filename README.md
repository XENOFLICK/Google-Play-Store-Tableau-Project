# App Insights Unlocked: Google Play Store Data Analytics Challenge

## 📌 Project Overview
This project presents a comprehensive data analytics case study on **Google Play Store applications** executed within **Tableau**. The core mission is to empower internal product managers, developers, and marketing teams to discover trends, uncover hidden market dynamics, and establish actionable benchmarks. 

By running data operations across a rich corpus of app names, category genres, ratings, installation footprints, download file sizes, and user review counts, this project bridges raw metadata into highly scannable visual indicators that optimize app engineering and deployment models.

---

## 💻 Requirements, Tools, and Technologies
* **Analytical Architecture:** Tableau Desktop (Workbook version 2026.2 / Hyper Extract engine)
* **Data Processing & Schema Management:** Tableau Calculated Fields (Regular Expressions, Type Casting, String Normalization, and Datetime Arithmetic)
* **Underlying Source Files:** `googleplaystore.csv` (App parameters catalog) joined via full relational schema mapping to `googleplaystore_user_reviews.csv` (Customer textual semantic feedback metrics)

---

## 🛠️ Data Preprocessing & Schema Engineering
Before initiating any front-end visualization, the raw string attributes were normalized into math-ready variables using optimized Tableau Calculated Fields:

1. **`[Cleaned Rating]`**: Filters garbage metrics and structural errors, setting an absolute 5-star constraint boundary.
   ```tableau
   IF ISNULL([Rating]) OR FLOAT([Rating]) > 5 THEN NULL ELSE FLOAT([Rating]) END
   ```
2. **`[Cleaned Size (MB)]`**: Strips string markers ("M" and "k") and harmonizes file scales directly into common Megabytes (1 MB = 1024 KB).
   ```tableau
   IF ENDSWITH([Size], 'M') THEN FLOAT(REPLACE([Size], 'M', ''))
   ELSEIF ENDSWITH([Size], 'k') THEN FLOAT(REPLACE([Size], 'k', '')) / 1024
   ELSE NULL END
   ```
3. **`[Numerical Installs]`**: Employs structural regular expressions to clean custom string additions like "+" and "," symbols, instantly converting alphanumeric strings into actionable integers.
   ```tableau
   INT(REGEXP_REPLACE(REGEXP_REPLACE([Installs], '\+', ''), ',', ''))
   ```
4. **`[Days Since Last Update]` & `[Update Frequency Tiers]`**: Calculates total elapsed time relative to the historical collection endpoint to track stale versus active updates.
   ```tableau
   DATEDIFF('day', [Last Updated], {MAX([Last Updated])})
   ```

---

## 📊 Core Analytical Insights Uncovered
The custom Tableau dashboard sheets reveal clear strategic trends across all tiers of inquiry:

### 1. Basic-Level Findings
* **Platform Quality Baseline:** The platform maintains a high-quality global standard, with average application reviews clustering at **4.19 out of 5 stars**.
* **Market Footprint:** A substantial tier of top-tier entries exists, with **7,368 apps achieving distinct ranks of 4.0 stars or higher**.
* **Category Variety:** The marketplace features **34 unique categories** of diverse services.

### 2. Medium-Level Insights
* **Popularity vs. Satisfaction Correlation:** A binned scale study exposes that larger customer scopes yield more stable feedback loops. Apps grouped as **"Global Giants" (10M+ downloads)** manifest higher, less volatile scores than low-scale, newer properties.
* **Genre Category Leaders:** The **Education** and **Video Players** categories capture the highest average ratings across the board.
* **Category Size Trajectories:** Categories like **Family, Game, and Video Players** show significantly higher average file sizes, yet maintain maximum conversion, meaning customers easily accommodate heavy spatial limits for high-fidelity content.
* **Commercial Splits:** Free utilities spark considerably higher audience engagement, logging a significantly vast volume of consumer reviews compared to premium paid listings.

### 3. Advanced-Level Patterns
* **Sentiment Architecture:** By matching reviews to text strings, applications tracking **Positive Sentiments** map to tight clusters resting directly between the 4.2 and 4.7 score limits.
* **Code Maintenance Cycles:** A concerning segment of the store reveals stagnant code practices; over half of all listings register as **"Stale" (unaltered for over 365 days)**, whereas entries adjusted inside a 30-day index continue to pull live traffic.

---

## ⚠️ Challenges Faced and Mitigation Strategies
* **Anomalous Values:** The entry tables contained random data injection anomalies where the `Rating` field was populated with corrupted inputs above the 5.0 scale boundary.
  * *Mitigation:* Eliminated using strict boundary rules inside the field extract model (`IF FLOAT([Rating]) > 5 THEN NULL`).
* **Alphanumeric Artifacts:** Columns such as `Installs` contained characters ("Varies with device", "+") that blocked regular math aggregations.
  * *Mitigation:* Remediated using nested `REGEXP_REPLACE` statements to cleanse the schema back to mathematical arrays.

---

## 💡 Strategic Recommendations for Improvements

* **Optimize Storage Footprints Globally:** 
  If engineering application pipelines within **Family or Game** environments, maintain high resolution asset designs; consumer download trends indicate zero resistance to large space footprints here. However, when introducing utilities into **Tools or Productivity**, minimize space constraints to encourage rapid installs.
* **Prioritize Ad-Supported and Free Deployment Architecture:** 
  Free listings grab a massive share of text reviews and user engagement over paywalled alternatives. Build customer reach through open-access pipelines backed by internal microtransactions rather than hard paywalls.
* **Adopt Strict Continuous Deployment Sprints:** 
  Store algorithms favor recent changes, while properties slipping beyond 12 months sit dead in search indices. Implement systematic feature cycles every 30 to 90 days to retain organic performance.
* **Target Highly Rated Verticals:** 
  Focus engineering targets toward **Education** or **Video Players** genres; these paths show historically higher consumer acceptance values.

***

If you would like to enhance this documentation or expand the repository, let me know if you need help with:
* Creating a dynamic **Python implementation script** for automated dataset cleaning before ingestion.
* Drafting an executive **Presentation Deck Outline** tailored for reporting these findings to leadership.
* Developing a **Markdown template** for a deep-dive comprehensive report of the data outcomes.
