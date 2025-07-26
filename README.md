# Session-3.5
**Which products contribute the most to carbon emissions?**
```
SELECT product_name, SUM(carbon_footprint_pcf)
FROM product_emissions
GROUP BY product_name
ORDER BY SUM(carbon_footprint_pcf) DESC
LIMIT 10
```
| product_name                                                                                                                       | SUM(carbon_footprint_pcf) | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044                   | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187                   | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608                   | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625                   | 
| TCDE                                                                                                                               | 198150                    | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687                    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000                    | 
| Electric Motor                                                                                                                     | 160655                    | 
| Audi A6                                                                                                                            | 111282                    | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | 100621                    | 

**What are the industry groups of these products?**
```
WITH industry AS (
  	SELECT product_name, SUM(carbon_footprint_pcf), industry_group_id
	FROM product_emissions
	GROUP BY product_name
	ORDER BY SUM(carbon_footprint_pcf) DESC
	LIMIT 10
  )
  SELECT DISTINCT industry_groups.industry_group
  FROM industry_groups
  JOIN industry ON industry.industry_group_id = industry_groups.id
```
| industry_group                     | 
| ---------------------------------: | 
| Electrical Equipment and Machinery | 
| Materials                          | 
| Automobiles & Components           | 
| Capital Goods                      | 

**What are the industries with the highest contribution to carbon emissions?**
```
SELECT 
	industry_groups.industry_group, 
	SUM(product_emissions.carbon_footprint_pcf) AS total_pcf
FROM industry_groups
JOIN product_emissions ON product_emissions.industry_group_id = industry_groups.id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```
| industry_group                     | total_pcf | 
| ---------------------------------: | --------: | 
| Electrical Equipment and Machinery | 9801558   | 
| Automobiles & Components           | 2582264   | 
| Materials                          | 577595    | 
| Technology Hardware & Equipment    | 363776    | 
| Capital Goods                      | 258712    | 

**What are the industry groups of these products?**
```
WITH industry AS (
  	SELECT product_name, SUM(carbon_footprint_pcf), industry_group_id
	FROM product_emissions
	GROUP BY product_name
	ORDER BY SUM(carbon_footprint_pcf) DESC
	LIMIT 10
  )
  SELECT DISTINCT industry_groups.industry_group
  FROM industry_groups
  JOIN industry ON industry.industry_group_id = industry_groups.id
```
| industry_group                     | 
| ---------------------------------: | 
| Electrical Equipment and Machinery | 
| Materials                          | 
| Automobiles & Components           | 
| Capital Goods                      | 

**What are the companies with the highest contribution to carbon emissions?**
```
SELECT 
	companies.company_name, 
	SUM(product_emissions.carbon_footprint_pcf) AS total_pcf
FROM companies
JOIN product_emissions ON product_emissions.company_id = companies.id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```
| company_name                            | total_pcf | 
| --------------------------------------: | --------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464   | 
| Daimler AG                              | 1594300   | 
| Volkswagen AG                           | 655960    | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016    | 
| "Hino Motors, Ltd."                     | 191687    | 

**What are the countries with the highest contribution to carbon emissions?**
```
SELECT 
	countries.country_name, 
	SUM(product_emissions.carbon_footprint_pcf) AS total_pcf
FROM countries
JOIN product_emissions ON product_emissions.country_id = countries.id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```
| country_name | total_pcf | 
| -----------: | --------: | 
| Spain        | 9786130   | 
| Germany      | 2251225   | 
| Japan        | 653237    | 
| USA          | 518381    | 
| South Korea  | 186965    | 

**What is the trend of carbon footprints (PCFs) over the years?**
```
SELECT 
	year,
	SUM(carbon_footprint_pcf) AS total_pcf
FROM product_emissions
GROUP BY year
```
| year | total_pcf | 
| ---: | --------: | 
| 2013 | 503857    | 
| 2014 | 624226    | 
| 2015 | 10840415  | 
| 2016 | 1640182   | 
| 2017 | 340271    | 

**Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?**
```
SELECT 
	product_emissions.year,
	industry_groups.industry_group,
	SUM(product_emissions.carbon_footprint_pcf) AS total_pcf
FROM product_emissions
LEFT JOIN industry_groups ON industry_groups.id = product_emissions.industry_group_id
GROUP BY 1, 2
ORDER BY 2 ASC, 1 ASC
```
| year | industry_group                                                         | total_pcf | 
| ---: | ---------------------------------------------------------------------: | --------: | 
| 2015 | "Consumer Durables, Household and Personal Products"                   | 931       | 
| 2013 | "Food, Beverage & Tobacco"                                             | 4995      | 
| 2014 | "Food, Beverage & Tobacco"                                             | 2685      | 
| 2015 | "Food, Beverage & Tobacco"                                             | 0         | 
| 2016 | "Food, Beverage & Tobacco"                                             | 100289    | 
| 2017 | "Food, Beverage & Tobacco"                                             | 3162      | 
| 2015 | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 8909      | 
| 2015 | "Mining - Iron, Aluminum, Other Metals"                                | 8181      | 
| 2013 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271     | 
| 2014 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 40215     | 
| 2015 | "Textiles, Apparel, Footwear and Luxury Goods"                         | 387       | 
| 2013 | Automobiles & Components                                               | 130189    | 
| 2014 | Automobiles & Components                                               | 230015    | 
| 2015 | Automobiles & Components                                               | 817227    | 
| 2016 | Automobiles & Components                                               | 1404833   | 
| 2013 | Capital Goods                                                          | 60190     | 
| 2014 | Capital Goods                                                          | 93699     | 
| 2015 | Capital Goods                                                          | 3505      | 
| 2016 | Capital Goods                                                          | 6369      | 
| 2017 | Capital Goods                                                          | 94949     | 
| 2015 | Chemicals                                                              | 62369     | 
| 2013 | Commercial & Professional Services                                     | 1157      | 
| 2014 | Commercial & Professional Services                                     | 477       | 
| 2016 | Commercial & Professional Services                                     | 2890      | 
| 2017 | Commercial & Professional Services                                     | 741       | 
| 2013 | Consumer Durables & Apparel                                            | 2867      | 
| 2014 | Consumer Durables & Apparel                                            | 3280      | 
| 2016 | Consumer Durables & Apparel                                            | 1162      | 
| 2015 | Containers & Packaging                                                 | 2988      | 
| 2015 | Electrical Equipment and Machinery                                     | 9801558   | 
| 2013 | Energy                                                                 | 750       | 
| 2016 | Energy                                                                 | 10024     | 
| 2015 | Food & Beverage Processing                                             | 141       | 
| 2014 | Food & Staples Retailing                                               | 773       | 
| 2015 | Food & Staples Retailing                                               | 706       | 
| 2016 | Food & Staples Retailing                                               | 2         | 
| 2015 | Gas Utilities                                                          | 122       | 
| 2013 | Household & Personal Products                                          | 0         | 
| 2013 | Materials                                                              | 200513    | 
| 2014 | Materials                                                              | 75678     | 
| 2016 | Materials                                                              | 88267     | 
| 2017 | Materials                                                              | 213137    | 
| 2013 | Media                                                                  | 9645      | 
| 2014 | Media                                                                  | 9645      | 
| 2015 | Media                                                                  | 1919      | 
| 2016 | Media                                                                  | 1808      | 
| 2014 | Retailing                                                              | 19        | 
| 2015 | Retailing                                                              | 11        | 
| 2014 | Semiconductors & Semiconductor Equipment                               | 50        | 
| 2016 | Semiconductors & Semiconductor Equipment                               | 4         | 
| 2015 | Semiconductors & Semiconductors Equipment                              | 3         | 
| 2013 | Software & Services                                                    | 6         | 
| 2014 | Software & Services                                                    | 146       | 
| 2015 | Software & Services                                                    | 22856     | 
| 2016 | Software & Services                                                    | 22846     | 
| 2017 | Software & Services                                                    | 690       | 
| 2013 | Technology Hardware & Equipment                                        | 61100     | 
| 2014 | Technology Hardware & Equipment                                        | 167361    | 
| 2015 | Technology Hardware & Equipment                                        | 106157    | 
| 2016 | Technology Hardware & Equipment                                        | 1566      | 
| 2017 | Technology Hardware & Equipment                                        | 27592     | 
| 2013 | Telecommunication Services                                             | 52        | 
| 2014 | Telecommunication Services                                             | 183       | 
| 2015 | Telecommunication Services                                             | 183       | 
| 2015 | Tires                                                                  | 2022      | 
| 2015 | Tobacco                                                                | 1         | 
| 2015 | Trading Companies & Distributors and Commercial Services & Supplies    | 239       | 
| 2013 | Utilities                                                              | 122       | 
| 2016 | Utilities                                                              | 122       | 
