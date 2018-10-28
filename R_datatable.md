data.table notes
================

    library(data.table)

    ## Warning: package 'data.table' was built under R version 3.5.1

    #dataset
    msleepDT <- as.data.table(ggplot2::msleep)
    irisDT <- as.data.table(iris)

General syntax
--------------

`DT[i, j, by]` - take data.table DT, subset rows i, manipulate columns j
by something.

Importing data
--------------

Selecting rows
--------------

-   `msleepDT[1:3, ]`: selecting rows 1-3
-   `msleepDT[1:3]`: selecting rows 1-3
-   `msleepDT[.N]`: selecting the last row. `.N` is a shortcut for
    `nrow()`

Logical subsetting: + `msleepDT[sleep_total > 15]`: selecting rows with
a column value above 15 + `msleepDT[vore == "omni"]`: selecting rows
with omni vores + `msleepDT[vore %in% c("omni", "carni")]`: selecting
rows with omni vores

To get to the boolean version: `msleepDT[, sleep_total > 15]`

<br>

Selecting columns:
------------------

-   `msleepDT[,1 ]`: selecting first column in a data.table
-   `msleepDT[, .(genus, vore)]`: selecting genus and vore columns.
    `.()` is a shortcut for list
-   `msleepDT[, name]`: returns a vector with all names
-   `msleepDT[, .(name)]`: returns a one column data.table with all
    names

<br>

Calculations in columns
-----------------------

-   `msleepDT[, mean(sleep_total)]`: calculating the mean, no column
    name
-   `msleepDT[, .(mean=mean(sleep_total))]`: calculating the mean and
    giving a colname
-   `msleepDT[, .(mean=mean(sleep_total), median=median(sleep_total))]`:
    calculating multiple columns
-   `msleepDT[, .(name, mean=mean(sleep_total), median=median(sleep_total))]`:
    keeping a column, adding the overall mean and median (value gets
    recylced)

<br>

Calculations by groups
----------------------

-   `msleepDT[, .(mean=mean(sleep_total)), by=vore]`: calculating the
    mean by vore.
-   `msleepDT[, .(mean=mean(sleep_total)), by=.(genus,vore)]`:
    calculating the mean by vore.

<br>

Adding and removing columns
---------------------------

These operations update by reference which means your source table is
affected and not limited to these lines of code!

-   `msleepDT[, colZ := "Z"]`: adding a column with values Z
-   `msleepDT[, colZ := NULL]`: removing column colZ
-   `msleepDT[, c("awake", "awake_s") := .(awake*60, awake*3600)]`:
    updating a column and adding a second one

Functional `:=` operator allows to chain operations without specifyin
the left hand side upfront.

    msleepDT[, `:=`(colZ = "Z")(colK = sleep_total/60)(colX = "X")]
    msleepDT

    ##                               name         genus    vore           order
    ##  1:                        Cheetah      Acinonyx   carni       Carnivora
    ##  2:                     Owl monkey         Aotus    omni        Primates
    ##  3:                Mountain beaver    Aplodontia   herbi        Rodentia
    ##  4:     Greater short-tailed shrew       Blarina    omni    Soricomorpha
    ##  5:                            Cow           Bos   herbi    Artiodactyla
    ##  6:               Three-toed sloth      Bradypus   herbi          Pilosa
    ##  7:              Northern fur seal   Callorhinus   carni       Carnivora
    ##  8:                   Vesper mouse       Calomys    <NA>        Rodentia
    ##  9:                            Dog         Canis   carni       Carnivora
    ## 10:                       Roe deer     Capreolus   herbi    Artiodactyla
    ## 11:                           Goat         Capri   herbi    Artiodactyla
    ## 12:                     Guinea pig         Cavis   herbi        Rodentia
    ## 13:                         Grivet Cercopithecus    omni        Primates
    ## 14:                     Chinchilla    Chinchilla   herbi        Rodentia
    ## 15:                Star-nosed mole     Condylura    omni    Soricomorpha
    ## 16:      African giant pouched rat    Cricetomys    omni        Rodentia
    ## 17:      Lesser short-tailed shrew     Cryptotis    omni    Soricomorpha
    ## 18:           Long-nosed armadillo       Dasypus   carni       Cingulata
    ## 19:                     Tree hyrax   Dendrohyrax   herbi      Hyracoidea
    ## 20:         North American Opossum     Didelphis    omni Didelphimorphia
    ## 21:                 Asian elephant       Elephas   herbi     Proboscidea
    ## 22:                  Big brown bat     Eptesicus insecti      Chiroptera
    ## 23:                          Horse         Equus   herbi  Perissodactyla
    ## 24:                         Donkey         Equus   herbi  Perissodactyla
    ## 25:              European hedgehog     Erinaceus    omni  Erinaceomorpha
    ## 26:                   Patas monkey  Erythrocebus    omni        Primates
    ## 27:      Western american chipmunk      Eutamias   herbi        Rodentia
    ## 28:                   Domestic cat         Felis   carni       Carnivora
    ## 29:                         Galago        Galago    omni        Primates
    ## 30:                        Giraffe       Giraffa   herbi    Artiodactyla
    ## 31:                    Pilot whale Globicephalus   carni         Cetacea
    ## 32:                      Gray seal  Haliochoerus   carni       Carnivora
    ## 33:                     Gray hyrax   Heterohyrax   herbi      Hyracoidea
    ## 34:                          Human          Homo    omni        Primates
    ## 35:                 Mongoose lemur         Lemur   herbi        Primates
    ## 36:               African elephant     Loxodonta   herbi     Proboscidea
    ## 37:           Thick-tailed opposum    Lutreolina   carni Didelphimorphia
    ## 38:                        Macaque        Macaca    omni        Primates
    ## 39:               Mongolian gerbil      Meriones   herbi        Rodentia
    ## 40:                 Golden hamster  Mesocricetus   herbi        Rodentia
    ## 41:                          Vole       Microtus   herbi        Rodentia
    ## 42:                    House mouse           Mus   herbi        Rodentia
    ## 43:               Little brown bat        Myotis insecti      Chiroptera
    ## 44:           Round-tailed muskrat      Neofiber   herbi        Rodentia
    ## 45:                     Slow loris     Nyctibeus   carni        Primates
    ## 46:                           Degu       Octodon   herbi        Rodentia
    ## 47:     Northern grasshopper mouse     Onychomys   carni        Rodentia
    ## 48:                         Rabbit   Oryctolagus   herbi      Lagomorpha
    ## 49:                          Sheep          Ovis   herbi    Artiodactyla
    ## 50:                     Chimpanzee           Pan    omni        Primates
    ## 51:                          Tiger      Panthera   carni       Carnivora
    ## 52:                         Jaguar      Panthera   carni       Carnivora
    ## 53:                           Lion      Panthera   carni       Carnivora
    ## 54:                         Baboon         Papio    omni        Primates
    ## 55:                Desert hedgehog   Paraechinus    <NA>  Erinaceomorpha
    ## 56:                          Potto  Perodicticus    omni        Primates
    ## 57:                     Deer mouse    Peromyscus    <NA>        Rodentia
    ## 58:                      Phalanger     Phalanger    <NA>   Diprotodontia
    ## 59:                   Caspian seal         Phoca   carni       Carnivora
    ## 60:                Common porpoise      Phocoena   carni         Cetacea
    ## 61:                        Potoroo      Potorous   herbi   Diprotodontia
    ## 62:                Giant armadillo    Priodontes insecti       Cingulata
    ## 63:                     Rock hyrax      Procavia    <NA>      Hyracoidea
    ## 64:                 Laboratory rat        Rattus   herbi        Rodentia
    ## 65:          African striped mouse     Rhabdomys    omni        Rodentia
    ## 66:                Squirrel monkey       Saimiri    omni        Primates
    ## 67:          Eastern american mole      Scalopus insecti    Soricomorpha
    ## 68:                     Cotton rat      Sigmodon   herbi        Rodentia
    ## 69:                       Mole rat        Spalax    <NA>        Rodentia
    ## 70:         Arctic ground squirrel  Spermophilus   herbi        Rodentia
    ## 71: Thirteen-lined ground squirrel  Spermophilus   herbi        Rodentia
    ## 72: Golden-mantled ground squirrel  Spermophilus   herbi        Rodentia
    ## 73:                     Musk shrew        Suncus    <NA>    Soricomorpha
    ## 74:                            Pig           Sus    omni    Artiodactyla
    ## 75:            Short-nosed echidna  Tachyglossus insecti     Monotremata
    ## 76:      Eastern american chipmunk        Tamias   herbi        Rodentia
    ## 77:                Brazilian tapir       Tapirus   herbi  Perissodactyla
    ## 78:                         Tenrec        Tenrec    omni    Afrosoricida
    ## 79:                     Tree shrew        Tupaia    omni      Scandentia
    ## 80:           Bottle-nosed dolphin      Tursiops   carni         Cetacea
    ## 81:                          Genet       Genetta   carni       Carnivora
    ## 82:                     Arctic fox        Vulpes   carni       Carnivora
    ## 83:                        Red fox        Vulpes   carni       Carnivora
    ##                               name         genus    vore           order
    ##     conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
    ##  1:           lc        12.1        NA          NA 11.90      NA   50.000
    ##  2:         <NA>        17.0       1.8          NA  7.00 0.01550    0.480
    ##  3:           nt        14.4       2.4          NA  9.60      NA    1.350
    ##  4:           lc        14.9       2.3   0.1333333  9.10 0.00029    0.019
    ##  5: domesticated         4.0       0.7   0.6666667 20.00 0.42300  600.000
    ##  6:         <NA>        14.4       2.2   0.7666667  9.60      NA    3.850
    ##  7:           vu         8.7       1.4   0.3833333 15.30      NA   20.490
    ##  8:         <NA>         7.0        NA          NA 17.00      NA    0.045
    ##  9: domesticated        10.1       2.9   0.3333333 13.90 0.07000   14.000
    ## 10:           lc         3.0        NA          NA 21.00 0.09820   14.800
    ## 11:           lc         5.3       0.6          NA 18.70 0.11500   33.500
    ## 12: domesticated         9.4       0.8   0.2166667 14.60 0.00550    0.728
    ## 13:           lc        10.0       0.7          NA 14.00      NA    4.750
    ## 14: domesticated        12.5       1.5   0.1166667 11.50 0.00640    0.420
    ## 15:           lc        10.3       2.2          NA 13.70 0.00100    0.060
    ## 16:         <NA>         8.3       2.0          NA 15.70 0.00660    1.000
    ## 17:           lc         9.1       1.4   0.1500000 14.90 0.00014    0.005
    ## 18:           lc        17.4       3.1   0.3833333  6.60 0.01080    3.500
    ## 19:           lc         5.3       0.5          NA 18.70 0.01230    2.950
    ## 20:           lc        18.0       4.9   0.3333333  6.00 0.00630    1.700
    ## 21:           en         3.9        NA          NA 20.10 4.60300 2547.000
    ## 22:           lc        19.7       3.9   0.1166667  4.30 0.00030    0.023
    ## 23: domesticated         2.9       0.6   1.0000000 21.10 0.65500  521.000
    ## 24: domesticated         3.1       0.4          NA 20.90 0.41900  187.000
    ## 25:           lc        10.1       3.5   0.2833333 13.90 0.00350    0.770
    ## 26:           lc        10.9       1.1          NA 13.10 0.11500   10.000
    ## 27:         <NA>        14.9        NA          NA  9.10      NA    0.071
    ## 28: domesticated        12.5       3.2   0.4166667 11.50 0.02560    3.300
    ## 29:         <NA>         9.8       1.1   0.5500000 14.20 0.00500    0.200
    ## 30:           cd         1.9       0.4          NA 22.10      NA  899.995
    ## 31:           cd         2.7       0.1          NA 21.35      NA  800.000
    ## 32:           lc         6.2       1.5          NA 17.80 0.32500   85.000
    ## 33:           lc         6.3       0.6          NA 17.70 0.01227    2.625
    ## 34:         <NA>         8.0       1.9   1.5000000 16.00 1.32000   62.000
    ## 35:           vu         9.5       0.9          NA 14.50      NA    1.670
    ## 36:           vu         3.3        NA          NA 20.70 5.71200 6654.000
    ## 37:           lc        19.4       6.6          NA  4.60      NA    0.370
    ## 38:         <NA>        10.1       1.2   0.7500000 13.90 0.17900    6.800
    ## 39:           lc        14.2       1.9          NA  9.80      NA    0.053
    ## 40:           en        14.3       3.1   0.2000000  9.70 0.00100    0.120
    ## 41:         <NA>        12.8        NA          NA 11.20      NA    0.035
    ## 42:           nt        12.5       1.4   0.1833333 11.50 0.00040    0.022
    ## 43:         <NA>        19.9       2.0   0.2000000  4.10 0.00025    0.010
    ## 44:           nt        14.6        NA          NA  9.40      NA    0.266
    ## 45:         <NA>        11.0        NA          NA 13.00 0.01250    1.400
    ## 46:           lc         7.7       0.9          NA 16.30      NA    0.210
    ## 47:           lc        14.5        NA          NA  9.50      NA    0.028
    ## 48: domesticated         8.4       0.9   0.4166667 15.60 0.01210    2.500
    ## 49: domesticated         3.8       0.6          NA 20.20 0.17500   55.500
    ## 50:         <NA>         9.7       1.4   1.4166667 14.30 0.44000   52.200
    ## 51:           en        15.8        NA          NA  8.20      NA  162.564
    ## 52:           nt        10.4        NA          NA 13.60 0.15700  100.000
    ## 53:           vu        13.5        NA          NA 10.50      NA  161.499
    ## 54:         <NA>         9.4       1.0   0.6666667 14.60 0.18000   25.235
    ## 55:           lc        10.3       2.7          NA 13.70 0.00240    0.550
    ## 56:           lc        11.0        NA          NA 13.00      NA    1.100
    ## 57:         <NA>        11.5        NA          NA 12.50      NA    0.021
    ## 58:         <NA>        13.7       1.8          NA 10.30 0.01140    1.620
    ## 59:           vu         3.5       0.4          NA 20.50      NA   86.000
    ## 60:           vu         5.6        NA          NA 18.45      NA   53.180
    ## 61:         <NA>        11.1       1.5          NA 12.90      NA    1.100
    ## 62:           en        18.1       6.1          NA  5.90 0.08100   60.000
    ## 63:           lc         5.4       0.5          NA 18.60 0.02100    3.600
    ## 64:           lc        13.0       2.4   0.1833333 11.00 0.00190    0.320
    ## 65:         <NA>         8.7        NA          NA 15.30      NA    0.044
    ## 66:         <NA>         9.6       1.4          NA 14.40 0.02000    0.743
    ## 67:           lc         8.4       2.1   0.1666667 15.60 0.00120    0.075
    ## 68:         <NA>        11.3       1.1   0.1500000 12.70 0.00118    0.148
    ## 69:         <NA>        10.6       2.4          NA 13.40 0.00300    0.122
    ## 70:           lc        16.6        NA          NA  7.40 0.00570    0.920
    ## 71:           lc        13.8       3.4   0.2166667 10.20 0.00400    0.101
    ## 72:           lc        15.9       3.0          NA  8.10      NA    0.205
    ## 73:         <NA>        12.8       2.0   0.1833333 11.20 0.00033    0.048
    ## 74: domesticated         9.1       2.4   0.5000000 14.90 0.18000   86.250
    ## 75:         <NA>         8.6        NA          NA 15.40 0.02500    4.500
    ## 76:         <NA>        15.8        NA          NA  8.20      NA    0.112
    ## 77:           vu         4.4       1.0   0.9000000 19.60 0.16900  207.501
    ## 78:         <NA>        15.6       2.3          NA  8.40 0.00260    0.900
    ## 79:         <NA>         8.9       2.6   0.2333333 15.10 0.00250    0.104
    ## 80:         <NA>         5.2        NA          NA 18.80      NA  173.330
    ## 81:         <NA>         6.3       1.3          NA 17.70 0.01750    2.000
    ## 82:         <NA>        12.5        NA          NA 11.50 0.04450    3.380
    ## 83:         <NA>         9.8       2.4   0.3500000 14.20 0.05040    4.230
    ##     conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
    ##     colX
    ##  1:    X
    ##  2:    X
    ##  3:    X
    ##  4:    X
    ##  5:    X
    ##  6:    X
    ##  7:    X
    ##  8:    X
    ##  9:    X
    ## 10:    X
    ## 11:    X
    ## 12:    X
    ## 13:    X
    ## 14:    X
    ## 15:    X
    ## 16:    X
    ## 17:    X
    ## 18:    X
    ## 19:    X
    ## 20:    X
    ## 21:    X
    ## 22:    X
    ## 23:    X
    ## 24:    X
    ## 25:    X
    ## 26:    X
    ## 27:    X
    ## 28:    X
    ## 29:    X
    ## 30:    X
    ## 31:    X
    ## 32:    X
    ## 33:    X
    ## 34:    X
    ## 35:    X
    ## 36:    X
    ## 37:    X
    ## 38:    X
    ## 39:    X
    ## 40:    X
    ## 41:    X
    ## 42:    X
    ## 43:    X
    ## 44:    X
    ## 45:    X
    ## 46:    X
    ## 47:    X
    ## 48:    X
    ## 49:    X
    ## 50:    X
    ## 51:    X
    ## 52:    X
    ## 53:    X
    ## 54:    X
    ## 55:    X
    ## 56:    X
    ## 57:    X
    ## 58:    X
    ## 59:    X
    ## 60:    X
    ## 61:    X
    ## 62:    X
    ## 63:    X
    ## 64:    X
    ## 65:    X
    ## 66:    X
    ## 67:    X
    ## 68:    X
    ## 69:    X
    ## 70:    X
    ## 71:    X
    ## 72:    X
    ## 73:    X
    ## 74:    X
    ## 75:    X
    ## 76:    X
    ## 77:    X
    ## 78:    X
    ## 79:    X
    ## 80:    X
    ## 81:    X
    ## 82:    X
    ## 83:    X
    ##     colX

<br>

Chaining operations
-------------------

You can chain multiple operations behind each other by bracketing.

    msleepDT[, .(mean=mean(sleep_total)), by=vore][order(vore)]

    ##       vore      mean
    ## 1:   carni 10.378947
    ## 2:   herbi  9.509375
    ## 3: insecti 14.940000
    ## 4:    omni 10.925000
    ## 5:    <NA> 10.185714

<br>

Looping through means
---------------------

If you want to get the mean of multiple columns, there is a more
efficient way than to repeat a few times `mean(col)`.

-   `.SD`: shortcut that contains all columns, except the ones mentioned
    in `by`.
-   `.SDcols`: specifies which columns of DT to include in .SD

<!-- -->

    irisDT[, lapply(.SD, median), by=Species]

    ##       Species Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1:     setosa          5.0         3.4         1.50         0.2
    ## 2: versicolor          5.9         2.8         4.35         1.3
    ## 3:  virginica          6.5         3.0         5.55         2.0

Using .SDcols to specify which ones:

    irisDT[, lapply(.SD, median), .SDcols = c("Sepal.Width","Sepal.Length"), by=Species]

    ##       Species Sepal.Width Sepal.Length
    ## 1:     setosa         3.4          5.0
    ## 2: versicolor         2.8          5.9
    ## 3:  virginica         3.0          6.5

<br>

Changing and reordering columns
-------------------------------

-   `setnames(DT, "oldcol", "newcol")`: changing a column name
-   `setnames(DT, tolower(names(DT)))`: all columns to lower case

-   \`setcolorder(DT, c("col2", "col1")): swaps ordering
