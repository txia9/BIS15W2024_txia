---
title: "Lab 4 Homework"
author: "Tian Xia"
date: "2024-01-30"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- read.csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fishe…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", "…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```


```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
homerange$taxon <- as.factor(homerange$taxon)
```


```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <fct> lake fishes, river fishes, river fishes, ri…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", "…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```


```r
homerange$order <- as.factor(homerange$order)
```


```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```


```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"       "afrosoricida"          "anguilliformes"       
##  [4] "anseriformes"          "apterygiformes"        "artiodactyla"         
##  [7] "caprimulgiformes"      "carnivora"             "charadriiformes"      
## [10] "columbidormes"         "columbiformes"         "coraciiformes"        
## [13] "cuculiformes"          "cypriniformes"         "dasyuromorpha"        
## [16] "dasyuromorpia"         "didelphimorphia"       "diprodontia"          
## [19] "diprotodontia"         "erinaceomorpha"        "esociformes"          
## [22] "falconiformes"         "gadiformes"            "galliformes"          
## [25] "gruiformes"            "lagomorpha"            "macroscelidea"        
## [28] "monotrematae"          "passeriformes"         "pelecaniformes"       
## [31] "peramelemorphia"       "perciformes"           "perissodactyla"       
## [34] "piciformes"            "pilosa"                "proboscidea"          
## [37] "psittaciformes"        "rheiformes"            "roden"                
## [40] "rodentia"              "salmoniformes"         "scorpaeniformes"      
## [43] "siluriformes"          "soricomorpha"          "squamata"             
## [46] "strigiformes"          "struthioniformes"      "syngnathiformes"      
## [49] "testudines"            "tetraodontiformes\xa0" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
homerange_new <- rename(homerange, common_name="common.name")
```


```r
taxa <- select(homerange_new, "taxon", "common_name", "class", "order", "family", "genus", "species")
```

**5. The variable `taxon` identifies the common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange_new$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange_new$species)
```

```
## 
##                   aberti                adspersus                    aedon 
##                        2                        1                        1 
##                aegyptius              aethiopicus         africaeaustralis 
##                        1                        1                        1 
##                 africana                africanus                agassizii 
##                        1                        1                        1 
##                 agrestis                     alba                albicauda 
##                        1                        2                        1 
##                    alces                    aluco                americana 
##                        1                        1                        4 
##               americanus                    ammon                  amoenus 
##                        3                        1                        1 
##                   analis                annularis                 anomalum 
##                        1                        1                        1 
##              antilopinus                 antimena                 apicalis 
##                        1                        1                        1 
##                   apodus                aquaticus                  arborea 
##                        1                        3                        2 
##                 arcticus                areolatus         argenteocinereus 
##                        2                        1                        1 
##                     argi                    argus                  aruanus 
##                        1                        1                        1 
##                    asper                assimilis               atlanticus 
##                        1                        1                        1 
##             atricapillus                    atrox                audeberti 
##                        2                        1                        1 
##                  auratus                  auritus             aurocapillus 
##                        2                        1                        1 
##             aurofrenatum                australis             avellanarius 
##                        1                        1                        1 
##                     axis                   bairdi                  barbata 
##                        1                        1                        1 
##                baronessa                 beecheyi                    belli 
##                        1                        1                        1 
##              bengalensis                 bergylta                 bewickiI 
##                        1                        1                        1 
##              bezoarticus                biarmicus                  bicolor 
##                        1                        1                        1 
##                 bicornis              bifasciatum               biocellata 
##                        1                        1                        1 
##                    bison               bivittatus               blandingii 
##                        1                        1                        1 
##                   bonaci                  bonasia                  bonasus 
##                        1                        1                        1 
##                  bonelli                   bottae                     bubo 
##                        1                        1                        1 
##              bungaroides               buselaphus                    buteo 
##                        1                        1                        1 
##                  butleri                 caballus                 cabrilla 
##                        1                        1                        1 
##            californianus             californicus               callipygus 
##                        1                        3                        1 
##           camelopardalis                  camelus               campestris 
##                        1                        1                        1 
##               canadensis                cannabina                  canorus 
##                        3                        1                        1 
##                    canus                 capensis                capreolus 
##                        1                        2                        1 
##                  caracal               carbonaria                carolinae 
##                        1                        1                        1 
##             carolinensis            caryocatactes               cataractae 
##                        2                        1                        1 
##                catenifer                    catus                 caudatus 
##                        1                        1                        1 
##                 caurinus                 cerastes                 cheriway 
##                        1                        1                        1 
##                  chromis               chrysaetos             chrysopterum 
##                        1                        1                        1 
##              chrysopygus                chrysurus                  cinerea 
##                        1                        1                        1 
##                 cinereus                   citrea                   clarki 
##                        1                        1                        1 
##               clathratus                  coelebs                 collurio 
##                        1                        1                        1 
##              columbianus                 concolor              constrictor 
##                        1                        1                        1 
## constrictor flaviventris               contortrix                  cooperi 
##                        1                        1                        1 
##                 cooperii                    corax                coronatus 
##                        1                        1                        1 
##                  couperi                     crex                 cristata 
##                        1                        1                        1 
##                cristatus                cruentata                 culpaeus 
##                        1                        1                        1 
##                cuniculus          cupido pinnatus                curzoniae 
##                        1                        1                        1 
##                  cuvieri                   cyanea                  cyaneus 
##                        1                        1                        1 
##                   cyanus                  cyclura                    dahli 
##                        1                        1                        1 
##                    dalli                     dama               damarensis 
##                        1                        1                        1 
##               decussatus             dentirostris               dimidiatus 
##                        1                        1                        1 
##                 dolomieu                 dorsalis                 dorsatum 
##                        1                        3                        1 
##                  elaphus                  elegans                    epops 
##                        1                        2                        1 
##                  erminea            erythrogaster               erythropus 
##                        1                        1                        1 
##                 europaea                europaeus                   exilis 
##                        2                        3                        1 
##                    falco               familiaris                 fasciata 
##                        1                        1                        1 
##                fasciatus                    ferox              flagellifer 
##                        1                        1                        1 
##                flagellum                    flava               flavescens 
##                        1                        1                        1 
##              flavicollis            flavimaculata             flaviventris 
##                        1                        1                        1 
##            flavolineatus                   flavus               floridanus 
##                        1                        1                        1 
##                    foina                 fraenata               franklinii 
##                        1                        1                        1 
##                  frenata                  fulgens              fuliginosus 
##                        1                        1                        1 
##              funduloides                 funereus                     furo 
##                        1                        1                        1 
##                    fusca               fuscicauda                 fuscipes 
##                        1                        1                        1 
##                   fuscus                 gaimardi                 gallicus 
##                        2                        1                        1 
##                  galloti                  garnoti                 garrulus 
##                        1                        1                        1 
##                  gazella                  genetta                 gentilis 
##                        1                        1                        1 
##                geoffroii                geoffroyi            getula getula 
##                        1                        1                        1 
##                 gibbosus                    gigal                giganteus 
##                        1                        1                        1 
##                    gilae                   glaber               glandarius 
##                        1                        1                        1 
##                    gobio               gossypinus              gracillimus 
##                        1                        1                        1 
##                   graeca                   granti                  griseus 
##                        1                        1                        3 
##                guentheri                     gulo           guttata emoryi 
##                        1                        1                        1 
##                 guttatus               guttulatus              habroptilus 
##                        1                        1                        1 
##                    harak                 hemionus              hemistiktos 
##                        1                        1                        1 
##                 hermanii                 hipoliti                   hircus 
##                        1                        1                        1 
##                 hispidua                 horridus               horsfieldi 
##                        1                        1                        1 
##              hottentotus               hudsonicus                   huntii 
##                        1                        1                        1 
##                hurrianae               idahoensis             ignicapillus 
##                        1                        1                        1 
##                ignobilis                 impressa                     inca 
##                        1                        1                        1 
##               inchneumon                   indica                  inermis 
##                        1                        1                        1 
##                   ingens                inornatus                    iseri 
##                        1                        1                        1 
##              jamaicensis                 japonica                johnstoni 
##                        1                        1                        1 
##                  jubatus                    julis                 juncidis 
##                        1                        1                        1 
##                kirtlandi               kleinmanni                 krefftii 
##                        1                        1                        1 
##                   labrax                  lagopus                 latastei 
##                        1                        2                        1 
##                leopardus                   lepida                  leprosa 
##                        1                        1                        1 
##                   lervia                 leucopus                 leucotos 
##                        1                        1                        1 
##                   lewisi                 lineatus               lineolatus 
##                        1                        2                        1 
##                      lis                lituratus              longicollis 
##                        1                        1                        1 
##                 longipes              longipinnis              longissimus 
##                        1                        1                        1 
##             ludovicianus           lumbriciformis                lumholtzi 
##                        2                        1                        1 
##                   lunare                  luridus                 lutreola 
##                        1                        1                        1 
##                     lynx              maccullochi              macrochirus 
##                        1                        1                        1 
##                  macroti                maculatus              maculipinna 
##                        1                        1                        1 
##                    magna                 magnolia                    major 
##                        1                        1                        1 
##                  maliger               marginatus               marmoratus 
##                        1                        1                        1 
##                   martes                  martius              masquinongy 
##                        1                        1                        1 
##                  maximus                   medius             megacephalus 
##                        2                        1                        1 
##                megalotis                 melampus              melanoleuca 
##                        1                        1                        1 
##             melanoleucus                 melanops                 merriami 
##                        1                        1                        1 
##                mexicanus                 micropus              microrhinos 
##                        1                        1                        1 
##                   milvus                  miniata                  minimus 
##                        1                        1                        2 
##                    minor           minor peltifer                 molossus 
##                        1                        1                        1 
##                    monax                 montanus                    morio 
##                        1                        1                        1 
##                 mustinus                   mykiss                  natalis 
##                        1                        1                        1 
##                   natrix                nebulifer                 neglecta 
##                        1                        1                        1 
##                nicholsii                    niger              nigricollis 
##                        1                        1                        1 
##                 nigripes                   nippon                    nisus 
##                        1                        1                        1 
##                  nivalis                   noctua                 obesulus 
##                        1                        1                        1 
##                   obesus                 obscurus                 obsoleta 
##                        1                        1                        1 
##              ochrogaster                 ocularis                 odoratus 
##                        1                        1                        1 
##                 oenanthe                olivaceus                     onca 
##                        1                        1                        1 
##              orbicularis                    ordii        oreganus concolor 
##                        1                        1                        1 
##                   ornata                     oryx               ostralegus 
##                        2                        1                        1 
##                     otus                palliatus                 pallidus 
##                        1                        1                        1 
##              paludinosus                 palumbus                palustris 
##                        1                        1                        2 
##                 pardalis                 pardinus                   pardus 
##                        2                        1                        1 
##                  parryii                 partitus                  parvula 
##                        1                        1                        1 
##                passerina               passerinum                patagonus 
##                        1                        1                        1 
##                   pecari              penicillata                 pennanti 
##                        1                        1                        2 
##                  pennata                 pennatus           pennsylvanicus 
##                        1                        1                        1 
##             pensylvanica             percnopterus                   perdix 
##                        1                        1                        1 
##               peregrinus                 petechia             philadelphia 
##                        1                        1                        1 
##              phoenicurus          picta marginata                pinetorum 
##                        1                        1                        1 
##                  pinguis                    pinos              piscivorous 
##                        1                        1                        1 
##              platirhinos                poecilura                    poeyi 
##                        1                        1                        1 
##               pollachius               polyglotta              polyglottos 
##                        1                        1                        1 
##               polyphemus               porphyreus             porphyriacus 
##                        1                        1                        1 
##              prehensilis                   pricei                 princeps 
##                        1                        1                        2 
##                     puda                  pulcher                  pumilio 
##                        1                        1                        1 
##                punctatus                 pygargus                pyrenaica 
##                        1                        1                        1 
##           quadrivittatus                   raddei               radiolosus 
##                        1                        1                        1 
##                raimondii              raviventris                  reevesi 
##                        1                        1                        1 
##                  regulus              reticularia              richardsoni 
##                        1                        1                        1 
##                rivulatus                 robustus                   romana 
##                        1                        1                        1 
##                 rostrata                  rubetra                   rubica 
##                        2                        1                        1 
##               rubripinne                rubrubrum                     rufa 
##                        1                        1                        1 
##                rufescens             rufodorsatus              rufogriseus 
##                        1                        1                        1 
##                    rufus                rupestris                rupicapra 
##                        4                        1                        1 
##                 ruppelli                  russula                ruticilla 
##                        1                        1                        1 
##                 sabrinus                    salar                salmoides 
##                        1                        1                        1 
##                    salpa                    sarda               savannarum 
##                        1                        1                        1 
##                scandiaca             schisticolor               schneideri 
##                        1                        1                        1 
##                   scriba                 scriptus                 scutatus 
##                        1                        1                        1 
##               scutulatus                sectatrix             semispinosus 
##                        1                        1                        1 
##                  senator                  serinus               serpentina 
##                        1                        1                        1 
##                   serval              shedaoensis                   sialis 
##                        1                        1                        1 
##                 sibirica                sibiricus                 simensis 
##                        1                        2                        1 
##                    simum                 sipeodon               sparverius 
##                        1                        1                        1 
##              spectabilis             spectrabilis                   spekii 
##                        1                        1                        1 
##                spilosoma        spilota imbricata                spinifera 
##                        1                        1                        1 
##                splendens                stellaris                stephensi 
##                        1                        1                        1 
##               stigmatica                 strepera             strepsiceros 
##                        1                        1                        1 
##                  striata                 striatus                 stuartii 
##                        1                        2                        1 
##                  suillus                swainsoni               sylvaticus 
##                        1                        1                        1 
##               sylvestris                   tajacu                 tarandus 
##                        1                        1                        1 
##                  tauvina                    taxus                    telum 
##                        1                        1                        1 
##                tentorius            tetradactylus                   tetrix 
##                        1                        1                        1 
##                   thetis                  tigrina                   tigris 
##                        1                        1                        2 
##                  timidus              tinnunculus                torquatus 
##                        1                        1                        1 
##                torquilla                 torridus             travancorica 
##                        1                        1                        1 
##               trevelyani               triangulum                  trichas 
##                        1                        1                        1 
##                trichrous              tridactylus         tridecemlineatus 
##                        1                        1                        1 
##             trifascialis             trifasciatus              troglodytes 
##                        1                        1                        1 
##                   trutta                   turtur                 tyrannus 
##                        1                        1                        1 
##                 umbrinus                    uncia                   undata 
##                        1                        1                        1 
##                undulatus             unguiculatus                unicornis 
##                        1                        1                        1 
##             unimaculatus                urogallus             urophasianus 
##                        1                        1                        1 
##                  ursinus               variabilis               variegatus 
##                        2                        1                        1 
##                    velox                   vermis                   virens 
##                        1                        1                        4 
##              virginianus                   viride             viridiflavus 
##                        2                        1                        1 
##                  viridis                  vittata                   volans 
##                        2                        1                        2 
##                 vulgaris                vulpecula                  wagneri 
##                        1                        1                        1 
##                    wardi                   wiedii                    wolfi 
##                        1                        1                        1 
##                 wrightii             yagouaroundi                  zibetha 
##                        1                        1                        1 
##                zibethica 
##                        1
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- filter(homerange_new, trophic.guild == "carnivore")
```


```r
herbivores <- filter(homerange_new, trophic.guild == "herbivore")
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
mean(carnivores$mean.hra.m2, na.rm = T)
```

```
## [1] 13039918
```


```r
mean(herbivores$mean.hra.m2, na.rm = T)
```

```
## [1] 34137012
```

Herbivores have a larger average of "mean.hra.m2".  

**9. Make a new dataframe `owls` that is limited to the mean mass, log10 mass, family, genus, and species of owls in the database. Which is the smallest owl? What is its common name? Do a little bit of searching online to see what you can learn about this species and provide a link below** 

```r
owl <- homerange %>% 
  select("mean.mass.g", "log10.hra", "family", "genus", "species", "common.name", "order") %>% 
  filter(order == "strigiformes") %>% 
  arrange(mean.mass.g)
```

The smallest owl is Glaucidium passerinum. Its common name is Eurasian pygmy owl. For more information, please see the link below: https://animaldiversity.org/accounts/Glaucidium_passerinum/  


**10. As measured by the data, which bird species has the largest homerange? Show all of your work, please. Look this species up online and tell me about it!**.  

```r
bird <- homerange %>% 
  select("mean.hra.m2", "taxon", "common.name", "family", "genus", "species", "order") %>% 
  filter(taxon == "birds") %>% 
  arrange(desc(mean.hra.m2))
```

The cheriway has the largest homerange. Please see the link below for more information: https://animaldiversity.org/accounts/Caracara_cheriway/  

##Please be sure that you check the `keep md` file in the knit preferences.   
