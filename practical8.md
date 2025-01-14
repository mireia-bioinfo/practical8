# Objectives

The learning objectives for this practical are:

-   How to create and use list objects.
-   How to perform implicit looping through lists.
-   Add new columns to data frames.
-   How to merge data frames.
-   Learn to explore and visualize data in different ways.

# Setup and background

To do this practical you need an installation of R and RStudio. You can
find the instructions in the [setup](/setup#r-and-rstudio) link on how
to install R and RStudio in your system. For a smooth development of
this practical, it is strongly recommended that you follow and finish
the previous [practical 7](/practical7/).

We will download COVID19 vaccination and demographic data for Catalonia
to illustrate some data wrangling in R and RStudio. Please follow the
next two steps:

-   **COVID19 vaccination by municipality**:

    1.  Go to the Catalan Health Departament COVID19 data portal at
        <https://dadescovid.cat> and switch the language to “ENGLISH”
        using the pull-down menu on the top-right corner of the page.
    2.  Follow the downloads link and on the next page, go to the
        section “Other Downloads” and click and download the “CSV” file
        corresponding to “Vaccination for COVID-19: administered doses
        by municipality”. Make sure you know exactly where in your
        filesystem this file has been downloaded. **Tip:** some browsers
        automatically download files into a folder called “Downloads” or
        under a name corresponding to the translation of “Downloads” to
        the default language of your operating system.
    3.  Make a directory in your filesystem, for instance at your *home*
        directory, called `practical8` and copy in it the downloaded
        file.
    4.  **Change the name of the file you just downloaded to
        `dosis_municipi.csv`**, so that you finally have a file called
        `dosis_municipi.csv` in the directory `practical8`.

-   **Population by municipality**:

    1.  Download the Catalan Urbanistic Map dataset at the [Dades
        Obertes de
        Catalunya](https://governobert.gencat.cat/ca/dades_obertes) data
        portal. You should follow the following
        [link](https://analisi.transparenciacatalunya.cat/en/Urbanisme-infraestructures/Dades-del-mapa-urban-stic-de-Catalunya/epsm-zskb)
        and donwload the data by going to the top right corner and
        select “Export” and then “CSV”. This dataset includes
        information of the population of Catalonia by municipality.
    2.  Copy the donwloaded file to the `practical8` directory and
        **change its name to `poblacio_municipis.csv`**.

If you are using the UPF [myapps](https://myapps.upf.edu) cloud to run
RStudio, then you need to either use an internet browser in *myapps* to
download the data file directly in the *myapps* cloud or upload to the
*myapps* cloud the file that you have downloaded in your own computer.

# Reading and filtering data

Let’s load the CSV file `poblacio_municipis.csv`, which contains some
demographic indicators for the 948 municipalities in Catalonia such as
the population (column `Poblacio_padro`).

    > pop <- read.csv("poblacio_municipis.csv")
    > dim(pop)

    [1] 8532   69

    > head(pop, n=3)

       Any Codi_ine_5_txt Codi_ine_6_txt               NomMun         Comarca AFT
    1 2020           8001          80018               Abrera  Baix Llobregat  MB
    2 2020           8006          80060        Arenys de Mar         Maresme  MB
    3 2020           8005          80057 l'Ametlla del Vallès Vallès Oriental  MB
      Costa Muntanya Poblacio_padro Superficie_ha   X05_SU X05_perce_SU  X05_SUC
    1     0                   12538       1994.69 262.5691      13.1634 243.3522
    2     1                   15941        649.16 274.3753      42.2665 272.7524
    3     0                    8646       1438.51 494.3307      34.3642 464.9938
      X05_perce_SUC X05_SNC X05_perce_SNC X05_SURB X05_perce_SUR  X05_SUD
    1       12.2000 19.2168        0.9634 376.8896       18.8947 376.8896
    2       42.0165  1.6229        0.2500  60.3569        9.2978  51.9942
    3       32.3248 29.3369        2.0394  92.2984        6.4163  80.7220
      X05_perce_SUD X05_SND X05_perce_SND   X05_SNU X05_perce_SNU X06_Class_no_SNU
    1       18.8947  0.0000        0.0000 1355.2279       67.9419         639.4587
    2        8.0095  8.3627        1.2882  314.4229       48.4357         334.7321
    3        5.6115 11.5764        0.8048  851.8768       59.2196         586.6291
      X06_perce_Class_no_snu X07_densitat_pob_km2 X08_densitat_pob_urba_Km2
    1                32.0581             628.8827                  4775.124
    2                51.5643            2362.6804                  5809.926
    3                40.7804             607.0177                  1749.031
      X09_Perce_SNC_SU X10_Perce_SUD_SU X11_Perce_SND_SU X012_Qual_SUC_AE
    1           7.3188         143.5392           0.0000          81.2095
    2           0.5915          18.9500           3.0479          22.4704
    3           5.9347          16.3295           2.3418          13.7252
      X12_A1_SUC X12_A2_SUC X12_A3_SUC X13_Qual_SUC_R X13_R1_SUC X13_R2_SUC
    1    77.1395     4.0700          0        78.1599     3.9925     3.0374
    2    13.8512     8.6192          0       100.7880    18.7890     6.2077
    3     9.2577     4.4675          0       278.5705     3.0580     1.4739
      X13_R3_SUC X13_R4_SUC X13_R5_SUC X13_R6_SUC X14_Qual_SUC_Altres X14_M1_SUC
    1          0     1.4836     1.1153    68.5311              0.2809      0.000
    2          0    11.0237     9.4110    55.3565              0.0000      0.000
    3          0     1.7002    11.2872   261.0512              2.2092      0.225
      X14_M2_SUC X14_M3_SUC X15_Qual_SUC_SISTEMES X15_SA_SUC X15_SC_SUC X15_SD_SUC
    1     0.2809          0               83.8462          0     0.0000     0.0000
    2     0.0000          0              143.6826          0    19.0641     0.2987
    3     1.9842          0              170.8169          0     0.0000     0.0000
      X15_SE_SUC X15_SF_SUC X15_SH_SUC X15_SP_SUC X15_SS_SUC X15_ST_SUC X15_SV_SUC
    1     8.4223     0.0000     0.0000     0.0000     0.4189     0.0000    35.9782
    2    21.5263     6.0203     1.7636    21.4994     0.5012     0.4495    33.0080
    3    21.3591     0.0000     0.0000     0.0000     7.7537     4.1926    67.5495
      X15_SX0_SUC X15_SX1_SUC X15_SX2_SUC X16_QUAL_snu X16_N1_SNU X16_N2_SNU
    1           0     10.3124     28.7144    1129.1305   497.0186   622.0906
    2           0     16.2256     29.3463     229.8781     0.0000   229.8781
    3           0     20.8761     49.0858     816.6905   528.2927   287.6800
      X16_N3_SNU X16_N4_SNU X17_Sol_resid_habt X18_Sol_AE_habt X19_Zverdes_habt
    1          0    10.0213           160.4147        154.3907          28.6953
    2          0     0.0000           158.1637        709.4218          20.7063
    3          0     0.7178            31.0370        629.9348          78.1281
      X20_Equip_habt
    1         6.7174
    2        13.5038
    3        24.7040

    > table(pop$Any)


    2012 2013 2014 2015 2016 2017 2018 2019 2020 
     948  948  948  948  948  948  948  948  948 

We can observe that this dataset contains data from different years. To
continue with our analysis, we will select the most recent data
corresponding to 2020 and, moreover, we will only keep the columns
`Codi_ine_5_txt` (identifier for the municipality), `NomMun`, `Comarca`,
`Poblacio_padro` and `Superficie_ha`.

    > ## build a logical mask to select for year 2020
    > mask <- pop$Any == 2020
    > ## build a character string vector of the selected columns
    > selcols <- c("Codi_ine_5_txt", "NomMun", "Comarca", "Poblacio_padro", "Superficie_ha")
    > ## subset the data.frame object 'pop' for rows in 'mask' and columns in 'selcols'
    > pop_sel <- pop[mask, selcols]
    > dim(pop_sel)

    [1] 948   5

    > head(pop_sel)

      Codi_ine_5_txt               NomMun         Comarca Poblacio_padro
    1           8001               Abrera  Baix Llobregat          12538
    2           8006        Arenys de Mar         Maresme          15941
    3           8005 l'Ametlla del Vallès Vallès Oriental           8646
    4           8004               Alpens           Osona            261
    5           8002   Aguilar de Segarra           Bages            282
    6           8003               Alella         Maresme           9904
      Superficie_ha
    1       1994.69
    2        649.16
    3       1438.51
    4       1377.88
    5       4321.98
    6        962.70

# Lists and implicit looping

Lists allow one to group values through their elements. Let’s say we
want to group the population number by county. We can do that using the
function `split()` to which we should give a first argument of the
values we want to group and a second argument with the grouping factor.

    > pbyc <- split(pop_sel$Poblacio_padro, pop_sel$Comarca)
    > class(pbyc)

    [1] "list"

    > length(pbyc)

    [1] 42

    > names(pbyc)

     [1] "Alt Camp"          "Alt Empordà"       "Alt Penedès"      
     [4] "Alt Urgell"        "Alta Ribagorça"    "Anoia"            
     [7] "Bages"             "Baix Camp"         "Baix Ebre"        
    [10] "Baix Empordà"      "Baix Llobregat"    "Baix Penedès"     
    [13] "Barcelonès"        "Berguedà"          "Cerdanya"         
    [16] "Conca de Barberà"  "Garraf"            "Garrigues"        
    [19] "Garrotxa"          "Gironès"           "Maresme"          
    [22] "Moianès"           "Montsià"           "Noguera"          
    [25] "Osona"             "Pallars Jussà"     "Pallars Sobirà"   
    [28] "Pla d'Urgell"      "Pla de l'Estany"   "Priorat"          
    [31] "Ribera d'Ebre"     "Ripollès"          "Segarra"          
    [34] "Segrià"            "Selva"             "Solsonès"         
    [37] "Tarragonès"        "Terra Alta"        "Urgell"           
    [40] "Val d'Aran"        "Vallès Occidental" "Vallès Oriental"  

    > head(pbyc)

    $`Alt Camp`
     [1]   930  5144   458   329   695  1175   372   494  1276   729 24477  1774
    [13]   168   403   190   280   190   488  1163   585   509   518  2348

    $`Alt Empordà`
     [1]   155   863   212  1161   377  1116   158   758   458   993  5421   386
    [13]   241  2090   195   856   368   846   166   761   265   703   338   445
    [25] 10244   537 47235   171   282 11100   451   412   611   787   853  1641
    [37]  1001   602    89   951   309  2695   267   968   272   733   184   958
    [49]  1063   260   516  1898  3320   707 19807   246   698   288  4775  1237
    [61]   844   191   198  1456    90  1427   557   361

    $`Alt Penedès`
     [1]  1009  2317  2380  3261  2428   528   524  1275 12841  1422  2398 40154
    [13]  1102  7676   367  3104  2382  1575  1697   979   918  1840  3768  7670
    [25]  2208  2369  1414

    $`Alt Urgell`
     [1]   320    45   540    93   125    81   152   217   245   244   149   103
    [13]   784  1843  1034   771   347 12206   932

    $`Alta Ribagorça`
    [1] 1019  549 2257

    $Anoia
     [1]   342  1228   210  2185  2980   151   239  1357   175  3741 12596   119
    [13] 10225   162   626   785 16134   321    64   219  2110  3656  1482  5342
    [25]   188   151   151  3668   833  9402 40742   903   537

Grouping values can be useful in data analysis when we want to examine
the data separately by groups. Let’s say we want to visualize the
distribution of the population for the two counties `Terra Alta` and
`Urgell`, next to each other. We can use the function `hist()` for that
purpose, creating a grid of two plotting panes using the `par()`
function, as follows:

    > par(mfrow=c(1, 2))
    > hist(pbyc[["Terra Alta"]], xlab="Population (inhabitants)", main="Terra Alta")
    > hist(pbyc$Urgell, xlab="Population (inhabitants)", main="Urgell")

![](comptaltaurgell-1.png)

Note that in the previous code we are using the double-bracket operator
`[[` instead of the dollar `$` to access the element `Terra Alta`
because this element has an space character in its name. Try to
interpret the previous plots, how many municipalities in *Urgell* and
*Terra Alta* have less than 5,000 inhabitants?

Now, let’s calculate the mean municipality population for the county of
*Terra Alta*. Having built the previous list object, we can make that
calculation applying the function `mean()` to the corresponding element
of the list:

    > mean(pbyc[["Terra Alta"]])

    [1] 952.5

Now, let’s compare it with the mean municipality population for
*Urgell*:

    > mean(pbyc$Urgell)

    [1] 1855.8

It would be tedious to do that calculation for each different county by
writing one such function call for each element of the list. As an
alternative, we could use a `while` or `for` loop that would iterate
over the elements of the list. However, R provides a more compact way to
iterating over lists, and other objects, by using functions for
*implicit*
[looping](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Looping)
such as `lapply()` or `sapply()`. These functions take a list as a first
argument, iterate through each element of that list, and at each
iteration apply the function given in the second argument. Additional
arguments can be given and will be passed to the *applied* function.

The function `lapply()` returns again the input list with its elements
replaced by the result given by the function on each corresponding
element, while the function `sapply()` attempts to simplify the
resulting data structure in that if each element of the resulting list
has length 1, then it return an atomic vector.

We can calculate the mean municipality population per county with the
following call to the `sapply()` function:

    > sapply(pbyc, mean)

             Alt Camp       Alt Empordà       Alt Penedès        Alt Urgell 
            1943.2609         2097.4118         4059.4815         1064.7895 
       Alta Ribagorça             Anoia             Bages         Baix Camp 
            1275.0000         3728.0000         6019.1667         6933.1071 
            Baix Ebre      Baix Empordà    Baix Llobregat      Baix Penedès 
            5572.2143         3772.3611        27821.7667         7700.2143 
           Barcelonès          Berguedà          Cerdanya  Conca de Barberà 
          462924.0000         1289.6774         1089.7059          906.6818 
               Garraf         Garrigues          Garrotxa           Gironès 
           25466.8333          786.3750         2791.4286         7039.4286 
              Maresme           Moianès           Montsià           Noguera 
           15287.5333         1391.9000         5688.5833         1304.4000 
                Osona     Pallars Jussà    Pallars Sobirà      Pla d'Urgell 
            3274.0400          944.7857          462.0000         2296.0625 
      Pla de l'Estany           Priorat     Ribera d'Ebre          Ripollès 
            2967.0000          398.2609         1562.1429         1329.1053 
              Segarra            Segrià             Selva          Solsonès 
            1110.3810         5574.7368         6716.8462          900.8667 
           Tarragonès        Terra Alta            Urgell        Val d'Aran 
           11884.8182          952.5000         1855.8000         1130.5556 
    Vallès Occidental   Vallès Oriental 
           40761.2174        10630.0000 

**Exercise:** calculate the total population per county and the total
population in Catalonia.

# Sorting and ordering

We have two functions in R that allow us to rearrange values in
particular order:

-   `sort()` returns the ordered values.
-   `order()` returns a permutation which rearranges its first argument
    into ascending or descending order.

By default, these functions return an ascending order, but by setting
the argument `decreasing=TRUE`, we can obtain a descending order.

**Exercise:** Using the previous functions `sort()` and `order()` and
the columns `NomMun` (municipality name) and `Poblacio_padro`
(municipality population), find out how many inhabitants have the three
most and three least populated municipalities and their names.

# Adding new columns

In some cases we might be interested in deriving new data columns from
the existing ones. For instance, let’s say we want to add a new column
to the previous `data.frame` object `pop_sel` that stores the population
density of each municipality in inhabitants per [squared kilometer
(Km2)](https://en.wikipedia.org/wiki/Square_kilometre). The column
`Superficie_ha` contains the area occupied by the municipality in
[hectares](https://en.wikipedia.org/wiki/Hectare) (ha), let’s convert it
first to Km2:

    > km2 <- pop_sel$Superficie_ha / 100

Finally, let’s calculate the population density dividing the number of
inhabitants by the area occupied by the municipality in Km2 and add it
to `pop_sel` as a new column called `density`:

    > pop_sel$Density <- pop_sel$Poblacio_padro / km2
    > head(pop_sel)

      Codi_ine_5_txt               NomMun         Comarca Poblacio_padro
    1           8001               Abrera  Baix Llobregat          12538
    2           8006        Arenys de Mar         Maresme          15941
    3           8005 l'Ametlla del Vallès Vallès Oriental           8646
    4           8004               Alpens           Osona            261
    5           8002   Aguilar de Segarra           Bages            282
    6           8003               Alella         Maresme           9904
      Superficie_ha     Density
    1       1994.69  628.568850
    2        649.16 2455.634974
    3       1438.51  601.038575
    4       1377.88   18.942143
    5       4321.98    6.524787
    6        962.70 1028.773242

Let’s say we want to visualize the relationship between population
density and absolute population, highlighting the two municipalities
with highest population and density. We can do that using the functions
`plot()` and `text()` as follows.

    > plot(pop_sel$Poblacio_padro, pop_sel$Density, xlab="Population (inhabitants)",
    +      ylab="Population density")
    > whmaxpop <- which.max(pop_sel$Poblacio_padro)
    > whmaxden <- which.max(pop_sel$Density)
    > text(pop_sel$Poblacio_padro[whmaxpop], pop_sel$Density[whmaxpop],
    +      pop_sel$NomMun[whmaxpop], pos=2)
    > text(pop_sel$Poblacio_padro[whmaxden], pop_sel$Density[whmaxden],
    +      pop_sel$NomMun[whmaxden], pos=1)

![](popabsvsden-1.png)

Note that in the previous code we have used the function `which.max()`
to obtain the position in the input vector that contains the maximum
value.

# Combining data

One of the most common operations required to answer a question with
data is to combine two datasets in some way. Let’s say we want to
compare municipalities in terms of how many vaccine does per 100,000
inhabitants have been administered. For that purpose, we load a second
dataset corresponding to the administered COVID19 vaccine doses by
municipality in Catalonia (`dosis_municipi.csv`) as follows.

    > vac <- read.csv("dosis_municipi.csv", sep=";", stringsAsFactors=FALSE)
    > dim(vac)

    [1] 744515     15

    > head(vac)

      SEXE_CODI SEXE PROVINCIA_CODI PROVINCIA COMARCA_CODI         COMARCA
    1         0 Home              8 Barcelona           17          GARRAF
    2         1 Dona              8 Barcelona           41 VALLES ORIENTAL
    3         1 Dona              8 Barcelona           11  BAIX LLOBREGAT
    4         1 Dona             43 Tarragona           30   RIBERA D'EBRE
    5         1 Dona              8 Barcelona           24           OSONA
    6         0 Home              8 Barcelona           24           OSONA
      MUNICIPI_CODI                   MUNICIPI DISTRICTE_CODI      DISTRICTE DOSI
    1          8270                     SITGES             NA No classificat    2
    2          8209 SANT FOST DE CAMPSENTELLES             NA No classificat    1
    3          8073      CORNELLÀ DE LLOBREGAT             NA No classificat    2
    4         43019                       ASCÓ             NA No classificat    2
    5          8233       SANT PERE DE TORELLÓ             NA No classificat    2
    6          8067                  CENTELLES             NA No classificat    1
            DATA            FABRICANT NO_VACUNAT RECOMPTE
    1 16/03/2021    BioNTech / Pfizer                  56
    2 11/01/2021    BioNTech / Pfizer                  43
    3 31/01/2021    BioNTech / Pfizer                  18
    4 06/05/2021    BioNTech / Pfizer                  11
    5 13/05/2021    BioNTech / Pfizer                   1
    6 16/04/2021 Oxford / AstraZeneca                   8

The column `FABRICANT` contains the vaccine manufacturer. Let’s tally
the number of administered doses per manufacturer.

    > table(vac$FABRICANT)


       BioNTech / Pfizer        J&J / Janssen      Moderna / Lonza 
                  365236                34025               165799 
         No administrada Oxford / AstraZeneca 
                   59915               119540 

The value `No administrada` corresponds to non-administered vaccine
doses. Let’s discard those and work with the corresponding subset of the
data in a `data.frame` object called `vac_admin`.

    > mask <- vac$FABRICANT !="No administrada"
    > vac_admin <- vac[mask, ]
    > dim(vac_admin)

    [1] 684600     15

We want to combine the filtered vaccination data with the population
data.

    > dim(vac_admin)

    [1] 684600     15

    > colnames(vac_admin)

     [1] "SEXE_CODI"      "SEXE"           "PROVINCIA_CODI" "PROVINCIA"     
     [5] "COMARCA_CODI"   "COMARCA"        "MUNICIPI_CODI"  "MUNICIPI"      
     [9] "DISTRICTE_CODI" "DISTRICTE"      "DOSI"           "DATA"          
    [13] "FABRICANT"      "NO_VACUNAT"     "RECOMPTE"      

    > dim(pop_sel)

    [1] 948   6

    > colnames(pop_sel)

    [1] "Codi_ine_5_txt" "NomMun"         "Comarca"        "Poblacio_padro"
    [5] "Superficie_ha"  "Density"       

Note that both datasets have different dimensions and different column
names, so we need combine them using some column that is common in both
datasets. Because our purpose is to compare vaccination rates among
municipalities, we should expect that the name of the municipality could
be use to combine both datasets.

    > head(vac_admin$MUNICIPI)

    [1] "SITGES"                     "SANT FOST DE CAMPSENTELLES"
    [3] "CORNELLÀ DE LLOBREGAT"      "ASCÓ"                      
    [5] "SANT PERE DE TORELLÓ"       "CENTELLES"                 

    > head(pop_sel$NomMun)

    [1] "Abrera"               "Arenys de Mar"        "l'Ametlla del Vallès"
    [4] "Alpens"               "Aguilar de Segarra"   "Alella"              

However, in the first dataset the name of the municipalities is all in
uppercase, while in the second is a combination of upper and lower
cases. As R compares characters in a case-sensitive manner, we won’t be
able to use directly these values to combine the data frames as we can
see from the following attempt to match names.

    > mt <- match(vac_admin$MUNICIPI, pop_sel$NomMun)
    > head(mt, n=20)

     [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA

However, both datasets also have a column with a municipality code:

    > head(vac_admin$MUNICIPI_CODI)

    [1]  8270  8209  8073 43019  8233  8067

    > head(pop_sel$Codi_ine_5_txt)

    [1] 8001 8006 8005 8004 8002 8003

    > mt <- match(vac_admin$MUNICIPI_CODI, pop_sel$Codi_ine_5_txt)
    > head(mt, n=20)

     [1] 160  18 189 704  32 195 198 353 372 208   9 379 574 329 163 783 699 189 303
    [20] 593

Once we know what column in each dataset can be used to combine them, we
can use the `merge()` function, which by default will return only rows
with common values in those two columns. By default, this function will
combine data using columns with identical names. Since in our datasets
the two columns with the common information are named differently, we
need to specify which are these columns in each `data.frame` object by
using the arguments `by.x` and `by.y`.

    > vac_pop_merge <- merge(vac_admin, pop_sel, 
    +                        by.x = "MUNICIPI_CODI", by.y = "Codi_ine_5_txt")
    > dim(vac_pop_merge)

    [1] 681089     20

    > head(vac_pop_merge)

      MUNICIPI_CODI SEXE_CODI SEXE PROVINCIA_CODI PROVINCIA COMARCA_CODI
    1          8001         1 Dona              8 Barcelona           11
    2          8001         0 Home              8 Barcelona           11
    3          8001         0 Home              8 Barcelona           11
    4          8001         1 Dona              8 Barcelona           11
    5          8001         1 Dona              8 Barcelona           11
    6          8001         0 Home              8 Barcelona           11
             COMARCA MUNICIPI DISTRICTE_CODI      DISTRICTE DOSI       DATA
    1 BAIX LLOBREGAT   ABRERA             NA No classificat    1 21/09/2021
    2 BAIX LLOBREGAT   ABRERA             NA No classificat    1 03/06/2021
    3 BAIX LLOBREGAT   ABRERA             NA No classificat    1 07/06/2021
    4 BAIX LLOBREGAT   ABRERA             NA No classificat    1 03/07/2021
    5 BAIX LLOBREGAT   ABRERA             NA No classificat    1 20/04/2021
    6 BAIX LLOBREGAT   ABRERA             NA No classificat    1 21/09/2021
                 FABRICANT NO_VACUNAT RECOMPTE NomMun        Comarca Poblacio_padro
    1    BioNTech / Pfizer                   1 Abrera Baix Llobregat          12538
    2      Moderna / Lonza                   5 Abrera Baix Llobregat          12538
    3      Moderna / Lonza                  28 Abrera Baix Llobregat          12538
    4    BioNTech / Pfizer                  31 Abrera Baix Llobregat          12538
    5 Oxford / AstraZeneca                   6 Abrera Baix Llobregat          12538
    6    BioNTech / Pfizer                   3 Abrera Baix Llobregat          12538
      Superficie_ha  Density
    1       1994.69 628.5689
    2       1994.69 628.5689
    3       1994.69 628.5689
    4       1994.69 628.5689
    5       1994.69 628.5689
    6       1994.69 628.5689

Now the number of rows of the output data frame `vac_pop_merge` is
slightly smaller than `vac_admin`, because it doesn’t keep rows that
didn’t find a match in `pop_sel`.

**Exercise:** Using `vac_pop_merge`, add a new column named
`doses_100K_h` containing how many doses were administered each day per
100,000 inhabitants. The column `RECOMPTE` contains the number of
administered doses. Which municipality administered the most and the
least doses and at which date?

To continue with our analysis, we need to add a column containing a
month as a factor. Thus, we repeat the steps explained in the previous
[practical 7](/practical7/) to convert the column `DATA` to a date,
extract the months and convert them to a factor with ordered levels:

    > vac_pop_merge$month <- months(as.Date(vac_pop_merge$DATA), abbreviate=TRUE)
    > vac_pop_merge$month <- factor(vac_pop_merge$month, levels=c("Jan", "Feb", "Mar", "Apr", "May", "Jun",
    +                          "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"))

Next, we will select the municipalities with high population (more than
200,000 residents) and save them into a data frame called `vac_high`.
Now we want to obtain the total doses administered per inhabitant every
month in this specific municipalities. Again, we take advantege of the
combination of `split` and `sapply` to do this calculation. Finally, we
create a new data frame with the summarised data, including a column
called `muni_type` that identifies the type of municipalities (“High” as
in “High population”) used for extracting this values.

    > vac_high <- vac_pop_merge[vac_pop_merge$Poblacio_padro > 200000,]
    > dosesh_high <- split(vac_high$doses_100K_h, vac_high$month)
    > 
    > total_dosesh_high <- sapply(dosesh_high, sum)
    > 
    > df_high <- data.frame("month"=names(total_dosesh_high),
    +                       "doses_h"=total_dosesh_high,
    +                       "muni_type"="High")
    > 
    > barplot(df_high$doses_h, names.arg = df_high$month,
    +         main="Highly populated municipalities")

![](barplotDosesHighPop-1.png)

**Exercise**: Create a data frame named `df_low` that contains the total
doses per 100,000 inhabitants administered per month in municipalities
with population smaller than 1,000 inhabitants. Make a bar plot showing
the administered doses per inhabitant per month, as we did above.

**Exercise**: Combine the data frames `df_high` and `df_low` into a
single data frame named `df_months` (**Hint**: Make sure that `df_high`
and `df_low` have the same columns, with the same name and in the same
order). Then make a box plot showing the distribution of doses per
inhabitant grouped by the type of municipality (low or high). Do you see
any difference?
