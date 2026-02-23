# Carbon Emission Analysis
## Introduction
<img width="512" height="341" alt="image" src="https://github.com/user-attachments/assets/2d1148ef-bd29-44d8-b239-90d3ada09b80" />

This project aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
<img width="687" height="487" alt="image" src="https://github.com/user-attachments/assets/d7380b0a-7389-448c-9f68-74fb4376566e" />

### Table: product_emissions
Query: 
```SQL
select * from product_emissions p
limit 10;
```
Result: 
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

### Table: companies
Query: 
```SQL
select * from companies c
limit 5; 
```
Result: 
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

### Table: countries
Query: 
```SQL
select * from countries ctr
limit 5; 
```
Result: 
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|

### Table: industry_group
Query: 
```SQL
select * from product_emissions p
limit 10;
```
Result: 
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|

### Table: Total observation as products are duplicated
Query:
```SQL
SELECT
	count(id) as 'Total Observations',
	count(distinct product_name) as 'Number of distinct products'
FROM product_emissions p 
```
Result:
|Total Observations|Number of distinct products|
|------------------|---------------------------|
|1037|661|

### Question 1: Which products contribute the most to carbon emissions?
### Question 2: What are the industry groups of these products?

Query for Q1 & Q2: 
```SQL
SELECT 	product_name as 'Product name', 
		round(avg(carbon_footprint_pcf),2) as 'Avg. Carbon Footprint pcf',
		i.industry_group AS 'Industry Group'
FROM product_emissions p 
LEFT JOIN industry_groups i 
on p.industry_group_id = i.id
GROUP BY product_name
ORDER BY 2 DESC 
LIMIT 10; 
```
Result: 
|Product name|Avg. Carbon Footprint pcf|Industry Group|
|------------|-------------------------|--------------|
|Wind Turbine G128 5 Megawats|3718044.00|Electrical Equipment and Machinery|
|Wind Turbine G132 5 Megawats|3276187.00|Electrical Equipment and Machinery|
|Wind Turbine G114 2 Megawats|1532608.00|Electrical Equipment and Machinery|
|Wind Turbine G90 2 Megawats|1251625.00|Electrical Equipment and Machinery|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|Automobiles & Components|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|Materials|
|TCDE|99075.00|Materials|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|Automobiles & Components|
|Mercedes-Benz S-Class (S 500)|85000.00|Automobiles & Components|
|Mercedes-Benz SL (SL 350)|72000.00|Automobiles & Components|

#### Answer: 
Q1: Products that contribute the highest to carbon emissions are Wind Turbine:
- G128 5 Megawats
- G132 5 Megawats
- G114 2 Megawats

Q2: The industry of these products is Electrical Equipment and Machinery

### Question 3: What are the industries with the highest contribution to carbon emissions?
Query:
```SQL
SELECT i.industry_group,
	   round(sum(carbon_footprint_pcf),2) as 'Total Carbon Footprint pcf'
FROM product_emissions p 
LEFT JOIN industry_groups i 
on p.industry_group_id = i.id
GROUP BY i.industry_group
ORDER BY 2 DESC 
LIMIT 10;
```
Result: 
|industry_group|Total Carbon Footprint pcf|
|--------------|--------------------------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

Top 3 industries: 
- Electrical Equipment and Machinery
- Automobiles & Components
- Materials

### Question 4: What are the companies with the highest contribution to carbon emissions?
Query: 
```SQL
SELECT c.company_name,
	   round(sum(carbon_footprint_pcf),2) as 'Total Carbon Footprint pcf'
FROM product_emissions p 
LEFT JOIN companies c  
on p.company_id  = c.id
GROUP BY c.company_name
ORDER BY 2 DESC 
LIMIT 10; 
```
Result: 
|company_name|Total Carbon Footprint pcf|
|------------|--------------------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

Top 3 companies: 
- "Gamesa Corporación Tecnológica, S.A."
- Daimler AG
- Volkswagen AG

### Question 5: What are the countries with the highest contribution to carbon emissions?
Query:
```SQL
SELECT ctr.country_name,
	   round(sum(carbon_footprint_pcf),2) as 'Total Carbon Footprint pcf'
FROM product_emissions p 
LEFT JOIN countries ctr  
on p.country_id  = ctr.id
GROUP BY ctr.country_name
ORDER BY 2 DESC 
LIMIT 10;
```
Result: 
|country_name|Total Carbon Footprint pcf|
|------------|--------------------------|
|Spain|9786130.00|
|Germany|2251225.00|
|Japan|653237.00|
|USA|518381.00|
|South Korea|186965.00|
|Brazil|169337.00|
|Luxembourg|167007.00|
|Netherlands|70417.00|
|Taiwan|62875.00|
|India|24574.00|

Top 3 countries: 
- Spain
- Germany
- Japan

### Question 6: What is the trend of carbon footprints (PCFs) over the years?
Query: 
```SQL
SELECT year,
		round(sum(carbon_footprint_pcf),2) as 'Total Carbon Footprint pcf'
FROM product_emissions p 
group by year
```

Result: 
|year|Total Carbon Footprint pcf|
|----|--------------------------|
|2013|503857.00|
|2014|624226.00|
|2015|10840415.00|
|2016|1640182.00|
|2017|340271.00|

Answer: The carbon footprints increased over the year. It increased highest during 2014 - 2015 for 10,840,415 pcf and lowest in 2017 for 340,271 pcf

### Question 7: Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
Query: 
```SQL
SELECT 	i.industry_group,
		SUM(CASE WHEN p.year = 2013 THEN p.carbon_footprint_pcf ELSE 0 END) AS '2013',
		SUM(CASE WHEN p.year = 2014 THEN p.carbon_footprint_pcf ELSE 0 END) AS '2014',
		SUM(CASE WHEN p.year = 2015 THEN p.carbon_footprint_pcf ELSE 0 END) AS '2015',
		SUM(CASE WHEN p.year = 2016 THEN p.carbon_footprint_pcf ELSE 0 END) AS '2016',
		SUM(CASE WHEN p.year = 2017 THEN p.carbon_footprint_pcf ELSE 0 END) AS '2017'
FROM product_emissions p 
LEFT JOIN industry_groups i
on p.industry_group_id = i.id
group by i.industry_group
order by i.industry_group
```
Result:
|industry_group|2013|2014|2015|2016|2017|
|--------------|----|----|----|----|----|
|"Consumer Durables, Household and Personal Products"|0|0|931|0|0|
|"Food, Beverage & Tobacco"|4995|2685|0|100289|3162|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0|0|8909|0|0|
|"Mining - Iron, Aluminum, Other Metals"|0|0|8181|0|0|
|"Pharmaceuticals, Biotechnology & Life Sciences"|32271|40215|0|0|0|
|"Textiles, Apparel, Footwear and Luxury Goods"|0|0|387|0|0|
|Automobiles & Components|130189|230015|817227|1404833|0|
|Capital Goods|60190|93699|3505|6369|94949|
|Chemicals|0|0|62369|0|0|
|Commercial & Professional Services|1157|477|0|2890|741|
|Consumer Durables & Apparel|2867|3280|0|1162|0|
|Containers & Packaging|0|0|2988|0|0|
|Electrical Equipment and Machinery|0|0|9801558|0|0|
|Energy|750|0|0|10024|0|
|Food & Beverage Processing|0|0|141|0|0|
|Food & Staples Retailing|0|773|706|2|0|
|Gas Utilities|0|0|122|0|0|
|Household & Personal Products|0|0|0|0|0|
|Materials|200513|75678|0|88267|213137|
|Media|9645|9645|1919|1808|0|
|Retailing|0|19|11|0|0|
|Semiconductors & Semiconductor Equipment|0|50|0|4|0|
|Semiconductors & Semiconductors Equipment|0|0|3|0|0|
|Software & Services|6|146|22856|22846|690|
|Technology Hardware & Equipment|61100|167361|106157|1566|27592|
|Telecommunication Services|52|183|183|0|0|
|Tires|0|0|2022|0|0|
|Tobacco|0|0|1|0|0|
|Trading Companies & Distributors and Commercial Services & Supplies|0|0|239|0|0|
|Utilities|122|0|0|122|0|

Answer: The industry groups with the most significant decreases in carbon footprint overtime are:
- Pharmaceuticals, Biotechnology & Life Sciences
- Consumer Durables & Apparel
- Food & Staples Retailing
- Telecommunication Services
- Mining – Iron, Aluminum & Other Metals

The percentage decreases overtime of these indsutries are nearly 100%, for example for Pharmaceuticals, Biotechnology & Life Sciences - The industry was accounted for 32,271 pcf in 2013 but dropped to 0 after 2015 and onward. 

## Notable insight, interesting facts and patterns:
1. Food & Staples Retailing is responsible for producing many plastic, glass products like Nescafe, Coca-Cola bottle. Yet these fast moving consumers good product little to no carbon footprint as their average PCF is 0
2. The production of circuit breaker in Electrical Equipment and Machinery has the highest total carbon emission compared to the rest of the product & industry
3. Both Colombia and Lithuania are the leading countries in term of the highest air quality as both countries have 0 Carbon Footprint. Next is Greece which accounted only 1 total PCF




