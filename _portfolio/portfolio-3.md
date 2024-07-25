---
title: "Data Exploration In SQL"
excerpt: "<img src='/images/covid-19-profile.png'>"
collection: portfolio
---
Project Goal
====

The goal of this project is to conduct a comprehensive exploration and analysis of COVID-19 data to uncover insights into infection rates, mortality rates, vaccination progress, and their implications on public health and healthcare systems globally.

Objectives
====

- Exploration of COVID-19 Data: Analyze trends in total cases, deaths, and vaccination rates across different continents and countries to identify patterns and variations.

- Mortality Analysis: Investigate the relationship between total cases and deaths to understand mortality rates and assess the severity of the pandemic's impact in various regions.

- Infection and Vaccination Impact: Evaluate the percentage of population infected with COVID-19 and the progress of vaccination campaigns to gauge the effectiveness of public health responses.

- High Infection Rate Identification: Identify countries with the highest infection rates relative to their population size to highlight areas of significant COVID-19 spread.

- Regional Comparison: Compare continents to identify those with the highest mortality rates per population, providing insights into regional disparities and impacts.

Findings
===

Mortality Patterns: Variations in mortality rates indicate differing impacts of COVID-19 on healthcare systems and public health measures across countries.

``` sql
--looking at total cases vs total deaths
-- shows likelihood of dying if you contract covid in your country

select location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
from [Portfolio Project 1]..CovidDeaths
where location like '%states%'and continent is not null
order by 1,2

--showing countries with highest death count per population

select location, Max(cast(total_deaths as int)) as totaldeathcount
from [Portfolio Project 1]..CovidDeaths
-- where location like '%states%'
where continent is not null
group by location 
order by totaldeathcount desc

-- showing continents with highest death count per population

select location , Max(cast(total_deaths as int)) as totaldeathcount
from [Portfolio Project 1]..CovidDeaths
-- where location like '%states%'
where continent is null
and location not in ('World','European Union', 'International')
group by location 
order by totaldeathcount desc

```

 Infection Spread: Analysis of infection rates reveals countries with higher percentages of their population affected by COVID-19, highlighting areas of intense transmission.

```sql
-- Looking at countries with highest infection rate compared to Population

select location, population,date, Max(total_cases) as HighestInfectionCount, Max((total_cases /population))*100 as PercentPopulationInfected
from [Portfolio Project 1]..CovidDeaths
-- where location like '%states%'
where continent is not null
group by location, population, date
order by PercentPopulationInfected desc

```
 Vaccination Progress: Insights into vaccination rates show varying levels of immunization coverage, reflecting the effectiveness of vaccination campaigns and healthcare infrastructure.

```sql
-- Looking at total population vs vaccinations

Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
	--sum(cast(vac.new_vaccinations as int)) over (partition by dea.location) or
	sum(convert(int, vac.new_vaccinations)) over (partition by dea.location order by dea.location, dea.date) as rollingpeoplevaccinated
-- , (rollingpeoplevaccinated/population)*100
From [Portfolio Project 1]..CovidDeaths dea
Join [Portfolio Project 1]..CovidVaccinations vac
	On dea.location = vac.location 
	and dea.date = vac.date
where dea.continent is not null
order by 2,3


-- With CTE 

with popvsvac (continent, location, date, population, new_vaccinations, rollingpeoplevaccinated)
as
(
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
	--sum(cast(vac.new_vaccinations as int)) over (partition by dea.location) or
	sum(convert(int, vac.new_vaccinations)) over (partition by dea.location order by dea.location, dea.date) as rollingpeoplevaccinated
-- , (rollingpeoplevaccinated/population)*100
From [Portfolio Project 1]..CovidDeaths dea
Join [Portfolio Project 1]..CovidVaccinations vac
	On dea.location = vac.location 
	and dea.date = vac.date
where dea.continent is not null
--order by 2,3
)
select *, (rollingpeoplevaccinated/population)*100 percentpopulationvaccinated
from popvsvac

```
Solution
====
This project involves querying and analyzing data from the "CovidDeaths" and "CovidVaccinations" tables within the COVID-19 database. Utilizing SQL queries, views, and temporary tables, the project aims to store and visualize data for deeper analysis and presentation of findings. By focusing on infection rates, mortality rates, and vaccination progress, this exploration seeks to provide actionable insights for healthcare professionals, policymakers, and researchers to enhance COVID-19 response strategies globally.