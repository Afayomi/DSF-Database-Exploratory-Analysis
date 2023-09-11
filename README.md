# DSF-Database-Exploratory-Analysis

INTERNATIONAL BREWERIES EXPLORATORY DATA ANALYSIS 

SELECT * FROM breweries

--CASE 1: What was the profit worth of the breweries in the last 3 years
SELECT DISTINCT years -- To see the range of the years
FROM breweries;

SELECT SUM(profit) AS Profit_worth
FROM breweries;

--CASE 2: Compare the profit between the anglophone and francophone territories
SELECT DISTINCT countries
FROM breweries;
  -- Anglophone = Nigeria, Ghana
SELECT countries, SUM(profit)
FROM breweries
WHERE countries IN ('Nigeria', 'Ghana')
GROUP BY countries
 -- Francophone = Senegal, Benin, Togo
SELECT countries, SUM(profit)
FROM breweries
WHERE countries IN ('Senegal', 'Benin', 'Togo')
GROUP BY countries;
        --OR
  -- Would first label each country as either anglophone or francophone, then find the total profit of the 2
ALTER TABLE breweries
ADD Territories VARCHAR; -- Adds a new column 'Territories to the table'

UPDATE breweries -- adding the appropriate territories for the countries
SET territories = 'Anglophone'
WHERE countries IN ('Nigeria','Ghana');

UPDATE breweries
SET territories = 'Francophone'
WHERE countries IN ('Senegal', 'Benin', 'Togo');

SELECT territories, SUM(profit) AS profit -- gives the profit per territory
FROM breweries
GROUP BY territories;

-- CASE 3: Country that generated the highest profit in 2019
SELECT countries,SUM(profit)
FROM breweries
WHERE years = 2019
GROUP BY countries
ORDER BY 2 DESC;

-- CASE 4: The year with the highest profit
SELECT years, SUM(profit)
FROM breweries
GROUP BY years
ORDER BY 2 DESC;

-- CASE 5: Which month in the three years was the least profit generated?
SELECT months, MIN(profit)
FROM breweries
GROUP BY months
ORDER BY 2 ASC;

-- CASE 6: What was the minimum profit in the month of December in 2018
SELECT months, years, MIN(profit)
FROM breweries
WHERE months ='December' AND years = 2018
GROUP BY months, years;

-- CASE 7: Compare the profit in percentage for each month
SELECT months, round(SUM(profit) * 100/SUM(SUM(profit)) over (),2) AS percentage
FROM breweries
WHERE years = 2019
GROUP BY months
ORDER BY 2 DESC;

-- CASE 8: Which particular brand generated the highest profit in Senegal?
SELECT brands, MAX(profit)
FROM breweries
WHERE countries = 'Senegal'
GROUP BY brands, countries
ORDER BY 2 DESC;

-- Case 9: Within the last two years, the brand manager wants to know the top three brands consumed in the francophone countries
SELECT DISTINCT brands, SUM(quantity) AS Amount_consumed, territories
FROM breweries
WHERE territories ='Francophone' AND years IN (2018,2019)
GROUP BY brands, territories
ORDER BY 2 DESC
LIMIT 3;

-- Case 10: Find out the top two choice of consumer brands in Ghana
SELECT DISTINCT brands, sum(quantity), countries
FROM breweries
WHERE countries = 'Ghana'
Group BY brands, countries
ORDER BY 2 DESC
LIMIT 2;

-- Case 11:Find out the details of beers consumed in the past three years in the most oil reached country in West Africa.
select distinct countries from breweries

-- The only oil rich country in the list is Nigeria
SELECT DISTINCT brands, plant_cost, unit_price, SUM(costs) AS COSTS, SUM(profit) AS PROFIT
FROM breweries
WHERE countries = 'Nigeria'
GROUP BY brands, plant_cost, unit_price;

-- Case 12: Favorites malt brand in Anglophone region between 2018 and 2019
select distinct brands from breweries

SELECT brands, SUM(quantity) AS Amount_consumed
FROM breweries
WHERE years IN (2018,2019) AND Brands IN ('grand malt', 'beta malt') AND territories = 'Anglophone'
GROUP BY brands;

SELECT brands, SUM(quantity) AS Amount_consumed
FROM breweries
WHERE years IN (2018,2019) AND Brands LIKE '%malt%' AND territories = 'Anglophone'
GROUP BY brands;

-- Case 13: Which brands sold the highest in 2019 in Nigeria?
SELECT DISTINCT brands, SUM(quantity) AS Amount_sold
FROM breweries
WHERE countries ='Nigeria' AND years=2019
GROUP BY brands
ORDER BY 2 DESC;

-- Case 14: Favorites brand in South_South region in Nigeria
SELECT DISTINCT brands, SUM(quantity) AS Amount_sold
FROM breweries
WHERE countries ='Nigeria' AND region ='southsouth'
GROUP BY brands
ORDER BY 2 DESC;

-- Case 15: Level of consumption of Budweiser in the regions in Nigeria
SELECT DISTINCT region, brands, SUM(quantity) AS Amount_consumed
FROM breweries
WHERE countries = 'Nigeria'  AND brands = 'budweiser'
GROUP BY region, brands
ORDER BY 3 DESC;

-- Case 16: Level of consumption of Budweiser in the regions in Nigeria in 2019
SELECT DISTINCT region, brands, SUM(quantity) AS Amount_consumed
FROM breweries
WHERE countries = 'Nigeria'  AND brands = 'budweiser' AND years= 2019
GROUP BY region, brands
ORDER BY 3 DESC;

-- Case 17: Country with the highest consumption of beer.
SELECT DISTINCT countries, SUM(quantity) AS Consumption
FROM breweries
WHERE brands NOT LIKE '%malt%'
GROUP BY countries
ORDER BY 2 DESC;

-- Case 18: Highest sales personnel of Budweiser in Senegal
SELECT DISTINCT sales_rep, SUM(quantity)
FROM breweries
WHERE countries = 'Senegal'
GROUP BY sales_rep
ORDER BY 2 DESC;

-- Case 19: Country with the highest profit of the fourth quarter in 2019
SELECT DISTINCT countries, SUM(profit)
FROM breweries
WHERE years=2019 AND months IN ('October', 'November', 'December')
GROUP BY countries
ORDER BY 2 DESC;
