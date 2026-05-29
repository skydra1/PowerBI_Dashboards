# PowerBI_Dashboards
Portfolio repository containing multiple Power BI dashboard projects focused on data cleaning, SQL analysis, DAX measures, KPI tracking, and interactive business insights.
# ESPN Cricinfo - India vs South Africa ODI Data Analysis Dashboard

An interactive, end-to-end Power BI data analysis project that visualizes comprehensive cricket statistics across batting, bowling, and fielding profiles. The underlying dataset was extracted and compiled from **ESPN Cricinfo**, capturing historical **One Day International (ODI) match data specifically between India and South Africa** to evaluate player career performances, efficiency metrics, and head-to-head longevity patterns.

---

## 🚀 Dashboard Interface

### 1. Batting Data Analysis
Evaluates run-scoring traits, volume of boundaries (6s/50s/100s), aggregate averages, and scoring paces across custom career spans during India vs. South Africa ODI encounters.

![Batting Analysis Dashboard](Dashboard_Sreenshots\Batting.png)

### 2. Bowling Data Analysis
Tracks workload thresholds (Overs/Matches), wicket-taking efficiency (Strike Rate), economy control, and peak execution (Best Innings/Five-Wicket hauls) against opposition lineups.

![Bowling Analysis Dashboard](Dashboard_Sreenshots\Bowling.png)

### 3. Fielding Data Analysis
Showcases glovework and ground fielding efficiency, analyzing stumpings, catch distributions (Wicketkeeper vs. Fielder), and dismissal-to-innings ratios for these rivalries.

![Fielding Analysis Dashboard](Dashboard_Sreenshots\Fielding.png)

---

## 📊 Core Features & Design Enhancements

* **Dynamic Data Aggregations:** Fixed default configuration anomalies by shifting metric evaluations from raw record counts to proper mathematical logic (`SUM`, `AVERAGE`).
* **Custom Dynamic Cards:** Leveraged the modern Power BI multi-metric KPI containers to create a unified, responsive grid UI across all dashboard views.
* **Integrated Slicers:** Enabled seamless multi-page cross-filtering using dedicated `Player` drop-down menus to isolate individual player profiles instantly.
* **Custom Assets:** Formatted utilizing transparent vector silhouette assets themed specifically to match individual analytical contexts (Batting, Bowling, and Fielding).

---

## 🛠️ Data Architecture & Modeling

### Data Sourcing
* **Source Platform:** ESPN Cricinfo 
* **Dataset Scope:** Historical One Day International (ODI) match records involving India vs South Africa.
* **Data Context:** Detailed historical tracking columns mapping aggregate spans, match metrics, innings counts, ball/delivery histories, and specific dismissal events.

### Advanced Logic Implementation (Data Binning)
To enable granular performance categorization, a dedicated conditional dimensions mapping table was designed to segment players by their absolute boundaries. Below is the structural logic applied to categorize modern scoring rates:

| Strike Rate Bound | Performance Category |
| :---: | :--- |
| **0 – 39** | Very Low |
| **40 – 75** | Low |
| **76 – 120** | Moderate |
| **121 – 200** | High |

This logical taxonomy can be dynamically generated or hardcoded into the relational layer via the following scalable DAX statement:

```dax
StrikeRate_Categories = 
VAR BaseTable = GENERATESERIES(0, 200, 1)
RETURN
    ADDCOLUMNS(
        BaseTable,
        "Category", 
        SWITCH(
            TRUE(),
            [Value] < 40, "Very Low",
            [Value] <= 75, "Low",
            [Value] <= 120, "Moderate",
            "High"
        )
    )