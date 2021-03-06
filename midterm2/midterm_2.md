---
title: "BIS 15L Midterm 2"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 10 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Your plots should use consistent aesthetics throughout.  

This exam is due by **12:00p on Tuesday, February 22**.  

```r
options(scipen=999)
```

## Gapminder
For this assignment, we are going to use data from  [gapminder](https://www.gapminder.org/). Gapminder includes information about economics, population, social issues, and life expectancy from countries all over the world. We will use three data sets, so please load all three as separate objects.    

```r
library(gapminder)
```

1. population_total.csv  
2. income_per_person_gdppercapita_ppp_inflation_adjusted.csv  
3. life_expectancy_years.csv  

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(naniar)
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(here)
```

```
## here() starts at C:/Users/ekarr/OneDrive/Desktop/GitHub/BIS15W2022_earriaga
```

```r
library(RColorBrewer)
library(paletteer)
```


```r
pop_total <- readr::read_csv("data/population_total.csv")
```

```
## Rows: 195 Columns: 302
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): country
## dbl (301): 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809, 1810,...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
income_ppp_gpd <-readr::read_csv("data/income_per_person_gdppercapita_ppp_inflation_adjusted.csv")
```

```
## Rows: 193 Columns: 242
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): country
## dbl (241): 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809, 1810,...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
life_expectancy <- readr::read_csv("data/life_expectancy_years.csv")
```

```
## Rows: 187 Columns: 302
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): country
## dbl (301): 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809, 1810,...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. (3 points) Once you have an idea of the structure of the data, please make each data set tidy (hint: think back to pivots) and store them as new objects. You will need both the original (wide) and tidy (long) data!  

```r
naniar::miss_var_summary(pop_total)
```

```
## # A tibble: 302 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 country       0        0
##  2 1800          0        0
##  3 1801          0        0
##  4 1802          0        0
##  5 1803          0        0
##  6 1804          0        0
##  7 1805          0        0
##  8 1806          0        0
##  9 1807          0        0
## 10 1808          0        0
## # ... with 292 more rows
```

```r
summary(pop_total)
```

```
##    country               1800                1801                1802          
##  Length:195         Min.   :      905   Min.   :      905   Min.   :      905  
##  Class :character   1st Qu.:   128500   1st Qu.:   128500   1st Qu.:   128500  
##  Mode  :character   Median :   637000   Median :   637000   Median :   637000  
##                     Mean   :  5038229   Mean   :  5055872   Mean   :  5074643  
##                     3rd Qu.:  2200000   3rd Qu.:  2200000   3rd Qu.:  2195000  
##                     Max.   :330000000   Max.   :332000000   Max.   :333000000  
##       1803                1804                1805          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   129000   1st Qu.:   129000   1st Qu.:   129000  
##  Median :   637000   Median :   637000   Median :   637000  
##  Mean   :  5092072   Mean   :  5105055   Mean   :  5128710  
##  3rd Qu.:  2195000   3rd Qu.:  2195000   3rd Qu.:  2175000  
##  Max.   :335000000   Max.   :336000000   Max.   :338000000  
##       1806                1807                1808          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   127500   1st Qu.:   127000   1st Qu.:   127000  
##  Median :   637000   Median :   637000   Median :   637000  
##  Mean   :  5143516   Mean   :  5167424   Mean   :  5186405  
##  3rd Qu.:  2145000   3rd Qu.:  2145000   3rd Qu.:  2145000  
##  Max.   :339000000   Max.   :341000000   Max.   :343000000  
##       1809                1810                1811          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   127000   1st Qu.:   127000   1st Qu.:   127000  
##  Median :   637000   Median :   639000   Median :   655000  
##  Mean   :  5199982   Mean   :  5232967   Mean   :  5254824  
##  3rd Qu.:  2145000   3rd Qu.:  2145000   3rd Qu.:  2145000  
##  Max.   :344000000   Max.   :347000000   Max.   :349000000  
##       1812                1813                1814          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   127000   1st Qu.:   127000   1st Qu.:   125000  
##  Median :   671000   Median :   688000   Median :   705000  
##  Mean   :  5294767   Mean   :  5325674   Mean   :  5361654  
##  3rd Qu.:  2145000   3rd Qu.:  2145000   3rd Qu.:  2155000  
##  Max.   :353000000   Max.   :356000000   Max.   :359000000  
##       1815                1816                1817          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   123000   1st Qu.:   121000   1st Qu.:   120000  
##  Median :   713000   Median :   713000   Median :   713000  
##  Mean   :  5398080   Mean   :  5440271   Mean   :  5472701  
##  3rd Qu.:  2170000   3rd Qu.:  2165000   3rd Qu.:  2155000  
##  Max.   :363000000   Max.   :367000000   Max.   :370000000  
##       1818                1819                1820          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   120000   1st Qu.:   120000   1st Qu.:   120000  
##  Median :   713000   Median :   714000   Median :   719000  
##  Mean   :  5517194   Mean   :  5550374   Mean   :  5588640  
##  3rd Qu.:  2145000   3rd Qu.:  2150000   3rd Qu.:  2165000  
##  Max.   :374000000   Max.   :377000000   Max.   :380000000  
##       1821                1822                1823          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   120500   1st Qu.:   121000   1st Qu.:   122000  
##  Median :   728000   Median :   736000   Median :   742000  
##  Mean   :  5627718   Mean   :  5661860   Mean   :  5701340  
##  3rd Qu.:  2185000   3rd Qu.:  2215000   3rd Qu.:  2250000  
##  Max.   :384000000   Max.   :386000000   Max.   :389000000  
##       1824                1825                1826          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   122500   1st Qu.:   123000   1st Qu.:   123500  
##  Median :   749000   Median :   756000   Median :   762000  
##  Mean   :  5734966   Mean   :  5774826   Mean   :  5816081  
##  3rd Qu.:  2285000   3rd Qu.:  2310000   3rd Qu.:  2335000  
##  Max.   :392000000   Max.   :395000000   Max.   :398000000  
##       1827                1828                1829          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   123000   1st Qu.:   122000   1st Qu.:   119500  
##  Median :   768000   Median :   775000   Median :   782000  
##  Mean   :  5853201   Mean   :  5894272   Mean   :  5936815  
##  3rd Qu.:  2370000   3rd Qu.:  2395000   3rd Qu.:  2420000  
##  Max.   :400000000   Max.   :403000000   Max.   :406000000  
##       1830                1831                1832          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   118000   1st Qu.:   116000   1st Qu.:   115000  
##  Median :   789000   Median :   789000   Median :   795000  
##  Mean   :  5967640   Mean   :  5999721   Mean   :  6031314  
##  3rd Qu.:  2445000   3rd Qu.:  2470000   3rd Qu.:  2495000  
##  Max.   :407000000   Max.   :409000000   Max.   :410000000  
##       1833                1834                1835          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   114000   1st Qu.:   113500   1st Qu.:   113000  
##  Median :   802000   Median :   810000   Median :   817000  
##  Mean   :  6057897   Mean   :  6086100   Mean   :  6113028  
##  3rd Qu.:  2520000   3rd Qu.:  2550000   3rd Qu.:  2570000  
##  Max.   :410000000   Max.   :410000000   Max.   :410000000  
##       1836                1837                1838          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   112500   1st Qu.:   113500   1st Qu.:   115500  
##  Median :   825000   Median :   823000   Median :   810000  
##  Mean   :  6146867   Mean   :  6174688   Mean   :  6203668  
##  3rd Qu.:  2595000   3rd Qu.:  2610000   3rd Qu.:  2630000  
##  Max.   :411000000   Max.   :411000000   Max.   :411000000  
##       1839                1840                1841          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   117500   1st Qu.:   120500   1st Qu.:   123500  
##  Median :   805000   Median :   818000   Median :   831000  
##  Mean   :  6236324   Mean   :  6265864   Mean   :  6288498  
##  3rd Qu.:  2645000   3rd Qu.:  2665000   3rd Qu.:  2680000  
##  Max.   :412000000   Max.   :412000000   Max.   :412000000  
##       1842                1843                1844          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   127000   1st Qu.:   129500   1st Qu.:   129500  
##  Median :   845000   Median :   859000   Median :   887000  
##  Mean   :  6316663   Mean   :  6344308   Mean   :  6374099  
##  3rd Qu.:  2700000   3rd Qu.:  2715000   3rd Qu.:  2735000  
##  Max.   :412000000   Max.   :412000000   Max.   :412000000  
##       1845                1846                1847          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   130000   1st Qu.:   132000   1st Qu.:   133500  
##  Median :   895000   Median :   904000   Median :   914000  
##  Mean   :  6402963   Mean   :  6431975   Mean   :  6462558  
##  3rd Qu.:  2755000   3rd Qu.:  2825000   3rd Qu.:  2825000  
##  Max.   :412000000   Max.   :412000000   Max.   :412000000  
##       1848                1849                1850          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   134500   1st Qu.:   135500   1st Qu.:   136500  
##  Median :   922000   Median :   930000   Median :   937000  
##  Mean   :  6492695   Mean   :  6518678   Mean   :  6543826  
##  3rd Qu.:  2860000   3rd Qu.:  2885000   3rd Qu.:  2905000  
##  Max.   :412000000   Max.   :411000000   Max.   :410000000  
##       1851                1852                1853          
##  Min.   :      905   Min.   :      905   Min.   :      905  
##  1st Qu.:   138500   1st Qu.:   143000   1st Qu.:   148000  
##  Median :   946000   Median :   954000   Median :   963000  
##  Mean   :  6560809   Mean   :  6572626   Mean   :  6580616  
##  3rd Qu.:  2925000   3rd Qu.:  2945000   3rd Qu.:  2965000  
##  Max.   :408000000   Max.   :405000000   Max.   :401000000  
##       1854                1855                1856          
##  Min.   :      905   Min.   :      906   Min.   :      906  
##  1st Qu.:   152500   1st Qu.:   154500   1st Qu.:   156000  
##  Median :   972000   Median :   981000   Median :   989000  
##  Mean   :  6593709   Mean   :  6601445   Mean   :  6614979  
##  3rd Qu.:  2975000   3rd Qu.:  2985000   3rd Qu.:  3015000  
##  Max.   :398000000   Max.   :394000000   Max.   :391000000  
##       1857                1858                1859          
##  Min.   :      906   Min.   :      906   Min.   :      906  
##  1st Qu.:   157500   1st Qu.:   158500   1st Qu.:   160000  
##  Median :  1010000   Median :  1020000   Median :  1030000  
##  Mean   :  6617604   Mean   :  6633016   Mean   :  6647610  
##  3rd Qu.:  3045000   3rd Qu.:  3075000   3rd Qu.:  3110000  
##  Max.   :387000000   Max.   :384000000   Max.   :381000000  
##       1860                1861                1862          
##  Min.   :      906   Min.   :      906   Min.   :      906  
##  1st Qu.:   161000   1st Qu.:   162500   1st Qu.:   163500  
##  Median :  1040000   Median :  1040000   Median :  1050000  
##  Mean   :  6661632   Mean   :  6682475   Mean   :  6698291  
##  3rd Qu.:  3140000   3rd Qu.:  3175000   3rd Qu.:  3205000  
##  Max.   :378000000   Max.   :376000000   Max.   :373000000  
##       1863                1864                1865          
##  Min.   :      906   Min.   :      906   Min.   :      907  
##  1st Qu.:   170500   1st Qu.:   178000   1st Qu.:   180500  
##  Median :  1060000   Median :  1060000   Median :  1070000  
##  Mean   :  6723903   Mean   :  6745063   Mean   :  6766812  
##  3rd Qu.:  3235000   3rd Qu.:  3270000   3rd Qu.:  3300000  
##  Max.   :372000000   Max.   :370000000   Max.   :368000000  
##       1866                1867                1868          
##  Min.   :      907   Min.   :      907   Min.   :      907  
##  1st Qu.:   181500   1st Qu.:   182000   1st Qu.:   183000  
##  Median :  1080000   Median :  1090000   Median :  1100000  
##  Mean   :  6789713   Mean   :  6811864   Mean   :  6835001  
##  3rd Qu.:  3335000   3rd Qu.:  3370000   3rd Qu.:  3400000  
##  Max.   :366000000   Max.   :364000000   Max.   :362000000  
##       1869                1870                1871          
##  Min.   :      907   Min.   :      907   Min.   :      907  
##  1st Qu.:   185500   1st Qu.:   188000   1st Qu.:   190500  
##  Median :  1110000   Median :  1120000   Median :  1130000  
##  Mean   :  6863545   Mean   :  6893385   Mean   :  6926927  
##  3rd Qu.:  3435000   3rd Qu.:  3470000   3rd Qu.:  3505000  
##  Max.   :361000000   Max.   :360000000   Max.   :360000000  
##       1872                1873                1874          
##  Min.   :      907   Min.   :      907   Min.   :      908  
##  1st Qu.:   193000   1st Qu.:   193500   1st Qu.:   194000  
##  Median :  1140000   Median :  1150000   Median :  1160000  
##  Mean   :  6963235   Mean   :  7003934   Mean   :  7045028  
##  3rd Qu.:  3540000   3rd Qu.:  3575000   3rd Qu.:  3610000  
##  Max.   :361000000   Max.   :362000000   Max.   :363000000  
##       1875                1876                1877          
##  Min.   :      908   Min.   :      908   Min.   :      908  
##  1st Qu.:   195000   1st Qu.:   196500   1st Qu.:   198000  
##  Median :  1170000   Median :  1190000   Median :  1200000  
##  Mean   :  7082339   Mean   :  7123451   Mean   :  7162378  
##  3rd Qu.:  3650000   3rd Qu.:  3680000   3rd Qu.:  3720000  
##  Max.   :364000000   Max.   :365000000   Max.   :366000000  
##       1878                1879                1880          
##  Min.   :      908   Min.   :      908   Min.   :      908  
##  1st Qu.:   200000   1st Qu.:   201500   1st Qu.:   203500  
##  Median :  1220000   Median :  1220000   Median :  1230000  
##  Mean   :  7204484   Mean   :  7248380   Mean   :  7293329  
##  3rd Qu.:  3720000   3rd Qu.:  3745000   3rd Qu.:  3800000  
##  Max.   :367000000   Max.   :368000000   Max.   :369000000  
##       1881                1882                1883          
##  Min.   :      908   Min.   :      908   Min.   :      908  
##  1st Qu.:   205000   1st Qu.:   206500   1st Qu.:   208500  
##  Median :  1240000   Median :  1250000   Median :  1260000  
##  Mean   :  7345366   Mean   :  7395656   Mean   :  7448344  
##  3rd Qu.:  3870000   3rd Qu.:  3910000   3rd Qu.:  3950000  
##  Max.   :370000000   Max.   :371000000   Max.   :372000000  
##       1884                1885                1886          
##  Min.   :      909   Min.   :      909   Min.   :      909  
##  1st Qu.:   212000   1st Qu.:   215500   1st Qu.:   220500  
##  Median :  1270000   Median :  1300000   Median :  1330000  
##  Mean   :  7507391   Mean   :  7565904   Mean   :  7621718  
##  3rd Qu.:  3990000   3rd Qu.:  4030000   3rd Qu.:  4070000  
##  Max.   :374000000   Max.   :375000000   Max.   :376000000  
##       1887                1888                1889          
##  Min.   :      909   Min.   :      909   Min.   :      909  
##  1st Qu.:   225000   1st Qu.:   230000   1st Qu.:   235000  
##  Median :  1350000   Median :  1370000   Median :  1380000  
##  Mean   :  7677260   Mean   :  7743123   Mean   :  7799709  
##  3rd Qu.:  4110000   3rd Qu.:  4150000   3rd Qu.:  4195000  
##  Max.   :377000000   Max.   :379000000   Max.   :380000000  
##       1890                1891                1892          
##  Min.   :      909   Min.   :      909   Min.   :      909  
##  1st Qu.:   239500   1st Qu.:   242500   1st Qu.:   245500  
##  Median :  1390000   Median :  1390000   Median :  1390000  
##  Mean   :  7859939   Mean   :  7911962   Mean   :  7966124  
##  3rd Qu.:  4235000   3rd Qu.:  4275000   3rd Qu.:  4290000  
##  Max.   :382000000   Max.   :383000000   Max.   :385000000  
##       1893                1894                1895          
##  Min.   :      910   Min.   :      910   Min.   :      910  
##  1st Qu.:   249000   1st Qu.:   253500   1st Qu.:   256500  
##  Median :  1390000   Median :  1400000   Median :  1410000  
##  Mean   :  8021656   Mean   :  8073309   Mean   :  8129882  
##  3rd Qu.:  4300000   3rd Qu.:  4315000   3rd Qu.:  4305000  
##  Max.   :387000000   Max.   :389000000   Max.   :391000000  
##       1896                1897                1898          
##  Min.   :      910   Min.   :      910   Min.   :      910  
##  1st Qu.:   259000   1st Qu.:   262500   1st Qu.:   265000  
##  Median :  1420000   Median :  1430000   Median :  1440000  
##  Mean   :  8187026   Mean   :  8239609   Mean   :  8297266  
##  3rd Qu.:  4285000   3rd Qu.:  4295000   3rd Qu.:  4350000  
##  Max.   :393000000   Max.   :395000000   Max.   :397000000  
##       1899                1900                1901          
##  Min.   :      910   Min.   :      910   Min.   :      910  
##  1st Qu.:   268500   1st Qu.:   271000   1st Qu.:   271000  
##  Median :  1460000   Median :  1470000   Median :  1480000  
##  Mean   :  8359438   Mean   :  8425604   Mean   :  8489851  
##  3rd Qu.:  4375000   3rd Qu.:  4350000   3rd Qu.:  4330000  
##  Max.   :399000000   Max.   :402000000   Max.   :404000000  
##       1902                1903                1904          
##  Min.   :      910   Min.   :      911   Min.   :      911  
##  1st Qu.:   272000   1st Qu.:   276500   1st Qu.:   281000  
##  Median :  1500000   Median :  1500000   Median :  1520000  
##  Mean   :  8560671   Mean   :  8633293   Mean   :  8710535  
##  3rd Qu.:  4330000   3rd Qu.:  4365000   3rd Qu.:  4405000  
##  Max.   :406000000   Max.   :408000000   Max.   :411000000  
##       1905                1906                1907          
##  Min.   :      911   Min.   :      911   Min.   :      911  
##  1st Qu.:   286000   1st Qu.:   290500   1st Qu.:   292500  
##  Median :  1540000   Median :  1550000   Median :  1540000  
##  Mean   :  8785832   Mean   :  8861704   Mean   :  8943411  
##  3rd Qu.:  4490000   3rd Qu.:  4580000   3rd Qu.:  4670000  
##  Max.   :413000000   Max.   :415000000   Max.   :418000000  
##       1908                1909                1910          
##  Min.   :      911   Min.   :      911   Min.   :      911  
##  1st Qu.:   297000   1st Qu.:   302500   1st Qu.:   306500  
##  Median :  1530000   Median :  1520000   Median :  1520000  
##  Mean   :  9019978   Mean   :  9094033   Mean   :  9167826  
##  3rd Qu.:  4760000   3rd Qu.:  4800000   3rd Qu.:  4825000  
##  Max.   :420000000   Max.   :423000000   Max.   :426000000  
##       1911                1912                1913          
##  Min.   :      911   Min.   :      912   Min.   :      912  
##  1st Qu.:   305000   1st Qu.:   304000   1st Qu.:   305500  
##  Median :  1520000   Median :  1530000   Median :  1550000  
##  Mean   :  9235432   Mean   :  9302106   Mean   :  9368667  
##  3rd Qu.:  4850000   3rd Qu.:  4845000   3rd Qu.:  4845000  
##  Max.   :430000000   Max.   :434000000   Max.   :439000000  
##       1914                1915                1916          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   306500   1st Qu.:   309000   1st Qu.:   310000  
##  Median :  1570000   Median :  1580000   Median :  1590000  
##  Mean   :  9432415   Mean   :  9498460   Mean   :  9564826  
##  3rd Qu.:  4845000   3rd Qu.:  4855000   3rd Qu.:  4895000  
##  Max.   :444000000   Max.   :449000000   Max.   :454000000  
##       1917                1918                1919          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   310500   1st Qu.:   311500   1st Qu.:   313000  
##  Median :  1600000   Median :  1610000   Median :  1630000  
##  Mean   :  9630391   Mean   :  9697385   Mean   :  9766291  
##  3rd Qu.:  4945000   3rd Qu.:  5015000   3rd Qu.:  5095000  
##  Max.   :459000000   Max.   :464000000   Max.   :468000000  
##       1920                1921                1922          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   315500   1st Qu.:   318000   1st Qu.:   321500  
##  Median :  1640000   Median :  1660000   Median :  1680000  
##  Mean   :  9843719   Mean   :  9925046   Mean   : 10010265  
##  3rd Qu.:  5180000   3rd Qu.:  5275000   3rd Qu.:  5330000  
##  Max.   :472000000   Max.   :475000000   Max.   :478000000  
##       1923                1924                1925          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   325500   1st Qu.:   329000   1st Qu.:   333500  
##  Median :  1710000   Median :  1730000   Median :  1760000  
##  Mean   : 10093537   Mean   : 10186294   Mean   : 10277638  
##  3rd Qu.:  5375000   3rd Qu.:  5425000   3rd Qu.:  5470000  
##  Max.   :479000000   Max.   :481000000   Max.   :483000000  
##       1926                1927                1928          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   337500   1st Qu.:   341000   1st Qu.:   345500  
##  Median :  1780000   Median :  1800000   Median :  1820000  
##  Mean   : 10361037   Mean   : 10454114   Mean   : 10550466  
##  3rd Qu.:  5515000   3rd Qu.:  5565000   3rd Qu.:  5615000  
##  Max.   :484000000   Max.   :486000000   Max.   :488000000  
##       1929                1930                1931          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   350000   1st Qu.:   355500   1st Qu.:   361000  
##  Median :  1840000   Median :  1870000   Median :  1880000  
##  Mean   : 10651250   Mean   : 10749049   Mean   : 10852892  
##  3rd Qu.:  5695000   3rd Qu.:  5775000   3rd Qu.:  5845000  
##  Max.   :490000000   Max.   :492000000   Max.   :495000000  
##       1932                1933                1934          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   367500   1st Qu.:   374500   1st Qu.:   382000  
##  Median :  1920000   Median :  1960000   Median :  1980000  
##  Mean   : 10964595   Mean   : 11073418   Mean   : 11187659  
##  3rd Qu.:  5915000   3rd Qu.:  5985000   3rd Qu.:  6055000  
##  Max.   :497000000   Max.   :500000000   Max.   :503000000  
##       1935                1936                1937          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   389500   1st Qu.:   398500   1st Qu.:   407500  
##  Median :  2010000   Median :  2030000   Median :  2050000  
##  Mean   : 11301979   Mean   : 11416411   Mean   : 11537737  
##  3rd Qu.:  6125000   3rd Qu.:  6195000   3rd Qu.:  6260000  
##  Max.   :506000000   Max.   :509000000   Max.   :512000000  
##       1938                1939                1940          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   413500   1st Qu.:   417500   1st Qu.:   417500  
##  Median :  2080000   Median :  2100000   Median :  2120000  
##  Mean   : 11656526   Mean   : 11770403   Mean   : 11881916  
##  3rd Qu.:  6320000   3rd Qu.:  6385000   3rd Qu.:  6450000  
##  Max.   :515000000   Max.   :518000000   Max.   :521000000  
##       1941                1942                1943          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   416000   1st Qu.:   418500   1st Qu.:   421500  
##  Median :  2140000   Median :  2150000   Median :  2180000  
##  Mean   : 11982732   Mean   : 12082436   Mean   : 12174120  
##  3rd Qu.:  6545000   3rd Qu.:  6660000   3rd Qu.:  6745000  
##  Max.   :524000000   Max.   :527000000   Max.   :530000000  
##       1944                1945                1946          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   424500   1st Qu.:   427500   1st Qu.:   430500  
##  Median :  2220000   Median :  2260000   Median :  2300000  
##  Mean   : 12261134   Mean   : 12353262   Mean   : 12448431  
##  3rd Qu.:  6815000   3rd Qu.:  6900000   3rd Qu.:  7045000  
##  Max.   :532000000   Max.   :535000000   Max.   :538000000  
##       1947                1948                1949          
##  Min.   :      912   Min.   :      912   Min.   :      912  
##  1st Qu.:   430500   1st Qu.:   440000   1st Qu.:   454000  
##  Median :  2350000   Median :  2390000   Median :  2440000  
##  Mean   : 12544251   Mean   : 12649006   Mean   : 12775571  
##  3rd Qu.:  7140000   3rd Qu.:  7235000   3rd Qu.:  7335000  
##  Max.   :541000000   Max.   :544000000   Max.   :548000000  
##       1950                1951                1952          
##  Min.   :      906   Min.   :      883   Min.   :      883  
##  1st Qu.:   464500   1st Qu.:   469000   1st Qu.:   474000  
##  Median :  2480000   Median :  2500000   Median :  2530000  
##  Mean   : 12935097   Mean   : 13176854   Mean   : 13419535  
##  3rd Qu.:  7450000   3rd Qu.:  7505000   3rd Qu.:  7525000  
##  Max.   :554000000   Max.   :570000000   Max.   :583000000  
##       1953                1954                1955          
##  Min.   :      890   Min.   :      899   Min.   :      900  
##  1st Qu.:   479000   1st Qu.:   484000   1st Qu.:   490000  
##  Median :  2590000   Median :  2610000   Median :  2640000  
##  Mean   : 13657720   Mean   : 13901826   Mean   : 14139348  
##  3rd Qu.:  7555000   3rd Qu.:  7640000   3rd Qu.:  7840000  
##  Max.   :593000000   Max.   :603000000   Max.   :612000000  
##       1956                1957                1958          
##  Min.   :      909   Min.   :      908   Min.   :      905  
##  1st Qu.:   503000   1st Qu.:   517000   1st Qu.:   532000  
##  Median :  2720000   Median :  2810000   Median :  2850000  
##  Mean   : 14389681   Mean   : 14652702   Mean   : 14914714  
##  3rd Qu.:  7940000   3rd Qu.:  8005000   3rd Qu.:  8085000  
##  Max.   :621000000   Max.   :631000000   Max.   :640000000  
##       1959                1960                1961          
##  Min.   :      905   Min.   :      904   Min.   :      904  
##  1st Qu.:   546500   1st Qu.:   562000   1st Qu.:   570500  
##  Median :  2930000   Median :  3000000   Median :  3060000  
##  Mean   : 15189836   Mean   : 15473214   Mean   : 15761233  
##  3rd Qu.:  8240000   3rd Qu.:  8400000   3rd Qu.:  8625000  
##  Max.   :650000000   Max.   :660000000   Max.   :671000000  
##       1962                1963                1964          
##  Min.   :      901   Min.   :      898   Min.   :      877  
##  1st Qu.:   578500   1st Qu.:   585500   1st Qu.:   594000  
##  Median :  3120000   Median :  3180000   Median :  3250000  
##  Mean   : 16055951   Mean   : 16363208   Mean   : 16680203  
##  3rd Qu.:  8815000   3rd Qu.:  9010000   3rd Qu.:  9310000  
##  Max.   :682000000   Max.   :694000000   Max.   :708000000  
##       1965                1966                1967          
##  Min.   :      853   Min.   :      814   Min.   :      766  
##  1st Qu.:   603000   1st Qu.:   613500   1st Qu.:   625500  
##  Median :  3310000   Median :  3370000   Median :  3430000  
##  Mean   : 17017910   Mean   : 17363647   Mean   : 17727175  
##  3rd Qu.:  9530000   3rd Qu.:  9805000   3rd Qu.:  9910000  
##  Max.   :724000000   Max.   :742000000   Max.   :763000000  
##       1968                1969                1970          
##  Min.   :      711   Min.   :      670   Min.   :      649  
##  1st Qu.:   656500   1st Qu.:   690500   1st Qu.:   705000  
##  Median :  3500000   Median :  3580000   Median :  3670000  
##  Mean   : 18098461   Mean   : 18469813   Mean   : 18858702  
##  3rd Qu.: 10055000   3rd Qu.: 10125000   3rd Qu.: 10350000  
##  Max.   :784000000   Max.   :806000000   Max.   :828000000  
##       1971                1972                1973          
##  Min.   :      645   Min.   :      661   Min.   :      685  
##  1st Qu.:   716000   1st Qu.:   727500   1st Qu.:   738000  
##  Median :  3770000   Median :  3850000   Median :  3910000  
##  Mean   : 19236839   Mean   : 19622929   Mean   : 20008507  
##  3rd Qu.: 10500000   3rd Qu.: 10800000   3rd Qu.: 11150000  
##  Max.   :849000000   Max.   :869000000   Max.   :889000000  
##       1974                1975                1976          
##  Min.   :      708   Min.   :      724   Min.   :      730  
##  1st Qu.:   748000   1st Qu.:   756000   1st Qu.:   770000  
##  Median :  3990000   Median :  4010000   Median :  4170000  
##  Mean   : 20399038   Mean   : 20785836   Mean   : 21176135  
##  3rd Qu.: 11500000   3rd Qu.: 11900000   3rd Qu.: 12300000  
##  Max.   :908000000   Max.   :926000000   Max.   :943000000  
##       1977                1978                1979          
##  Min.   :      730   Min.   :      725   Min.   :      722  
##  1st Qu.:   786500   1st Qu.:   833500   1st Qu.:   898500  
##  Median :  4260000   Median :  4340000   Median :  4420000  
##  Mean   : 21543941   Mean   : 21927694   Mean   : 22317685  
##  3rd Qu.: 12650000   3rd Qu.: 13000000   3rd Qu.: 13350000  
##  Max.   :958000000   Max.   :972000000   Max.   :986000000  
##       1980                 1981                 1982           
##  Min.   :       725   Min.   :       723   Min.   :       725  
##  1st Qu.:    932000   1st Qu.:    954000   1st Qu.:    976000  
##  Median :   4510000   Median :   4620000   Median :   4720000  
##  Mean   :  22710820   Mean   :  23098738   Mean   :  23532361  
##  3rd Qu.:  13550000   3rd Qu.:  13600000   3rd Qu.:  13750000  
##  Max.   :1000000000   Max.   :1010000000   Max.   :1030000000  
##       1983                 1984                 1985           
##  Min.   :       730   Min.   :       733   Min.   :       740  
##  1st Qu.:    997500   1st Qu.:   1020000   1st Qu.:   1045000  
##  Median :   4800000   Median :   4870000   Median :   4910000  
##  Mean   :  23925361   Mean   :  24383544   Mean   :  24839507  
##  3rd Qu.:  14000000   3rd Qu.:  14250000   3rd Qu.:  14550000  
##  Max.   :1040000000   Max.   :1060000000   Max.   :1080000000  
##       1986                 1987                 1988           
##  Min.   :       747   Min.   :       751   Min.   :       754  
##  1st Qu.:   1065000   1st Qu.:   1090000   1st Qu.:   1120000  
##  Median :   4930000   Median :   5030000   Median :   5120000  
##  Mean   :  25305437   Mean   :  25768131   Mean   :  26230331  
##  3rd Qu.:  14850000   3rd Qu.:  15150000   3rd Qu.:  15650000  
##  Max.   :1100000000   Max.   :1120000000   Max.   :1140000000  
##       1989                 1990                 1991           
##  Min.   :       759   Min.   :       762   Min.   :       762  
##  1st Qu.:   1130000   1st Qu.:   1140000   1st Qu.:   1150000  
##  Median :   5150000   Median :   5270000   Median :   5310000  
##  Mean   :  26701765   Mean   :  27165412   Mean   :  27587862  
##  3rd Qu.:  16000000   3rd Qu.:  16300000   3rd Qu.:  16600000  
##  Max.   :1160000000   Max.   :1180000000   Max.   :1190000000  
##       1992                 1993                 1994           
##  Min.   :       773   Min.   :       780   Min.   :       777  
##  1st Qu.:   1160000   1st Qu.:   1170000   1st Qu.:   1180000  
##  Median :   5310000   Median :   5350000   Median :   5360000  
##  Mean   :  28037338   Mean   :  28455482   Mean   :  28861469  
##  3rd Qu.:  16800000   3rd Qu.:  16900000   3rd Qu.:  17450000  
##  Max.   :1210000000   Max.   :1220000000   Max.   :1230000000  
##       1995                 1996                 1997           
##  Min.   :       778   Min.   :       784   Min.   :       787  
##  1st Qu.:   1190000   1st Qu.:   1210000   1st Qu.:   1230000  
##  Median :   5380000   Median :   5420000   Median :   5570000  
##  Mean   :  29270590   Mean   :  29684869   Mean   :  30083742  
##  3rd Qu.:  18050000   3rd Qu.:  18300000   3rd Qu.:  18450000  
##  Max.   :1240000000   Max.   :1250000000   Max.   :1260000000  
##       1998                 1999                 2000           
##  Min.   :       786   Min.   :       788   Min.   :       790  
##  1st Qu.:   1250000   1st Qu.:   1270000   1st Qu.:   1295000  
##  Median :   5700000   Median :   5840000   Median :   5950000  
##  Mean   :  30499470   Mean   :  30915602   Mean   :  31336214  
##  3rd Qu.:  18600000   3rd Qu.:  18800000   3rd Qu.:  19150000  
##  Max.   :1270000000   Max.   :1280000000   Max.   :1290000000  
##       2001                 2002                 2003           
##  Min.   :       792   Min.   :       792   Min.   :       792  
##  1st Qu.:   1315000   1st Qu.:   1335000   1st Qu.:   1345000  
##  Median :   6060000   Median :   6170000   Median :   6280000  
##  Mean   :  31748300   Mean   :  32126600   Mean   :  32551468  
##  3rd Qu.:  19500000   3rd Qu.:  19800000   3rd Qu.:  20200000  
##  Max.   :1300000000   Max.   :1310000000   Max.   :1320000000  
##       2004                 2005                 2006           
##  Min.   :       795   Min.   :       798   Min.   :       795  
##  1st Qu.:   1355000   1st Qu.:   1375000   1st Qu.:   1405000  
##  Median :   6400000   Median :   6530000   Median :   6680000  
##  Mean   :  32925631   Mean   :  33358510   Mean   :  33804556  
##  3rd Qu.:  20600000   3rd Qu.:  20950000   3rd Qu.:  21150000  
##  Max.   :1320000000   Max.   :1330000000   Max.   :1340000000  
##       2007                 2008                 2009           
##  Min.   :       794   Min.   :       792   Min.   :       787  
##  1st Qu.:   1440000   1st Qu.:   1485000   1st Qu.:   1610000  
##  Median :   6850000   Median :   7090000   Median :   7360000  
##  Mean   :  34194260   Mean   :  34592827   Mean   :  35051809  
##  3rd Qu.:  21500000   3rd Qu.:  22100000   3rd Qu.:  22700000  
##  Max.   :1350000000   Max.   :1350000000   Max.   :1360000000  
##       2010                 2011                 2012           
##  Min.   :       783   Min.   :       786   Min.   :       786  
##  1st Qu.:   1705000   1st Qu.:   1765000   1st Qu.:   1830000  
##  Median :   7430000   Median :   7660000   Median :   7870000  
##  Mean   :  35463801   Mean   :  35927010   Mean   :  36334120  
##  3rd Qu.:  23450000   3rd Qu.:  24200000   3rd Qu.:  24850000  
##  Max.   :1370000000   Max.   :1380000000   Max.   :1380000000  
##       2013                 2014                 2015           
##  Min.   :       786   Min.   :       785   Min.   :       790  
##  1st Qu.:   1890000   1st Qu.:   1950000   1st Qu.:   1975000  
##  Median :   8060000   Median :   8210000   Median :   8300000  
##  Mean   :  36762152   Mean   :  37239160   Mean   :  37651938  
##  3rd Qu.:  25350000   3rd Qu.:  26050000   3rd Qu.:  26750000  
##  Max.   :1390000000   Max.   :1400000000   Max.   :1410000000  
##       2016                 2017                 2018           
##  Min.   :       791   Min.   :       798   Min.   :       810  
##  1st Qu.:   1990000   1st Qu.:   2005000   1st Qu.:   2005000  
##  Median :   8380000   Median :   8460000   Median :   8610000  
##  Mean   :  38035941   Mean   :  38513451   Mean   :  38925067  
##  3rd Qu.:  27250000   3rd Qu.:  27700000   3rd Qu.:  28300000  
##  Max.   :1410000000   Max.   :1420000000   Max.   :1430000000  
##       2019                 2020                 2021           
##  Min.   :       815   Min.   :       809   Min.   :       812  
##  1st Qu.:   2000000   1st Qu.:   2025000   1st Qu.:   2050000  
##  Median :   8770000   Median :   8740000   Median :   8790000  
##  Mean   :  39356033   Mean   :  39776967   Mean   :  40133317  
##  3rd Qu.:  28550000   3rd Qu.:  28750000   3rd Qu.:  29200000  
##  Max.   :1430000000   Max.   :1440000000   Max.   :1440000000  
##       2022                 2023                 2024           
##  Min.   :       808   Min.   :       808   Min.   :       803  
##  1st Qu.:   2070000   1st Qu.:   2080000   1st Qu.:   2080000  
##  Median :   8920000   Median :   9060000   Median :   9100000  
##  Mean   :  40617215   Mean   :  40979613   Mean   :  41386823  
##  3rd Qu.:  29750000   3rd Qu.:  30400000   3rd Qu.:  31050000  
##  Max.   :1450000000   Max.   :1450000000   Max.   :1460000000  
##       2025                 2026                 2027           
##  Min.   :       799   Min.   :       795   Min.   :       793  
##  1st Qu.:   2070000   1st Qu.:   2070000   1st Qu.:   2070000  
##  Median :   9310000   Median :   9370000   Median :   9340000  
##  Mean   :  41794739   Mean   :  42156517   Mean   :  42522279  
##  3rd Qu.:  31650000   3rd Qu.:  32250000   3rd Qu.:  32800000  
##  Max.   :1460000000   Max.   :1460000000   Max.   :1470000000  
##       2028                 2029                 2030           
##  Min.   :       803   Min.   :       797   Min.   :       799  
##  1st Qu.:   2060000   1st Qu.:   2060000   1st Qu.:   2055000  
##  Median :   9320000   Median :   9380000   Median :   9340000  
##  Mean   :  42880313   Mean   :  43238552   Mean   :  43590590  
##  3rd Qu.:  33400000   3rd Qu.:  34200000   3rd Qu.:  35200000  
##  Max.   :1480000000   Max.   :1490000000   Max.   :1500000000  
##       2031                 2032                 2033           
##  Min.   :       794   Min.   :       795   Min.   :       789  
##  1st Qu.:   2045000   1st Qu.:   2055000   1st Qu.:   2060000  
##  Median :   9300000   Median :   9270000   Median :   9310000  
##  Mean   :  43938099   Mean   :  44289922   Mean   :  44644246  
##  3rd Qu.:  36200000   3rd Qu.:  36650000   3rd Qu.:  36700000  
##  Max.   :1510000000   Max.   :1520000000   Max.   :1530000000  
##       2034                 2035                 2036           
##  Min.   :       801   Min.   :       801   Min.   :       805  
##  1st Qu.:   2080000   1st Qu.:   2115000   1st Qu.:   2145000  
##  Median :   9350000   Median :   9390000   Median :   9420000  
##  Mean   :  44994616   Mean   :  45333335   Mean   :  45690889  
##  3rd Qu.:  37000000   3rd Qu.:  37650000   3rd Qu.:  38150000  
##  Max.   :1540000000   Max.   :1550000000   Max.   :1560000000  
##       2037                 2038                 2039           
##  Min.   :       807   Min.   :       806   Min.   :       800  
##  1st Qu.:   2175000   1st Qu.:   2210000   1st Qu.:   2245000  
##  Median :   9460000   Median :   9490000   Median :   9520000  
##  Mean   :  46028786   Mean   :  46310827   Mean   :  46662861  
##  3rd Qu.:  38500000   3rd Qu.:  38450000   3rd Qu.:  38400000  
##  Max.   :1570000000   Max.   :1580000000   Max.   :1590000000  
##       2040                 2041                 2042           
##  Min.   :       797   Min.   :       795   Min.   :       800  
##  1st Qu.:   2240000   1st Qu.:   2240000   1st Qu.:   2240000  
##  Median :   9510000   Median :   9470000   Median :   9420000  
##  Mean   :  46960306   Mean   :  47295965   Mean   :  47576594  
##  3rd Qu.:  38700000   3rd Qu.:  38900000   3rd Qu.:  39100000  
##  Max.   :1590000000   Max.   :1600000000   Max.   :1610000000  
##       2043                 2044                 2045           
##  Min.   :       798   Min.   :       794   Min.   :       796  
##  1st Qu.:   2235000   1st Qu.:   2230000   1st Qu.:   2230000  
##  Median :   9380000   Median :   9380000   Median :   9340000  
##  Mean   :  47861406   Mean   :  48146269   Mean   :  48423070  
##  3rd Qu.:  39300000   3rd Qu.:  39450000   3rd Qu.:  39650000  
##  Max.   :1610000000   Max.   :1620000000   Max.   :1620000000  
##       2046                 2047                 2048           
##  Min.   :       802   Min.   :       811   Min.   :       811  
##  1st Qu.:   2230000   1st Qu.:   2230000   1st Qu.:   2225000  
##  Median :   9310000   Median :   9360000   Median :   9400000  
##  Mean   :  48691922   Mean   :  48968913   Mean   :  49173180  
##  3rd Qu.:  40050000   3rd Qu.:  40950000   3rd Qu.:  41750000  
##  Max.   :1630000000   Max.   :1630000000   Max.   :1630000000  
##       2049                 2050                 2051           
##  Min.   :       807   Min.   :       806   Min.   :       804  
##  1st Qu.:   2225000   1st Qu.:   2220000   1st Qu.:   2215000  
##  Median :   9440000   Median :   9480000   Median :   9520000  
##  Mean   :  49507944   Mean   :  49702902   Mean   :  49970213  
##  3rd Qu.:  42650000   3rd Qu.:  43150000   3rd Qu.:  43250000  
##  Max.   :1640000000   Max.   :1640000000   Max.   :1640000000  
##       2052                 2053                 2054           
##  Min.   :       806   Min.   :       803   Min.   :       805  
##  1st Qu.:   2210000   1st Qu.:   2210000   1st Qu.:   2215000  
##  Median :   9640000   Median :   9800000   Median :   9860000  
##  Mean   :  50170956   Mean   :  50432046   Mean   :  50679833  
##  3rd Qu.:  43250000   3rd Qu.:  43200000   3rd Qu.:  43100000  
##  Max.   :1640000000   Max.   :1650000000   Max.   :1650000000  
##       2055                 2056                 2057           
##  Min.   :       803   Min.   :       805   Min.   :       805  
##  1st Qu.:   2215000   1st Qu.:   2210000   1st Qu.:   2205000  
##  Median :   9850000   Median :   9940000   Median :   9960000  
##  Mean   :  50878145   Mean   :  51066860   Mean   :  51303949  
##  3rd Qu.:  43400000   3rd Qu.:  43900000   3rd Qu.:  43900000  
##  Max.   :1650000000   Max.   :1650000000   Max.   :1650000000  
##       2058                 2059                 2060           
##  Min.   :       804   Min.   :       819   Min.   :       822  
##  1st Qu.:   2205000   1st Qu.:   2200000   1st Qu.:   2195000  
##  Median :   9980000   Median :   9990000   Median :  10000000  
##  Mean   :  51489721   Mean   :  51656918   Mean   :  51832228  
##  3rd Qu.:  43800000   3rd Qu.:  44200000   3rd Qu.:  44600000  
##  Max.   :1650000000   Max.   :1650000000   Max.   :1650000000  
##       2061                 2062                 2063           
##  Min.   :       816   Min.   :       817   Min.   :       817  
##  1st Qu.:   2195000   1st Qu.:   2185000   1st Qu.:   2185000  
##  Median :  10000000   Median :  10100000   Median :  10200000  
##  Mean   :  52068033   Mean   :  52240214   Mean   :  52397949  
##  3rd Qu.:  44750000   3rd Qu.:  45050000   3rd Qu.:  45500000  
##  Max.   :1650000000   Max.   :1650000000   Max.   :1650000000  
##       2064                 2065                 2066           
##  Min.   :       814   Min.   :       810   Min.   :       808  
##  1st Qu.:   2175000   1st Qu.:   2170000   1st Qu.:   2165000  
##  Median :  10300000   Median :  10300000   Median :  10300000  
##  Mean   :  52566170   Mean   :  52723935   Mean   :  52881223  
##  3rd Qu.:  45900000   3rd Qu.:  46350000   3rd Qu.:  46450000  
##  Max.   :1650000000   Max.   :1640000000   Max.   :1640000000  
##       2067                 2068                 2069           
##  Min.   :       804   Min.   :       803   Min.   :       797  
##  1st Qu.:   2150000   1st Qu.:   2125000   1st Qu.:   2105000  
##  Median :  10300000   Median :  10300000   Median :  10300000  
##  Mean   :  53022058   Mean   :  53195269   Mean   :  53320648  
##  3rd Qu.:  46500000   3rd Qu.:  46600000   3rd Qu.:  46700000  
##  Max.   :1640000000   Max.   :1640000000   Max.   :1630000000  
##       2070                 2071                 2072           
##  Min.   :       788   Min.   :       784   Min.   :       787  
##  1st Qu.:   2100000   1st Qu.:   2095000   1st Qu.:   2090000  
##  Median :  10400000   Median :  10400000   Median :  10400000  
##  Mean   :  53446746   Mean   :  53609024   Mean   :  53688691  
##  3rd Qu.:  46850000   3rd Qu.:  47050000   3rd Qu.:  47500000  
##  Max.   :1630000000   Max.   :1630000000   Max.   :1620000000  
##       2073                 2074                 2075           
##  Min.   :       793   Min.   :       793   Min.   :       793  
##  1st Qu.:   2085000   1st Qu.:   2080000   1st Qu.:   2070000  
##  Median :  10400000   Median :  10400000   Median :  10500000  
##  Mean   :  53870773   Mean   :  53937000   Mean   :  54059692  
##  3rd Qu.:  47800000   3rd Qu.:  48050000   3rd Qu.:  48400000  
##  Max.   :1620000000   Max.   :1610000000   Max.   :1610000000  
##       2076                 2077                 2078           
##  Min.   :       794   Min.   :       793   Min.   :       795  
##  1st Qu.:   2060000   1st Qu.:   2055000   1st Qu.:   2050000  
##  Median :  10500000   Median :  10600000   Median :  10600000  
##  Mean   :  54136676   Mean   :  54301004   Mean   :  54352875  
##  3rd Qu.:  48650000   3rd Qu.:  48950000   3rd Qu.:  49250000  
##  Max.   :1600000000   Max.   :1600000000   Max.   :1590000000  
##       2079                 2080                 2081           
##  Min.   :       792   Min.   :       791   Min.   :       793  
##  1st Qu.:   2045000   1st Qu.:   2035000   1st Qu.:   2050000  
##  Median :  10600000   Median :  10700000   Median :  10700000  
##  Mean   :  54475286   Mean   :  54588932   Mean   :  54629708  
##  3rd Qu.:  49450000   3rd Qu.:  49250000   3rd Qu.:  49150000  
##  Max.   :1590000000   Max.   :1580000000   Max.   :1570000000  
##       2082                 2083                 2084           
##  Min.   :       792   Min.   :       794   Min.   :       795  
##  1st Qu.:   2065000   1st Qu.:   2080000   1st Qu.:   2095000  
##  Median :  10700000   Median :  10700000   Median :  10800000  
##  Mean   :  54729150   Mean   :  54821670   Mean   :  54912716  
##  3rd Qu.:  48950000   3rd Qu.:  48750000   3rd Qu.:  48550000  
##  Max.   :1570000000   Max.   :1560000000   Max.   :1560000000  
##       2085                 2086                 2087           
##  Min.   :       793   Min.   :       791   Min.   :       789  
##  1st Qu.:   2105000   1st Qu.:   2120000   1st Qu.:   2130000  
##  Median :  10800000   Median :  10800000   Median :  10800000  
##  Mean   :  54945072   Mean   :  55030349   Mean   :  55117114  
##  3rd Qu.:  48350000   3rd Qu.:  48150000   3rd Qu.:  47950000  
##  Max.   :1550000000   Max.   :1540000000   Max.   :1540000000  
##       2088                 2089                 2090           
##  Min.   :       789   Min.   :       791   Min.   :       790  
##  1st Qu.:   2120000   1st Qu.:   2110000   1st Qu.:   2115000  
##  Median :  10900000   Median :  10900000   Median :  10900000  
##  Mean   :  55136105   Mean   :  55213033   Mean   :  55265315  
##  3rd Qu.:  47650000   3rd Qu.:  47450000   3rd Qu.:  47400000  
##  Max.   :1530000000   Max.   :1520000000   Max.   :1520000000  
##       2091                 2092                 2093           
##  Min.   :       789   Min.   :       790   Min.   :       790  
##  1st Qu.:   2125000   1st Qu.:   2130000   1st Qu.:   2130000  
##  Median :  10900000   Median :  10900000   Median :  10900000  
##  Mean   :  55280456   Mean   :  55352378   Mean   :  55404660  
##  3rd Qu.:  47450000   3rd Qu.:  47550000   3rd Qu.:  47650000  
##  Max.   :1510000000   Max.   :1500000000   Max.   :1500000000  
##       2094                 2095                 2096           
##  Min.   :       791   Min.   :       791   Min.   :       793  
##  1st Qu.:   2125000   1st Qu.:   2110000   1st Qu.:   2090000  
##  Median :  11000000   Median :  11000000   Median :  11000000  
##  Mean   :  55464717   Mean   :  55459866   Mean   :  55543471  
##  3rd Qu.:  47550000   3rd Qu.:  47200000   3rd Qu.:  46850000  
##  Max.   :1490000000   Max.   :1480000000   Max.   :1480000000  
##       2097                 2098                 2099           
##  Min.   :       796   Min.   :       796   Min.   :       795  
##  1st Qu.:   2075000   1st Qu.:   2060000   1st Qu.:   2040000  
##  Median :  11000000   Median :  11000000   Median :  11000000  
##  Mean   :  55559477   Mean   :  55586126   Mean   :  55557763  
##  3rd Qu.:  46550000   3rd Qu.:  46550000   3rd Qu.:  46550000  
##  Max.   :1470000000   Max.   :1460000000   Max.   :1450000000  
##       2100           
##  Min.   :       797  
##  1st Qu.:   2025000  
##  Median :  11000000  
##  Mean   :  55594536  
##  3rd Qu.:  46600000  
##  Max.   :1450000000
```

```r
glimpse(pop_total)
```

```
## Rows: 195
## Columns: 302
## $ country <chr> "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Ant~
## $ `1800`  <dbl> 3280000, 400000, 2500000, 2650, 1570000, 37000, 534000, 413000~
## $ `1801`  <dbl> 3280000, 402000, 2510000, 2650, 1570000, 37000, 520000, 413000~
## $ `1802`  <dbl> 3280000, 404000, 2520000, 2650, 1570000, 37000, 506000, 413000~
## $ `1803`  <dbl> 3280000, 405000, 2530000, 2650, 1570000, 37000, 492000, 413000~
## $ `1804`  <dbl> 3280000, 407000, 2540000, 2650, 1570000, 37000, 479000, 413000~
## $ `1805`  <dbl> 3280000, 409000, 2550000, 2650, 1570000, 37000, 466000, 413000~
## $ `1806`  <dbl> 3280000, 411000, 2560000, 2650, 1570000, 37000, 453000, 413000~
## $ `1807`  <dbl> 3280000, 413000, 2560000, 2650, 1570000, 37000, 441000, 413000~
## $ `1808`  <dbl> 3280000, 414000, 2570000, 2650, 1570000, 37000, 429000, 413000~
## $ `1809`  <dbl> 3280000, 416000, 2580000, 2650, 1570000, 37000, 417000, 413000~
## $ `1810`  <dbl> 3280000, 418000, 2590000, 2650, 1570000, 37000, 420000, 413000~
## $ `1811`  <dbl> 3280000, 420000, 2600000, 2650, 1570000, 37000, 422000, 413000~
## $ `1812`  <dbl> 3280000, 422000, 2610000, 2650, 1570000, 37000, 429000, 413000~
## $ `1813`  <dbl> 3280000, 424000, 2620000, 2650, 1570000, 37000, 441000, 413000~
## $ `1814`  <dbl> 3280000, 426000, 2630000, 2650, 1570000, 37000, 453000, 413000~
## $ `1815`  <dbl> 3280000, 427000, 2640000, 2650, 1570000, 37000, 466000, 413000~
## $ `1816`  <dbl> 3280000, 429000, 2650000, 2650, 1570000, 37000, 479000, 413000~
## $ `1817`  <dbl> 3280000, 431000, 2660000, 2650, 1570000, 37000, 492000, 413000~
## $ `1818`  <dbl> 3280000, 433000, 2670000, 2650, 1570000, 37000, 506000, 413000~
## $ `1819`  <dbl> 3280000, 435000, 2680000, 2660, 1570000, 37000, 519000, 414000~
## $ `1820`  <dbl> 3290000, 437000, 2690000, 2670, 1580000, 37000, 531000, 416000~
## $ `1821`  <dbl> 3300000, 439000, 2690000, 2680, 1590000, 37000, 542000, 418000~
## $ `1822`  <dbl> 3310000, 441000, 2700000, 2690, 1610000, 37000, 553000, 421000~
## $ `1823`  <dbl> 3320000, 443000, 2700000, 2710, 1640000, 37000, 562000, 425000~
## $ `1824`  <dbl> 3340000, 445000, 2710000, 2730, 1660000, 37000, 572000, 429000~
## $ `1825`  <dbl> 3350000, 447000, 2710000, 2750, 1690000, 37000, 582000, 433000~
## $ `1826`  <dbl> 3370000, 449000, 2720000, 2770, 1710000, 37000, 592000, 437000~
## $ `1827`  <dbl> 3380000, 451000, 2720000, 2790, 1740000, 37000, 602000, 441000~
## $ `1828`  <dbl> 3400000, 453000, 2730000, 2810, 1760000, 37000, 613000, 445000~
## $ `1829`  <dbl> 3410000, 455000, 2730000, 2830, 1790000, 37000, 624000, 449000~
## $ `1830`  <dbl> 3430000, 457000, 2740000, 2850, 1810000, 37000, 635000, 453000~
## $ `1831`  <dbl> 3450000, 459000, 2740000, 2870, 1840000, 37000, 647000, 457000~
## $ `1832`  <dbl> 3460000, 461000, 2750000, 2890, 1870000, 37000, 659000, 461000~
## $ `1833`  <dbl> 3480000, 463000, 2750000, 2910, 1900000, 37000, 672000, 465000~
## $ `1834`  <dbl> 3490000, 465000, 2760000, 2930, 1920000, 37000, 685000, 470000~
## $ `1835`  <dbl> 3510000, 467000, 2760000, 2950, 1950000, 37000, 698000, 474000~
## $ `1836`  <dbl> 3520000, 470000, 2770000, 2970, 1980000, 37000, 712000, 478000~
## $ `1837`  <dbl> 3540000, 472000, 2770000, 2990, 2010000, 37000, 725000, 483000~
## $ `1838`  <dbl> 3550000, 474000, 2780000, 3010, 2040000, 37000, 739000, 487000~
## $ `1839`  <dbl> 3570000, 476000, 2780000, 3030, 2070000, 37000, 756000, 492000~
## $ `1840`  <dbl> 3590000, 478000, 2790000, 3060, 2100000, 37000, 776000, 496000~
## $ `1841`  <dbl> 3600000, 480000, 2790000, 3080, 2130000, 37000, 800000, 501000~
## $ `1842`  <dbl> 3620000, 482000, 2800000, 3100, 2160000, 37000, 826000, 505000~
## $ `1843`  <dbl> 3630000, 485000, 2800000, 3120, 2190000, 37000, 857000, 510000~
## $ `1844`  <dbl> 3650000, 487000, 2810000, 3140, 2230000, 37000, 888000, 515000~
## $ `1845`  <dbl> 3670000, 489000, 2810000, 3170, 2260000, 37000, 920000, 519000~
## $ `1846`  <dbl> 3680000, 491000, 2820000, 3190, 2290000, 37000, 954000, 524000~
## $ `1847`  <dbl> 3700000, 493000, 2820000, 3210, 2330000, 37000, 989000, 529000~
## $ `1848`  <dbl> 3720000, 496000, 2830000, 3230, 2360000, 37000, 1030000, 53400~
## $ `1849`  <dbl> 3730000, 498000, 2840000, 3260, 2390000, 37000, 1060000, 53900~
## $ `1850`  <dbl> 3750000, 501000, 2850000, 3280, 2420000, 37000, 1090000, 54400~
## $ `1851`  <dbl> 3770000, 505000, 2870000, 3300, 2450000, 37000, 1120000, 54900~
## $ `1852`  <dbl> 3790000, 510000, 2900000, 3320, 2470000, 37000, 1140000, 55300~
## $ `1853`  <dbl> 3810000, 515000, 2920000, 3350, 2490000, 37000, 1160000, 55800~
## $ `1854`  <dbl> 3830000, 520000, 2950000, 3370, 2510000, 37000, 1180000, 56300~
## $ `1855`  <dbl> 3840000, 525000, 2980000, 3390, 2530000, 37000, 1200000, 56800~
## $ `1856`  <dbl> 3860000, 530000, 3010000, 3420, 2550000, 37000, 1220000, 57400~
## $ `1857`  <dbl> 3880000, 535000, 3040000, 3440, 2570000, 37000, 1240000, 57900~
## $ `1858`  <dbl> 3900000, 540000, 3070000, 3460, 2600000, 36900, 1260000, 58400~
## $ `1859`  <dbl> 3920000, 545000, 3100000, 3490, 2620000, 36900, 1290000, 58900~
## $ `1860`  <dbl> 3940000, 551000, 3130000, 3510, 2640000, 36800, 1320000, 59400~
## $ `1861`  <dbl> 3960000, 556000, 3170000, 3540, 2660000, 36700, 1350000, 60000~
## $ `1862`  <dbl> 3980000, 561000, 3200000, 3560, 2680000, 36500, 1390000, 60500~
## $ `1863`  <dbl> 4000000, 567000, 3230000, 3590, 2710000, 36300, 1440000, 61000~
## $ `1864`  <dbl> 4020000, 572000, 3260000, 3610, 2730000, 36100, 1480000, 61600~
## $ `1865`  <dbl> 4040000, 578000, 3290000, 3640, 2750000, 35900, 1530000, 62100~
## $ `1866`  <dbl> 4060000, 584000, 3330000, 3660, 2780000, 35700, 1580000, 62700~
## $ `1867`  <dbl> 4080000, 589000, 3360000, 3690, 2800000, 35500, 1630000, 63300~
## $ `1868`  <dbl> 4100000, 595000, 3390000, 3710, 2820000, 35300, 1680000, 63800~
## $ `1869`  <dbl> 4120000, 601000, 3430000, 3740, 2840000, 35100, 1740000, 64400~
## $ `1870`  <dbl> 4140000, 606000, 3460000, 3760, 2870000, 35000, 1790000, 65000~
## $ `1871`  <dbl> 4160000, 612000, 3500000, 3790, 2890000, 34900, 1850000, 65500~
## $ `1872`  <dbl> 4180000, 618000, 3530000, 3820, 2910000, 34900, 1910000, 66100~
## $ `1873`  <dbl> 4210000, 624000, 3570000, 3840, 2930000, 34900, 1970000, 66700~
## $ `1874`  <dbl> 4230000, 630000, 3600000, 3870, 2940000, 34900, 2030000, 67300~
## $ `1875`  <dbl> 4250000, 636000, 3640000, 3900, 2960000, 34800, 2100000, 67900~
## $ `1876`  <dbl> 4270000, 642000, 3670000, 3920, 2980000, 34800, 2170000, 68500~
## $ `1877`  <dbl> 4290000, 648000, 3710000, 3950, 3000000, 34800, 2240000, 69100~
## $ `1878`  <dbl> 4310000, 655000, 3750000, 3980, 3020000, 34800, 2310000, 69800~
## $ `1879`  <dbl> 4330000, 661000, 3790000, 4010, 3040000, 34900, 2380000, 70400~
## $ `1880`  <dbl> 4350000, 667000, 3820000, 4030, 3060000, 34900, 2460000, 71000~
## $ `1881`  <dbl> 4380000, 674000, 3860000, 4060, 3080000, 35000, 2540000, 71600~
## $ `1882`  <dbl> 4400000, 680000, 3900000, 4090, 3100000, 35200, 2620000, 72300~
## $ `1883`  <dbl> 4420000, 687000, 3940000, 4120, 3120000, 35400, 2700000, 72900~
## $ `1884`  <dbl> 4440000, 693000, 3980000, 4150, 3140000, 35600, 2790000, 73600~
## $ `1885`  <dbl> 4460000, 700000, 4020000, 4170, 3160000, 35800, 2880000, 74200~
## $ `1886`  <dbl> 4490000, 707000, 4060000, 4200, 3180000, 36000, 2970000, 74900~
## $ `1887`  <dbl> 4510000, 714000, 4100000, 4230, 3200000, 36200, 3060000, 75600~
## $ `1888`  <dbl> 4530000, 720000, 4140000, 4260, 3220000, 36300, 3160000, 76300~
## $ `1889`  <dbl> 4550000, 727000, 4180000, 4290, 3230000, 36500, 3260000, 76900~
## $ `1890`  <dbl> 4580000, 735000, 4220000, 4320, 3240000, 36500, 3370000, 77600~
## $ `1891`  <dbl> 4600000, 742000, 4260000, 4350, 3240000, 36400, 3480000, 78300~
## $ `1892`  <dbl> 4620000, 749000, 4300000, 4380, 3240000, 36300, 3600000, 79000~
## $ `1893`  <dbl> 4650000, 757000, 4340000, 4410, 3230000, 36100, 3720000, 79700~
## $ `1894`  <dbl> 4670000, 764000, 4390000, 4440, 3210000, 35900, 3840000, 80400~
## $ `1895`  <dbl> 4690000, 772000, 4430000, 4470, 3200000, 35700, 3970000, 81200~
## $ `1896`  <dbl> 4710000, 780000, 4470000, 4500, 3190000, 35500, 4100000, 81900~
## $ `1897`  <dbl> 4740000, 788000, 4510000, 4540, 3180000, 35300, 4240000, 82600~
## $ `1898`  <dbl> 4760000, 795000, 4560000, 4570, 3170000, 35100, 4380000, 83400~
## $ `1899`  <dbl> 4790000, 803000, 4610000, 4600, 3150000, 34900, 4530000, 84100~
## $ `1900`  <dbl> 4830000, 811000, 4660000, 4630, 3140000, 34600, 4690000, 84900~
## $ `1901`  <dbl> 4880000, 819000, 4730000, 4660, 3120000, 34400, 4860000, 85600~
## $ `1902`  <dbl> 4940000, 826000, 4800000, 4690, 3100000, 34100, 5040000, 86400~
## $ `1903`  <dbl> 5000000, 834000, 4880000, 4730, 3080000, 33800, 5230000, 87100~
## $ `1904`  <dbl> 5060000, 842000, 4950000, 4760, 3060000, 33400, 5430000, 87900~
## $ `1905`  <dbl> 5130000, 849000, 5030000, 4790, 3040000, 33100, 5640000, 88700~
## $ `1906`  <dbl> 5200000, 857000, 5120000, 4830, 3020000, 32800, 5860000, 89500~
## $ `1907`  <dbl> 5260000, 865000, 5200000, 4860, 3000000, 32500, 6080000, 90300~
## $ `1908`  <dbl> 5330000, 873000, 5280000, 4890, 2980000, 32200, 6310000, 91100~
## $ `1909`  <dbl> 5470000, 881000, 5360000, 4940, 2960000, 32000, 6540000, 91900~
## $ `1910`  <dbl> 5680000, 888000, 5420000, 4990, 2950000, 31700, 6760000, 92800~
## $ `1911`  <dbl> 5980000, 895000, 5470000, 5060, 2930000, 31500, 6960000, 93600~
## $ `1912`  <dbl> 6360000, 901000, 5510000, 5150, 2920000, 31200, 7160000, 94400~
## $ `1913`  <dbl> 6850000, 907000, 5530000, 5240, 2920000, 31000, 7350000, 95300~
## $ `1914`  <dbl> 7370000, 913000, 5560000, 5330, 2910000, 30800, 7540000, 96100~
## $ `1915`  <dbl> 7920000, 919000, 5590000, 5430, 2900000, 30600, 7740000, 97000~
## $ `1916`  <dbl> 8520000, 926000, 5620000, 5530, 2890000, 30400, 7940000, 97800~
## $ `1917`  <dbl> 9170000, 932000, 5640000, 5630, 2880000, 30200, 8150000, 98700~
## $ `1918`  <dbl> 9880000, 938000, 5670000, 5740, 2870000, 30000, 8360000, 99600~
## $ `1919`  <dbl> 10300000, 944000, 5710000, 5800, 2870000, 29900, 8590000, 1010~
## $ `1920`  <dbl> 10600000, 949000, 5750000, 5820, 2880000, 29800, 8830000, 1010~
## $ `1921`  <dbl> 10500000, 955000, 5800000, 5800, 2900000, 29800, 9080000, 1020~
## $ `1922`  <dbl> 10300000, 960000, 5860000, 5740, 2940000, 29800, 9350000, 1030~
## $ `1923`  <dbl> 9710000, 965000, 5920000, 5630, 2980000, 29900, 9630000, 10400~
## $ `1924`  <dbl> 9200000, 970000, 5990000, 5530, 3030000, 30000, 9910000, 10500~
## $ `1925`  <dbl> 8720000, 975000, 6050000, 5430, 3070000, 30100, 10200000, 1060~
## $ `1926`  <dbl> 8260000, 980000, 6120000, 5330, 3120000, 30200, 10500000, 1070~
## $ `1927`  <dbl> 7830000, 985000, 6190000, 5240, 3160000, 30300, 10800000, 1080~
## $ `1928`  <dbl> 7420000, 990000, 6260000, 5140, 3210000, 30400, 11200000, 1090~
## $ `1929`  <dbl> 7100000, 997000, 6340000, 5080, 3260000, 30700, 11500000, 1100~
## $ `1930`  <dbl> 6870000, 1000000, 6420000, 5040, 3310000, 31100, 11700000, 111~
## $ `1931`  <dbl> 6720000, 1010000, 6520000, 5040, 3350000, 31800, 12000000, 112~
## $ `1932`  <dbl> 6640000, 1020000, 6630000, 5060, 3400000, 32600, 12200000, 113~
## $ `1933`  <dbl> 6640000, 1030000, 6740000, 5120, 3450000, 33600, 12500000, 114~
## $ `1934`  <dbl> 6640000, 1040000, 6860000, 5170, 3500000, 34600, 12700000, 115~
## $ `1935`  <dbl> 6640000, 1050000, 6990000, 5230, 3550000, 35700, 12900000, 116~
## $ `1936`  <dbl> 6640000, 1060000, 7110000, 5280, 3600000, 36800, 13100000, 117~
## $ `1937`  <dbl> 6640000, 1080000, 7230000, 5340, 3650000, 37900, 13400000, 119~
## $ `1938`  <dbl> 6640000, 1090000, 7360000, 5390, 3700000, 39100, 13600000, 120~
## $ `1939`  <dbl> 6660000, 1100000, 7490000, 5450, 3750000, 40100, 13800000, 121~
## $ `1940`  <dbl> 6700000, 1110000, 7610000, 5510, 3810000, 41000, 14100000, 122~
## $ `1941`  <dbl> 6770000, 1120000, 7730000, 5570, 3870000, 41700, 14400000, 123~
## $ `1942`  <dbl> 6850000, 1140000, 7860000, 5630, 3940000, 42200, 14600000, 124~
## $ `1943`  <dbl> 6950000, 1150000, 7970000, 5690, 4010000, 42600, 14900000, 125~
## $ `1944`  <dbl> 7060000, 1160000, 8090000, 5750, 4080000, 43000, 15200000, 126~
## $ `1945`  <dbl> 7170000, 1180000, 8210000, 5810, 4160000, 43300, 15500000, 127~
## $ `1946`  <dbl> 7280000, 1190000, 8340000, 5870, 4230000, 43700, 15800000, 129~
## $ `1947`  <dbl> 7390000, 1210000, 8460000, 5930, 4310000, 44100, 16100000, 130~
## $ `1948`  <dbl> 7500000, 1220000, 8590000, 5990, 4390000, 44500, 16400000, 131~
## $ `1949`  <dbl> 7620000, 1240000, 8730000, 6080, 4470000, 45000, 16700000, 133~
## $ `1950`  <dbl> 7750000, 1260000, 8870000, 6200, 4550000, 45500, 17000000, 135~
## $ `1951`  <dbl> 7840000, 1290000, 9020000, 6690, 4620000, 46500, 17400000, 138~
## $ `1952`  <dbl> 7940000, 1320000, 9190000, 7250, 4710000, 47400, 17700000, 142~
## $ `1953`  <dbl> 8040000, 1350000, 9360000, 7860, 4820000, 48100, 18100000, 146~
## $ `1954`  <dbl> 8150000, 1380000, 9560000, 8530, 4940000, 48900, 18400000, 151~
## $ `1955`  <dbl> 8270000, 1420000, 9770000, 9230, 5040000, 49600, 18800000, 156~
## $ `1956`  <dbl> 8400000, 1460000, 10000000, 9980, 5140000, 50500, 19100000, 16~
## $ `1957`  <dbl> 8540000, 1500000, 10300000, 10800, 5230000, 51400, 19500000, 1~
## $ `1958`  <dbl> 8680000, 1540000, 10500000, 11600, 5310000, 52300, 19800000, 1~
## $ `1959`  <dbl> 8830000, 1590000, 10800000, 12500, 5380000, 53200, 20100000, 1~
## $ `1960`  <dbl> 9000000, 1640000, 11100000, 13400, 5450000, 54100, 20500000, 1~
## $ `1961`  <dbl> 9170000, 1690000, 11300000, 14400, 5530000, 55000, 20800000, 1~
## $ `1962`  <dbl> 9350000, 1740000, 11600000, 15400, 5610000, 55800, 21200000, 2~
## $ `1963`  <dbl> 9540000, 1790000, 11900000, 16400, 5680000, 56700, 21500000, 2~
## $ `1964`  <dbl> 9740000, 1840000, 12200000, 17500, 5730000, 57600, 21800000, 2~
## $ `1965`  <dbl> 9960000, 1900000, 12600000, 18500, 5770000, 58700, 22200000, 2~
## $ `1966`  <dbl> 10200000, 1950000, 12900000, 19600, 5780000, 59900, 22500000, ~
## $ `1967`  <dbl> 10400000, 2000000, 13300000, 20800, 5770000, 61200, 22800000, ~
## $ `1968`  <dbl> 10600000, 2050000, 13700000, 21900, 5770000, 62500, 23200000, ~
## $ `1969`  <dbl> 10900000, 2100000, 14100000, 23100, 5800000, 63600, 23500000, ~
## $ `1970`  <dbl> 11200000, 2150000, 14500000, 24300, 5890000, 64200, 23900000, ~
## $ `1971`  <dbl> 11500000, 2200000, 14900000, 25600, 6040000, 64400, 24300000, ~
## $ `1972`  <dbl> 11800000, 2250000, 15300000, 26900, 6250000, 64100, 24700000, ~
## $ `1973`  <dbl> 12100000, 2310000, 15700000, 28200, 6500000, 63600, 25100000, ~
## $ `1974`  <dbl> 12400000, 2360000, 16100000, 29500, 6760000, 63100, 25500000, ~
## $ `1975`  <dbl> 12700000, 2410000, 16600000, 30700, 7020000, 62700, 25900000, ~
## $ `1976`  <dbl> 12900000, 2460000, 17100000, 31800, 7280000, 62400, 26300000, ~
## $ `1977`  <dbl> 13200000, 2520000, 17600000, 32800, 7530000, 62200, 26700000, ~
## $ `1978`  <dbl> 13300000, 2570000, 18100000, 33700, 7790000, 62000, 27100000, ~
## $ `1979`  <dbl> 13400000, 2630000, 18600000, 34800, 8060000, 61900, 27500000, ~
## $ `1980`  <dbl> 13400000, 2680000, 19200000, 36100, 8340000, 61900, 27900000, ~
## $ `1981`  <dbl> 13200000, 2740000, 19800000, 37500, 8640000, 61800, 28300000, ~
## $ `1982`  <dbl> 12900000, 2790000, 20500000, 39100, 8950000, 61800, 28800000, ~
## $ `1983`  <dbl> 12500000, 2840000, 21100000, 40900, 9280000, 61800, 29300000, ~
## $ `1984`  <dbl> 12200000, 2900000, 21800000, 42700, 9610000, 61800, 29700000, ~
## $ `1985`  <dbl> 11900000, 2970000, 22400000, 44600, 9960000, 61800, 30200000, ~
## $ `1986`  <dbl> 11700000, 3040000, 23100000, 46500, 10300000, 61800, 30700000,~
## $ `1987`  <dbl> 11600000, 3120000, 23800000, 48500, 10700000, 61700, 31200000,~
## $ `1988`  <dbl> 11600000, 3200000, 24400000, 50400, 11100000, 61800, 31700000,~
## $ `1989`  <dbl> 11900000, 3260000, 25100000, 52500, 11500000, 62000, 32100000,~
## $ `1990`  <dbl> 12400000, 3290000, 25800000, 54500, 11800000, 62500, 32600000,~
## $ `1991`  <dbl> 13300000, 3280000, 26400000, 56700, 12200000, 63400, 33100000,~
## $ `1992`  <dbl> 14500000, 3250000, 27000000, 58900, 12700000, 64500, 33500000,~
## $ `1993`  <dbl> 15800000, 3200000, 27600000, 61000, 13100000, 65800, 34000000,~
## $ `1994`  <dbl> 17100000, 3150000, 28200000, 62700, 13500000, 67200, 34400000,~
## $ `1995`  <dbl> 18100000, 3110000, 28800000, 63900, 13900000, 68700, 34800000,~
## $ `1996`  <dbl> 18900000, 3100000, 29300000, 64400, 14400000, 70200, 35200000,~
## $ `1997`  <dbl> 19400000, 3100000, 29700000, 64300, 14900000, 71700, 35700000,~
## $ `1998`  <dbl> 19700000, 3110000, 30200000, 64100, 15400000, 73200, 36100000,~
## $ `1999`  <dbl> 20200000, 3120000, 30600000, 64400, 15900000, 74700, 36500000,~
## $ `2000`  <dbl> 20800000, 3130000, 31000000, 65400, 16400000, 76000, 36900000,~
## $ `2001`  <dbl> 21600000, 3130000, 31500000, 67300, 16900000, 77200, 37300000,~
## $ `2002`  <dbl> 22600000, 3130000, 31900000, 70000, 17500000, 78300, 37700000,~
## $ `2003`  <dbl> 23700000, 3120000, 32300000, 73200, 18100000, 79300, 38100000,~
## $ `2004`  <dbl> 24700000, 3100000, 32700000, 76300, 18800000, 80300, 38500000,~
## $ `2005`  <dbl> 25700000, 3090000, 33100000, 78900, 19400000, 81500, 38900000,~
## $ `2006`  <dbl> 26400000, 3060000, 33600000, 81000, 20100000, 82700, 39300000,~
## $ `2007`  <dbl> 27100000, 3030000, 34200000, 82700, 20900000, 84000, 39700000,~
## $ `2008`  <dbl> 27700000, 3000000, 34700000, 83900, 21700000, 85400, 40100000,~
## $ `2009`  <dbl> 28400000, 2970000, 35300000, 84500, 22500000, 86700, 40500000,~
## $ `2010`  <dbl> 29200000, 2950000, 36000000, 84500, 23400000, 88000, 40900000,~
## $ `2011`  <dbl> 30100000, 2930000, 36700000, 83700, 24200000, 89300, 41300000,~
## $ `2012`  <dbl> 31200000, 2910000, 37400000, 82400, 25100000, 90400, 41800000,~
## $ `2013`  <dbl> 32300000, 2900000, 38100000, 80800, 26000000, 91500, 42200000,~
## $ `2014`  <dbl> 33400000, 2900000, 38900000, 79200, 26900000, 92600, 42600000,~
## $ `2015`  <dbl> 34400000, 2890000, 39700000, 78000, 27900000, 93600, 43100000,~
## $ `2016`  <dbl> 35400000, 2890000, 40600000, 77300, 28800000, 94500, 43500000,~
## $ `2017`  <dbl> 36300000, 2880000, 41400000, 77000, 29800000, 95400, 43900000,~
## $ `2018`  <dbl> 37200000, 2880000, 42200000, 77000, 30800000, 96300, 44400000,~
## $ `2019`  <dbl> 38000000, 2880000, 43100000, 77100, 31800000, 97100, 44800000,~
## $ `2020`  <dbl> 38900000, 2880000, 43900000, 77300, 32900000, 97900, 45200000,~
## $ `2021`  <dbl> 39800000, 2870000, 44600000, 77400, 33900000, 98700, 45600000,~
## $ `2022`  <dbl> 40800000, 2870000, 45400000, 77500, 35000000, 99500, 46000000,~
## $ `2023`  <dbl> 41700000, 2860000, 46100000, 77600, 36100000, 100000, 46400000~
## $ `2024`  <dbl> 42600000, 2850000, 46700000, 77700, 37300000, 101000, 46800000~
## $ `2025`  <dbl> 43500000, 2840000, 47400000, 77800, 38500000, 102000, 47200000~
## $ `2026`  <dbl> 44400000, 2830000, 48000000, 77900, 39700000, 102000, 47600000~
## $ `2027`  <dbl> 45400000, 2820000, 48600000, 77900, 40900000, 103000, 48000000~
## $ `2028`  <dbl> 46300000, 2810000, 49200000, 77900, 42200000, 104000, 48300000~
## $ `2029`  <dbl> 47200000, 2800000, 49800000, 77900, 43500000, 104000, 48700000~
## $ `2030`  <dbl> 48100000, 2790000, 50400000, 77900, 44800000, 105000, 49100000~
## $ `2031`  <dbl> 49000000, 2770000, 50900000, 77900, 46200000, 105000, 49400000~
## $ `2032`  <dbl> 49900000, 2760000, 51400000, 78000, 47600000, 106000, 49800000~
## $ `2033`  <dbl> 50800000, 2750000, 52000000, 78000, 49000000, 106000, 50100000~
## $ `2034`  <dbl> 51700000, 2740000, 52500000, 78000, 50500000, 107000, 50400000~
## $ `2035`  <dbl> 52600000, 2720000, 53000000, 78000, 52000000, 107000, 50800000~
## $ `2036`  <dbl> 53500000, 2710000, 53500000, 78000, 53500000, 108000, 51100000~
## $ `2037`  <dbl> 54300000, 2690000, 54100000, 78000, 55000000, 108000, 51400000~
## $ `2038`  <dbl> 55200000, 2670000, 54600000, 77900, 56600000, 108000, 51700000~
## $ `2039`  <dbl> 56100000, 2650000, 55100000, 77900, 58200000, 109000, 52000000~
## $ `2040`  <dbl> 56900000, 2630000, 55600000, 77900, 59800000, 109000, 52300000~
## $ `2041`  <dbl> 57700000, 2620000, 56200000, 77800, 61400000, 109000, 52600000~
## $ `2042`  <dbl> 58600000, 2600000, 56700000, 77700, 63100000, 110000, 52900000~
## $ `2043`  <dbl> 59400000, 2580000, 57300000, 77600, 64800000, 110000, 53100000~
## $ `2044`  <dbl> 60200000, 2550000, 57800000, 77500, 66500000, 110000, 53400000~
## $ `2045`  <dbl> 61000000, 2530000, 58300000, 77300, 68300000, 110000, 53700000~
## $ `2046`  <dbl> 61700000, 2510000, 58900000, 77100, 70100000, 110000, 53900000~
## $ `2047`  <dbl> 62500000, 2490000, 59400000, 76900, 71900000, 111000, 54200000~
## $ `2048`  <dbl> 63200000, 2470000, 59900000, 76700, 73700000, 111000, 54400000~
## $ `2049`  <dbl> 64000000, 2450000, 60400000, 76400, 75500000, 111000, 54600000~
## $ `2050`  <dbl> 64700000, 2420000, 60900000, 76100, 77400000, 111000, 54900000~
## $ `2051`  <dbl> 65400000, 2400000, 61400000, 75800, 79300000, 111000, 55100000~
## $ `2052`  <dbl> 66100000, 2380000, 61900000, 75400, 81200000, 111000, 55300000~
## $ `2053`  <dbl> 66700000, 2360000, 62300000, 75100, 83200000, 111000, 55500000~
## $ `2054`  <dbl> 67400000, 2330000, 62800000, 74600, 85100000, 111000, 55700000~
## $ `2055`  <dbl> 68000000, 2310000, 63200000, 74200, 87100000, 111000, 55900000~
## $ `2056`  <dbl> 68600000, 2290000, 63600000, 73800, 89100000, 111000, 56100000~
## $ `2057`  <dbl> 69200000, 2260000, 64000000, 73300, 91100000, 111000, 56200000~
## $ `2058`  <dbl> 69800000, 2240000, 64300000, 72800, 93200000, 111000, 56400000~
## $ `2059`  <dbl> 70300000, 2210000, 64700000, 72300, 95300000, 111000, 56600000~
## $ `2060`  <dbl> 70800000, 2190000, 65000000, 71800, 97300000, 111000, 56700000~
## $ `2061`  <dbl> 71400000, 2170000, 65300000, 71300, 99500000, 111000, 56800000~
## $ `2062`  <dbl> 71800000, 2140000, 65600000, 70800, 102000000, 110000, 5700000~
## $ `2063`  <dbl> 72300000, 2120000, 65800000, 70200, 104000000, 110000, 5710000~
## $ `2064`  <dbl> 72800000, 2090000, 66100000, 69700, 106000000, 110000, 5720000~
## $ `2065`  <dbl> 73200000, 2070000, 66300000, 69200, 108000000, 110000, 5730000~
## $ `2066`  <dbl> 73600000, 2040000, 66600000, 68700, 110000000, 110000, 5740000~
## $ `2067`  <dbl> 74000000, 2020000, 66800000, 68300, 112000000, 110000, 5750000~
## $ `2068`  <dbl> 74300000, 1990000, 67000000, 67800, 115000000, 110000, 5760000~
## $ `2069`  <dbl> 74700000, 1970000, 67200000, 67400, 117000000, 109000, 5760000~
## $ `2070`  <dbl> 75000000, 1940000, 67400000, 66900, 119000000, 109000, 5770000~
## $ `2071`  <dbl> 75300000, 1920000, 67500000, 66500, 121000000, 109000, 5780000~
## $ `2072`  <dbl> 75500000, 1890000, 67700000, 66200, 124000000, 109000, 5780000~
## $ `2073`  <dbl> 75800000, 1860000, 67900000, 65800, 126000000, 109000, 5780000~
## $ `2074`  <dbl> 76000000, 1830000, 68100000, 65500, 128000000, 109000, 5790000~
## $ `2075`  <dbl> 76200000, 1810000, 68200000, 65200, 131000000, 108000, 5790000~
## $ `2076`  <dbl> 76400000, 1780000, 68400000, 64900, 133000000, 108000, 5790000~
## $ `2077`  <dbl> 76500000, 1750000, 68600000, 64600, 135000000, 108000, 5790000~
## $ `2078`  <dbl> 76700000, 1720000, 68700000, 64400, 137000000, 108000, 5790000~
## $ `2079`  <dbl> 76800000, 1690000, 68900000, 64200, 140000000, 108000, 5790000~
## $ `2080`  <dbl> 76900000, 1660000, 69000000, 64000, 142000000, 107000, 5790000~
## $ `2081`  <dbl> 76900000, 1630000, 69200000, 63900, 144000000, 107000, 5790000~
## $ `2082`  <dbl> 77000000, 1600000, 69300000, 63700, 147000000, 107000, 5790000~
## $ `2083`  <dbl> 77000000, 1570000, 69500000, 63600, 149000000, 107000, 5790000~
## $ `2084`  <dbl> 77000000, 1540000, 69600000, 63500, 151000000, 106000, 5790000~
## $ `2085`  <dbl> 77000000, 1510000, 69700000, 63400, 154000000, 106000, 5780000~
## $ `2086`  <dbl> 77000000, 1480000, 69900000, 63300, 156000000, 106000, 5780000~
## $ `2087`  <dbl> 76900000, 1450000, 70000000, 63300, 158000000, 106000, 5770000~
## $ `2088`  <dbl> 76900000, 1420000, 70100000, 63200, 161000000, 105000, 5770000~
## $ `2089`  <dbl> 76800000, 1390000, 70200000, 63100, 163000000, 105000, 5770000~
## $ `2090`  <dbl> 76700000, 1360000, 70300000, 63100, 165000000, 105000, 5760000~
## $ `2091`  <dbl> 76600000, 1330000, 70400000, 63000, 168000000, 105000, 5750000~
## $ `2092`  <dbl> 76400000, 1300000, 70500000, 62900, 170000000, 104000, 5750000~
## $ `2093`  <dbl> 76300000, 1270000, 70500000, 62900, 172000000, 104000, 5740000~
## $ `2094`  <dbl> 76100000, 1250000, 70600000, 62800, 175000000, 104000, 5730000~
## $ `2095`  <dbl> 76000000, 1220000, 70700000, 62700, 177000000, 104000, 5720000~
## $ `2096`  <dbl> 75800000, 1190000, 70700000, 62700, 179000000, 103000, 5720000~
## $ `2097`  <dbl> 75600000, 1170000, 70700000, 62600, 182000000, 103000, 5710000~
## $ `2098`  <dbl> 75400000, 1140000, 70700000, 62500, 184000000, 103000, 5700000~
## $ `2099`  <dbl> 75200000, 1110000, 70700000, 62500, 186000000, 102000, 5690000~
## $ `2100`  <dbl> 74900000, 1090000, 70700000, 62400, 188000000, 102000, 5680000~
```

```r
pop_total
```

```
## # A tibble: 195 x 302
##    country `1800` `1801` `1802` `1803` `1804` `1805` `1806` `1807` `1808` `1809`
##    <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Afghan~ 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6
##  2 Albania 4   e5 4.02e5 4.04e5 4.05e5 4.07e5 4.09e5 4.11e5 4.13e5 4.14e5 4.16e5
##  3 Algeria 2.5 e6 2.51e6 2.52e6 2.53e6 2.54e6 2.55e6 2.56e6 2.56e6 2.57e6 2.58e6
##  4 Andorra 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3
##  5 Angola  1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6
##  6 Antigu~ 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4 3.7 e4
##  7 Argent~ 5.34e5 5.2 e5 5.06e5 4.92e5 4.79e5 4.66e5 4.53e5 4.41e5 4.29e5 4.17e5
##  8 Armenia 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5
##  9 Austra~ 2   e5 2.05e5 2.11e5 2.16e5 2.22e5 2.27e5 2.33e5 2.39e5 2.46e5 2.52e5
## 10 Austria 3   e6 3.02e6 3.04e6 3.05e6 3.07e6 3.09e6 3.11e6 3.12e6 3.14e6 3.16e6
## # ... with 185 more rows, and 291 more variables: 1810 <dbl>, 1811 <dbl>,
## #   1812 <dbl>, 1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>,
## #   1818 <dbl>, 1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>,
## #   1824 <dbl>, 1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>,
## #   1830 <dbl>, 1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>,
## #   1836 <dbl>, 1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>,
## #   1842 <dbl>, 1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>, ...
```

```r
long_pop_total <-
  pop_total%>%
  pivot_longer("1800":"2100",names_to = "years", values_to = "total_ population")
long_pop_total
```

```
## # A tibble: 58,695 x 3
##    country     years `total_ population`
##    <chr>       <chr>               <dbl>
##  1 Afghanistan 1800              3280000
##  2 Afghanistan 1801              3280000
##  3 Afghanistan 1802              3280000
##  4 Afghanistan 1803              3280000
##  5 Afghanistan 1804              3280000
##  6 Afghanistan 1805              3280000
##  7 Afghanistan 1806              3280000
##  8 Afghanistan 1807              3280000
##  9 Afghanistan 1808              3280000
## 10 Afghanistan 1809              3280000
## # ... with 58,685 more rows
```

```r
wide_pop_total<-pop_total
```



```r
glimpse(income_ppp_gpd)
```

```
## Rows: 193
## Columns: 242
## $ country <chr> "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Ant~
## $ `1800`  <dbl> 603, 667, 715, 1200, 618, 757, 1640, 514, 817, 1850, 775, 1450~
## $ `1801`  <dbl> 603, 667, 716, 1200, 620, 757, 1640, 514, 822, 1850, 775, 1450~
## $ `1802`  <dbl> 603, 667, 717, 1200, 623, 757, 1650, 514, 826, 1860, 775, 1450~
## $ `1803`  <dbl> 603, 667, 718, 1200, 626, 757, 1650, 514, 831, 1870, 775, 1450~
## $ `1804`  <dbl> 603, 667, 719, 1210, 628, 757, 1660, 514, 836, 1880, 775, 1450~
## $ `1805`  <dbl> 603, 668, 720, 1210, 631, 757, 1660, 514, 841, 1880, 776, 1450~
## $ `1806`  <dbl> 603, 668, 721, 1210, 634, 757, 1670, 514, 845, 1890, 776, 1450~
## $ `1807`  <dbl> 603, 668, 722, 1210, 637, 758, 1680, 514, 850, 1900, 776, 1450~
## $ `1808`  <dbl> 603, 668, 723, 1220, 640, 758, 1680, 514, 855, 1910, 776, 1450~
## $ `1809`  <dbl> 603, 668, 724, 1220, 642, 758, 1690, 514, 860, 1920, 776, 1450~
## $ `1810`  <dbl> 604, 668, 725, 1220, 645, 758, 1690, 514, 865, 1920, 777, 1450~
## $ `1811`  <dbl> 604, 668, 726, 1220, 648, 758, 1700, 515, 869, 1940, 777, 1450~
## $ `1812`  <dbl> 604, 668, 727, 1220, 651, 758, 1710, 515, 874, 1960, 777, 1450~
## $ `1813`  <dbl> 604, 668, 728, 1230, 654, 758, 1710, 515, 879, 1980, 777, 1450~
## $ `1814`  <dbl> 604, 668, 729, 1230, 657, 758, 1720, 515, 884, 2000, 777, 1450~
## $ `1815`  <dbl> 604, 669, 730, 1230, 660, 758, 1720, 515, 889, 2020, 778, 1450~
## $ `1816`  <dbl> 604, 669, 731, 1230, 662, 759, 1730, 515, 894, 2040, 778, 1450~
## $ `1817`  <dbl> 604, 669, 732, 1240, 665, 759, 1740, 515, 899, 2050, 778, 1450~
## $ `1818`  <dbl> 604, 669, 733, 1240, 668, 759, 1740, 515, 904, 2070, 778, 1450~
## $ `1819`  <dbl> 604, 669, 734, 1240, 671, 759, 1750, 515, 910, 2090, 778, 1450~
## $ `1820`  <dbl> 604, 669, 735, 1240, 674, 759, 1750, 515, 941, 2110, 779, 1450~
## $ `1821`  <dbl> 607, 671, 743, 1260, 677, 764, 1770, 517, 950, 2140, 783, 1460~
## $ `1822`  <dbl> 609, 672, 751, 1270, 680, 769, 1780, 519, 977, 2170, 787, 1470~
## $ `1823`  <dbl> 611, 674, 759, 1290, 683, 775, 1790, 521, 1010, 2200, 792, 148~
## $ `1824`  <dbl> 613, 675, 767, 1300, 686, 780, 1810, 523, 1070, 2230, 796, 149~
## $ `1825`  <dbl> 615, 677, 775, 1320, 689, 785, 1820, 525, 1120, 2260, 800, 150~
## $ `1826`  <dbl> 617, 678, 784, 1330, 692, 790, 1830, 527, 1140, 2300, 805, 151~
## $ `1827`  <dbl> 619, 680, 792, 1350, 695, 796, 1850, 529, 1170, 2330, 809, 152~
## $ `1828`  <dbl> 621, 681, 801, 1370, 698, 801, 1860, 531, 1200, 2360, 814, 153~
## $ `1829`  <dbl> 623, 683, 810, 1380, 701, 806, 1880, 533, 1270, 2390, 818, 154~
## $ `1830`  <dbl> 625, 684, 819, 1400, 704, 812, 1890, 535, 1540, 2430, 823, 155~
## $ `1831`  <dbl> 627, 686, 828, 1410, 708, 817, 1910, 538, 1590, 2440, 827, 156~
## $ `1832`  <dbl> 630, 688, 837, 1430, 711, 823, 1920, 540, 1630, 2460, 832, 157~
## $ `1833`  <dbl> 632, 689, 846, 1450, 714, 828, 1930, 542, 1640, 2480, 837, 158~
## $ `1834`  <dbl> 634, 691, 855, 1470, 717, 834, 1950, 544, 1720, 2500, 841, 159~
## $ `1835`  <dbl> 636, 692, 864, 1480, 720, 840, 1960, 546, 2080, 2520, 846, 160~
## $ `1836`  <dbl> 638, 694, 874, 1500, 723, 845, 1980, 548, 2050, 2540, 851, 161~
## $ `1837`  <dbl> 640, 695, 883, 1520, 726, 851, 1990, 550, 2140, 2560, 855, 163~
## $ `1838`  <dbl> 643, 697, 893, 1540, 730, 857, 2010, 552, 2120, 2590, 860, 164~
## $ `1839`  <dbl> 645, 699, 903, 1550, 733, 863, 2020, 554, 1980, 2610, 865, 165~
## $ `1840`  <dbl> 647, 700, 912, 1570, 736, 868, 2040, 556, 2500, 2630, 870, 166~
## $ `1841`  <dbl> 649, 702, 922, 1590, 739, 874, 2050, 559, 2130, 2650, 874, 167~
## $ `1842`  <dbl> 651, 703, 932, 1610, 743, 880, 2070, 561, 1940, 2670, 879, 168~
## $ `1843`  <dbl> 654, 705, 943, 1630, 746, 886, 2090, 563, 2250, 2690, 884, 169~
## $ `1844`  <dbl> 656, 707, 953, 1650, 749, 892, 2100, 565, 2560, 2720, 889, 170~
## $ `1845`  <dbl> 658, 708, 963, 1670, 752, 898, 2120, 567, 2630, 2740, 894, 172~
## $ `1846`  <dbl> 660, 710, 974, 1690, 756, 904, 2130, 569, 2910, 2770, 899, 173~
## $ `1847`  <dbl> 663, 711, 984, 1710, 759, 910, 2150, 572, 3370, 2790, 904, 174~
## $ `1848`  <dbl> 665, 713, 995, 1730, 762, 917, 2170, 574, 3850, 2810, 909, 175~
## $ `1849`  <dbl> 667, 715, 1010, 1750, 766, 923, 2180, 576, 3810, 2840, 914, 17~
## $ `1850`  <dbl> 669, 716, 1020, 1770, 769, 929, 2200, 578, 3590, 2860, 919, 17~
## $ `1851`  <dbl> 672, 718, 1030, 1790, 773, 935, 2220, 581, 4260, 2880, 924, 17~
## $ `1852`  <dbl> 674, 720, 1040, 1810, 776, 942, 2230, 583, 5000, 2900, 929, 18~
## $ `1853`  <dbl> 676, 721, 1050, 1830, 779, 948, 2250, 585, 5470, 2930, 934, 18~
## $ `1854`  <dbl> 678, 723, 1060, 1860, 783, 954, 2270, 587, 4790, 2950, 939, 18~
## $ `1855`  <dbl> 681, 724, 1070, 1880, 786, 961, 2290, 590, 4550, 2970, 945, 18~
## $ `1856`  <dbl> 683, 726, 1090, 1900, 790, 967, 2310, 592, 5550, 2990, 950, 18~
## $ `1857`  <dbl> 685, 728, 1100, 1920, 793, 974, 2320, 594, 4890, 3010, 955, 18~
## $ `1858`  <dbl> 688, 729, 1110, 1950, 797, 980, 2340, 596, 4170, 3040, 960, 18~
## $ `1859`  <dbl> 690, 731, 1120, 1970, 800, 987, 2360, 599, 5390, 3060, 966, 18~
## $ `1860`  <dbl> 692, 733, 1130, 1990, 804, 994, 2380, 601, 5260, 3080, 971, 19~
## $ `1861`  <dbl> 695, 738, 1150, 2020, 807, 1000, 2400, 603, 5180, 3100, 976, 1~
## $ `1862`  <dbl> 697, 745, 1160, 2040, 811, 1010, 2420, 606, 4980, 3110, 982, 1~
## $ `1863`  <dbl> 699, 751, 1170, 2060, 814, 1010, 2440, 608, 4960, 3130, 987, 1~
## $ `1864`  <dbl> 702, 757, 1180, 2090, 818, 1020, 2460, 610, 5260, 3140, 993, 1~
## $ `1865`  <dbl> 704, 763, 1200, 2110, 822, 1030, 2480, 613, 5020, 3160, 998, 1~
## $ `1866`  <dbl> 707, 770, 1210, 2140, 825, 1040, 2500, 615, 5170, 3170, 1000, ~
## $ `1867`  <dbl> 709, 776, 1220, 2160, 829, 1040, 2520, 617, 5650, 3180, 1010, ~
## $ `1868`  <dbl> 711, 782, 1240, 2190, 832, 1050, 2540, 620, 5740, 3200, 1020, ~
## $ `1869`  <dbl> 714, 788, 1250, 2220, 836, 1060, 2560, 622, 5620, 3210, 1020, ~
## $ `1870`  <dbl> 716, 795, 1260, 2240, 840, 1060, 2580, 625, 5950, 3230, 1030, ~
## $ `1871`  <dbl> 719, 807, 1280, 2270, 844, 1070, 2630, 627, 5990, 3430, 1030, ~
## $ `1872`  <dbl> 721, 819, 1290, 2300, 847, 1080, 2690, 629, 6460, 3430, 1040, ~
## $ `1873`  <dbl> 724, 831, 1310, 2320, 851, 1090, 2750, 632, 6950, 3320, 1040, ~
## $ `1874`  <dbl> 726, 843, 1320, 2350, 855, 1090, 2810, 634, 6970, 3440, 1050, ~
## $ `1875`  <dbl> 729, 855, 1330, 2380, 858, 1100, 2870, 637, 7520, 3420, 1060, ~
## $ `1876`  <dbl> 731, 868, 1350, 2410, 862, 1110, 2880, 639, 7280, 3470, 1060, ~
## $ `1877`  <dbl> 734, 881, 1360, 2440, 866, 1110, 3110, 642, 7330, 3560, 1070, ~
## $ `1878`  <dbl> 736, 894, 1380, 2460, 870, 1120, 2890, 644, 7770, 3640, 1070, ~
## $ `1879`  <dbl> 739, 907, 1390, 2490, 874, 1130, 2940, 647, 7640, 3590, 1080, ~
## $ `1880`  <dbl> 741, 920, 1410, 2520, 878, 1140, 2820, 649, 7790, 3600, 1080, ~
## $ `1881`  <dbl> 758, 934, 1420, 2550, 881, 1150, 2800, 652, 8090, 3720, 1090, ~
## $ `1882`  <dbl> 774, 947, 1440, 2580, 885, 1150, 3430, 654, 7380, 3710, 1100, ~
## $ `1883`  <dbl> 791, 961, 1450, 2610, 889, 1160, 3720, 657, 8130, 3830, 1100, ~
## $ `1884`  <dbl> 807, 976, 1470, 2650, 893, 1170, 3840, 659, 7820, 3900, 1110, ~
## $ `1885`  <dbl> 824, 990, 1490, 2680, 897, 1180, 4300, 662, 8040, 3840, 1120, ~
## $ `1886`  <dbl> 840, 1010, 1500, 2710, 901, 1180, 4160, 635, 7870, 3930, 1070,~
## $ `1887`  <dbl> 858, 1020, 1520, 2740, 905, 1190, 4230, 741, 8420, 4170, 1250,~
## $ `1888`  <dbl> 874, 1030, 1540, 2770, 909, 1200, 4650, 711, 8180, 4130, 1200,~
## $ `1889`  <dbl> 892, 1050, 1550, 2810, 913, 1210, 4730, 660, 8620, 4050, 1120,~
## $ `1890`  <dbl> 909, 1070, 1570, 2840, 917, 1220, 4250, 656, 8100, 4240, 1110,~
## $ `1891`  <dbl> 926, 1080, 1590, 2870, 921, 1220, 4000, 599, 8480, 4350, 1020,~
## $ `1892`  <dbl> 944, 1100, 1600, 2910, 925, 1230, 4680, 655, 7260, 4400, 1110,~
## $ `1893`  <dbl> 961, 1110, 1620, 2940, 929, 1240, 4860, 740, 6740, 4380, 1260,~
## $ `1894`  <dbl> 979, 1130, 1640, 2980, 933, 1250, 5480, 840, 6840, 4590, 1430,~
## $ `1895`  <dbl> 997, 1140, 1660, 3010, 937, 1260, 5930, 776, 6340, 4660, 1320,~
## $ `1896`  <dbl> 1010, 1160, 1680, 3050, 941, 1270, 6300, 853, 6700, 4680, 1460~
## $ `1897`  <dbl> 1030, 1170, 1690, 3080, 946, 1280, 4960, 837, 6210, 4730, 1430~
## $ `1898`  <dbl> 1050, 1190, 1710, 3120, 950, 1280, 5220, 857, 7070, 4950, 1470~
## $ `1899`  <dbl> 1070, 1200, 1730, 3160, 954, 1290, 5960, 908, 6980, 5000, 1560~
## $ `1900`  <dbl> 1090, 1220, 1750, 3200, 958, 1300, 5050, 886, 7290, 5000, 1520~
## $ `1901`  <dbl> 1110, 1240, 1770, 3230, 962, 1310, 5060, 906, 6970, 4970, 1560~
## $ `1902`  <dbl> 1120, 1250, 1790, 3270, 967, 1320, 4770, 983, 6950, 5110, 1690~
## $ `1903`  <dbl> 1140, 1270, 1810, 3310, 971, 1330, 5260, 913, 7410, 5100, 1580~
## $ `1904`  <dbl> 1160, 1290, 1830, 3350, 975, 1340, 5610, 1010, 7800, 5130, 174~
## $ `1905`  <dbl> 1180, 1300, 1850, 3390, 979, 1350, 6110, 890, 7780, 5360, 1540~
## $ `1906`  <dbl> 1200, 1320, 1870, 3430, 984, 1360, 6180, 849, 8190, 5510, 1470~
## $ `1907`  <dbl> 1220, 1340, 1890, 3470, 988, 1360, 6080, 814, 8390, 5790, 1410~
## $ `1908`  <dbl> 1240, 1350, 1910, 3510, 992, 1370, 6420, 887, 8530, 5760, 1540~
## $ `1909`  <dbl> 1260, 1370, 1930, 3550, 997, 1380, 6500, 919, 9050, 5680, 1600~
## $ `1910`  <dbl> 1280, 1390, 1950, 3600, 1000, 1390, 6710, 979, 9470, 5710, 170~
## $ `1911`  <dbl> 1300, 1410, 2000, 3640, 1010, 1400, 6580, 902, 9270, 5840, 157~
## $ `1912`  <dbl> 1320, 1430, 2050, 3680, 1010, 1410, 6860, 973, 9260, 6080, 170~
## $ `1913`  <dbl> 1340, 1450, 2100, 3730, 1010, 1420, 6670, 1020, 9370, 6010, 17~
## $ `1914`  <dbl> 1360, 1460, 2130, 3790, 1050, 1460, 5800, 972, 9130, 4990, 170~
## $ `1915`  <dbl> 1380, 1470, 2170, 3850, 1080, 1490, 5700, 998, 8900, 4600, 175~
## $ `1916`  <dbl> 1400, 1480, 2210, 3920, 1110, 1530, 5430, 967, 8860, 4560, 155~
## $ `1917`  <dbl> 1430, 1490, 2240, 3980, 1140, 1570, 4900, 806, 8710, 4480, 136~
## $ `1918`  <dbl> 1450, 1510, 2280, 4050, 1170, 1600, 5710, 671, 8360, 4430, 834~
## $ `1919`  <dbl> 1470, 1520, 2320, 4120, 1210, 1640, 5810, 560, 8570, 3920, 718~
## $ `1920`  <dbl> 1490, 1530, 2360, 4190, 1250, 1680, 6100, 531, 8660, 4180, 721~
## $ `1921`  <dbl> 1520, 1550, 2420, 4260, 1290, 1730, 6100, 504, 8920, 4600, 660~
## $ `1922`  <dbl> 1550, 1560, 2480, 4330, 1330, 1770, 6390, 547, 9200, 4990, 767~
## $ `1923`  <dbl> 1570, 1570, 2540, 4410, 1370, 1810, 6850, 593, 9430, 4930, 884~
## $ `1924`  <dbl> 1600, 1580, 2600, 4480, 1410, 1860, 7120, 642, 9840, 5480, 112~
## $ `1925`  <dbl> 1630, 1600, 2660, 4560, 1460, 1900, 6890, 696, 10100, 5840, 14~
## $ `1926`  <dbl> 1650, 1610, 2720, 4640, 1500, 1950, 7020, 755, 10100, 5920, 15~
## $ `1927`  <dbl> 1680, 1620, 2780, 4720, 1550, 2000, 7300, 819, 10100, 6080, 16~
## $ `1928`  <dbl> 1710, 1640, 2830, 4800, 1600, 2040, 7540, 887, 9910, 6340, 173~
## $ `1929`  <dbl> 1740, 1650, 2890, 4880, 1650, 2090, 7670, 962, 9560, 6410, 175~
## $ `1930`  <dbl> 1770, 1660, 2950, 4970, 1700, 2150, 7170, 1000, 8550, 6220, 18~
## $ `1931`  <dbl> 1790, 1660, 3030, 5050, 1760, 2200, 6520, 1010, 7910, 5700, 18~
## $ `1932`  <dbl> 1820, 1670, 3110, 5140, 1810, 2250, 6190, 991, 8290, 5100, 182~
## $ `1933`  <dbl> 1850, 1680, 3190, 5230, 1870, 2310, 6360, 1030, 8800, 4910, 18~
## $ `1934`  <dbl> 1880, 1680, 3280, 5320, 1930, 2360, 6750, 1120, 9190, 4940, 20~
## $ `1935`  <dbl> 1910, 1690, 3360, 5410, 1990, 2420, 6940, 1270, 9660, 5040, 23~
## $ `1936`  <dbl> 1940, 1690, 3440, 5500, 2060, 2480, 6870, 1360, 10000, 5190, 2~
## $ `1937`  <dbl> 1970, 1700, 3530, 5600, 2120, 2540, 7250, 1470, 10400, 5470, 2~
## $ `1938`  <dbl> 2000, 1710, 3610, 5690, 2190, 2600, 7150, 1460, 10700, 6170, 2~
## $ `1939`  <dbl> 2030, 1710, 3690, 5790, 2260, 2670, 7290, 1510, 10600, 7100, 2~
## $ `1940`  <dbl> 2060, 1720, 3780, 5890, 2330, 2730, 7310, 1450, 11200, 6870, 2~
## $ `1941`  <dbl> 2090, 1730, 3870, 5990, 2400, 2800, 7560, 1410, 12300, 7310, 2~
## $ `1942`  <dbl> 2130, 1730, 3950, 6100, 2480, 2870, 7530, 1380, 13600, 6910, 2~
## $ `1943`  <dbl> 2160, 1740, 4040, 6200, 2560, 2940, 7350, 1350, 14000, 7050, 2~
## $ `1944`  <dbl> 2190, 1750, 4120, 6310, 2640, 3010, 8040, 1340, 13400, 7200, 2~
## $ `1945`  <dbl> 2220, 1750, 4210, 6410, 2720, 3080, 7650, 1330, 12600, 2990, 2~
## $ `1946`  <dbl> 2260, 1760, 4300, 6520, 2810, 3160, 8200, 1310, 12000, 3390, 2~
## $ `1947`  <dbl> 2290, 1760, 4380, 6640, 2900, 3230, 8940, 1470, 12100, 3760, 2~
## $ `1948`  <dbl> 2320, 1770, 4470, 6750, 2990, 3310, 9230, 1670, 12700, 4790, 3~
## $ `1949`  <dbl> 2360, 1780, 4560, 6870, 3080, 3390, 8870, 1840, 13100, 5710, 3~
## $ `1950`  <dbl> 2390, 1780, 4640, 6980, 3180, 3470, 8760, 2010, 13500, 6430, 3~
## $ `1951`  <dbl> 2420, 1860, 4650, 7550, 3260, 3560, 8910, 2000, 13600, 6870, 3~
## $ `1952`  <dbl> 2460, 1860, 4810, 8160, 3340, 3650, 8290, 2110, 13500, 6880, 3~
## $ `1953`  <dbl> 2570, 1940, 4860, 8830, 3420, 3730, 8560, 2180, 13600, 7170, 4~
## $ `1954`  <dbl> 2580, 2000, 5170, 9540, 3290, 3830, 8750, 2270, 14200, 7900, 4~
## $ `1955`  <dbl> 2580, 2100, 5260, 10300, 3510, 3920, 9200, 2440, 14600, 8760, ~
## $ `1956`  <dbl> 2640, 2130, 5730, 11200, 3400, 4010, 9280, 2640, 14700, 9360, ~
## $ `1957`  <dbl> 2590, 2260, 6320, 12100, 3670, 4110, 9590, 2670, 14700, 9910, ~
## $ `1958`  <dbl> 2680, 2360, 6500, 13000, 3810, 4210, 10000, 2840, 15100, 10200~
## $ `1959`  <dbl> 2710, 2460, 7620, 14100, 3770, 4310, 9210, 2780, 15700, 10500,~
## $ `1960`  <dbl> 2740, 2590, 8080, 15200, 3860, 4420, 9770, 3000, 16000, 11300,~
## $ `1961`  <dbl> 2700, 2610, 7780, 16500, 4310, 4570, 10300, 3120, 15700, 11900~
## $ `1962`  <dbl> 2680, 2690, 4910, 17800, 4130, 4730, 9980, 3150, 16300, 12100,~
## $ `1963`  <dbl> 2670, 2790, 6030, 19200, 4280, 4890, 9590, 3030, 16900, 12500,~
## $ `1964`  <dbl> 2650, 2880, 6460, 20800, 4700, 5060, 10400, 3380, 17700, 13200~
## $ `1965`  <dbl> 2640, 2990, 6850, 22500, 4970, 5230, 11200, 3520, 18200, 13600~
## $ `1966`  <dbl> 2600, 3100, 6210, 24300, 5190, 5410, 11100, 3650, 18300, 14300~
## $ `1967`  <dbl> 2600, 3220, 6700, 26300, 5410, 5590, 11300, 3770, 19100, 14600~
## $ `1968`  <dbl> 2620, 3330, 7200, 28400, 5250, 5780, 11600, 3950, 19800, 15200~
## $ `1969`  <dbl> 2590, 3440, 7570, 30700, 5320, 5970, 12400, 3960, 20500, 16200~
## $ `1970`  <dbl> 2570, 3570, 8070, 33200, 5570, 6170, 12900, 4220, 21300, 17300~
## $ `1971`  <dbl> 2500, 3710, 7170, 33000, 5790, 6550, 13300, 4280, 21700, 18100~
## $ `1972`  <dbl> 2030, 3860, 8410, 34000, 5690, 6960, 13400, 4260, 21800, 19200~
## $ `1973`  <dbl> 2030, 4050, 8430, 34900, 6010, 7490, 14000, 4560, 22600, 20100~
## $ `1974`  <dbl> 2080, 4070, 8670, 35300, 6060, 7680, 14700, 4480, 22700, 20900~
## $ `1975`  <dbl> 2150, 4080, 8990, 34100, 5620, 7210, 14300, 4400, 23000, 20900~
## $ `1976`  <dbl> 2210, 4100, 9290, 34000, 5010, 6530, 14000, 4330, 23600, 21900~
## $ `1977`  <dbl> 2040, 4120, 9810, 34000, 4960, 6970, 14600, 4250, 23500, 23000~
## $ `1978`  <dbl> 2130, 4140, 10700, 33500, 5070, 7200, 13800, 4170, 23800, 2300~
## $ `1979`  <dbl> 2030, 4160, 11300, 32500, 5080, 7980, 14500, 4100, 24700, 2440~
## $ `1980`  <dbl> 2020, 4190, 11200, 32100, 5110, 8560, 14500, 4020, 24800, 2500~
## $ `1981`  <dbl> 2260, 4210, 11100, 30800, 4930, 9060, 13400, 4090, 25100, 2500~
## $ `1982`  <dbl> 2500, 4230, 11400, 29900, 4610, 9200, 12800, 4160, 24600, 2540~
## $ `1983`  <dbl> 2650, 4260, 11600, 29100, 4340, 9980, 13000, 4140, 24300, 2630~
## $ `1984`  <dbl> 2620, 4280, 11800, 28400, 4260, 10900, 13100, 4220, 25600, 264~
## $ `1985`  <dbl> 2550, 4300, 12100, 27800, 4210, 11900, 12100, 4300, 26500, 271~
## $ `1986`  <dbl> 2620, 4330, 11600, 27600, 3760, 13200, 12800, 4210, 26600, 277~
## $ `1987`  <dbl> 2360, 4360, 11200, 27900, 4130, 14600, 12900, 3920, 27400, 282~
## $ `1988`  <dbl> 2140, 4390, 10700, 28200, 4650, 15900, 12500, 3690, 28100, 291~
## $ `1989`  <dbl> 1940, 4420, 10700, 28400, 4670, 16900, 11500, 4120, 28800, 303~
## $ `1990`  <dbl> 1860, 4460, 10300, 28400, 4760, 17500, 11400, 3740, 28700, 313~
## $ `1991`  <dbl> 1650, 3230, 9930, 28000, 4650, 17600, 12200, 3340, 28200, 3210~
## $ `1992`  <dbl> 1520, 3020, 9870, 27200, 4240, 17500, 13000, 1980, 28000, 3240~
## $ `1993`  <dbl> 1010, 3320, 9450, 26000, 3120, 18100, 13900, 1850, 28800, 3230~
## $ `1994`  <dbl> 721, 3620, 9170, 25900, 3060, 18900, 14500, 1990, 29600, 33000~
## $ `1995`  <dbl> 1030, 4130, 9340, 26100, 3410, 17700, 14000, 2170, 30400, 3380~
## $ `1996`  <dbl> 942, 4530, 9560, 27200, 3750, 18400, 14600, 2340, 31200, 34500~
## $ `1997`  <dbl> 865, 4060, 9510, 29700, 3890, 19000, 15600, 2440, 32100, 35200~
## $ `1998`  <dbl> 800, 4450, 9840, 30800, 3950, 19500, 16000, 2640, 33200, 36400~
## $ `1999`  <dbl> 735, 5060, 10000, 31900, 3900, 19800, 15300, 2740, 34500, 3770~
## $ `2000`  <dbl> 687, 5440, 10300, 31700, 3890, 20800, 15000, 2930, 35400, 3880~
## $ `2001`  <dbl> 646, 5950, 10400, 31800, 3920, 19500, 14200, 3220, 35600, 3920~
## $ `2002`  <dbl> 1020, 6240, 10900, 31900, 4320, 19400, 12500, 3670, 36600, 396~
## $ `2003`  <dbl> 1060, 6610, 11500, 34500, 4300, 20300, 13400, 4210, 37200, 398~
## $ `2004`  <dbl> 1030, 7000, 11800, 36300, 4610, 21200, 14500, 4670, 38300, 407~
## $ `2005`  <dbl> 1100, 7430, 12400, 39800, 5110, 22200, 15600, 5360, 39000, 413~
## $ `2006`  <dbl> 1120, 7910, 12400, 42700, 5500, 24700, 16700, 6110, 39500, 425~
## $ `2007`  <dbl> 1250, 8450, 12600, 43400, 6040, 26500, 18000, 7010, 40700, 439~
## $ `2008`  <dbl> 1270, 9160, 12700, 41400, 6470, 26100, 18600, 7560, 41400, 444~
## $ `2009`  <dbl> 1500, 9530, 12700, 41700, 6290, 22600, 17300, 6530, 41300, 427~
## $ `2010`  <dbl> 1670, 9930, 12900, 39000, 6360, 20700, 18900, 6700, 41500, 433~
## $ `2011`  <dbl> 1630, 10200, 13000, 42000, 6350, 20000, 19800, 7020, 42000, 44~
## $ `2012`  <dbl> 1770, 10400, 13200, 41900, 6650, 20400, 19400, 7510, 42800, 44~
## $ `2013`  <dbl> 1810, 10500, 13300, 43700, 6730, 20100, 19600, 7720, 43200, 44~
## $ `2014`  <dbl> 1800, 10700, 13500, 44900, 6810, 20800, 18900, 7950, 43700, 44~
## $ `2015`  <dbl> 1770, 11000, 13800, 46600, 6650, 21400, 19200, 8170, 44100, 44~
## $ `2016`  <dbl> 1760, 11400, 13900, 48200, 6260, 22400, 18600, 8160, 44600, 44~
## $ `2017`  <dbl> 1760, 11800, 13900, 49800, 6050, 22900, 18900, 8750, 44900, 45~
## $ `2018`  <dbl> 1740, 12300, 13900, 51500, 5730, 23800, 18300, 9180, 45400, 46~
## $ `2019`  <dbl> 1760, 12700, 14000, 53200, 5540, 24500, 17500, 9730, 45500, 46~
## $ `2020`  <dbl> 1800, 13200, 14000, 55000, 5440, 25000, 17100, 10200, 45800, 4~
## $ `2021`  <dbl> 1850, 13800, 14000, 56900, 5440, 25400, 17200, 10700, 46200, 4~
## $ `2022`  <dbl> 1900, 14400, 14000, 58700, 5460, 25600, 17400, 11100, 46800, 4~
## $ `2023`  <dbl> 1970, 15000, 13900, 60400, 5520, 25800, 17700, 11600, 47200, 4~
## $ `2024`  <dbl> 2050, 15600, 13800, 62100, 5560, 26100, 18100, 12200, 47700, 4~
## $ `2025`  <dbl> 2140, 16200, 13700, 63900, 5600, 26300, 18500, 12700, 48300, 4~
## $ `2026`  <dbl> 2220, 16800, 13700, 65600, 5660, 26600, 18800, 13200, 48800, 5~
## $ `2027`  <dbl> 2290, 17400, 13700, 67300, 5720, 27000, 19200, 13700, 49500, 5~
## $ `2028`  <dbl> 2360, 18000, 13800, 68900, 5800, 27300, 19600, 14100, 50300, 5~
## $ `2029`  <dbl> 2430, 18500, 13900, 70500, 5890, 27800, 20000, 14500, 51100, 5~
## $ `2030`  <dbl> 2490, 18900, 14100, 72100, 6000, 28300, 20400, 14900, 52000, 5~
## $ `2031`  <dbl> 2550, 19400, 14300, 73600, 6110, 28800, 20800, 15300, 53000, 5~
## $ `2032`  <dbl> 2600, 19800, 14600, 75100, 6230, 29400, 21300, 15600, 54000, 5~
## $ `2033`  <dbl> 2660, 20200, 14900, 76700, 6350, 30000, 21700, 15900, 55100, 5~
## $ `2034`  <dbl> 2710, 20600, 15200, 78300, 6480, 30600, 22100, 16300, 56200, 5~
## $ `2035`  <dbl> 2770, 21000, 15500, 79900, 6610, 31200, 22600, 16600, 57400, 5~
## $ `2036`  <dbl> 2820, 21500, 15800, 81500, 6750, 31800, 23100, 16900, 58600, 6~
## $ `2037`  <dbl> 2880, 21900, 16100, 83100, 6880, 32500, 23500, 17300, 59700, 6~
## $ `2038`  <dbl> 2940, 22300, 16500, 84800, 7020, 33100, 24000, 17600, 61000, 6~
## $ `2039`  <dbl> 3000, 22800, 16800, 86500, 7170, 33800, 24500, 18000, 62200, 6~
## $ `2040`  <dbl> 3060, 23300, 17100, 88300, 7310, 34500, 25000, 18400, 63500, 6~
```

```r
summary(income_ppp_gpd)
```

```
##    country               1800             1801             1802       
##  Length:193         Min.   : 250.0   Min.   : 250.0   Min.   : 249.0  
##  Class :character   1st Qu.: 592.0   1st Qu.: 592.0   1st Qu.: 592.0  
##  Mode  :character   Median : 817.0   Median : 822.0   Median : 826.0  
##                     Mean   : 978.5   Mean   : 978.9   Mean   : 980.7  
##                     3rd Qu.:1160.0   3rd Qu.:1170.0   3rd Qu.:1170.0  
##                     Max.   :3840.0   Max.   :3840.0   Max.   :3840.0  
##       1803             1804             1805             1806       
##  Min.   : 249.0   Min.   : 249.0   Min.   : 249.0   Min.   : 248.0  
##  1st Qu.: 592.0   1st Qu.: 592.0   1st Qu.: 593.0   1st Qu.: 593.0  
##  Median : 831.0   Median : 836.0   Median : 836.0   Median : 836.0  
##  Mean   : 980.9   Mean   : 981.9   Mean   : 982.5   Mean   : 982.8  
##  3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0  
##  Max.   :3840.0   Max.   :3840.0   Max.   :3840.0   Max.   :3840.0  
##       1807             1808             1809             1810       
##  Min.   : 248.0   Min.   : 248.0   Min.   : 248.0   Min.   : 248.0  
##  1st Qu.: 593.0   1st Qu.: 593.0   1st Qu.: 593.0   1st Qu.: 593.0  
##  Median : 836.0   Median : 836.0   Median : 836.0   Median : 836.0  
##  Mean   : 985.4   Mean   : 980.9   Mean   : 982.4   Mean   : 983.7  
##  3rd Qu.:1170.0   3rd Qu.:1160.0   3rd Qu.:1170.0   3rd Qu.:1170.0  
##  Max.   :3840.0   Max.   :3840.0   Max.   :3840.0   Max.   :3840.0  
##       1811             1812             1813             1814       
##  Min.   : 247.0   Min.   : 247.0   Min.   : 247.0   Min.   : 246.0  
##  1st Qu.: 593.0   1st Qu.: 594.0   1st Qu.: 594.0   1st Qu.: 594.0  
##  Median : 836.0   Median : 830.0   Median : 833.0   Median : 837.0  
##  Mean   : 981.6   Mean   : 981.4   Mean   : 987.1   Mean   : 989.2  
##  3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0  
##  Max.   :3840.0   Max.   :3840.0   Max.   :3840.0   Max.   :3840.0  
##       1815             1816             1817             1818       
##  Min.   : 246.0   Min.   : 246.0   Min.   : 246.0   Min.   : 245.0  
##  1st Qu.: 594.0   1st Qu.: 594.0   1st Qu.: 594.0   1st Qu.: 594.0  
##  Median : 840.0   Median : 840.0   Median : 840.0   Median : 840.0  
##  Mean   : 989.5   Mean   : 988.9   Mean   : 990.7   Mean   : 994.7  
##  3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0   3rd Qu.:1170.0  
##  Max.   :3840.0   Max.   :3850.0   Max.   :3850.0   Max.   :3850.0  
##       1819             1820             1821           1822           1823     
##  Min.   : 245.0   Min.   : 245.0   Min.   : 245   Min.   : 245   Min.   : 245  
##  1st Qu.: 594.0   1st Qu.: 594.0   1st Qu.: 595   1st Qu.: 596   1st Qu.: 597  
##  Median : 840.0   Median : 841.0   Median : 844   Median : 841   Median : 838  
##  Mean   : 993.7   Mean   : 997.6   Mean   :1005   Mean   :1010   Mean   :1017  
##  3rd Qu.:1180.0   3rd Qu.:1180.0   3rd Qu.:1190   3rd Qu.:1200   3rd Qu.:1210  
##  Max.   :3850.0   Max.   :3850.0   Max.   :3850   Max.   :3860   Max.   :3870  
##       1824           1825           1826           1827           1828     
##  Min.   : 245   Min.   : 245   Min.   : 245   Min.   : 245   Min.   : 245  
##  1st Qu.: 598   1st Qu.: 600   1st Qu.: 604   1st Qu.: 606   1st Qu.: 610  
##  Median : 836   Median : 834   Median : 832   Median : 841   Median : 844  
##  Mean   :1024   Mean   :1030   Mean   :1035   Mean   :1042   Mean   :1049  
##  3rd Qu.:1210   3rd Qu.:1210   3rd Qu.:1220   3rd Qu.:1220   3rd Qu.:1230  
##  Max.   :3870   Max.   :3880   Max.   :3890   Max.   :3890   Max.   :3900  
##       1829           1830           1831           1832           1833     
##  Min.   : 245   Min.   : 245   Min.   : 245   Min.   : 245   Min.   : 246  
##  1st Qu.: 617   1st Qu.: 621   1st Qu.: 621   1st Qu.: 621   1st Qu.: 621  
##  Median : 851   Median : 859   Median : 867   Median : 875   Median : 876  
##  Mean   :1054   Mean   :1060   Mean   :1066   Mean   :1075   Mean   :1082  
##  3rd Qu.:1250   3rd Qu.:1250   3rd Qu.:1260   3rd Qu.:1260   3rd Qu.:1270  
##  Max.   :3910   Max.   :3920   Max.   :3920   Max.   :3930   Max.   :3940  
##       1834           1835           1836           1837           1838     
##  Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246  
##  1st Qu.: 621   1st Qu.: 621   1st Qu.: 621   1st Qu.: 624   1st Qu.: 627  
##  Median : 876   Median : 876   Median : 876   Median : 884   Median : 893  
##  Mean   :1089   Mean   :1100   Mean   :1103   Mean   :1109   Mean   :1115  
##  3rd Qu.:1280   3rd Qu.:1290   3rd Qu.:1290   3rd Qu.:1300   3rd Qu.:1310  
##  Max.   :3940   Max.   :3950   Max.   :3960   Max.   :4010   Max.   :4050  
##       1839           1840           1841           1842           1843     
##  Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246  
##  1st Qu.: 630   1st Qu.: 633   1st Qu.: 636   1st Qu.: 639   1st Qu.: 642  
##  Median : 892   Median : 900   Median : 912   Median : 914   Median : 910  
##  Mean   :1117   Mean   :1126   Mean   :1131   Mean   :1135   Mean   :1140  
##  3rd Qu.:1330   3rd Qu.:1340   3rd Qu.:1350   3rd Qu.:1360   3rd Qu.:1360  
##  Max.   :4130   Max.   :4220   Max.   :4310   Max.   :4410   Max.   :4500  
##       1844           1845           1846           1847           1848     
##  Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246  
##  1st Qu.: 645   1st Qu.: 648   1st Qu.: 652   1st Qu.: 656   1st Qu.: 658  
##  Median : 917   Median : 920   Median : 922   Median : 928   Median : 929  
##  Mean   :1151   Mean   :1158   Mean   :1164   Mean   :1173   Mean   :1183  
##  3rd Qu.:1370   3rd Qu.:1380   3rd Qu.:1390   3rd Qu.:1400   3rd Qu.:1440  
##  Max.   :4600   Max.   :4690   Max.   :4790   Max.   :4890   Max.   :5000  
##       1849           1850           1851           1852           1853     
##  Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246  
##  1st Qu.: 661   1st Qu.: 664   1st Qu.: 669   1st Qu.: 672   1st Qu.: 675  
##  Median : 931   Median : 933   Median : 939   Median : 946   Median : 952  
##  Mean   :1191   Mean   :1202   Mean   :1220   Mean   :1236   Mean   :1246  
##  3rd Qu.:1440   3rd Qu.:1450   3rd Qu.:1460   3rd Qu.:1470   3rd Qu.:1480  
##  Max.   :5100   Max.   :5210   Max.   :5350   Max.   :5540   Max.   :5470  
##       1854           1855           1856           1857           1858     
##  Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 246   Min.   : 247  
##  1st Qu.: 678   1st Qu.: 680   1st Qu.: 683   1st Qu.: 685   1st Qu.: 688  
##  Median : 958   Median : 964   Median : 970   Median : 977   Median : 983  
##  Mean   :1249   Mean   :1262   Mean   :1270   Mean   :1284   Mean   :1297  
##  3rd Qu.:1480   3rd Qu.:1490   3rd Qu.:1500   3rd Qu.:1520   3rd Qu.:1520  
##  Max.   :4810   Max.   :5030   Max.   :5550   Max.   :5330   Max.   :7280  
##       1859           1860           1861           1862           1863     
##  Min.   : 247   Min.   : 247   Min.   : 247   Min.   : 247   Min.   : 247  
##  1st Qu.: 690   1st Qu.: 692   1st Qu.: 695   1st Qu.: 697   1st Qu.: 707  
##  Median : 989   Median :1000   Median :1000   Median :1010   Median :1020  
##  Mean   :1307   Mean   :1314   Mean   :1319   Mean   :1326   Mean   :1338  
##  3rd Qu.:1550   3rd Qu.:1540   3rd Qu.:1550   3rd Qu.:1550   3rd Qu.:1560  
##  Max.   :6610   Max.   :5460   Max.   :5860   Max.   :6320   Max.   :6030  
##       1864           1865           1866           1867           1868     
##  Min.   : 247   Min.   : 247   Min.   : 247   Min.   : 247   Min.   : 247  
##  1st Qu.: 710   1st Qu.: 704   1st Qu.: 707   1st Qu.: 709   1st Qu.: 718  
##  Median :1020   Median :1030   Median :1040   Median :1040   Median :1040  
##  Mean   :1349   Mean   :1357   Mean   :1366   Mean   :1370   Mean   :1388  
##  3rd Qu.:1580   3rd Qu.:1580   3rd Qu.:1590   3rd Qu.:1630   3rd Qu.:1640  
##  Max.   :5510   Max.   :6150   Max.   :6000   Max.   :5650   Max.   :6390  
##       1869           1870           1871           1872           1873     
##  Min.   : 247   Min.   : 247   Min.   : 249   Min.   : 251   Min.   : 253  
##  1st Qu.: 714   1st Qu.: 710   1st Qu.: 719   1st Qu.: 721   1st Qu.: 724  
##  Median :1050   Median :1060   Median :1070   Median :1080   Median :1080  
##  Mean   :1406   Mean   :1419   Mean   :1430   Mean   :1454   Mean   :1474  
##  3rd Qu.:1670   3rd Qu.:1690   3rd Qu.:1690   3rd Qu.:1700   3rd Qu.:1740  
##  Max.   :7030   Max.   :6710   Max.   :6840   Max.   :6460   Max.   :6950  
##       1874           1875           1876           1877           1878     
##  Min.   : 255   Min.   : 258   Min.   : 260   Min.   : 262   Min.   : 264  
##  1st Qu.: 726   1st Qu.: 729   1st Qu.: 732   1st Qu.: 741   1st Qu.: 741  
##  Median :1090   Median :1100   Median :1100   Median :1110   Median :1100  
##  Mean   :1488   Mean   :1502   Mean   :1508   Mean   :1516   Mean   :1530  
##  3rd Qu.:1750   3rd Qu.:1750   3rd Qu.:1770   3rd Qu.:1780   3rd Qu.:1800  
##  Max.   :7410   Max.   :8380   Max.   :7940   Max.   :7330   Max.   :7770  
##       1879           1880           1881           1882           1883     
##  Min.   : 266   Min.   : 269   Min.   : 271   Min.   : 273   Min.   : 276  
##  1st Qu.: 740   1st Qu.: 742   1st Qu.: 750   1st Qu.: 750   1st Qu.: 752  
##  Median :1120   Median :1130   Median :1130   Median :1140   Median :1130  
##  Mean   :1533   Mean   :1560   Mean   :1573   Mean   :1587   Mean   :1606  
##  3rd Qu.:1830   3rd Qu.:1850   3rd Qu.:1850   3rd Qu.:1870   3rd Qu.:1880  
##  Max.   :7690   Max.   :8000   Max.   :8350   Max.   :8000   Max.   :8260  
##       1884           1885            1886            1887            1888      
##  Min.   : 278   Min.   :  280   Min.   :  283   Min.   :  285   Min.   :  288  
##  1st Qu.: 756   1st Qu.:  759   1st Qu.:  760   1st Qu.:  767   1st Qu.:  771  
##  Median :1150   Median : 1160   Median : 1140   Median : 1190   Median : 1190  
##  Mean   :1623   Mean   : 1638   Mean   : 1650   Mean   : 1680   Mean   : 1698  
##  3rd Qu.:1880   3rd Qu.: 1890   3rd Qu.: 1910   3rd Qu.: 1990   3rd Qu.: 2000  
##  Max.   :9370   Max.   :10400   Max.   :10800   Max.   :10600   Max.   :11000  
##       1889            1890            1891            1892      
##  Min.   :  290   Min.   :  292   Min.   :  295   Min.   :  297  
##  1st Qu.:  775   1st Qu.:  778   1st Qu.:  782   1st Qu.:  796  
##  Median : 1160   Median : 1140   Median : 1140   Median : 1180  
##  Mean   : 1704   Mean   : 1716   Mean   : 1714   Mean   : 1736  
##  3rd Qu.: 1990   3rd Qu.: 2010   3rd Qu.: 2050   3rd Qu.: 2070  
##  Max.   :10600   Max.   :11300   Max.   :10600   Max.   :11200  
##       1893            1894            1895            1896      
##  Min.   :  300   Min.   :  302   Min.   :  305   Min.   :  308  
##  1st Qu.:  815   1st Qu.:  828   1st Qu.:  825   1st Qu.:  836  
##  Median : 1180   Median : 1220   Median : 1240   Median : 1200  
##  Mean   : 1755   Mean   : 1786   Mean   : 1800   Mean   : 1830  
##  3rd Qu.: 2110   3rd Qu.: 2140   3rd Qu.: 2140   3rd Qu.: 2190  
##  Max.   :11300   Max.   :11100   Max.   :11500   Max.   :12100  
##       1897            1898            1899            1900      
##  Min.   :  310   Min.   :  313   Min.   :  315   Min.   :  318  
##  1st Qu.:  838   1st Qu.:  841   1st Qu.:  843   1st Qu.:  845  
##  Median : 1200   Median : 1220   Median : 1240   Median : 1250  
##  Mean   : 1835   Mean   : 1869   Mean   : 1890   Mean   : 1898  
##  3rd Qu.: 2240   3rd Qu.: 2270   3rd Qu.: 2250   3rd Qu.: 2250  
##  Max.   :12400   Max.   :12800   Max.   :13100   Max.   :13800  
##       1901            1902            1903            1904      
##  Min.   :  321   Min.   :  324   Min.   :  326   Min.   :  329  
##  1st Qu.:  851   1st Qu.:  871   1st Qu.:  876   1st Qu.:  883  
##  Median : 1260   Median : 1280   Median : 1290   Median : 1340  
##  Mean   : 1915   Mean   : 1946   Mean   : 1964   Mean   : 1994  
##  3rd Qu.: 2310   3rd Qu.: 2330   3rd Qu.: 2360   3rd Qu.: 2400  
##  Max.   :13100   Max.   :13500   Max.   :13300   Max.   :14200  
##       1905            1906            1907            1908      
##  Min.   :  332   Min.   :  335   Min.   :  337   Min.   :  340  
##  1st Qu.:  872   1st Qu.:  873   1st Qu.:  875   1st Qu.:  887  
##  Median : 1320   Median : 1350   Median : 1340   Median : 1350  
##  Mean   : 2007   Mean   : 2044   Mean   : 2059   Mean   : 2068  
##  3rd Qu.: 2450   3rd Qu.: 2460   3rd Qu.: 2520   3rd Qu.: 2580  
##  Max.   :14700   Max.   :15400   Max.   :15300   Max.   :15300  
##       1909            1910            1911            1912      
##  Min.   :  343   Min.   :  346   Min.   :  349   Min.   :  352  
##  1st Qu.:  919   1st Qu.:  924   1st Qu.:  905   1st Qu.:  917  
##  Median : 1370   Median : 1390   Median : 1410   Median : 1430  
##  Mean   : 2104   Mean   : 2144   Mean   : 2158   Mean   : 2194  
##  3rd Qu.: 2600   3rd Qu.: 2660   3rd Qu.: 2600   3rd Qu.: 2630  
##  Max.   :15600   Max.   :16100   Max.   :16200   Max.   :16800  
##       1913            1914            1915            1916      
##  Min.   :  355   Min.   :  356   Min.   :  356   Min.   :  356  
##  1st Qu.:  932   1st Qu.:  943   1st Qu.:  949   1st Qu.:  969  
##  Median : 1450   Median : 1460   Median : 1460   Median : 1450  
##  Mean   : 2214   Mean   : 2174   Mean   : 2177   Mean   : 2198  
##  3rd Qu.: 2640   3rd Qu.: 2680   3rd Qu.: 2670   3rd Qu.: 2640  
##  Max.   :16500   Max.   :15500   Max.   :15500   Max.   :16500  
##       1917            1918            1919            1920      
##  Min.   :  356   Min.   :  356   Min.   :  356   Min.   :  356  
##  1st Qu.:  964   1st Qu.:  914   1st Qu.:  917   1st Qu.:  905  
##  Median : 1450   Median : 1450   Median : 1470   Median : 1490  
##  Mean   : 2167   Mean   : 2112   Mean   : 2146   Mean   : 2179  
##  3rd Qu.: 2650   3rd Qu.: 2580   3rd Qu.: 2690   3rd Qu.: 2590  
##  Max.   :15900   Max.   :14500   Max.   :14400   Max.   :15300  
##       1921            1922            1923            1924      
##  Min.   :  360   Min.   :  364   Min.   :  367   Min.   :  371  
##  1st Qu.:  924   1st Qu.:  962   1st Qu.:  985   1st Qu.: 1010  
##  Median : 1540   Median : 1560   Median : 1570   Median : 1600  
##  Mean   : 2182   Mean   : 2274   Mean   : 2332   Mean   : 2410  
##  3rd Qu.: 2750   3rd Qu.: 2760   3rd Qu.: 2800   3rd Qu.: 2850  
##  Max.   :12900   Max.   :15100   Max.   :15700   Max.   :16900  
##       1925            1926            1927            1928      
##  Min.   :  374   Min.   :  378   Min.   :  379   Min.   :  380  
##  1st Qu.: 1030   1st Qu.: 1060   1st Qu.: 1070   1st Qu.: 1090  
##  Median : 1650   Median : 1730   Median : 1780   Median : 1830  
##  Mean   : 2484   Mean   : 2528   Mean   : 2577   Mean   : 2664  
##  3rd Qu.: 2910   3rd Qu.: 2910   3rd Qu.: 2940   3rd Qu.: 3050  
##  Max.   :17200   Max.   :17500   Max.   :18600   Max.   :19500  
##       1929            1930            1931            1932      
##  Min.   :  380   Min.   :  380   Min.   :  381   Min.   :  381  
##  1st Qu.: 1100   1st Qu.: 1110   1st Qu.: 1120   1st Qu.: 1120  
##  Median : 1880   Median : 1830   Median : 1800   Median : 1820  
##  Mean   : 2726   Mean   : 2702   Mean   : 2631   Mean   : 2575  
##  3rd Qu.: 3190   3rd Qu.: 3090   3rd Qu.: 3050   3rd Qu.: 3050  
##  Max.   :20100   Max.   :19800   Max.   :19700   Max.   :18300  
##       1933            1934            1935            1936      
##  Min.   :  382   Min.   :  382   Min.   :  382   Min.   :  379  
##  1st Qu.: 1140   1st Qu.: 1160   1st Qu.: 1170   1st Qu.: 1200  
##  Median : 1860   Median : 1910   Median : 1910   Median : 2050  
##  Mean   : 2639   Mean   : 2717   Mean   : 2795   Mean   : 2885  
##  3rd Qu.: 3190   3rd Qu.: 3300   3rd Qu.: 3340   3rd Qu.: 3520  
##  Max.   :18800   Max.   :18900   Max.   :18000   Max.   :17700  
##       1937            1938            1939            1940      
##  Min.   :  377   Min.   :  375   Min.   :  372   Min.   :  370  
##  1st Qu.: 1220   1st Qu.: 1250   1st Qu.: 1250   1st Qu.: 1240  
##  Median : 2090   Median : 2090   Median : 2110   Median : 2090  
##  Mean   : 2992   Mean   : 3037   Mean   : 3131   Mean   : 3123  
##  3rd Qu.: 3650   3rd Qu.: 3740   3rd Qu.: 3920   3rd Qu.: 3890  
##  Max.   :19500   Max.   :18600   Max.   :18900   Max.   :19000  
##       1941            1942            1943            1944      
##  Min.   :  367   Min.   :  364   Min.   :  362   Min.   :  360  
##  1st Qu.: 1250   1st Qu.: 1270   1st Qu.: 1250   1st Qu.: 1220  
##  Median : 2100   Median : 2130   Median : 2180   Median : 2150  
##  Mean   : 3169   Mean   : 3204   Mean   : 3246   Mean   : 3268  
##  3rd Qu.: 3860   3rd Qu.: 3860   3rd Qu.: 3760   3rd Qu.: 3700  
##  Max.   :18800   Max.   :17900   Max.   :19600   Max.   :23000  
##       1945            1946            1947            1948      
##  Min.   :  357   Min.   :  355   Min.   :  353   Min.   :  350  
##  1st Qu.: 1210   1st Qu.: 1230   1st Qu.: 1250   1st Qu.: 1290  
##  Median : 2140   Median : 2260   Median : 2290   Median : 2420  
##  Mean   : 3248   Mean   : 3370   Mean   : 3512   Mean   : 3695  
##  3rd Qu.: 3640   3rd Qu.: 3770   3rd Qu.: 3850   3rd Qu.: 4160  
##  Max.   :26900   Max.   :31600   Max.   :37100   Max.   :43500  
##       1949            1950            1951            1952      
##  Min.   :  348   Min.   :  345   Min.   :  351   Min.   :  358  
##  1st Qu.: 1320   1st Qu.: 1350   1st Qu.: 1380   1st Qu.: 1400  
##  Median : 2510   Median : 2580   Median : 2600   Median : 2710  
##  Mean   : 3844   Mean   : 4020   Mean   : 4147   Mean   : 4249  
##  3rd Qu.: 4440   3rd Qu.: 4500   3rd Qu.: 4650   3rd Qu.: 4700  
##  Max.   :51000   Max.   :59800   Max.   :61200   Max.   :62500  
##       1953            1954            1955            1956      
##  Min.   :  365   Min.   :  367   Min.   :  383   Min.   :  378  
##  1st Qu.: 1430   1st Qu.: 1440   1st Qu.: 1460   1st Qu.: 1480  
##  Median : 2760   Median : 2820   Median : 2930   Median : 2960  
##  Mean   : 4414   Mean   : 4582   Mean   : 4797   Mean   : 4975  
##  3rd Qu.: 4770   3rd Qu.: 5020   3rd Qu.: 5350   3rd Qu.: 5660  
##  Max.   :64000   Max.   :65400   Max.   :66900   Max.   :68400  
##       1957            1958            1959            1960      
##  Min.   :  379   Min.   :  389   Min.   :  402   Min.   :  404  
##  1st Qu.: 1510   1st Qu.: 1510   1st Qu.: 1550   1st Qu.: 1560  
##  Median : 3050   Median : 3140   Median : 3190   Median : 3160  
##  Mean   : 5138   Mean   : 5263   Mean   : 5448   Mean   : 5710  
##  3rd Qu.: 5890   3rd Qu.: 6110   3rd Qu.: 6160   3rd Qu.: 6610  
##  Max.   :69900   Max.   :71500   Max.   :73100   Max.   :74700  
##       1961            1962            1963            1964      
##  Min.   :  409   Min.   :  430   Min.   :  408   Min.   :  418  
##  1st Qu.: 1590   1st Qu.: 1600   1st Qu.: 1660   1st Qu.: 1750  
##  Median : 3380   Median : 3420   Median : 3590   Median : 3670  
##  Mean   : 5961   Mean   : 6190   Mean   : 6445   Mean   : 6861  
##  3rd Qu.: 7130   3rd Qu.: 7410   3rd Qu.: 7950   3rd Qu.: 8280  
##  Max.   :76300   Max.   :78000   Max.   :79800   Max.   :81500  
##       1965            1966            1967            1968       
##  Min.   :  421   Min.   :  427   Min.   :  447   Min.   :   488  
##  1st Qu.: 1830   1st Qu.: 1880   1st Qu.: 1970   1st Qu.:  1930  
##  Median : 3760   Median : 3890   Median : 3830   Median :  4200  
##  Mean   : 7227   Mean   : 7608   Mean   : 8012   Mean   :  8547  
##  3rd Qu.: 8840   3rd Qu.: 9410   3rd Qu.: 9690   3rd Qu.: 10300  
##  Max.   :83300   Max.   :85100   Max.   :87200   Max.   :110000  
##       1969             1970             1971             1972       
##  Min.   :   536   Min.   :   555   Min.   :   580   Min.   :   565  
##  1st Qu.:  2030   1st Qu.:  2110   1st Qu.:  2180   1st Qu.:  2180  
##  Median :  4170   Median :  4400   Median :  4470   Median :  4470  
##  Mean   :  9118   Mean   :  9629   Mean   :  9938   Mean   : 10256  
##  3rd Qu.: 11200   3rd Qu.: 11800   3rd Qu.: 12300   3rd Qu.: 13000  
##  Max.   :138000   Max.   :174000   Max.   :174000   Max.   :176000  
##       1973             1974             1975             1976       
##  Min.   :   604   Min.   :   545   Min.   :   457   Min.   :   423  
##  1st Qu.:  2290   1st Qu.:  2340   1st Qu.:  2310   1st Qu.:  2420  
##  Median :  4910   Median :  5240   Median :  5180   Median :  5010  
##  Mean   : 10773   Mean   : 11935   Mean   : 11852   Mean   : 12197  
##  3rd Qu.: 13300   3rd Qu.: 13600   3rd Qu.: 13400   3rd Qu.: 14000  
##  Max.   :178000   Max.   :173000   Max.   :171000   Max.   :174000  
##       1977             1978             1979             1980       
##  Min.   :   414   Min.   :   407   Min.   :   401   Min.   :   405  
##  1st Qu.:  2330   1st Qu.:  2490   1st Qu.:  2360   1st Qu.:  2330  
##  Median :  5210   Median :  5530   Median :  5650   Median :  5270  
##  Mean   : 12279   Mean   : 12371   Mean   : 12813   Mean   : 12640  
##  3rd Qu.: 14800   3rd Qu.: 15500   3rd Qu.: 15900   3rd Qu.: 15000  
##  Max.   :163000   Max.   :157000   Max.   :166000   Max.   :179000  
##       1981             1982             1983             1984       
##  Min.   :   406   Min.   :   389   Min.   :   357   Min.   :   346  
##  1st Qu.:  2370   1st Qu.:  2410   1st Qu.:  2440   1st Qu.:  2400  
##  Median :  5370   Median :  5420   Median :  5610   Median :  5740  
##  Mean   : 12241   Mean   : 11944   Mean   : 11782   Mean   : 11922  
##  3rd Qu.: 15100   3rd Qu.: 15100   3rd Qu.: 15000   3rd Qu.: 15100  
##  Max.   :177000   Max.   :155000   Max.   :142000   Max.   :137000  
##       1985             1986            1987            1988      
##  Min.   :   312   Min.   :  316   Min.   :  335   Min.   :  367  
##  1st Qu.:  2360   1st Qu.: 2500   1st Qu.: 2360   1st Qu.: 2390  
##  Median :  5830   Median : 5860   Median : 6030   Median : 6320  
##  Mean   : 11862   Mean   :11759   Mean   :11821   Mean   :11924  
##  3rd Qu.: 14700   3rd Qu.:15200   3rd Qu.:15400   3rd Qu.:15900  
##  Max.   :127000   Max.   :98000   Max.   :99100   Max.   :86800  
##       1989            1990             1991             1992       
##  Min.   :  385   Min.   :   386   Min.   :   395   Min.   :   361  
##  1st Qu.: 2340   1st Qu.:  2390   1st Qu.:  2370   1st Qu.:  2320  
##  Median : 6060   Median :  6380   Median :  6070   Median :  6170  
##  Mean   :12163   Mean   : 12238   Mean   : 12002   Mean   : 12004  
##  3rd Qu.:16500   3rd Qu.: 15600   3rd Qu.: 15100   3rd Qu.: 14700  
##  Max.   :90100   Max.   :112000   Max.   :107000   Max.   :104000  
##       1993            1994             1995             1996       
##  Min.   :  377   Min.   :   385   Min.   :   380   Min.   :   413  
##  1st Qu.: 2270   1st Qu.:  2220   1st Qu.:  2320   1st Qu.:  2400  
##  Median : 6290   Median :  6130   Median :  6200   Median :  6460  
##  Mean   :12034   Mean   : 12257   Mean   : 12535   Mean   : 12834  
##  3rd Qu.:14100   3rd Qu.: 14600   3rd Qu.: 14600   3rd Qu.: 15200  
##  Max.   :99800   Max.   :101000   Max.   :102000   Max.   :103000  
##       1997             1998             1999             2000       
##  Min.   :   505   Min.   :   550   Min.   :   579   Min.   :   573  
##  1st Qu.:  2460   1st Qu.:  2640   1st Qu.:  2660   1st Qu.:  2710  
##  Median :  6680   Median :  6920   Median :  7130   Median :  7220  
##  Mean   : 13296   Mean   : 13562   Mean   : 13781   Mean   : 14236  
##  3rd Qu.: 15800   3rd Qu.: 15900   3rd Qu.: 15600   3rd Qu.: 16300  
##  Max.   :106000   Max.   :103000   Max.   :104000   Max.   :108000  
##       2001             2002             2003             2004       
##  Min.   :   545   Min.   :   545   Min.   :   558   Min.   :   577  
##  1st Qu.:  2710   1st Qu.:  2710   1st Qu.:  2740   1st Qu.:  2910  
##  Median :  7460   Median :  7620   Median :  7700   Median :  7930  
##  Mean   : 14394   Mean   : 14577   Mean   : 14900   Mean   : 15489  
##  3rd Qu.: 17700   3rd Qu.: 17700   3rd Qu.: 18600   3rd Qu.: 19300  
##  Max.   :108000   Max.   :111000   Max.   :109000   Max.   :117000  
##       2005             2006             2007             2008       
##  Min.   :   594   Min.   :   605   Min.   :   615   Min.   :   615  
##  1st Qu.:  2880   1st Qu.:  2950   1st Qu.:  3160   1st Qu.:  3200  
##  Median :  8180   Median :  8780   Median :  8990   Median :  9460  
##  Mean   : 15898   Mean   : 16504   Mean   : 17070   Mean   : 17181  
##  3rd Qu.: 20100   3rd Qu.: 21700   3rd Qu.: 23500   3rd Qu.: 24000  
##  Max.   :110000   Max.   :117000   Max.   :116000   Max.   :116000  
##       2009             2010             2011             2012       
##  Min.   :   615   Min.   :   614   Min.   :   614   Min.   :   616  
##  1st Qu.:  3250   1st Qu.:  3340   1st Qu.:  3420   1st Qu.:  3570  
##  Median :  9530   Median :  9930   Median : 10000   Median : 10200  
##  Mean   : 16445   Mean   : 16703   Mean   : 16940   Mean   : 17093  
##  3rd Qu.: 22200   3rd Qu.: 22400   3rd Qu.: 22900   3rd Qu.: 24000  
##  Max.   :113000   Max.   :120000   Max.   :124000   Max.   :120000  
##       2013             2014             2015             2016       
##  Min.   :   619   Min.   :   621   Min.   :   623   Min.   :   625  
##  1st Qu.:  3630   1st Qu.:  3560   1st Qu.:  3390   1st Qu.:  3470  
##  Median : 10500   Median : 10900   Median : 11100   Median : 11400  
##  Mean   : 17227   Mean   : 17428   Mean   : 17686   Mean   : 17895  
##  3rd Qu.: 23600   3rd Qu.: 24400   3rd Qu.: 25100   3rd Qu.: 25600  
##  Max.   :118000   Max.   :117000   Max.   :116000   Max.   :114000  
##       2017             2018             2019             2020       
##  Min.   :   627   Min.   :   629   Min.   :   631   Min.   :   628  
##  1st Qu.:  3650   1st Qu.:  3740   1st Qu.:  3910   1st Qu.:  4030  
##  Median : 11700   Median : 12100   Median : 12000   Median : 12300  
##  Mean   : 18173   Mean   : 18463   Mean   : 18647   Mean   : 18950  
##  3rd Qu.: 26400   3rd Qu.: 27100   3rd Qu.: 27800   3rd Qu.: 28300  
##  Max.   :113000   Max.   :113000   Max.   :113000   Max.   :116000  
##       2021             2022             2023             2024       
##  Min.   :   613   Min.   :   598   Min.   :   583   Min.   :   569  
##  1st Qu.:  4170   1st Qu.:  4440   1st Qu.:  4450   1st Qu.:  4480  
##  Median : 12700   Median : 13000   Median : 13400   Median : 13400  
##  Mean   : 19278   Mean   : 19595   Mean   : 19926   Mean   : 20273  
##  3rd Qu.: 29000   3rd Qu.: 29800   3rd Qu.: 30200   3rd Qu.: 30500  
##  Max.   :119000   Max.   :122000   Max.   :124000   Max.   :127000  
##       2025             2026             2027             2028       
##  Min.   :   557   Min.   :   548   Min.   :   542   Min.   :   541  
##  1st Qu.:  4700   1st Qu.:  4890   1st Qu.:  4910   1st Qu.:  4960  
##  Median : 13700   Median : 13800   Median : 14000   Median : 14300  
##  Mean   : 20641   Mean   : 21012   Mean   : 21404   Mean   : 21816  
##  3rd Qu.: 30900   3rd Qu.: 31500   3rd Qu.: 32300   3rd Qu.: 33000  
##  Max.   :131000   Max.   :134000   Max.   :137000   Max.   :140000  
##       2029             2030             2031             2032       
##  Min.   :   543   Min.   :   549   Min.   :   557   Min.   :   566  
##  1st Qu.:  5020   1st Qu.:  5090   1st Qu.:  5180   1st Qu.:  5280  
##  Median : 14700   Median : 15000   Median : 15400   Median : 15700  
##  Mean   : 22240   Mean   : 22681   Mean   : 23142   Mean   : 23613  
##  3rd Qu.: 33200   3rd Qu.: 33600   3rd Qu.: 34200   3rd Qu.: 34800  
##  Max.   :143000   Max.   :146000   Max.   :149000   Max.   :153000  
##       2033             2034             2035             2036       
##  Min.   :   577   Min.   :   588   Min.   :   600   Min.   :   612  
##  1st Qu.:  5380   1st Qu.:  5490   1st Qu.:  5600   1st Qu.:  5710  
##  Median : 16000   Median : 16400   Median : 16700   Median : 17000  
##  Mean   : 24083   Mean   : 24577   Mean   : 25078   Mean   : 25576  
##  3rd Qu.: 35500   3rd Qu.: 36200   3rd Qu.: 37000   3rd Qu.: 37700  
##  Max.   :156000   Max.   :159000   Max.   :162000   Max.   :165000  
##       2037             2038             2039             2040       
##  Min.   :   625   Min.   :   637   Min.   :   650   Min.   :   664  
##  1st Qu.:  5830   1st Qu.:  5950   1st Qu.:  6070   1st Qu.:  6190  
##  Median : 17400   Median : 17700   Median : 18100   Median : 18500  
##  Mean   : 26108   Mean   : 26636   Mean   : 27181   Mean   : 27731  
##  3rd Qu.: 38500   3rd Qu.: 39300   3rd Qu.: 40100   3rd Qu.: 40900  
##  Max.   :169000   Max.   :172000   Max.   :176000   Max.   :179000
```

```r
naniar::miss_var_summary(income_ppp_gpd)
```

```
## # A tibble: 242 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 country       0        0
##  2 1800          0        0
##  3 1801          0        0
##  4 1802          0        0
##  5 1803          0        0
##  6 1804          0        0
##  7 1805          0        0
##  8 1806          0        0
##  9 1807          0        0
## 10 1808          0        0
## # ... with 232 more rows
```

```r
wide_income_ppp_gpd<- income_ppp_gpd
wide_income_ppp_gpd
```

```
## # A tibble: 193 x 242
##    country `1800` `1801` `1802` `1803` `1804` `1805` `1806` `1807` `1808` `1809`
##    <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Afghan~    603    603    603    603    603    603    603    603    603    603
##  2 Albania    667    667    667    667    667    668    668    668    668    668
##  3 Algeria    715    716    717    718    719    720    721    722    723    724
##  4 Andorra   1200   1200   1200   1200   1210   1210   1210   1210   1220   1220
##  5 Angola     618    620    623    626    628    631    634    637    640    642
##  6 Antigu~    757    757    757    757    757    757    757    758    758    758
##  7 Argent~   1640   1640   1650   1650   1660   1660   1670   1680   1680   1690
##  8 Armenia    514    514    514    514    514    514    514    514    514    514
##  9 Austra~    817    822    826    831    836    841    845    850    855    860
## 10 Austria   1850   1850   1860   1870   1880   1880   1890   1900   1910   1920
## # ... with 183 more rows, and 231 more variables: 1810 <dbl>, 1811 <dbl>,
## #   1812 <dbl>, 1813 <dbl>, 1814 <dbl>, 1815 <dbl>, 1816 <dbl>, 1817 <dbl>,
## #   1818 <dbl>, 1819 <dbl>, 1820 <dbl>, 1821 <dbl>, 1822 <dbl>, 1823 <dbl>,
## #   1824 <dbl>, 1825 <dbl>, 1826 <dbl>, 1827 <dbl>, 1828 <dbl>, 1829 <dbl>,
## #   1830 <dbl>, 1831 <dbl>, 1832 <dbl>, 1833 <dbl>, 1834 <dbl>, 1835 <dbl>,
## #   1836 <dbl>, 1837 <dbl>, 1838 <dbl>, 1839 <dbl>, 1840 <dbl>, 1841 <dbl>,
## #   1842 <dbl>, 1843 <dbl>, 1844 <dbl>, 1845 <dbl>, 1846 <dbl>, 1847 <dbl>, ...
```


```r
long_income_ppp_gpd<-
  income_ppp_gpd%>%
  pivot_longer("1800":"2040", names_to = "years", values_to = "average_income_pp")
long_income_ppp_gpd
```

```
## # A tibble: 46,513 x 3
##    country     years average_income_pp
##    <chr>       <chr>             <dbl>
##  1 Afghanistan 1800                603
##  2 Afghanistan 1801                603
##  3 Afghanistan 1802                603
##  4 Afghanistan 1803                603
##  5 Afghanistan 1804                603
##  6 Afghanistan 1805                603
##  7 Afghanistan 1806                603
##  8 Afghanistan 1807                603
##  9 Afghanistan 1808                603
## 10 Afghanistan 1809                603
## # ... with 46,503 more rows
```

```r
naniar::miss_var_summary(life_expectancy)
```

```
## # A tibble: 302 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 1800          3     1.60
##  2 1801          3     1.60
##  3 1802          3     1.60
##  4 1803          3     1.60
##  5 1804          3     1.60
##  6 1805          3     1.60
##  7 1806          3     1.60
##  8 1807          3     1.60
##  9 1808          3     1.60
## 10 1809          3     1.60
## # ... with 292 more rows
```

```r
glimpse(life_expectancy)
```

```
## Rows: 187
## Columns: 302
## $ country <chr> "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Ant~
## $ `1800`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1801`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1802`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1803`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1804`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1805`  <dbl> 28.2, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1806`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1807`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1808`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1809`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1810`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1811`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1812`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1813`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1814`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1815`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1816`  <dbl> 28.1, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1817`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1818`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1819`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1820`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1821`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1822`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1823`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1824`  <dbl> 28.0, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1825`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1826`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1827`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1828`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1829`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1830`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1831`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1832`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1833`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1834`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1835`  <dbl> 27.9, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1836`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1837`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1838`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1839`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1840`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1841`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1842`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1843`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1844`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1845`  <dbl> 27.8, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1846`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1847`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1848`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1849`  <dbl> 27.7, 35.4, 20.0, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1850`  <dbl> 27.7, 35.4, 15.0, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1851`  <dbl> 27.7, 35.4, 22.0, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1852`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1853`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1854`  <dbl> 27.7, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1855`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1856`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1857`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1858`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1859`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1860`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1861`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1862`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1863`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 34.0, 34.0, 34.4, 29.2~
## $ `1864`  <dbl> 27.6, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 33.5, 34.0, 34.4, 28.6~
## $ `1865`  <dbl> 27.5, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 33.0, 34.0, 34.4, 28.1~
## $ `1866`  <dbl> 27.5, 35.4, 28.8, NA, 27.0, 33.5, 33.2, 32.4, 34.0, 34.4, 27.6~
## $ `1867`  <dbl> 27.5, 35.4, 21.0, NA, 27.0, 33.5, 33.2, 31.9, 34.0, 34.4, 27.1~
## $ `1868`  <dbl> 27.5, 35.4, 11.0, NA, 27.0, 33.5, 33.2, 31.4, 34.0, 34.4, 26.6~
## $ `1869`  <dbl> 27.5, 35.4, 15.0, NA, 27.0, 33.5, 33.2, 31.4, 34.0, 34.4, 26.6~
## $ `1870`  <dbl> 27.5, 35.4, 22.0, NA, 27.0, 33.5, 33.2, 31.5, 34.0, 34.4, 26.6~
## $ `1871`  <dbl> 27.6, 35.4, 28.9, NA, 27.1, 33.5, 33.2, 31.5, 34.6, 34.5, 26.7~
## $ `1872`  <dbl> 27.6, 35.4, 28.9, NA, 27.1, 33.5, 33.2, 31.5, 35.1, 34.5, 26.7~
## $ `1873`  <dbl> 27.7, 35.4, 29.0, NA, 27.2, 33.6, 33.2, 31.5, 35.6, 34.6, 26.7~
## $ `1874`  <dbl> 27.8, 35.4, 29.0, NA, 27.3, 33.6, 33.2, 31.7, 36.2, 34.6, 26.9~
## $ `1875`  <dbl> 27.8, 35.4, 29.0, NA, 27.3, 33.6, 33.2, 31.9, 36.7, 34.7, 27.1~
## $ `1876`  <dbl> 27.9, 35.4, 29.1, NA, 27.4, 33.6, 33.1, 32.1, 37.2, 34.7, 27.3~
## $ `1877`  <dbl> 27.9, 35.4, 29.1, NA, 27.5, 33.6, 33.1, 32.2, 37.8, 34.8, 27.5~
## $ `1878`  <dbl> 28.0, 35.4, 29.2, NA, 27.5, 33.6, 33.1, 32.4, 38.3, 34.9, 27.7~
## $ `1879`  <dbl> 28.1, 35.4, 29.2, NA, 27.6, 33.6, 33.1, 32.6, 38.8, 34.9, 27.9~
## $ `1880`  <dbl> 28.1, 35.4, 29.3, NA, 27.7, 33.6, 33.1, 32.8, 39.4, 35.0, 28.1~
## $ `1881`  <dbl> 28.2, 35.4, 29.3, NA, 27.7, 33.6, 33.0, 32.9, 39.9, 35.0, 28.2~
## $ `1882`  <dbl> 28.3, 35.4, 29.4, NA, 27.8, 33.6, 32.9, 33.1, 40.4, 35.2, 28.4~
## $ `1883`  <dbl> 28.3, 35.4, 29.4, NA, 27.9, 33.6, 32.7, 33.3, 41.0, 35.5, 28.6~
## $ `1884`  <dbl> 28.4, 35.4, 29.4, NA, 27.9, 33.6, 32.6, 33.5, 41.5, 35.7, 28.8~
## $ `1885`  <dbl> 28.5, 35.4, 29.5, NA, 28.0, 33.6, 32.5, 33.6, 42.0, 35.9, 29.0~
## $ `1886`  <dbl> 28.5, 35.4, 29.5, NA, 28.1, 33.6, 32.8, 33.8, 42.5, 36.2, 29.2~
## $ `1887`  <dbl> 28.6, 35.4, 29.6, NA, 28.1, 33.7, 33.1, 34.0, 43.1, 36.4, 29.4~
## $ `1888`  <dbl> 28.6, 35.4, 29.6, NA, 28.2, 33.7, 33.4, 34.2, 43.6, 36.6, 29.5~
## $ `1889`  <dbl> 28.7, 35.4, 29.7, NA, 28.3, 33.7, 33.6, 34.3, 44.1, 36.9, 29.7~
## $ `1890`  <dbl> 28.8, 35.4, 29.7, NA, 28.4, 33.7, 33.9, 34.5, 44.7, 37.1, 29.9~
## $ `1891`  <dbl> 28.8, 35.4, 29.8, NA, 28.4, 33.7, 33.8, 34.7, 45.2, 37.3, 30.1~
## $ `1892`  <dbl> 28.9, 35.4, 29.8, NA, 28.5, 33.7, 33.6, 34.9, 45.7, 37.8, 30.3~
## $ `1893`  <dbl> 29.0, 35.4, 29.8, NA, 28.6, 33.7, 33.5, 35.0, 46.3, 38.3, 30.5~
## $ `1894`  <dbl> 29.0, 35.4, 29.9, NA, 28.6, 33.7, 33.3, 35.2, 46.8, 38.7, 30.6~
## $ `1895`  <dbl> 29.1, 35.4, 29.9, NA, 28.7, 33.7, 33.2, 35.4, 47.3, 39.2, 30.8~
## $ `1896`  <dbl> 29.1, 35.4, 30.0, NA, 28.8, 33.7, 33.9, 35.5, 47.9, 39.7, 31.0~
## $ `1897`  <dbl> 29.2, 35.4, 30.0, NA, 28.8, 33.7, 34.6, 35.7, 48.4, 40.2, 31.2~
## $ `1898`  <dbl> 29.3, 35.4, 30.1, NA, 28.9, 33.7, 35.3, 35.5, 48.9, 40.6, 31.0~
## $ `1899`  <dbl> 29.3, 35.4, 30.1, NA, 29.0, 33.8, 36.0, 35.3, 49.5, 41.1, 30.9~
## $ `1900`  <dbl> 29.4, 35.4, 30.2, NA, 29.0, 33.8, 36.6, 35.1, 50.0, 41.6, 30.7~
## $ `1901`  <dbl> 29.5, 35.4, 30.3, NA, 29.1, 33.8, 37.2, 35.4, 50.5, 42.0, 30.9~
## $ `1902`  <dbl> 29.5, 35.4, 30.4, NA, 29.2, 33.8, 37.8, 35.6, 51.0, 41.1, 31.1~
## $ `1903`  <dbl> 29.6, 35.4, 31.4, NA, 29.3, 33.8, 38.4, 35.8, 51.6, 40.1, 31.4~
## $ `1904`  <dbl> 29.7, 35.4, 25.4, NA, 29.3, 33.8, 38.9, 36.0, 52.1, 40.8, 31.6~
## $ `1905`  <dbl> 29.7, 35.4, 28.1, NA, 29.4, 33.8, 39.5, 36.2, 52.6, 41.4, 31.8~
## $ `1906`  <dbl> 29.8, 35.4, 29.6, NA, 29.4, 33.8, 40.3, 36.5, 53.2, 42.0, 32.1~
## $ `1907`  <dbl> 29.9, 35.4, 29.5, NA, 29.5, 33.8, 41.0, 36.7, 53.7, 42.6, 32.3~
## $ `1908`  <dbl> 29.9, 35.4, 29.5, NA, 29.6, 33.8, 41.8, 36.9, 54.2, 43.3, 32.5~
## $ `1909`  <dbl> 30.0, 35.4, 31.0, NA, 29.7, 33.8, 42.5, 37.1, 54.8, 43.9, 32.8~
## $ `1910`  <dbl> 30.0, 35.4, 32.7, NA, 29.7, 33.8, 43.3, 37.3, 55.3, 44.5, 33.0~
## $ `1911`  <dbl> 30.1, 35.4, 32.4, NA, 29.8, 33.8, 43.8, 40.2, 55.8, 45.2, 35.9~
## $ `1912`  <dbl> 30.2, 35.4, 33.8, NA, 29.9, 33.8, 44.3, 40.6, 56.4, 45.7, 36.3~
## $ `1913`  <dbl> 30.2, 35.4, 31.6, NA, 29.9, 33.9, 44.9, 38.4, 56.9, 46.3, 34.1~
## $ `1914`  <dbl> 30.3, 35.4, 31.1, NA, 30.0, 33.9, 45.4, 38.0, 57.4, 46.9, 33.7~
## $ `1915`  <dbl> 30.3, 35.4, 30.6, NA, 30.1, 33.9, 45.9, 37.6, 58.0, 47.4, 33.3~
## $ `1916`  <dbl> 30.4, 35.4, 30.3, NA, 30.1, 33.9, 46.7, 37.6, 58.5, 48.0, 33.3~
## $ `1917`  <dbl> 30.5, 35.4, 30.3, NA, 30.2, 33.9, 47.5, 34.6, 59.0, 48.6, 30.4~
## $ `1918`  <dbl> 7.97, 19.40, 23.70, NA, 11.70, 22.00, 41.50, 26.10, 54.90, 32.~
## $ `1919`  <dbl> 30.6, 35.4, 30.4, NA, 30.4, 33.9, 49.1, 35.7, 60.1, 49.7, 31.3~
## $ `1920`  <dbl> 30.6, 35.4, 29.6, NA, 30.4, 33.9, 49.9, 27.0, 60.6, 50.3, 29.4~
## $ `1921`  <dbl> 30.7, 35.4, 29.6, NA, 30.5, 33.9, 50.3, 36.7, 61.1, 50.9, 32.3~
## $ `1922`  <dbl> 30.8, 35.4, 29.4, NA, 30.6, 34.8, 50.8, 37.6, 63.0, 51.4, 33.3~
## $ `1923`  <dbl> 30.8, 35.4, 32.0, NA, 30.6, 35.6, 51.3, 38.4, 61.8, 52.0, 34.3~
## $ `1924`  <dbl> 30.9, 35.4, 33.5, NA, 30.7, 36.4, 51.7, 40.8, 62.6, 52.6, 36.7~
## $ `1925`  <dbl> 31.0, 35.4, 34.3, NA, 30.8, 37.3, 52.2, 39.9, 63.3, 53.1, 35.8~
## $ `1926`  <dbl> 31.0, 35.4, 33.6, NA, 30.8, 38.1, 52.5, 42.9, 63.1, 53.7, 38.9~
## $ `1927`  <dbl> 31.1, 35.4, 28.8, NA, 30.9, 39.0, 52.9, 42.3, 63.0, 54.3, 38.2~
## $ `1928`  <dbl> 31.1, 35.4, 32.4, NA, 31.0, 39.8, 53.2, 43.7, 63.1, 54.9, 39.7~
## $ `1929`  <dbl> 31.2, 35.4, 32.7, NA, 31.0, 40.7, 53.5, 42.5, 63.3, 55.4, 38.5~
## $ `1930`  <dbl> 31.3, 36.3, 34.0, NA, 31.1, 41.5, 53.9, 41.8, 65.1, 56.0, 37.8~
## $ `1931`  <dbl> 31.3, 37.2, 31.9, NA, 31.2, 42.4, 53.9, 40.2, 65.5, 56.6, 36.1~
## $ `1932`  <dbl> 31.4, 38.1, 33.3, NA, 31.2, 43.2, 53.9, 37.8, 65.8, 56.9, 33.6~
## $ `1933`  <dbl> 31.4, 39.0, 34.6, NA, 31.3, 44.1, 53.8, 31.6, 65.7, 57.2, 26.3~
## $ `1934`  <dbl> 31.5, 39.9, 34.0, NA, 31.4, 44.9, 53.8, 43.9, 65.0, 57.5, 40.0~
## $ `1935`  <dbl> 31.6, 40.7, 35.9, NA, 31.4, 45.8, 53.8, 45.4, 65.3, 57.8, 41.5~
## $ `1936`  <dbl> 31.6, 41.6, 37.1, NA, 31.5, 46.6, 54.8, 46.9, 65.4, 58.1, 43.1~
## $ `1937`  <dbl> 31.7, 42.5, 35.2, NA, 31.6, 47.5, 55.7, 46.0, 66.0, 58.4, 42.1~
## $ `1938`  <dbl> 31.8, 43.4, 34.6, NA, 31.6, 48.3, 56.7, 47.5, 66.0, 58.7, 43.8~
## $ `1939`  <dbl> 31.8, 43.0, 36.9, NA, 31.7, 49.2, 57.6, 49.8, 66.0, 58.2, 46.1~
## $ `1940`  <dbl> 31.9, 42.0, 37.3, NA, 31.8, 50.0, 58.5, 47.4, 66.4, 57.9, 43.6~
## $ `1941`  <dbl> 31.9, 41.5, 35.6, NA, 32.1, 50.9, 59.1, 26.6, 66.3, 56.6, 29.4~
## $ `1942`  <dbl> 32.0, 40.0, 34.9, NA, 32.5, 51.7, 59.7, 23.3, 66.0, 54.2, 26.0~
## $ `1943`  <dbl> 32.1, 37.0, 30.2, NA, 32.8, 52.6, 60.3, 21.4, 66.6, 50.3, 24.0~
## $ `1944`  <dbl> 32.1, 34.0, 35.7, NA, 33.1, 53.4, 60.8, 28.5, 68.2, 39.2, 31.0~
## $ `1945`  <dbl> 32.2, 47.0, 33.5, NA, 33.5, 54.3, 61.4, 37.5, 68.6, 31.5, 40.7~
## $ `1946`  <dbl> 32.2, 50.0, 35.7, NA, 33.8, 55.1, 61.3, 53.8, 68.2, 56.1, 50.3~
## $ `1947`  <dbl> 32.3, 51.5, 39.1, NA, 34.1, 56.0, 61.1, 46.2, 68.8, 61.5, 42.5~
## $ `1948`  <dbl> 32.4, 52.4, 42.3, NA, 34.5, 56.8, 61.0, 53.8, 68.7, 63.4, 50.3~
## $ `1949`  <dbl> 32.4, 53.3, 44.8, NA, 34.8, 57.7, 60.9, 56.4, 69.3, 63.6, 53.0~
## $ `1950`  <dbl> 32.5, 54.1, 47.3, NA, 35.2, 58.5, 60.8, 58.5, 69.2, 65.0, 55.2~
## $ `1951`  <dbl> 32.9, 54.4, 47.5, NA, 35.5, 58.8, 61.1, 58.7, 68.9, 65.4, 55.4~
## $ `1952`  <dbl> 33.6, 54.8, 48.0, NA, 36.0, 59.3, 61.6, 59.0, 69.3, 67.0, 55.8~
## $ `1953`  <dbl> 34.3, 55.4, 48.6, NA, 36.6, 59.8, 62.2, 59.4, 69.9, 67.5, 56.1~
## $ `1954`  <dbl> 35.0, 56.1, 49.1, NA, 37.1, 60.4, 62.6, 59.8, 70.0, 67.5, 56.5~
## $ `1955`  <dbl> 35.7, 57.0, 49.6, NA, 37.7, 60.9, 63.0, 60.1, 70.4, 67.8, 56.9~
## $ `1956`  <dbl> 36.4, 57.9, 50.2, NA, 38.3, 61.4, 63.4, 60.5, 70.3, 67.9, 57.3~
## $ `1957`  <dbl> 37.1, 58.9, 50.8, NA, 38.8, 61.9, 63.7, 60.8, 70.5, 67.7, 57.6~
## $ `1958`  <dbl> 37.9, 60.0, 51.3, NA, 39.4, 62.4, 63.9, 61.2, 71.1, 68.7, 58.0~
## $ `1959`  <dbl> 38.6, 61.1, 51.9, NA, 40.0, 62.9, 64.1, 61.5, 70.7, 68.6, 58.4~
## $ `1960`  <dbl> 39.3, 62.2, 52.5, NA, 40.6, 63.3, 64.2, 61.9, 71.1, 69.0, 58.7~
## $ `1961`  <dbl> 40.0, 63.3, 53.1, NA, 41.1, 63.8, 64.3, 62.2, 71.4, 70.0, 59.1~
## $ `1962`  <dbl> 40.8, 64.2, 53.7, NA, 41.7, 64.2, 64.4, 62.6, 71.2, 69.8, 59.5~
## $ `1963`  <dbl> 41.5, 64.9, 54.3, NA, 42.3, 64.7, 64.5, 62.9, 71.2, 69.9, 59.9~
## $ `1964`  <dbl> 42.3, 65.4, 54.9, NA, 42.9, 65.1, 64.6, 63.3, 70.9, 70.4, 60.2~
## $ `1965`  <dbl> 43.0, 65.8, 55.4, NA, 43.5, 65.5, 64.7, 63.7, 71.2, 70.2, 60.6~
## $ `1966`  <dbl> 43.7, 66.1, 56.0, NA, 44.1, 65.8, 64.8, 64.0, 71.0, 70.5, 61.0~
## $ `1967`  <dbl> 44.5, 66.3, 56.5, NA, 44.7, 66.2, 64.9, 64.4, 71.3, 70.4, 61.4~
## $ `1968`  <dbl> 45.2, 66.4, 57.0, NA, 45.3, 66.6, 65.1, 64.7, 71.0, 70.5, 61.7~
## $ `1969`  <dbl> 45.9, 66.6, 57.6, NA, 45.9, 67.0, 65.3, 65.0, 71.4, 70.3, 62.0~
## $ `1970`  <dbl> 46.6, 66.9, 58.1, 75.5, 46.5, 67.3, 65.6, 65.3, 71.0, 70.3, 62~
## $ `1971`  <dbl> 46.8, 67.5, 58.4, 75.8, 46.7, 67.7, 65.8, 65.9, 71.3, 70.5, 63~
## $ `1972`  <dbl> 46.8, 68.1, 58.8, 76.1, 46.9, 68.1, 66.2, 66.4, 71.6, 70.7, 63~
## $ `1973`  <dbl> 46.9, 68.7, 59.2, 76.4, 47.1, 68.5, 66.6, 66.9, 71.9, 71.1, 64~
## $ `1974`  <dbl> 47.0, 69.3, 59.7, 76.7, 47.3, 68.8, 67.0, 67.4, 72.1, 71.3, 65~
## $ `1975`  <dbl> 47.1, 69.8, 60.1, 77.0, 47.2, 68.9, 67.5, 67.8, 72.5, 71.5, 65~
## $ `1976`  <dbl> 47.3, 70.3, 60.6, 77.2, 47.2, 69.1, 68.0, 68.3, 72.9, 72.0, 66~
## $ `1977`  <dbl> 47.5, 70.8, 61.2, 77.5, 47.3, 69.5, 68.4, 68.7, 73.3, 72.3, 66~
## $ `1978`  <dbl> 45.9, 71.2, 61.8, 77.8, 47.4, 69.8, 69.0, 69.0, 73.7, 72.5, 66~
## $ `1979`  <dbl> 44.4, 71.5, 62.5, 78.1, 47.5, 70.5, 69.6, 69.2, 74.1, 72.7, 66~
## $ `1980`  <dbl> 44.1, 71.7, 62.8, 78.2, 47.6, 71.1, 70.1, 69.4, 74.4, 72.8, 66~
## $ `1981`  <dbl> 44.9, 71.8, 64.0, 78.3, 47.7, 71.6, 70.4, 69.8, 74.7, 73.1, 66~
## $ `1982`  <dbl> 44.6, 72.0, 65.1, 78.3, 47.7, 72.1, 70.8, 70.2, 74.8, 73.3, 66~
## $ `1983`  <dbl> 42.8, 72.1, 66.4, 78.3, 47.7, 72.6, 70.8, 70.1, 75.2, 73.5, 67~
## $ `1984`  <dbl> 40.5, 72.3, 67.6, 78.5, 47.8, 72.8, 71.2, 70.5, 75.4, 73.9, 66~
## $ `1985`  <dbl> 42.4, 72.4, 68.7, 78.6, 47.9, 73.0, 71.5, 70.5, 75.5, 74.2, 67~
## $ `1986`  <dbl> 43.4, 72.6, 69.5, 78.7, 47.9, 73.3, 71.8, 71.0, 75.8, 74.6, 67~
## $ `1987`  <dbl> 45.5, 72.6, 70.2, 78.8, 47.7, 73.7, 72.0, 71.2, 76.0, 75.0, 67~
## $ `1988`  <dbl> 47.9, 72.8, 70.8, 78.8, 47.5, 73.8, 72.1, 54.9, 76.3, 75.3, 67~
## $ `1989`  <dbl> 51.7, 73.1, 71.3, 78.9, 47.8, 74.0, 72.3, 70.4, 76.4, 75.6, 67~
## $ `1990`  <dbl> 52.6, 73.3, 71.7, 79.0, 47.9, 74.3, 72.4, 70.1, 76.9, 75.8, 67~
## $ `1991`  <dbl> 52.4, 73.0, 72.2, 79.1, 48.5, 74.5, 72.6, 69.7, 77.3, 75.9, 66~
## $ `1992`  <dbl> 52.9, 73.4, 72.5, 79.1, 48.6, 74.4, 72.6, 68.6, 77.4, 76.1, 64~
## $ `1993`  <dbl> 53.1, 73.9, 72.7, 79.3, 47.3, 74.3, 72.9, 68.9, 77.8, 76.2, 64~
## $ `1994`  <dbl> 52.7, 74.5, 72.8, 79.5, 48.7, 74.3, 73.3, 69.4, 77.9, 76.6, 63~
## $ `1995`  <dbl> 53.3, 74.6, 72.9, 79.7, 49.5, 74.1, 73.3, 69.9, 78.2, 76.8, 65~
## $ `1996`  <dbl> 53.8, 74.4, 73.3, 80.0, 50.1, 74.2, 73.3, 70.3, 78.3, 77.1, 65~
## $ `1997`  <dbl> 53.7, 72.5, 73.2, 80.3, 50.2, 74.4, 73.5, 71.0, 78.6, 77.6, 65~
## $ `1998`  <dbl> 52.8, 74.5, 73.5, 80.6, 49.3, 74.5, 73.5, 71.5, 78.9, 77.8, 65~
## $ `1999`  <dbl> 54.4, 74.6, 73.9, 80.9, 50.7, 74.6, 73.6, 71.9, 79.3, 78.0, 66~
## $ `2000`  <dbl> 54.6, 74.9, 74.0, 81.2, 51.7, 74.7, 74.2, 72.3, 79.5, 78.3, 66~
## $ `2001`  <dbl> 54.8, 75.2, 74.2, 81.4, 52.4, 74.9, 74.3, 72.6, 80.0, 78.7, 67~
## $ `2002`  <dbl> 55.6, 75.3, 74.5, 81.6, 53.5, 75.2, 74.2, 72.7, 80.2, 78.9, 67~
## $ `2003`  <dbl> 56.4, 75.4, 74.6, 81.7, 54.2, 75.4, 74.3, 72.9, 80.5, 79.0, 67~
## $ `2004`  <dbl> 56.9, 75.6, 75.0, 81.7, 54.8, 75.6, 74.9, 73.2, 80.8, 79.4, 67~
## $ `2005`  <dbl> 57.4, 76.0, 75.3, 81.8, 55.7, 75.8, 75.3, 73.4, 81.2, 79.6, 67~
## $ `2006`  <dbl> 57.6, 76.4, 75.5, 82.0, 56.4, 76.0, 75.4, 73.6, 81.4, 80.1, 68~
## $ `2007`  <dbl> 58.0, 76.9, 75.7, 82.1, 57.5, 76.3, 75.1, 73.9, 81.4, 80.2, 68~
## $ `2008`  <dbl> 58.8, 77.0, 75.9, 82.2, 58.4, 76.4, 75.5, 73.8, 81.5, 80.4, 68~
## $ `2009`  <dbl> 59.3, 77.5, 76.1, 82.2, 59.1, 76.6, 75.6, 73.9, 81.7, 80.3, 69~
## $ `2010`  <dbl> 59.9, 77.6, 76.3, 82.3, 59.9, 76.6, 75.8, 74.0, 82.0, 80.6, 69~
## $ `2011`  <dbl> 60.4, 77.7, 76.5, 82.4, 60.6, 76.7, 75.9, 74.3, 82.1, 80.8, 69~
## $ `2012`  <dbl> 60.8, 77.8, 76.8, 82.4, 61.3, 76.7, 76.0, 74.7, 82.5, 81.0, 69~
## $ `2013`  <dbl> 61.3, 77.9, 76.9, 82.5, 61.9, 76.8, 76.1, 75.2, 82.6, 81.2, 70~
## $ `2014`  <dbl> 61.2, 77.9, 77.0, 82.5, 62.8, 76.8, 76.4, 75.3, 82.5, 81.4, 70~
## $ `2015`  <dbl> 61.2, 78.0, 77.1, 82.6, 63.3, 76.9, 76.5, 75.3, 82.5, 81.5, 70~
## $ `2016`  <dbl> 61.2, 78.1, 77.4, 82.7, 63.8, 77.0, 76.5, 75.4, 82.5, 81.7, 70~
## $ `2017`  <dbl> 63.4, 78.2, 77.7, 82.7, 64.2, 77.0, 76.7, 75.6, 82.4, 81.8, 70~
## $ `2018`  <dbl> 63.7, 78.3, 77.9, NA, 64.6, 77.2, 76.8, 75.8, 82.5, 81.9, 70.9~
## $ `2019`  <dbl> 64.1, 78.5, 78.1, NA, 65.0, 77.3, 77.0, 75.9, 82.7, 82.0, 71.1~
## $ `2020`  <dbl> 64.4, 78.6, 78.3, NA, 65.4, 77.4, 77.1, 76.0, 82.8, 82.1, 71.2~
## $ `2021`  <dbl> 64.7, 78.7, 78.5, NA, 65.7, 77.5, 77.3, 76.2, 83.0, 82.3, 71.3~
## $ `2022`  <dbl> 65.1, 78.8, 78.7, NA, 66.0, 77.7, 77.4, 76.3, 83.1, 82.4, 71.4~
## $ `2023`  <dbl> 65.4, 78.9, 78.8, NA, 66.4, 77.8, 77.5, 76.5, 83.3, 82.6, 71.5~
## $ `2024`  <dbl> 65.7, 79.1, 79.0, NA, 66.7, 77.9, 77.7, 76.6, 83.4, 82.7, 71.5~
## $ `2025`  <dbl> 65.9, 79.2, 79.2, NA, 67.0, 78.1, 77.8, 76.7, 83.5, 82.9, 71.6~
## $ `2026`  <dbl> 66.2, 79.3, 79.4, NA, 67.3, 78.2, 78.0, 76.9, 83.6, 83.0, 71.8~
## $ `2027`  <dbl> 66.5, 79.5, 79.6, NA, 67.6, 78.3, 78.1, 77.0, 83.8, 83.2, 71.8~
## $ `2028`  <dbl> 66.7, 79.7, 79.7, NA, 67.9, 78.5, 78.3, 77.1, 83.9, 83.4, 72.0~
## $ `2029`  <dbl> 67.0, 79.8, 79.9, NA, 68.2, 78.6, 78.4, 77.3, 84.0, 83.5, 72.1~
## $ `2030`  <dbl> 67.2, 80.0, 80.1, NA, 68.5, 78.7, 78.6, 77.4, 84.1, 83.7, 72.2~
## $ `2031`  <dbl> 67.4, 80.2, 80.3, NA, 68.8, 78.8, 78.7, 77.5, 84.3, 83.9, 72.3~
## $ `2032`  <dbl> 67.7, 80.3, 80.4, NA, 69.1, 79.0, 78.9, 77.7, 84.4, 84.0, 72.4~
## $ `2033`  <dbl> 67.9, 80.5, 80.6, NA, 69.3, 79.1, 79.0, 77.8, 84.5, 84.2, 72.5~
## $ `2034`  <dbl> 68.1, 80.6, 80.8, NA, 69.6, 79.2, 79.2, 78.0, 84.6, 84.3, 72.6~
## $ `2035`  <dbl> 68.3, 80.8, 80.9, NA, 69.9, 79.4, 79.3, 78.1, 84.7, 84.5, 72.7~
## $ `2036`  <dbl> 68.5, 80.9, 81.1, NA, 70.1, 79.5, 79.5, 78.2, 84.8, 84.6, 72.8~
## $ `2037`  <dbl> 68.7, 81.0, 81.2, NA, 70.4, 79.6, 79.6, 78.4, 85.0, 84.7, 73.0~
## $ `2038`  <dbl> 68.9, 81.2, 81.4, NA, 70.6, 79.7, 79.8, 78.5, 85.1, 84.8, 73.1~
## $ `2039`  <dbl> 69.0, 81.3, 81.6, NA, 70.8, 79.9, 79.9, 78.6, 85.2, 85.0, 73.2~
## $ `2040`  <dbl> 69.2, 81.4, 81.7, NA, 71.1, 80.0, 80.1, 78.8, 85.3, 85.1, 73.3~
## $ `2041`  <dbl> 69.4, 81.6, 81.9, NA, 71.3, 80.1, 80.2, 78.9, 85.4, 85.2, 73.4~
## $ `2042`  <dbl> 69.6, 81.7, 82.0, NA, 71.5, 80.3, 80.4, 79.0, 85.5, 85.3, 73.5~
## $ `2043`  <dbl> 69.7, 81.8, 82.2, NA, 71.7, 80.4, 80.5, 79.2, 85.7, 85.4, 73.7~
## $ `2044`  <dbl> 69.9, 82.0, 82.3, NA, 71.9, 80.5, 80.7, 79.3, 85.8, 85.6, 73.8~
## $ `2045`  <dbl> 70.1, 82.1, 82.5, NA, 72.1, 80.6, 80.8, 79.4, 85.9, 85.7, 73.9~
## $ `2046`  <dbl> 70.2, 82.3, 82.7, NA, 72.3, 80.8, 81.0, 79.6, 86.0, 85.8, 74.0~
## $ `2047`  <dbl> 70.4, 82.4, 82.8, NA, 72.5, 80.9, 81.1, 79.7, 86.1, 85.9, 74.1~
## $ `2048`  <dbl> 70.5, 82.5, 83.0, NA, 72.7, 81.0, 81.3, 79.8, 86.2, 86.0, 74.2~
## $ `2049`  <dbl> 70.7, 82.7, 83.1, NA, 72.8, 81.1, 81.4, 80.0, 86.3, 86.1, 74.3~
## $ `2050`  <dbl> 70.8, 82.8, 83.3, NA, 73.0, 81.3, 81.5, 80.1, 86.4, 86.3, 74.5~
## $ `2051`  <dbl> 71.0, 83.0, 83.4, NA, 73.2, 81.4, 81.7, 80.2, 86.6, 86.4, 74.6~
## $ `2052`  <dbl> 71.1, 83.1, 83.6, NA, 73.3, 81.5, 81.8, 80.4, 86.7, 86.5, 74.7~
## $ `2053`  <dbl> 71.3, 83.2, 83.7, NA, 73.5, 81.7, 82.0, 80.5, 86.8, 86.6, 74.8~
## $ `2054`  <dbl> 71.4, 83.4, 83.8, NA, 73.7, 81.8, 82.1, 80.6, 86.9, 86.7, 74.9~
## $ `2055`  <dbl> 71.6, 83.5, 84.0, NA, 73.8, 81.9, 82.3, 80.8, 87.0, 86.8, 75.0~
## $ `2056`  <dbl> 71.7, 83.6, 84.1, NA, 74.0, 82.0, 82.4, 80.9, 87.1, 86.9, 75.2~
## $ `2057`  <dbl> 71.8, 83.7, 84.3, NA, 74.2, 82.2, 82.6, 81.0, 87.2, 87.0, 75.3~
## $ `2058`  <dbl> 72.0, 83.9, 84.4, NA, 74.3, 82.3, 82.7, 81.2, 87.3, 87.2, 75.4~
## $ `2059`  <dbl> 72.1, 84.0, 84.6, NA, 74.5, 82.4, 82.8, 81.3, 87.5, 87.3, 75.5~
## $ `2060`  <dbl> 72.3, 84.1, 84.7, NA, 74.6, 82.5, 83.0, 81.5, 87.6, 87.4, 75.7~
## $ `2061`  <dbl> 72.4, 84.2, 84.8, NA, 74.8, 82.7, 83.1, 81.6, 87.7, 87.5, 75.8~
## $ `2062`  <dbl> 72.5, 84.3, 85.0, NA, 74.9, 82.8, 83.3, 81.7, 87.8, 87.6, 75.9~
## $ `2063`  <dbl> 72.7, 84.5, 85.1, NA, 75.0, 82.9, 83.4, 81.9, 87.9, 87.7, 76.0~
## $ `2064`  <dbl> 72.8, 84.6, 85.2, NA, 75.2, 83.0, 83.5, 82.0, 88.0, 87.8, 76.1~
## $ `2065`  <dbl> 72.9, 84.7, 85.3, NA, 75.3, 83.2, 83.7, 82.1, 88.1, 87.9, 76.3~
## $ `2066`  <dbl> 73.1, 84.8, 85.5, NA, 75.5, 83.3, 83.8, 82.3, 88.3, 88.0, 76.4~
## $ `2067`  <dbl> 73.2, 84.9, 85.6, NA, 75.6, 83.4, 83.9, 82.4, 88.4, 88.2, 76.5~
## $ `2068`  <dbl> 73.3, 85.0, 85.7, NA, 75.7, 83.5, 84.0, 82.5, 88.5, 88.3, 76.6~
## $ `2069`  <dbl> 73.5, 85.1, 85.8, NA, 75.9, 83.7, 84.2, 82.7, 88.6, 88.4, 76.8~
## $ `2070`  <dbl> 73.6, 85.2, 86.0, NA, 76.0, 83.8, 84.3, 82.8, 88.7, 88.5, 76.9~
## $ `2071`  <dbl> 73.7, 85.3, 86.1, NA, 76.2, 83.9, 84.4, 82.9, 88.8, 88.6, 77.0~
## $ `2072`  <dbl> 73.9, 85.4, 86.2, NA, 76.3, 84.0, 84.5, 83.0, 88.9, 88.7, 77.1~
## $ `2073`  <dbl> 74.0, 85.5, 86.3, NA, 76.4, 84.1, 84.6, 83.2, 89.0, 88.8, 77.3~
## $ `2074`  <dbl> 74.2, 85.7, 86.4, NA, 76.5, 84.3, 84.8, 83.3, 89.2, 88.9, 77.4~
## $ `2075`  <dbl> 74.3, 85.8, 86.5, NA, 76.7, 84.4, 84.9, 83.4, 89.3, 89.0, 77.5~
## $ `2076`  <dbl> 74.4, 85.8, 86.7, NA, 76.8, 84.5, 85.0, 83.6, 89.4, 89.2, 77.7~
## $ `2077`  <dbl> 74.6, 86.0, 86.8, NA, 77.0, 84.6, 85.1, 83.7, 89.5, 89.3, 77.8~
## $ `2078`  <dbl> 74.7, 86.0, 86.9, NA, 77.1, 84.7, 85.2, 83.8, 89.6, 89.4, 77.9~
## $ `2079`  <dbl> 74.8, 86.2, 87.0, NA, 77.2, 84.8, 85.3, 84.0, 89.7, 89.5, 78.0~
## $ `2080`  <dbl> 75.0, 86.3, 87.1, NA, 77.3, 84.9, 85.4, 84.1, 89.8, 89.6, 78.2~
## $ `2081`  <dbl> 75.1, 86.4, 87.2, NA, 77.5, 85.0, 85.5, 84.2, 89.9, 89.7, 78.3~
## $ `2082`  <dbl> 75.2, 86.5, 87.3, NA, 77.6, 85.2, 85.7, 84.3, 90.0, 89.8, 78.5~
## $ `2083`  <dbl> 75.4, 86.6, 87.4, NA, 77.7, 85.3, 85.8, 84.4, 90.2, 89.9, 78.6~
## $ `2084`  <dbl> 75.5, 86.7, 87.5, NA, 77.8, 85.4, 85.9, 84.6, 90.3, 90.0, 78.7~
## $ `2085`  <dbl> 75.6, 86.8, 87.6, NA, 78.0, 85.5, 86.0, 84.7, 90.4, 90.2, 78.9~
## $ `2086`  <dbl> 75.8, 86.9, 87.7, NA, 78.1, 85.6, 86.1, 84.8, 90.5, 90.3, 79.0~
## $ `2087`  <dbl> 75.9, 87.0, 87.8, NA, 78.2, 85.7, 86.2, 84.9, 90.6, 90.4, 79.1~
## $ `2088`  <dbl> 76.0, 87.1, 87.9, NA, 78.4, 85.8, 86.3, 85.0, 90.7, 90.5, 79.3~
## $ `2089`  <dbl> 76.2, 87.2, 88.0, NA, 78.5, 85.9, 86.4, 85.2, 90.8, 90.6, 79.4~
## $ `2090`  <dbl> 76.3, 87.3, 88.2, NA, 78.6, 86.0, 86.5, 85.3, 90.9, 90.7, 79.5~
## $ `2091`  <dbl> 76.5, 87.4, 88.3, NA, 78.7, 86.1, 86.7, 85.4, 91.0, 90.8, 79.7~
## $ `2092`  <dbl> 76.6, 87.5, 88.4, NA, 78.9, 86.2, 86.8, 85.5, 91.2, 90.9, 79.8~
## $ `2093`  <dbl> 76.7, 87.6, 88.5, NA, 79.0, 86.3, 86.9, 85.6, 91.3, 91.0, 80.0~
## $ `2094`  <dbl> 76.9, 87.7, 88.6, NA, 79.1, 86.4, 87.0, 85.7, 91.4, 91.1, 80.1~
## $ `2095`  <dbl> 77.0, 87.8, 88.7, NA, 79.3, 86.5, 87.1, 85.8, 91.5, 91.2, 80.2~
## $ `2096`  <dbl> 77.1, 87.9, 88.8, NA, 79.4, 86.6, 87.2, 85.9, 91.6, 91.3, 80.3~
## $ `2097`  <dbl> 77.3, 88.0, 88.9, NA, 79.5, 86.7, 87.3, 86.0, 91.7, 91.5, 80.5~
## $ `2098`  <dbl> 77.4, 88.1, 89.0, NA, 79.7, 86.8, 87.4, 86.2, 91.8, 91.6, 80.6~
## $ `2099`  <dbl> 77.5, 88.2, 89.1, NA, 79.8, 86.9, 87.5, 86.3, 91.9, 91.7, 80.7~
## $ `2100`  <dbl> 77.7, 88.3, 89.2, NA, 79.9, 87.0, 87.6, 86.4, 92.0, 91.8, 80.9~
```

```r
summary(life_expectancy)
```

```
##    country               1800            1801            1802      
##  Length:187         Min.   :23.40   Min.   :23.40   Min.   :23.40  
##  Class :character   1st Qu.:29.07   1st Qu.:28.98   1st Qu.:28.90  
##  Mode  :character   Median :31.75   Median :31.65   Median :31.55  
##                     Mean   :31.50   Mean   :31.46   Mean   :31.48  
##                     3rd Qu.:33.83   3rd Qu.:33.90   3rd Qu.:33.83  
##                     Max.   :42.90   Max.   :40.30   Max.   :44.40  
##                     NA's   :3       NA's   :3       NA's   :3      
##       1803            1804            1805            1806      
##  Min.   :19.60   Min.   :23.40   Min.   :23.40   Min.   :23.40  
##  1st Qu.:28.90   1st Qu.:28.98   1st Qu.:29.07   1st Qu.:29.07  
##  Median :31.50   Median :31.55   Median :31.65   Median :31.75  
##  Mean   :31.38   Mean   :31.46   Mean   :31.59   Mean   :31.64  
##  3rd Qu.:33.62   3rd Qu.:33.73   3rd Qu.:33.83   3rd Qu.:33.92  
##  Max.   :44.80   Max.   :42.80   Max.   :44.30   Max.   :45.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1807            1808            1809            1810      
##  Min.   :23.40   Min.   :12.50   Min.   :13.40   Min.   :23.40  
##  1st Qu.:29.07   1st Qu.:28.98   1st Qu.:28.88   1st Qu.:29.07  
##  Median :31.75   Median :31.55   Median :31.50   Median :31.75  
##  Mean   :31.60   Mean   :31.38   Mean   :31.31   Mean   :31.54  
##  3rd Qu.:33.92   3rd Qu.:33.73   3rd Qu.:33.62   3rd Qu.:33.83  
##  Max.   :43.60   Max.   :43.50   Max.   :41.70   Max.   :43.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1811            1812            1813            1814      
##  Min.   :23.40   Min.   :23.00   Min.   :23.40   Min.   :23.40  
##  1st Qu.:29.07   1st Qu.:29.07   1st Qu.:29.07   1st Qu.:29.07  
##  Median :31.65   Median :31.70   Median :31.65   Median :31.65  
##  Mean   :31.50   Mean   :31.49   Mean   :31.48   Mean   :31.54  
##  3rd Qu.:33.83   3rd Qu.:33.83   3rd Qu.:33.73   3rd Qu.:33.92  
##  Max.   :40.10   Max.   :43.50   Max.   :43.00   Max.   :41.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1815            1816            1817            1818      
##  Min.   :23.40   Min.   :23.40   Min.   :23.40   Min.   : 5.50  
##  1st Qu.:29.07   1st Qu.:29.07   1st Qu.:29.07   1st Qu.:29.07  
##  Median :31.75   Median :31.65   Median :31.75   Median :31.75  
##  Mean   :31.68   Mean   :31.66   Mean   :31.76   Mean   :31.60  
##  3rd Qu.:34.00   3rd Qu.:33.92   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :45.60   Max.   :46.30   Max.   :48.90   Max.   :46.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1819            1820            1821            1822      
##  Min.   : 1.50   Min.   : 6.50   Min.   :23.40   Min.   :23.40  
##  1st Qu.:29.07   1st Qu.:29.15   1st Qu.:29.15   1st Qu.:29.15  
##  Median :31.75   Median :31.75   Median :31.65   Median :31.80  
##  Mean   :31.50   Mean   :31.58   Mean   :31.65   Mean   :31.76  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:33.92   3rd Qu.:34.00  
##  Max.   :45.80   Max.   :47.00   Max.   :44.70   Max.   :48.40  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1823            1824            1825            1826      
##  Min.   :23.40   Min.   :23.40   Min.   :23.40   Min.   :23.40  
##  1st Qu.:29.15   1st Qu.:29.15   1st Qu.:29.15   1st Qu.:28.98  
##  Median :31.80   Median :31.80   Median :31.80   Median :31.75  
##  Mean   :31.81   Mean   :31.75   Mean   :31.70   Mean   :31.62  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :48.80   Max.   :47.60   Max.   :49.20   Max.   :47.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1827            1828            1829            1830      
##  Min.   :23.40   Min.   :23.40   Min.   :23.40   Min.   :23.40  
##  1st Qu.:28.98   1st Qu.:28.98   1st Qu.:29.15   1st Qu.:29.15  
##  Median :31.75   Median :31.75   Median :31.75   Median :31.85  
##  Mean   :31.66   Mean   :31.60   Mean   :31.58   Mean   :31.67  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :48.40   Max.   :46.20   Max.   :46.30   Max.   :45.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1831            1832            1833            1834      
##  Min.   :23.40   Min.   :23.00   Min.   :20.40   Min.   :23.40  
##  1st Qu.:29.15   1st Qu.:29.15   1st Qu.:29.15   1st Qu.:29.15  
##  Median :31.80   Median :31.75   Median :31.80   Median :31.80  
##  Mean   :31.63   Mean   :31.59   Mean   :31.59   Mean   :31.57  
##  3rd Qu.:34.00   3rd Qu.:33.92   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :45.70   Max.   :47.60   Max.   :44.90   Max.   :42.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1835            1836            1837            1838      
##  Min.   :23.40   Min.   :23.40   Min.   :23.40   Min.   :23.40  
##  1st Qu.:28.98   1st Qu.:29.15   1st Qu.:28.98   1st Qu.:29.15  
##  Median :31.80   Median :31.65   Median :31.80   Median :31.85  
##  Mean   :31.71   Mean   :31.67   Mean   :31.61   Mean   :31.67  
##  3rd Qu.:34.00   3rd Qu.:33.92   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :47.10   Max.   :46.50   Max.   :44.20   Max.   :43.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1839            1840            1841            1842      
##  Min.   :23.40   Min.   :23.40   Min.   :23.00   Min.   :22.40  
##  1st Qu.:28.98   1st Qu.:29.20   1st Qu.:29.15   1st Qu.:29.15  
##  Median :31.80   Median :31.90   Median :31.85   Median :31.85  
##  Mean   :31.66   Mean   :31.75   Mean   :31.80   Mean   :31.77  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.05   3rd Qu.:34.05  
##  Max.   :43.00   Max.   :45.60   Max.   :49.50   Max.   :48.40  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1843            1844            1845            1846      
##  Min.   :23.40   Min.   :15.00   Min.   :23.40   Min.   :18.30  
##  1st Qu.:28.98   1st Qu.:29.15   1st Qu.:29.15   1st Qu.:28.98  
##  Median :31.80   Median :31.85   Median :31.85   Median :31.75  
##  Mean   :31.76   Mean   :31.82   Mean   :31.88   Mean   :31.57  
##  3rd Qu.:34.00   3rd Qu.:34.05   3rd Qu.:34.05   3rd Qu.:34.00  
##  Max.   :48.40   Max.   :49.70   Max.   :50.10   Max.   :48.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1847            1848            1849            1850      
##  Min.   :23.40   Min.   :14.90   Min.   :14.10   Min.   :14.00  
##  1st Qu.:29.15   1st Qu.:28.98   1st Qu.:29.15   1st Qu.:29.20  
##  Median :31.85   Median :31.85   Median :31.80   Median :31.80  
##  Mean   :31.65   Mean   :31.64   Mean   :31.48   Mean   :31.67  
##  3rd Qu.:34.00   3rd Qu.:34.05   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :44.80   Max.   :45.10   Max.   :48.00   Max.   :49.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1851            1852            1853            1854      
##  Min.   :22.00   Min.   :23.40   Min.   :23.40   Min.   : 7.50  
##  1st Qu.:29.20   1st Qu.:29.20   1st Qu.:29.15   1st Qu.:28.90  
##  Median :31.80   Median :31.85   Median :31.85   Median :31.80  
##  Mean   :31.82   Mean   :31.83   Mean   :31.83   Mean   :31.70  
##  3rd Qu.:34.05   3rd Qu.:34.05   3rd Qu.:34.00   3rd Qu.:34.05  
##  Max.   :49.70   Max.   :48.50   Max.   :48.60   Max.   :51.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1855            1856            1857            1858      
##  Min.   :23.40   Min.   :17.60   Min.   :23.40   Min.   :23.40  
##  1st Qu.:28.98   1st Qu.:28.98   1st Qu.:29.15   1st Qu.:29.15  
##  Median :31.80   Median :31.75   Median :31.80   Median :31.85  
##  Mean   :31.74   Mean   :31.75   Mean   :31.76   Mean   :31.76  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.05   3rd Qu.:34.05  
##  Max.   :50.40   Max.   :50.40   Max.   :50.20   Max.   :51.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1859            1860            1861            1862      
##  Min.   :23.40   Min.   :19.80   Min.   :22.00   Min.   :22.70  
##  1st Qu.:29.15   1st Qu.:28.98   1st Qu.:29.15   1st Qu.:28.98  
##  Median :31.70   Median :31.75   Median :31.75   Median :31.75  
##  Mean   :31.73   Mean   :31.81   Mean   :31.77   Mean   :31.70  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00  
##  Max.   :49.90   Max.   :50.00   Max.   :47.60   Max.   :47.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1863            1864            1865            1866      
##  Min.   :23.40   Min.   :23.20   Min.   :22.30   Min.   :21.00  
##  1st Qu.:29.15   1st Qu.:28.90   1st Qu.:28.90   1st Qu.:28.88  
##  Median :31.80   Median :31.70   Median :31.70   Median :31.65  
##  Mean   :31.76   Mean   :31.69   Mean   :31.65   Mean   :31.52  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:33.73  
##  Max.   :47.60   Max.   :48.80   Max.   :50.40   Max.   :49.90  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1867            1868            1869            1870      
##  Min.   : 4.00   Min.   : 8.11   Min.   :15.00   Min.   :19.90  
##  1st Qu.:28.90   1st Qu.:28.88   1st Qu.:28.88   1st Qu.:29.15  
##  Median :31.70   Median :31.55   Median :31.55   Median :31.75  
##  Mean   :31.53   Mean   :31.25   Mean   :31.57   Mean   :31.82  
##  3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.00   3rd Qu.:34.25  
##  Max.   :47.90   Max.   :47.20   Max.   :49.30   Max.   :50.90  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1871            1872            1873            1874      
##  Min.   :20.00   Min.   :20.00   Min.   :20.10   Min.   :20.30  
##  1st Qu.:29.15   1st Qu.:29.00   1st Qu.:29.20   1st Qu.:29.48  
##  Median :31.65   Median :31.70   Median :31.70   Median :31.80  
##  Mean   :31.76   Mean   :31.85   Mean   :31.95   Mean   :32.01  
##  3rd Qu.:34.25   3rd Qu.:34.60   3rd Qu.:34.70   3rd Qu.:34.70  
##  Max.   :49.70   Max.   :50.10   Max.   :49.70   Max.   :47.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1875            1876            1877            1878      
##  Min.   : 1.01   Min.   :20.00   Min.   :19.00   Min.   :20.00  
##  1st Qu.:29.48   1st Qu.:29.50   1st Qu.:29.50   1st Qu.:29.50  
##  Median :31.75   Median :31.80   Median :31.85   Median :31.90  
##  Mean   :31.90   Mean   :32.11   Mean   :32.20   Mean   :32.21  
##  3rd Qu.:35.02   3rd Qu.:34.85   3rd Qu.:34.92   3rd Qu.:35.00  
##  Max.   :47.60   Max.   :46.80   Max.   :49.80   Max.   :51.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1879            1880            1881            1882      
##  Min.   :21.20   Min.   :21.40   Min.   :21.60   Min.   :17.70  
##  1st Qu.:29.60   1st Qu.:29.68   1st Qu.:29.75   1st Qu.:29.50  
##  Median :31.95   Median :32.00   Median :32.05   Median :32.00  
##  Mean   :32.34   Mean   :32.35   Mean   :32.41   Mean   :32.37  
##  3rd Qu.:35.02   3rd Qu.:35.02   3rd Qu.:35.02   3rd Qu.:35.12  
##  Max.   :53.20   Max.   :51.90   Max.   :50.50   Max.   :48.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1883            1884            1885            1886      
##  Min.   :22.00   Min.   :22.20   Min.   :22.40   Min.   :22.60  
##  1st Qu.:29.70   1st Qu.:29.95   1st Qu.:29.80   1st Qu.:30.05  
##  Median :32.00   Median :32.05   Median :32.10   Median :32.15  
##  Mean   :32.53   Mean   :32.69   Mean   :32.76   Mean   :32.83  
##  3rd Qu.:35.20   3rd Qu.:35.23   3rd Qu.:35.33   3rd Qu.:35.33  
##  Max.   :49.60   Max.   :50.80   Max.   :51.00   Max.   :51.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1887            1888            1889            1890      
##  Min.   :22.80   Min.   :17.00   Min.   : 5.00   Min.   : 4.00  
##  1st Qu.:29.88   1st Qu.:30.12   1st Qu.:29.95   1st Qu.:29.90  
##  Median :32.10   Median :32.20   Median :32.15   Median :32.15  
##  Mean   :32.89   Mean   :32.94   Mean   :32.93   Mean   :32.71  
##  3rd Qu.:35.42   3rd Qu.:35.42   3rd Qu.:35.50   3rd Qu.:35.42  
##  Max.   :51.70   Max.   :52.30   Max.   :52.50   Max.   :50.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1891            1892            1893            1894      
##  Min.   : 8.00   Min.   :14.00   Min.   : 8.08   Min.   :21.80  
##  1st Qu.:30.10   1st Qu.:30.05   1st Qu.:29.98   1st Qu.:30.00  
##  Median :32.20   Median :32.20   Median :32.20   Median :32.35  
##  Mean   :32.92   Mean   :32.94   Mean   :33.01   Mean   :33.25  
##  3rd Qu.:35.52   3rd Qu.:35.42   3rd Qu.:35.42   3rd Qu.:35.45  
##  Max.   :51.10   Max.   :52.70   Max.   :52.60   Max.   :52.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1895            1896            1897            1898      
##  Min.   :21.60   Min.   :19.60   Min.   :18.60   Min.   :19.90  
##  1st Qu.:29.98   1st Qu.:30.00   1st Qu.:30.07   1st Qu.:30.05  
##  Median :32.30   Median :32.35   Median :32.40   Median :32.45  
##  Mean   :33.34   Mean   :33.44   Mean   :33.52   Mean   :33.57  
##  3rd Qu.:35.60   3rd Qu.:35.60   3rd Qu.:35.60   3rd Qu.:35.60  
##  Max.   :54.10   Max.   :53.80   Max.   :54.10   Max.   :54.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1899            1900            1901            1902      
##  Min.   :19.10   Min.   :18.40   Min.   :21.20   Min.   :12.90  
##  1st Qu.:30.05   1st Qu.:30.20   1st Qu.:30.15   1st Qu.:30.05  
##  Median :32.50   Median :32.55   Median :32.65   Median :32.65  
##  Mean   :33.55   Mean   :33.61   Mean   :33.80   Mean   :33.90  
##  3rd Qu.:35.62   3rd Qu.:35.62   3rd Qu.:35.62   3rd Qu.:35.70  
##  Max.   :51.60   Max.   :53.40   Max.   :54.50   Max.   :56.40  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1903            1904            1905            1906      
##  Min.   :20.30   Min.   : 5.19   Min.   :10.40   Min.   :20.30  
##  1st Qu.:30.10   1st Qu.:30.18   1st Qu.:30.27   1st Qu.:30.10  
##  Median :32.75   Median :32.80   Median :32.75   Median :32.65  
##  Mean   :33.95   Mean   :33.99   Mean   :34.04   Mean   :34.18  
##  3rd Qu.:35.73   3rd Qu.:35.73   3rd Qu.:35.80   3rd Qu.:35.73  
##  Max.   :55.10   Max.   :56.00   Max.   :55.00   Max.   :56.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1907            1908            1909            1910      
##  Min.   :18.10   Min.   :21.20   Min.   :21.40   Min.   :21.50  
##  1st Qu.:30.18   1st Qu.:30.40   1st Qu.:30.68   1st Qu.:30.68  
##  Median :32.75   Median :32.90   Median :32.85   Median :32.90  
##  Mean   :34.30   Mean   :34.42   Mean   :34.68   Mean   :34.83  
##  3rd Qu.:35.73   3rd Qu.:35.83   3rd Qu.:35.83   3rd Qu.:35.83  
##  Max.   :57.00   Max.   :56.40   Max.   :58.40   Max.   :58.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1911            1912            1913            1914      
##  Min.   :21.70   Min.   :21.80   Min.   :22.00   Min.   :22.10  
##  1st Qu.:30.77   1st Qu.:30.70   1st Qu.:30.70   1st Qu.:30.57  
##  Median :33.05   Median :33.15   Median :32.95   Median :32.90  
##  Mean   :34.96   Mean   :35.19   Mean   :35.22   Mean   :34.96  
##  3rd Qu.:36.02   3rd Qu.:36.10   3rd Qu.:36.12   3rd Qu.:36.12  
##  Max.   :58.00   Max.   :58.00   Max.   :58.90   Max.   :58.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1915            1916            1917            1918            1919     
##  Min.   : 7.21   Min.   :19.60   Min.   :20.10   Min.   : 1.10   Min.   :11.8  
##  1st Qu.:30.60   1st Qu.:30.60   1st Qu.:30.40   1st Qu.:13.20   1st Qu.:30.6  
##  Median :33.00   Median :33.00   Median :32.90   Median :22.00   Median :33.1  
##  Mean   :34.59   Mean   :34.71   Mean   :34.55   Mean   :22.95   Mean   :34.7  
##  3rd Qu.:35.92   3rd Qu.:35.92   3rd Qu.:35.73   3rd Qu.:29.18   3rd Qu.:35.9  
##  Max.   :58.40   Max.   :58.50   Max.   :59.00   Max.   :56.20   Max.   :60.1  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3       NA's   :3     
##       1920            1921            1922            1923      
##  Min.   :15.20   Min.   :11.90   Min.   :13.90   Min.   :23.40  
##  1st Qu.:30.48   1st Qu.:30.57   1st Qu.:30.75   1st Qu.:31.20  
##  Median :32.90   Median :33.05   Median :33.55   Median :33.70  
##  Mean   :34.96   Mean   :35.44   Mean   :35.77   Mean   :36.52  
##  3rd Qu.:35.90   3rd Qu.:36.33   3rd Qu.:36.73   3rd Qu.:37.92  
##  Max.   :60.60   Max.   :61.70   Max.   :63.00   Max.   :63.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1924            1925            1926            1927      
##  Min.   :23.60   Min.   :23.60   Min.   :23.60   Min.   :23.60  
##  1st Qu.:31.27   1st Qu.:31.27   1st Qu.:31.40   1st Qu.:31.45  
##  Median :33.90   Median :34.05   Median :34.25   Median :34.35  
##  Mean   :36.81   Mean   :36.99   Mean   :37.38   Mean   :37.54  
##  3rd Qu.:38.62   3rd Qu.:39.30   3rd Qu.:40.02   3rd Qu.:40.65  
##  Max.   :63.00   Max.   :63.30   Max.   :63.10   Max.   :63.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1928            1929            1930            1931      
##  Min.   :23.60   Min.   :23.60   Min.   :23.70   Min.   :16.30  
##  1st Qu.:31.57   1st Qu.:31.60   1st Qu.:31.90   1st Qu.:31.85  
##  Median :34.40   Median :34.50   Median :34.65   Median :34.50  
##  Mean   :37.86   Mean   :37.88   Mean   :38.37   Mean   :38.35  
##  3rd Qu.:41.27   3rd Qu.:41.70   3rd Qu.:42.42   3rd Qu.:43.12  
##  Max.   :63.80   Max.   :63.30   Max.   :65.10   Max.   :65.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1932            1933            1934            1935      
##  Min.   : 8.15   Min.   : 4.07   Min.   :23.70   Min.   :23.70  
##  1st Qu.:31.85   1st Qu.:31.68   1st Qu.:32.40   1st Qu.:32.48  
##  Median :34.60   Median :34.70   Median :35.75   Median :36.35  
##  Mean   :38.34   Mean   :38.13   Mean   :39.67   Mean   :40.07  
##  3rd Qu.:43.55   3rd Qu.:44.10   3rd Qu.:44.90   3rd Qu.:46.12  
##  Max.   :65.80   Max.   :66.20   Max.   :66.70   Max.   :66.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1936            1937            1938            1939      
##  Min.   :23.00   Min.   :23.70   Min.   :23.70   Min.   :23.70  
##  1st Qu.:33.08   1st Qu.:33.30   1st Qu.:33.45   1st Qu.:33.73  
##  Median :36.75   Median :36.80   Median :37.25   Median :38.10  
##  Mean   :40.56   Mean   :40.88   Mean   :41.40   Mean   :41.87  
##  3rd Qu.:47.23   3rd Qu.:47.75   3rd Qu.:48.55   3rd Qu.:49.33  
##  Max.   :66.80   Max.   :67.10   Max.   :67.50   Max.   :67.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1940            1941            1942            1943      
##  Min.   :23.70   Min.   :12.00   Min.   :14.90   Min.   :13.90  
##  1st Qu.:33.75   1st Qu.:32.90   1st Qu.:32.50   1st Qu.:32.48  
##  Median :38.40   Median :36.70   Median :36.60   Median :37.15  
##  Mean   :41.82   Mean   :40.34   Mean   :40.14   Mean   :39.91  
##  3rd Qu.:49.35   3rd Qu.:47.02   3rd Qu.:48.15   3rd Qu.:47.90  
##  Max.   :66.70   Max.   :67.00   Max.   :68.90   Max.   :68.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1944            1945            1946            1947      
##  Min.   :15.40   Min.   :15.90   Min.   :22.30   Min.   :11.10  
##  1st Qu.:32.60   1st Qu.:33.90   1st Qu.:35.77   1st Qu.:37.00  
##  Median :36.45   Median :38.90   Median :44.50   Median :43.40  
##  Mean   :39.72   Mean   :41.55   Mean   :45.14   Mean   :45.67  
##  3rd Qu.:46.45   3rd Qu.:47.17   3rd Qu.:53.70   3rd Qu.:54.23  
##  Max.   :68.20   Max.   :68.60   Max.   :69.50   Max.   :69.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1948            1949            1950            1951      
##  Min.   :23.70   Min.   :23.80   Min.   :23.80   Min.   :24.20  
##  1st Qu.:38.27   1st Qu.:39.42   1st Qu.:40.77   1st Qu.:40.92  
##  Median :47.35   Median :48.75   Median :50.00   Median :50.25  
##  Mean   :47.83   Mean   :48.96   Mean   :50.08   Mean   :50.29  
##  3rd Qu.:56.85   3rd Qu.:58.00   3rd Qu.:58.95   3rd Qu.:59.48  
##  Max.   :71.20   Max.   :71.40   Max.   :71.60   Max.   :72.30  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1952            1953            1954            1955      
##  Min.   :25.20   Min.   :26.20   Min.   :27.10   Min.   :28.10  
##  1st Qu.:41.17   1st Qu.:41.83   1st Qu.:42.38   1st Qu.:43.00  
##  Median :50.70   Median :51.30   Median :52.05   Median :52.65  
##  Mean   :50.90   Mean   :51.54   Mean   :52.22   Mean   :52.80  
##  3rd Qu.:59.92   3rd Qu.:60.98   3rd Qu.:61.35   3rd Qu.:61.92  
##  Max.   :72.50   Max.   :73.00   Max.   :73.30   Max.   :73.30  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1956            1957            1958            1959      
##  Min.   :29.10   Min.   :30.10   Min.   :31.00   Min.   :32.00  
##  1st Qu.:43.60   1st Qu.:44.30   1st Qu.:45.08   1st Qu.:45.42  
##  Median :53.80   Median :54.45   Median :55.00   Median :55.45  
##  Mean   :53.33   Mean   :53.80   Mean   :54.42   Mean   :54.86  
##  3rd Qu.:62.45   3rd Qu.:63.15   3rd Qu.:63.90   3rd Qu.:64.12  
##  Max.   :73.30   Max.   :73.40   Max.   :73.40   Max.   :73.30  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1960            1961            1962            1963      
##  Min.   :31.60   Min.   :33.90   Min.   :34.50   Min.   :34.90  
##  1st Qu.:45.98   1st Qu.:46.40   1st Qu.:46.75   1st Qu.:47.27  
##  Median :55.90   Median :56.35   Median :56.80   Median :57.45  
##  Mean   :55.41   Mean   :55.93   Mean   :56.40   Mean   :56.92  
##  3rd Qu.:64.62   3rd Qu.:65.00   3rd Qu.:65.33   3rd Qu.:65.65  
##  Max.   :74.00   Max.   :73.70   Max.   :73.60   Max.   :73.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1964            1965            1966            1967      
##  Min.   :35.30   Min.   :35.80   Min.   :36.50   Min.   :37.20  
##  1st Qu.:47.67   1st Qu.:48.17   1st Qu.:48.60   1st Qu.:49.50  
##  Median :58.15   Median :58.90   Median :59.95   Median :60.90  
##  Mean   :57.46   Mean   :57.86   Mean   :58.32   Mean   :58.77  
##  3rd Qu.:66.05   3rd Qu.:66.50   3rd Qu.:66.92   3rd Qu.:67.42  
##  Max.   :73.90   Max.   :73.80   Max.   :74.10   Max.   :74.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       1968            1969            1970            1971      
##  Min.   :38.00   Min.   :36.90   Min.   :39.70   Min.   :39.90  
##  1st Qu.:50.23   1st Qu.:50.58   1st Qu.:51.65   1st Qu.:52.20  
##  Median :61.40   Median :61.70   Median :62.30   Median :63.00  
##  Mean   :59.14   Mean   :59.51   Mean   :60.16   Mean   :60.59  
##  3rd Qu.:67.92   3rd Qu.:68.25   3rd Qu.:68.80   3rd Qu.:68.95  
##  Max.   :74.00   Max.   :74.10   Max.   :75.50   Max.   :75.80  
##  NA's   :3       NA's   :3                                      
##       1972            1973            1974            1975      
##  Min.   :18.50   Min.   :40.50   Min.   :38.90   Min.   :24.90  
##  1st Qu.:52.45   1st Qu.:53.25   1st Qu.:53.70   1st Qu.:54.30  
##  Median :63.40   Median :63.50   Median :63.90   Median :64.50  
##  Mean   :60.82   Mean   :61.22   Mean   :61.62   Mean   :61.95  
##  3rd Qu.:69.30   3rd Qu.:69.25   3rd Qu.:69.35   3rd Qu.:69.70  
##  Max.   :76.10   Max.   :76.40   Max.   :76.70   Max.   :77.00  
##                                                                 
##       1976            1977            1978            1979      
##  Min.   :24.80   Min.   :24.60   Min.   :24.30   Min.   :24.10  
##  1st Qu.:54.55   1st Qu.:55.10   1st Qu.:55.40   1st Qu.:56.05  
##  Median :64.60   Median :65.20   Median :65.50   Median :66.20  
##  Mean   :62.18   Mean   :62.66   Mean   :62.93   Mean   :63.22  
##  3rd Qu.:69.85   3rd Qu.:70.10   3rd Qu.:70.40   3rd Qu.:70.60  
##  Max.   :77.20   Max.   :77.50   Max.   :77.80   Max.   :78.10  
##                                                                 
##       1980            1981            1982            1983      
##  Min.   :43.40   Min.   :43.20   Min.   :43.70   Min.   :41.40  
##  1st Qu.:56.05   1st Qu.:56.25   1st Qu.:56.25   1st Qu.:57.25  
##  Median :66.50   Median :66.70   Median :66.80   Median :67.40  
##  Mean   :63.74   Mean   :63.95   Mean   :64.05   Mean   :64.47  
##  3rd Qu.:70.75   3rd Qu.:71.00   3rd Qu.:71.15   3rd Qu.:71.25  
##  Max.   :78.20   Max.   :78.30   Max.   :78.30   Max.   :78.30  
##                                                                 
##       1984            1985            1986            1987      
##  Min.   :40.50   Min.   :42.40   Min.   :43.40   Min.   :44.80  
##  1st Qu.:57.85   1st Qu.:58.25   1st Qu.:58.65   1st Qu.:58.65  
##  Median :67.80   Median :68.10   Median :68.60   Median :68.90  
##  Mean   :64.73   Mean   :65.00   Mean   :65.36   Mean   :65.53  
##  3rd Qu.:71.30   3rd Qu.:71.55   3rd Qu.:71.75   3rd Qu.:71.85  
##  Max.   :78.50   Max.   :78.60   Max.   :78.70   Max.   :78.80  
##                                                                 
##       1988            1989            1990            1991      
##  Min.   :45.40   Min.   :46.00   Min.   :46.60   Min.   :46.90  
##  1st Qu.:58.65   1st Qu.:59.45   1st Qu.:59.95   1st Qu.:60.05  
##  Median :68.90   Median :69.60   Median :69.50   Median :69.70  
##  Mean   :65.66   Mean   :66.07   Mean   :66.19   Mean   :66.30  
##  3rd Qu.:71.95   3rd Qu.:71.90   3rd Qu.:72.15   3rd Qu.:72.50  
##  Max.   :78.90   Max.   :79.10   Max.   :79.30   Max.   :79.40  
##                                                                 
##       1992            1993            1994            1995      
##  Min.   :46.50   Min.   :46.10   Min.   : 9.64   Min.   :44.30  
##  1st Qu.:60.15   1st Qu.:60.20   1st Qu.:60.60   1st Qu.:60.35  
##  Median :69.40   Median :69.20   Median :69.50   Median :69.40  
##  Mean   :66.36   Mean   :66.37   Mean   :66.23   Mean   :66.48  
##  3rd Qu.:72.60   3rd Qu.:72.80   3rd Qu.:73.10   3rd Qu.:73.30  
##  Max.   :79.50   Max.   :79.70   Max.   :80.10   Max.   :80.00  
##                                                                 
##       1996            1997            1998            1999      
##  Min.   :43.90   Min.   :43.60   Min.   :44.30   Min.   :43.40  
##  1st Qu.:60.15   1st Qu.:60.20   1st Qu.:60.15   1st Qu.:59.95  
##  Median :70.10   Median :70.40   Median :70.60   Median :70.70  
##  Mean   :66.69   Mean   :66.82   Mean   :66.98   Mean   :67.14  
##  3rd Qu.:73.40   3rd Qu.:73.75   3rd Qu.:73.80   3rd Qu.:74.05  
##  Max.   :80.50   Max.   :80.80   Max.   :80.80   Max.   :81.00  
##                                                                 
##       2000            2001            2002            2003      
##  Min.   :44.30   Min.   :44.30   Min.   :44.40   Min.   :44.50  
##  1st Qu.:60.55   1st Qu.:61.00   1st Qu.:61.35   1st Qu.:61.55  
##  Median :71.10   Median :71.10   Median :71.30   Median :72.00  
##  Mean   :67.49   Mean   :67.77   Mean   :67.97   Mean   :68.26  
##  3rd Qu.:74.50   3rd Qu.:74.80   3rd Qu.:74.95   3rd Qu.:75.15  
##  Max.   :81.40   Max.   :81.70   Max.   :82.00   Max.   :82.10  
##                                                                 
##       2004            2005            2006            2007      
##  Min.   :43.90   Min.   :43.60   Min.   :43.90   Min.   :44.20  
##  1st Qu.:61.60   1st Qu.:61.70   1st Qu.:62.05   1st Qu.:62.45  
##  Median :71.90   Median :72.20   Median :72.60   Median :72.60  
##  Mean   :68.51   Mean   :68.84   Mean   :69.19   Mean   :69.54  
##  3rd Qu.:75.50   3rd Qu.:75.60   3rd Qu.:76.00   3rd Qu.:76.30  
##  Max.   :82.30   Max.   :82.30   Max.   :82.60   Max.   :82.80  
##                                                                 
##       2008            2009            2010            2011      
##  Min.   :44.50   Min.   :44.90   Min.   :32.50   Min.   :48.00  
##  1st Qu.:62.70   1st Qu.:63.30   1st Qu.:63.90   1st Qu.:64.20  
##  Median :72.80   Median :72.90   Median :73.30   Median :73.40  
##  Mean   :69.85   Mean   :70.21   Mean   :70.48   Mean   :70.91  
##  3rd Qu.:76.45   3rd Qu.:76.75   3rd Qu.:77.00   3rd Qu.:77.15  
##  Max.   :82.90   Max.   :83.10   Max.   :83.20   Max.   :83.40  
##                                                                 
##       2012            2013            2014            2015      
##  Min.   :48.90   Min.   :48.50   Min.   :48.70   Min.   :50.50  
##  1st Qu.:65.00   1st Qu.:65.45   1st Qu.:65.95   1st Qu.:66.95  
##  Median :73.20   Median :73.10   Median :73.10   Median :73.30  
##  Mean   :71.31   Mean   :71.64   Mean   :71.87   Mean   :72.14  
##  3rd Qu.:77.45   3rd Qu.:77.60   3rd Qu.:77.75   3rd Qu.:77.85  
##  Max.   :83.60   Max.   :83.90   Max.   :84.20   Max.   :84.40  
##                                                                 
##       2016            2017            2018            2019      
##  Min.   :51.70   Min.   :51.90   Min.   :52.40   Min.   :52.90  
##  1st Qu.:67.30   1st Qu.:67.80   1st Qu.:68.10   1st Qu.:68.28  
##  Median :73.70   Median :74.00   Median :74.15   Median :74.20  
##  Mean   :72.45   Mean   :72.74   Mean   :72.97   Mean   :73.18  
##  3rd Qu.:78.05   3rd Qu.:78.15   3rd Qu.:78.33   3rd Qu.:78.50  
##  Max.   :84.70   Max.   :84.80   Max.   :85.00   Max.   :85.10  
##                                  NA's   :3       NA's   :3      
##       2020            2021            2022            2023      
##  Min.   :53.30   Min.   :53.60   Min.   :53.90   Min.   :54.20  
##  1st Qu.:68.47   1st Qu.:68.72   1st Qu.:69.00   1st Qu.:69.20  
##  Median :74.35   Median :74.50   Median :74.65   Median :74.95  
##  Mean   :73.39   Mean   :73.59   Mean   :73.78   Mean   :73.97  
##  3rd Qu.:78.60   3rd Qu.:78.72   3rd Qu.:78.90   3rd Qu.:79.00  
##  Max.   :85.30   Max.   :85.40   Max.   :85.50   Max.   :85.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2024            2025            2026            2027      
##  Min.   :54.50   Min.   :54.80   Min.   :55.10   Min.   :55.40  
##  1st Qu.:69.35   1st Qu.:69.50   1st Qu.:69.67   1st Qu.:69.97  
##  Median :75.10   Median :75.25   Median :75.40   Median :75.50  
##  Mean   :74.16   Mean   :74.34   Mean   :74.52   Mean   :74.72  
##  3rd Qu.:79.12   3rd Qu.:79.30   3rd Qu.:79.40   3rd Qu.:79.60  
##  Max.   :85.80   Max.   :85.90   Max.   :86.00   Max.   :86.20  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2028            2029            2030            2031      
##  Min.   :55.70   Min.   :56.00   Min.   :56.30   Min.   :56.60  
##  1st Qu.:70.28   1st Qu.:70.50   1st Qu.:70.70   1st Qu.:70.95  
##  Median :75.70   Median :75.80   Median :75.95   Median :76.10  
##  Mean   :74.90   Mean   :75.09   Mean   :75.27   Mean   :75.44  
##  3rd Qu.:79.72   3rd Qu.:79.92   3rd Qu.:80.10   3rd Qu.:80.30  
##  Max.   :86.30   Max.   :86.40   Max.   :86.50   Max.   :86.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2032            2033            2034            2035      
##  Min.   :56.90   Min.   :57.20   Min.   :57.40   Min.   :57.70  
##  1st Qu.:71.20   1st Qu.:71.30   1st Qu.:71.47   1st Qu.:71.60  
##  Median :76.30   Median :76.45   Median :76.65   Median :76.75  
##  Mean   :75.62   Mean   :75.79   Mean   :75.96   Mean   :76.13  
##  3rd Qu.:80.50   3rd Qu.:80.65   3rd Qu.:80.83   3rd Qu.:80.95  
##  Max.   :86.80   Max.   :86.90   Max.   :87.00   Max.   :87.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2036            2037            2038            2039      
##  Min.   :58.00   Min.   :58.30   Min.   :58.50   Min.   :58.80  
##  1st Qu.:71.83   1st Qu.:71.95   1st Qu.:72.17   1st Qu.:72.30  
##  Median :76.90   Median :77.05   Median :77.20   Median :77.30  
##  Mean   :76.30   Mean   :76.46   Mean   :76.61   Mean   :76.78  
##  3rd Qu.:81.12   3rd Qu.:81.33   3rd Qu.:81.42   3rd Qu.:81.60  
##  Max.   :87.30   Max.   :87.40   Max.   :87.50   Max.   :87.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2040            2041            2042            2043      
##  Min.   :59.00   Min.   :59.30   Min.   :59.50   Min.   :59.70  
##  1st Qu.:72.40   1st Qu.:72.67   1st Qu.:72.88   1st Qu.:73.08  
##  Median :77.40   Median :77.55   Median :77.70   Median :77.85  
##  Mean   :76.93   Mean   :77.09   Mean   :77.25   Mean   :77.40  
##  3rd Qu.:81.72   3rd Qu.:81.90   3rd Qu.:82.03   3rd Qu.:82.20  
##  Max.   :87.70   Max.   :87.80   Max.   :88.00   Max.   :88.10  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2044            2045            2046            2047      
##  Min.   :60.00   Min.   :60.20   Min.   :60.40   Min.   :60.60  
##  1st Qu.:73.20   1st Qu.:73.30   1st Qu.:73.50   1st Qu.:73.60  
##  Median :77.95   Median :78.15   Median :78.30   Median :78.45  
##  Mean   :77.55   Mean   :77.70   Mean   :77.85   Mean   :78.00  
##  3rd Qu.:82.30   3rd Qu.:82.50   3rd Qu.:82.62   3rd Qu.:82.80  
##  Max.   :88.20   Max.   :88.30   Max.   :88.50   Max.   :88.60  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2048            2049            2050            2051      
##  Min.   :60.80   Min.   :61.00   Min.   :61.20   Min.   :61.40  
##  1st Qu.:73.70   1st Qu.:73.80   1st Qu.:73.97   1st Qu.:74.08  
##  Median :78.60   Median :78.75   Median :78.90   Median :79.05  
##  Mean   :78.15   Mean   :78.29   Mean   :78.44   Mean   :78.58  
##  3rd Qu.:82.92   3rd Qu.:83.10   3rd Qu.:83.22   3rd Qu.:83.40  
##  Max.   :88.70   Max.   :88.80   Max.   :88.90   Max.   :89.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2052            2053            2054            2055      
##  Min.   :61.60   Min.   :61.80   Min.   :62.00   Min.   :62.20  
##  1st Qu.:74.28   1st Qu.:74.38   1st Qu.:74.50   1st Qu.:74.67  
##  Median :79.25   Median :79.35   Median :79.55   Median :79.70  
##  Mean   :78.72   Mean   :78.87   Mean   :79.00   Mean   :79.14  
##  3rd Qu.:83.50   3rd Qu.:83.70   3rd Qu.:83.80   3rd Qu.:83.92  
##  Max.   :89.20   Max.   :89.30   Max.   :89.40   Max.   :89.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2056            2057            2058            2059      
##  Min.   :62.30   Min.   :62.50   Min.   :62.70   Min.   :62.90  
##  1st Qu.:74.80   1st Qu.:74.90   1st Qu.:75.08   1st Qu.:75.20  
##  Median :79.85   Median :80.00   Median :80.10   Median :80.25  
##  Mean   :79.28   Mean   :79.42   Mean   :79.56   Mean   :79.69  
##  3rd Qu.:84.03   3rd Qu.:84.20   3rd Qu.:84.33   3rd Qu.:84.42  
##  Max.   :89.60   Max.   :89.80   Max.   :89.90   Max.   :90.00  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2060            2061            2062            2063      
##  Min.   :63.00   Min.   :63.20   Min.   :63.40   Min.   :63.50  
##  1st Qu.:75.30   1st Qu.:75.50   1st Qu.:75.60   1st Qu.:75.78  
##  Median :80.40   Median :80.55   Median :80.70   Median :80.85  
##  Mean   :79.83   Mean   :79.96   Mean   :80.10   Mean   :80.24  
##  3rd Qu.:84.60   3rd Qu.:84.72   3rd Qu.:84.83   3rd Qu.:85.00  
##  Max.   :90.10   Max.   :90.20   Max.   :90.30   Max.   :90.50  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2064            2065            2066            2067      
##  Min.   :63.70   Min.   :63.80   Min.   :64.00   Min.   :64.10  
##  1st Qu.:75.90   1st Qu.:76.00   1st Qu.:76.17   1st Qu.:76.30  
##  Median :81.00   Median :81.15   Median :81.25   Median :81.40  
##  Mean   :80.36   Mean   :80.50   Mean   :80.62   Mean   :80.76  
##  3rd Qu.:85.10   3rd Qu.:85.20   3rd Qu.:85.30   3rd Qu.:85.50  
##  Max.   :90.60   Max.   :90.70   Max.   :90.80   Max.   :90.90  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2068            2069            2070            2071      
##  Min.   :64.30   Min.   :64.40   Min.   :64.50   Min.   :64.70  
##  1st Qu.:76.40   1st Qu.:76.58   1st Qu.:76.70   1st Qu.:76.80  
##  Median :81.55   Median :81.65   Median :81.80   Median :81.95  
##  Mean   :80.89   Mean   :81.01   Mean   :81.15   Mean   :81.27  
##  3rd Qu.:85.60   3rd Qu.:85.70   3rd Qu.:85.80   3rd Qu.:85.92  
##  Max.   :91.00   Max.   :91.20   Max.   :91.30   Max.   :91.40  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2072            2073            2074            2075      
##  Min.   :64.80   Min.   :64.90   Min.   :65.10   Min.   :65.20  
##  1st Qu.:77.00   1st Qu.:77.10   1st Qu.:77.28   1st Qu.:77.38  
##  Median :82.05   Median :82.20   Median :82.30   Median :82.50  
##  Mean   :81.40   Mean   :81.53   Mean   :81.66   Mean   :81.78  
##  3rd Qu.:86.03   3rd Qu.:86.12   3rd Qu.:86.30   3rd Qu.:86.40  
##  Max.   :91.50   Max.   :91.60   Max.   :91.70   Max.   :91.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2076            2077            2078            2079      
##  Min.   :65.30   Min.   :65.50   Min.   :65.60   Min.   :65.70  
##  1st Qu.:77.58   1st Qu.:77.67   1st Qu.:77.78   1st Qu.:77.97  
##  Median :82.60   Median :82.70   Median :82.80   Median :82.95  
##  Mean   :81.91   Mean   :82.04   Mean   :82.17   Mean   :82.28  
##  3rd Qu.:86.50   3rd Qu.:86.62   3rd Qu.:86.72   3rd Qu.:86.83  
##  Max.   :92.00   Max.   :92.10   Max.   :92.20   Max.   :92.30  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2080            2081            2082            2083      
##  Min.   :65.80   Min.   :66.00   Min.   :66.10   Min.   :66.20  
##  1st Qu.:78.08   1st Qu.:78.25   1st Qu.:78.38   1st Qu.:78.47  
##  Median :83.05   Median :83.15   Median :83.25   Median :83.35  
##  Mean   :82.41   Mean   :82.53   Mean   :82.66   Mean   :82.78  
##  3rd Qu.:86.92   3rd Qu.:87.03   3rd Qu.:87.12   3rd Qu.:87.22  
##  Max.   :92.40   Max.   :92.50   Max.   :92.70   Max.   :92.80  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2084            2085            2086            2087      
##  Min.   :66.30   Min.   :66.50   Min.   :66.60   Min.   :66.70  
##  1st Qu.:78.65   1st Qu.:78.78   1st Qu.:78.88   1st Qu.:79.00  
##  Median :83.45   Median :83.55   Median :83.70   Median :83.80  
##  Mean   :82.91   Mean   :83.03   Mean   :83.15   Mean   :83.27  
##  3rd Qu.:87.33   3rd Qu.:87.50   3rd Qu.:87.53   3rd Qu.:87.70  
##  Max.   :92.90   Max.   :93.00   Max.   :93.10   Max.   :93.30  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2088            2089            2090            2091      
##  Min.   :66.80   Min.   :66.90   Min.   :67.00   Min.   :67.10  
##  1st Qu.:79.17   1st Qu.:79.28   1st Qu.:79.40   1st Qu.:79.50  
##  Median :83.90   Median :84.00   Median :84.10   Median :84.20  
##  Mean   :83.39   Mean   :83.52   Mean   :83.63   Mean   :83.76  
##  3rd Qu.:87.80   3rd Qu.:87.90   3rd Qu.:88.00   3rd Qu.:88.12  
##  Max.   :93.40   Max.   :93.50   Max.   :93.60   Max.   :93.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2092            2093            2094            2095      
##  Min.   :67.30   Min.   :67.40   Min.   :67.50   Min.   :67.60  
##  1st Qu.:79.70   1st Qu.:79.80   1st Qu.:79.90   1st Qu.:80.08  
##  Median :84.35   Median :84.45   Median :84.55   Median :84.65  
##  Mean   :83.88   Mean   :84.00   Mean   :84.12   Mean   :84.24  
##  3rd Qu.:88.22   3rd Qu.:88.33   3rd Qu.:88.50   3rd Qu.:88.60  
##  Max.   :93.90   Max.   :94.00   Max.   :94.10   Max.   :94.20  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2096            2097            2098            2099      
##  Min.   :67.70   Min.   :67.80   Min.   :67.90   Min.   :68.00  
##  1st Qu.:80.20   1st Qu.:80.38   1st Qu.:80.47   1st Qu.:80.58  
##  Median :84.75   Median :84.85   Median :85.00   Median :85.15  
##  Mean   :84.36   Mean   :84.48   Mean   :84.59   Mean   :84.71  
##  3rd Qu.:88.70   3rd Qu.:88.80   3rd Qu.:88.90   3rd Qu.:89.00  
##  Max.   :94.30   Max.   :94.40   Max.   :94.50   Max.   :94.70  
##  NA's   :3       NA's   :3       NA's   :3       NA's   :3      
##       2100      
##  Min.   :68.10  
##  1st Qu.:80.78  
##  Median :85.25  
##  Mean   :84.83  
##  3rd Qu.:89.10  
##  Max.   :94.80  
##  NA's   :3
```

```r
wide_life_expectancy <- life_expectancy
```

```r
long_life_expectancy<- 
  life_expectancy%>%
  pivot_longer("1800":"2100", names_to = "years", values_to = "average_life_expectancy")
long_life_expectancy
```

```
## # A tibble: 56,287 x 3
##    country     years average_life_expectancy
##    <chr>       <chr>                   <dbl>
##  1 Afghanistan 1800                     28.2
##  2 Afghanistan 1801                     28.2
##  3 Afghanistan 1802                     28.2
##  4 Afghanistan 1803                     28.2
##  5 Afghanistan 1804                     28.2
##  6 Afghanistan 1805                     28.2
##  7 Afghanistan 1806                     28.1
##  8 Afghanistan 1807                     28.1
##  9 Afghanistan 1808                     28.1
## 10 Afghanistan 1809                     28.1
## # ... with 56,277 more rows
```

2. (1 point) How many different countries are represented in the data? Provide the total number and their names. Since each data set includes different numbers of countries, you will need to do this for each one.  

```r
wide_pop_total%>%
  count(country)
```

```
## # A tibble: 195 x 2
##    country                 n
##    <chr>               <int>
##  1 Afghanistan             1
##  2 Albania                 1
##  3 Algeria                 1
##  4 Andorra                 1
##  5 Angola                  1
##  6 Antigua and Barbuda     1
##  7 Argentina               1
##  8 Armenia                 1
##  9 Australia               1
## 10 Austria                 1
## # ... with 185 more rows
```

```r
wide_income_ppp_gpd%>%
  count(country)
```

```
## # A tibble: 193 x 2
##    country                 n
##    <chr>               <int>
##  1 Afghanistan             1
##  2 Albania                 1
##  3 Algeria                 1
##  4 Andorra                 1
##  5 Angola                  1
##  6 Antigua and Barbuda     1
##  7 Argentina               1
##  8 Armenia                 1
##  9 Australia               1
## 10 Austria                 1
## # ... with 183 more rows
```

```r
wide_life_expectancy%>%
  count(country)
```

```
## # A tibble: 187 x 2
##    country                 n
##    <chr>               <int>
##  1 Afghanistan             1
##  2 Albania                 1
##  3 Algeria                 1
##  4 Andorra                 1
##  5 Angola                  1
##  6 Antigua and Barbuda     1
##  7 Argentina               1
##  8 Armenia                 1
##  9 Australia               1
## 10 Austria                 1
## # ... with 177 more rows
```

## Life Expectancy  

3. (2 points) Let's limit the data to 100 years (1920-2020). For these years, which country has the highest average life expectancy? How about the lowest average life expectancy?  

```r
long_life_expectancy%>%
  filter(years >= "1920"& years<= "2020")%>%
  group_by(country)%>%
  summarise( mean_life= mean(average_life_expectancy))%>%
  arrange(desc(mean_life))
```

```
## # A tibble: 187 x 2
##    country        mean_life
##    <chr>              <dbl>
##  1 Sweden              73.5
##  2 Norway              73.2
##  3 Netherlands         73.0
##  4 Iceland             72.9
##  5 Australia           72.7
##  6 Switzerland         72.7
##  7 Denmark             71.9
##  8 Canada              71.8
##  9 New Zealand         71.4
## 10 United Kingdom      71.1
## # ... with 177 more rows
```

4. (3 points) Although we can see which country has the highest life expectancy for the past 100 years, we don't know which countries have changed the most. What are the top 5 countries that have experienced the biggest improvement in life expectancy between 1920-2020?  

```r
long_life_expectancy%>%
  filter(years == "1920" | years == "2020")%>%
  pivot_wider(names_from = years, values_from = average_life_expectancy)%>%
  mutate(delta= 1920-2020)%>%
 arrange(desc(delta))
```

```
## # A tibble: 187 x 4
##    country             `1920` `2020` delta
##    <chr>                <dbl>  <dbl> <dbl>
##  1 Afghanistan           30.6   64.4  -100
##  2 Albania               35.4   78.6  -100
##  3 Algeria               29.6   78.3  -100
##  4 Andorra               NA     NA    -100
##  5 Angola                30.4   65.4  -100
##  6 Antigua and Barbuda   33.9   77.4  -100
##  7 Argentina             49.9   77.1  -100
##  8 Armenia               27     76    -100
##  9 Australia             60.6   82.8  -100
## 10 Austria               50.3   82.1  -100
## # ... with 177 more rows
```

```r
long_life_expectancy%>%
  filter(years>="1920" & years<="2020") %>% 
  group_by(country)%>%
  summarise(min= min(average_life_expectancy), mean=mean(average_life_expectancy), max=max(average_life_expectancy))%>% 
  arrange(desc(max))
```

```
## # A tibble: 187 x 4
##    country       min  mean   max
##    <chr>       <dbl> <dbl> <dbl>
##  1 Singapore    34.3  65.0  85.3
##  2 Japan        30.7  67.6  84.7
##  3 Switzerland  54.5  72.7  84.4
##  4 Italy        45.5  69.2  83.6
##  5 Spain        39.2  67.9  83.5
##  6 Israel       32    64.5  83.4
##  7 Kuwait       26.6  60.2  83.4
##  8 France       47.3  70.1  83.2
##  9 Iceland      54    72.9  83.2
## 10 South Korea  28.2  59.2  83.2
## # ... with 177 more rows
```

5. (3 points) Make a plot that shows the change over the past 100 years for the country with the biggest improvement in life expectancy. Be sure to add appropriate aesthetics to make the plot clean and clear. Once you have made the plot, do a little internet searching and see if you can discover what historical event may have contributed to this remarkable change.  

```r
long_life_expectancy%>%
  filter(country== "Japan", years>= "1920"& years<= "2020")%>%
  ggplot(aes(x= years, y= average_life_expectancy))+
  geom_line()+
  geom_point(shape= 6, color= "cornflowerblue")+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Japan's Life Expectancy Change", x= "Year (between 1920 to 2020)", y= "Average Life Expectancy")
```

```
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
```

![](midterm_2_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

```r
long_life_expectancy%>%
  filter(country== "Kazakhstan", years>= "1920"& years<= "2020")%>%
  ggplot(aes(x= years, y= average_life_expectancy))+
  geom_line()+
  geom_point(shape= 6, color= "cornflowerblue")+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Kazahhstan's Life Expectancy Change", x= "Year (between 1920 to 2020)", y= "Average Life Expectancy")
```

```
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
```

![](midterm_2_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

```r
names(long_pop_total)
```

```
## [1] "country"           "years"             "total_ population"
```

## Population Growth
6. (3 points) Which 5 countries have had the highest population growth over the past 100 years (1920-2020)?  

```r
long_pop_total%>%
  filter(years >="1920" & years <="2020")%>% 
  group_by(country)%>%
  summarise(min= min(`total_ population`), mean= mean(`total_ population`), max= max(`total_ population`))%>%
  arrange(desc(min))
```

```
## # A tibble: 195 x 4
##    country              min       mean        max
##    <chr>              <dbl>      <dbl>      <dbl>
##  1 China          472000000 885366337. 1440000000
##  2 India          317000000 684613861. 1380000000
##  3 United States  111000000 211485149.  331000000
##  4 Russia          77900000 121885149.  148000000
##  5 Germany         62400000  75167327.   83800000
##  6 Japan           55300000 100702970.  129000000
##  7 Indonesia       47300000 133104950.  274000000
##  8 United Kingdom  46500000  54655446.   67900000
##  9 France          38500000  50408911.   65300000
## 10 Italy           37100000  51302970.   60700000
## # ... with 185 more rows
```

7. (4 points) Produce a plot that shows the 5 countries that have had the highest population growth over the past 100 years (1920-2020). Which countries appear to have had exponential growth?  

```r
long_pop_total %>% 
  filter(country=="China" | country=="India" | country=="United States" | country=="Russia" | country=="Germany", years >="1920" & years <="2020") %>% 
  select(country, years, `total_ population`) %>% 
  ggplot(aes(x=years, y= `total_ population`,group= country, color=country))+
  geom_line()+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Top 5 Countries Population Transition", y= "Population", x= "Year")
```

![](midterm_2_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

## Income
The units used for income are gross domestic product per person adjusted for differences in purchasing power in international dollars.

```r
names(long_income_ppp_gpd)
```

```
## [1] "country"           "years"             "average_income_pp"
```

8. (4 points) As in the previous questions, which countries have experienced the biggest growth in per person GDP. Show this as a table and then plot the changes for the top 5 countries. With a bit of research, you should be able to explain the dramatic downturns of the wealthiest economies that occurred during the 1980's.  

```r
long_income_ppp_gpd%>%
  filter(years >="1920" & years <="2020")%>% 
  group_by(country)%>%
  summarise(min= min(average_income_pp), mean= mean(average_income_pp), max= max(average_income_pp))%>%
  arrange(desc(min, max))
```

```
## # A tibble: 193 x 4
##    country          min   mean   max
##    <chr>          <dbl>  <dbl> <dbl>
##  1 Switzerland    12900 35676. 59100
##  2 Norway          7980 32904. 67500
##  3 Australia       7910 22795. 45800
##  4 United States   7270 27304. 57500
##  5 New Zealand     7130 19635. 36700
##  6 Denmark         6880 24547. 48900
##  7 United Kingdom  6660 20199. 40700
##  8 Monaco          6630 29004. 62000
##  9 Belgium         6410 21085. 43900
## 10 Argentina       6100 11752. 19800
## # ... with 183 more rows
```

```r
long_income_ppp_gpd %>% 
  filter(country=="Switzerland" | country=="Norway" | country=="United States" | country=="Australia" | country=="New Zealand", years >="1920" & years <="2020") %>% 
  select(country, years, average_income_pp) %>% 
  ggplot(aes(x=years, y= average_income_pp,group= country, color=country))+
  geom_line()+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Top 5 Countries Population Income", y= "Income (GDP)", x= "Year")
```

![](midterm_2_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

Looking at the change in GDP, this can be the GDP at start year and end year = subtraction, this does not account for good and bad years.MAybe look at min and max of both years.  

9. (3 points) Create three new objects that restrict each data set (life expectancy, population, income) to the years 1920-2020. Hint: I suggest doing this with the long form of your data. Once this is done, merge all three data sets using the code I provide below. You may need to adjust the code depending on how you have named your objects. I called mine `life_expectancy_100`, `population_100`, and `income_100`. For some of you, learning these `joins` will be important for your project.  

```r
life_1920_2020 <-
  long_life_expectancy%>%
  filter(years >="1920" & years <="2020")

pop_1920_2020 <-
  long_pop_total%>%
  filter(years >="1920" & years <="2020")
income_1920_2020 <-
  long_income_ppp_gpd%>%
  filter(years >="1920" & years <="2020")
```


```r
gapminder_join <- inner_join(life_1920_2020, pop_1920_2020, by= c("country", "years"))
gapminder_join <- inner_join(gapminder_join, income_1920_2020, by= c("country", "years"))
gapminder_join
```

```
## # A tibble: 18,887 x 5
##    country     years average_life_expectancy `total_ populatio~ average_income_~
##    <chr>       <chr>                   <dbl>              <dbl>            <dbl>
##  1 Afghanistan 1920                     30.6           10600000             1490
##  2 Afghanistan 1921                     30.7           10500000             1520
##  3 Afghanistan 1922                     30.8           10300000             1550
##  4 Afghanistan 1923                     30.8            9710000             1570
##  5 Afghanistan 1924                     30.9            9200000             1600
##  6 Afghanistan 1925                     31              8720000             1630
##  7 Afghanistan 1926                     31              8260000             1650
##  8 Afghanistan 1927                     31.1            7830000             1680
##  9 Afghanistan 1928                     31.1            7420000             1710
## 10 Afghanistan 1929                     31.2            7100000             1740
## # ... with 18,877 more rows
```

10. (4 points) Use the joined data to perform an analysis of your choice. The analysis should include a comparison between two or more of the variables `life_expectancy`, `population`, or `income.`  

```r
gapminder_join%>% 
  filter(country=="United States")%>%
  ggplot(aes(x= average_life_expectancy, y= average_life_expectancy )) +
  geom_point(shape=4, color= "cornflowerblue")+
  geom_smooth(method="lm")+
  labs(title = "How Income affects Life Expectancy ", x= "Average Age (Years)", y= "Income (GDP)")
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](midterm_2_files/figure-html/unnamed-chunk-35-1.png)<!-- -->

