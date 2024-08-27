# Coronavirus-EDA

### Project brief

---
This project utilized a comprehensive dataset that encompasses COVID-19 data from around the globe. This dataset includes information on confirmed cases, deaths, testing, vaccinations and other relevant variables that provide a holistic view of the pandemic's progression. Using SQL queries and Microsoft SQL Server, an in-depth analysis on the COVID-19 dane was done.


### Tools
---
- MYSQL data analysis software
    - [Download here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

### Queries worked with
  ---

select * 
from [Portfolio project]..['covid vaccinations data$']
where continent is not null
order by 3,4

select * 
from [Portfolio project]..['covid deaths data$']
order by 3,4

select location, 
date, 
total_cases, 
new_cases, total_deaths, population 
from 
[Portfolio project]..['covid deaths data$']

--percentage of deaths in select countries across the continents 

SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%Nigeria%' 
ORDER BY 1,2

SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%united%' 
ORDER BY 1,2 desc
-- united states - John hopkins university estimates are similar to database total cases and death percentage as of 2024. 

-- united kingdom
SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%kingdom%' 
ORDER BY 1,2

SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%New zealand%' 
ORDER BY 1,2

SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%china%' 
ORDER BY 1,2

SELECT location, 
       date, 
       total_cases, 
       new_cases, 
       total_deaths, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_deaths / total_cases) * 100 
       END AS DeathPercentage 
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%brazil%' 
ORDER BY 1,2

-- Percentage with covid 
SELECT location, 
       date, 
	   Population
       total_cases, 
       CASE 
           WHEN total_cases = 0 THEN 0 
           ELSE (total_cases / population) * 100 
       END AS percentofinfectedpopulation
FROM [Portfolio project]..['covid deaths data$'] 
WHERE location LIKE '%Nigeria%' 
ORDER BY 1,2 

-- Highest infection rate by population
SELECT location, 
	   Population,
       max(total_cases) as Highestinfections,  
	   max((total_cases / Population)) * 100 as Percentpopulationinfected 
FROM [Portfolio project]..['covid deaths data$'] 
group by location, Population
ORDER BY Percentpopulationinfected desc

-- Countries with death count by population
SELECT Location, max(total_deaths) as total_deathcount 
FROM [Portfolio project]..['covid deaths data$'] 
where continent is not null
group by location, Population
ORDER BY total_deathcount desc

-- Deathcount by continent 
SELECT continent, max(total_deaths) as total_deathcount 
FROM [Portfolio project]..['covid deaths data$'] 
where continent is not null
group by continent
ORDER BY total_deathcount desc

-- Global numbers 
select date,
	sum(new_cases) as Total_cases, 
    sum(new_deaths) as Total_deaths,
	case
           when sum(new_cases) = 0 THEN 0 
           else (sum(new_deaths) / sum(new_cases)) * 100
       END AS DeathPercentage 
from [Portfolio project]..['covid deaths data$'] 
where continent is not null
group by date
order by 1,2

select 
	sum(new_cases) as Total_cases, 
    sum(new_deaths) as Total_deaths,
	case
           when sum(new_cases) = 0 THEN 0 
           else (sum(new_deaths) / sum(new_cases)) * 100
       END AS DeathPercentage 
from [Portfolio project]..['covid deaths data$'] 
where continent is not null
order by 1,2

select *
from [Portfolio project]..['covid vaccinations data$']

-- Total population vs vaccinaton
select *
from [Portfolio project]..['covid deaths data$'] dea
join [Portfolio project]..['covid vaccinations data$'] vac
on dea.location = vac.location
and dea.date = vac.date

select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
from [Portfolio project]..['covid deaths data$'] dea
join [Portfolio project]..['covid vaccinations data$'] vac
on dea.location = vac.location
and dea.date = vac.date
where dea.continent is not null

select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
sum(coalesce(convert(bigint, vac.new_vaccinations),0)) over(Partition by dea.location order by dea.location, dea.date) 
as Rolling_people_vaccinated
from [Portfolio project]..['covid deaths data$'] dea
join [Portfolio project]..['covid vaccinations data$'] vac
on dea.location = vac.location
and dea.date = vac.date
where dea.continent is not null
order by 2,3
