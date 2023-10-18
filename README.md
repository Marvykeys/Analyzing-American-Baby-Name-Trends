# Analyzing-American-Baby-Name-Trends :baby:
I'll be working with data provided by the United States Social Security Administration, which lists first names along with the number and sex of babies they were given to in each year. The dataset has been limited to the first names which were given to over 5,000 American babies in a given year. The data spans 101 years, from 1920 through 2020.

<h3 id="baby_names"><code>baby_names</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>year</td>
</tr>
<tr>
<td style="text-align:left;"><code>first_name</code></td>
<td>varchar</td>
<td>first name</td>
</tr>
<tr>
<td style="text-align:left;"><code>sex</code></td>
<td>varchar</td>
<td><code>sex</code> of babies given <code>first_name</code></td>
</tr>
<tr>
<td style="text-align:left;"><code>num</code></td>
<td>int</td>
<td>number of babies of <code>sex</code> given <code>first_name</code> in that <code>year</code></td>
</tr>
</tbody>
</table>



#### 1. Let's find names that have been given to over 5,000 babies of either sex every year for 101 years from 1920 through 2020.
```Python
SELECT first_name,
       SUM(num)
FROM baby_names
GROUP BY first_name
HAVING COUNT(year) = 101
ORDER BY SUM(num) DESC;
```
| first_nam   |  sum    |
|-------------|---------|
|  James 	| 4748138 |
|  John 	| 4510721 |
|  William 	| 3614424 |
|  David 	| 3571498 |
|  Joseph 	| 2361382 |
|  Thomas 	| 2166802 |
|  Charles 	| 2112352 |
|  Elizabeth 	| 1436286 |

#### 2. Let's classify each name's popularity according to the number of years that the name appears in the dataset.
```Python
SELECT first_name, SUM(num),
   CASE WHEN COUNT(num) > 80 THEN 'Classic'
        WHEN COUNT(num) > 50 THEN 'Semi-classic'
        WHEN COUNT(num) > 20 THEN 'Semi-trendy'  
        ELSE 'Trendy' END AS popularity_type
FROM baby_names
GROUP BY first_name
ORDER BY first_name
LIMIT 5;
```
| first_nam   |  sum    |popularity type|
|-------------|---------|---------------|
|  Aaliyah 	|  15870  |	  Trendy    |
|  Aaron 	| 530592  |  Semi-classic |
|  Abigail    | 338485  |  Semi-trendy  |
|  Adam 	| 497293  |  Semi-trendy  |
|  Addison 	| 107433  |  Trendy       |

#### 3. Let's take a look at the ten highest-ranked American female names in our dataset.
```Python
SELECT
    RANK() OVER(ORDER BY SUM(num) DESC) AS name_rank,
    first_name, SUM(num)
FROM baby_names
WHERE sex = 'F'
GROUP BY first_name
LIMIT 5;
```
| name_rank   |  first_name |   Sum     |
|-------------|-------------|-----------|
|     1 	|  Mary 	|  3215850  |
|     2 	|  Patricia 	|  1479802  |
|     3 	|  Elizabeth 	|  1436286  |
|     4 	|  Jennifer 	|  1404743  | 
|     5 	|  Linda 	|  1361021  |

#### 4. We will return a list of female first names which ends with the letter 'a' and has been popular in the years since 2015.
```Python
SELECT first_name
FROM baby_names
WHERE sex = 'F' 
AND year > 2015
AND first_name LIKE '%a'
GROUP BY first_name
ORDER BY SUM(num) DESC;
```
<table>
    <thead>
        <tr>
            <th>first_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Olivia</td>
        </tr>
        <tr>
            <td>Emma</td>
        </tr>
        <tr>
            <td>Ava</td>
        </tr>
        <tr>
            <td>Sophia</td>
        </tr>
        <tr>
            <td>Isabella</td>
        </tr>
        <tr>
            <td>Mia</td>
        </tr>
        <tr>
            <td>Amelia</td>
        </tr>
        <tr>
            <td>Ella</td>
        </tr>
        <tr>
            <td>Sofia</td>
        </tr>
        <tr>
            <td>Camila</td>
        </tr>
        <tr>
            <td>Aria</td>
        </tr>
        <tr>
            <td>Victoria</td>
        </tr>
        <tr>
            <td>Layla</td>
        </tr>
        <tr>
            <td>Nora</td>
        </tr>
        <tr>
            <td>Mila</td>
        </tr>
        <tr>
            <td>Luna</td>
        </tr>
        <tr>
            <td>Stella</td>
        </tr>
        <tr>
            <td>Gianna</td>
        </tr>
        <tr>
            <td>Aurora</td>
        </tr>
    </tbody>
</table>

#### 5. Using a window function, we will find the cumulative number of babies named Olivia over the years since the name first appeared in our dataset.
```Python
SELECT year, first_name, num,
       SUM(num) OVER (ORDER BY year) AS cumulative_olivias
FROM baby_names
WHERE first_name = 'Olivia'
ORDER BY year ASC;
```

<table>
    <thead>
        <tr>
            <th>year</th>
            <th>first_name</th>
            <th>num</th>
            <th>cumulative_olivias</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1991</td>
            <td>Olivia</td>
            <td>5601</td>
            <td>5601</td>
        </tr>
        <tr>
            <td>1992</td>
            <td>Olivia</td>
            <td>5809</td>
            <td>11410</td>
        </tr>
        <tr>
            <td>1993</td>
            <td>Olivia</td>
            <td>6340</td>
            <td>17750</td>
        </tr>
        <tr>
            <td>1994</td>
            <td>Olivia</td>
            <td>6434</td>
            <td>24184</td>
        </tr>
        <tr>
            <td>1995</td>
            <td>Olivia</td>
            <td>7624</td>
            <td>31808</td>
        </tr>
        <tr>
            <td>1996</td>
            <td>Olivia</td>
            <td>8124</td>
            <td>39932</td>
        </tr>
        <tr>
            <td>1997</td>
            <td>Olivia</td>
            <td>9477</td>
            <td>49409</td>
        </tr>
        <tr>
            <td>1998</td>
            <td>Olivia</td>
            <td>10610</td>
            <td>60019</td>
        </tr>
        <tr>
            <td>1999</td>
            <td>Olivia</td>
            <td>11255</td>
            <td>71274</td>
        </tr>
        <tr>
            <td>2000</td>
            <td>Olivia</td>
            <td>12852</td>
            <td>84126</td>
        </tr>
        <tr>
            <td>2001</td>
            <td>Olivia</td>
            <td>13977</td>
            <td>98103</td>
        </tr>
        <tr>
            <td>2002</td>
            <td>Olivia</td>
            <td>14630</td>
            <td>112733</td>
        </tr>
        <tr>
            <td>2003</td>
            <td>Olivia</td>
            <td>16152</td>
            <td>128885</td>
        </tr>
        <tr>
            <td>2004</td>
            <td>Olivia</td>
            <td>16106</td>
            <td>144991</td>
        </tr>
        <tr>
            <td>2005</td>
            <td>Olivia</td>
            <td>15694</td>
            <td>160685</td>
        </tr>
        <tr>
            <td>2006</td>
            <td>Olivia</td>
            <td>15501</td>
            <td>176186</td>
        </tr>
        <tr>
            <td>2007</td>
            <td>Olivia</td>
            <td>16584</td>
            <td>192770</td>
        </tr>
        <tr>
            <td>2008</td>
            <td>Olivia</td>
            <td>17084</td>
            <td>209854</td>
        </tr>
        <tr>
            <td>2009</td>
            <td>Olivia</td>
            <td>17438</td>
            <td>227292</td>
        </tr>
        <tr>
            <td>2010</td>
            <td>Olivia</td>
            <td>17029</td>
            <td>244321</td>
        </tr>
        <tr>
            <td>2011</td>
            <td>Olivia</td>
            <td>17327</td>
            <td>261648</td>
        </tr>
        <tr>
            <td>2012</td>
            <td>Olivia</td>
            <td>17320</td>
            <td>278968</td>
        </tr>
        <tr>
            <td>2013</td>
            <td>Olivia</td>
            <td>18439</td>
            <td>297407</td>
        </tr>
        <tr>
            <td>2014</td>
            <td>Olivia</td>
            <td>19823</td>
            <td>317230</td>
        </tr>
        <tr>
            <td>2015</td>
            <td>Olivia</td>
            <td>19710</td>
            <td>336940</td>
        </tr>
        <tr>
            <td>2016</td>
            <td>Olivia</td>
            <td>19380</td>
            <td>356320</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>Olivia</td>
            <td>18744</td>
            <td>375064</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>Olivia</td>
            <td>18011</td>
            <td>393075</td>
        </tr>
        <tr>
            <td>2019</td>
            <td>Olivia</td>
            <td>18508</td>
            <td>411583</td>
        </tr>
        <tr>
            <td>2020</td>
            <td>Olivia</td>
            <td>17535</td>
            <td>429118</td>
        </tr>
    </tbody>
</table>

#### 6. Let's write a query that selects the year and the maximum num of babies given any male name in that year.
```Python
SELECT year, 
       MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year
LIMIT 5;
```
|year |max_num|
|-----|-------|
|1970 |85291  |
|2000 |34483  |
|1947 |94764  |
|1962 |85041  |
|1975 |68451  |

#### 7. Using the previous task's code as a subquery, we will look up the first_name that corresponds to the maximum number of babies given a specific male name in a year.
```Python
SELECT b.year, b.first_name, b.num

FROM baby_names AS b

INNER JOIN (SELECT year, 
       MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year) AS subquery

ON subquery.year = b.year AND subquery.max_num = b.num
ORDER BY year DESC
LIMIT 5;
```
|year|first_name|num|
|----|------|------|
|2020| Liam | 19659|
|2019| Liam | 20555|
|2018| Liam |	19924|
|2017| Liam | 18824|
|2016| Noah |	19154|

#### 8. Let's Return a list of first names that have been the top male first name in any year along with a count of the number of years that name has been the top name.
```Python
WITH top_male_names AS (SELECT b.year, b.first_name, b.num

FROM baby_names AS b

INNER JOIN (SELECT year, 
       MAX(num) AS num
FROM baby_names
WHERE sex = 'M'
GROUP BY year) AS subquery

ON subquery.year = b.year
   AND subquery.num = b.num
ORDER BY year DESC)

SELECT first_name, COUNT(first_name) AS count_top_name
FROM top_male_names
GROUP BY first_name
ORDER BY COUNT(first_name) DESC;
```

<table>
    <thead>
        <tr>
            <th>first_name</th>
            <th>count_top_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Michael</td>
            <td>44</td>
        </tr>
        <tr>
            <td>Robert</td>
            <td>17</td>
        </tr>
        <tr>
            <td>Jacob</td>
            <td>14</td>
        </tr>
        <tr>
            <td>James</td>
            <td>13</td>
        </tr>
        <tr>
            <td>Noah</td>
            <td>4</td>
        </tr>
        <tr>
            <td>John</td>
            <td>4</td>
        </tr>
        <tr>
            <td>Liam</td>
            <td>4</td>
        </tr>
        <tr>
            <td>David</td>
            <td>1</td>
        </tr>
    </tbody>
</table>

