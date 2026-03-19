# 🦠 COVID-19 Data Exploration | SQL Analysis

> Part of my data analytics portfolio — [View all projects](https://github.com/KevinProbetsado)

## Overview

An exploratory data analysis of global COVID-19 trends using SQL Server, with a focused lens on the **Philippines and Southeast Asia**. This project investigates mortality rates, infection spread, and vaccination progress across countries — translating raw pandemic data into actionable insights.

**Tools:** Microsoft SQL Server · Tableau Public  
**Dataset:** [Our World in Data – COVID-19 Dataset](https://ourworldindata.org/covid-deaths) *(CovidDeaths & CovidVaccinations tables)*  
**Skills Demonstrated:** CTEs · Window Functions · Joins · Aggregate Functions · Temp Tables · Views

---

## Business Questions Answered

1. What was the **death rate** in the Philippines relative to confirmed cases over time?
2. What percentage of the **Philippine population** was infected at peak?
3. Which countries had the **highest infection rates** relative to their population?
4. Which countries recorded the **highest total death counts**?
5. What does the **global death percentage** look like across all reported cases?
6. How did **vaccination rollout** progress relative to each country's population?

---

## Key Findings

| Insight | Detail |
|---|---|
| PH Death Rate (peak) | Death % among confirmed PH cases peaked in early pandemic before testing scaled |
| Highest Infected Countries | Small European nations showed disproportionately high infection-to-population ratios |
| Global Death Rate | Approximately 1–2% of confirmed global cases resulted in death |
| Vaccination Coverage | Rolling vaccination tracked by country reveals significant rollout disparities |

> 📊 Full interactive visualizations available on [Tableau Public](https://public.tableau.com/app/profile/kevin.probetsado/vizzes)

---

## Project Structure

```
FirstProject/
│
└── firstproject.sql       # All SQL queries for data exploration and analysis
```

---

## SQL Highlights

### Death Rate in the Philippines
```sql
SELECT location, date, total_cases, total_deaths,
       (total_deaths / total_cases) * 100 AS DeathPercentage
FROM PortfolioProject..CovidDeaths$
WHERE location LIKE 'ph%'
ORDER BY 1, 2;
```

### Rolling Vaccination Progress (CTE)
```sql
WITH PopvsVac AS (
    SELECT dea.continent, dea.location, dea.date, dea.population,
           vac.new_vaccinations,
           SUM(CAST(vac.new_vaccinations AS int))
               OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date)
               AS rolling_people_vaccinated
    FROM PortfolioProject..CovidDeaths$ dea
    JOIN PortfolioProject..CovidVaccinations$ vac
        ON dea.location = vac.location AND dea.date = vac.date
    WHERE dea.continent IS NOT NULL
)
SELECT *, (rolling_people_vaccinated / population) * 100 AS pct_vaccinated
FROM PopvsVac;
```

---

## How to Run

1. Download the [Our World in Data COVID-19 dataset](https://ourworldindata.org/covid-deaths) and split into `CovidDeaths` and `CovidVaccinations` tables
2. Import both tables into a database named `PortfolioProject` in SQL Server
3. Open `firstproject.sql` and run queries sequentially in SSMS

---

## About Me

I'm an aspiring data analyst based in the Philippines, passionate about turning data into clear, compelling stories.

📫 [LinkedIn](https://www.linkedin.com/in/kevin-c-probetsado-aa9a64287/) · 📊 [Tableau Public](https://public.tableau.com/app/profile/kevin.probetsado/vizzes) · 🏠 [GitHub Profile](https://github.com/KevinProbetsado)

---

*Dataset source: Hannah Ritchie et al. (2020) — "Coronavirus Pandemic (COVID-19)". Published online at OurWorldInData.org.*
