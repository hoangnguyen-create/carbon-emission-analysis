# carbon-emission-analysis
## Introduction
<img width="640" height="427" alt="image" src="https://github.com/user-attachments/assets/2d1148ef-bd29-44d8-b239-90d3ada09b80" />

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



