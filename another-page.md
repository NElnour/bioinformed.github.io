---
layout: default
title:  "Integrating Protein-Protein and Genetic Interactions"
date:   2019-04-09 01:46:31 -0500
categories: jekyll update
---

## Preamble [^1]
PPIs describe the physical relationships between the proteins, and genetic interactions provide somewhat of a logical framework to organize this information. By integrating both, we can construct a hypothetical pathway that accounts for: redundancy/salvage, activation, and inhibition. For an interaction pair (protein 1 $$\rightarrow$$ protein 2), I define the following terms:

1. **cis-regulates**: if protein 2 is inhibitor of a phenotype, protein 1 inhibits protein 2; protein 2 is an activator, protein 1 activates protein 2.
2. **trans-regulates**: if protein 2 is inhibitor of a phenotype, protein 1 activates protein 2; protein 2 is an activator, protein 1 inhibits protein 2.
3. **equivalent**: functionally equivalent; may be acting in a redundant pathway.
4. **negative-parallels**: protein 2 is in a pathway opposite to the one in which protein 1 acts. The two pathways have the opposite downstream effect with respect to the phenotype measured, but pathway of protein 2 may have smaller/weaker contribution.
5. **positive-parallels**: protein 2 is in a pathway redundant to the one in which protein 1 acts. Both pathways have the same downstream effect with respect to the phenotype measured, but pathway of protein 2 may have smaller/weaker contribution.

The GGI interpretation map for physically interacting components is called an EMAP here. In the absence of the physical interaction assumption, I call it a GMAP. Thus, for the genetic interaction BioGRID tags, we have the following mappings:

| **BioGRID tag** | **EMAPping** | **GMAPping** |
**Dosage Growth Defect** | inhibited by | negative-parallels |
**Dosage Lethality** | inhibited by | negative-parallels |
**Dosage Rescue** | equivalent/activates | positive-parallels |
**Negative Genetic** | cis-regulates | synergizes with |
**Phenotypic Enhancement** | trans-regulates | synergizes with |
**Phenotypic Suppression** | cis-regulates | antagonizes |
**Positive Genetic** | trans-regulates | synergizes with |
**Synthetic Growth Defect** | equivalent/activates | synergizes with |
**Synthetic Haploinsufficiency** | equivalent/activates | synergizes with |
**Synthetic Lethality** | equivalent/activates | synergizes with |
**Synthetic Rescue** | inhibited by | antagonizes|

## The Genetic Additivity Plane
I am going to be using data from Shen *et al*.'s combinatorial CRISPR-Cas 9 screen  [^2]. I reorganzed the data of the two tables for the purposes of this exploration. Both new tables can be found here: [S3](www.github.com/Nelnour/nelnour.github.io/_includes/nmeth.4225-S3.xlsx) and [S4](www.github.com/Nelnour/nelnour.github.io/_includes/nmeth.4225-S4.xlsx). The study reports on the results of 141,912 potential interactors in three cancer cell lines: HeLa, A549, and 293T.

~~~ R
library(readxl)
library(ggplot2)
library(scatterplot3d)

sko <- read_excel("~/data/ideker/nmeth.4225-S3.xlsx",  sheet = "Table_S2B_CRISPR-KO-assay")
sko <- sko[complete.cases(sko),  ]
head(sko)
# A tibble: 6 x 33
  gene_gene geneA geneB fA_HeLa  fB_HeLa `fA+fB_HeLa`  pi_HeLa sd_HeLa
  <chr>     <chr> <chr>   <dbl>    <dbl>        <dbl>    <dbl>   <dbl>
1 ABL1_ADA  ABL1  ADA   -0.0109 -0.0234      -0.0343  -0.0387   0.0360
2 ABL1_AKT1 ABL1  AKT1  -0.0109 -0.0141      -0.0250  -0.00933  0.0376
3 ABL1_ALK  ABL1  ALK   -0.0109 -0.0136      -0.0245  -0.0154   0.0208
4 ABL1_APC  ABL1  APC   -0.0109 -0.0102      -0.0211  -0.0113   0.0195
5 ABL1_ARI… ABL1  ARID… -0.0109 -0.0160      -0.0269  -0.0296   0.0322
6 ABL1_ARI… ABL1  ARID2 -0.0109  0.00519     -0.00570 -0.0119   0.0165
# … with 25 more variables: PP_HeLa <dbl>, abs_pi_HeLa <dbl>,
#   FDR_left_HeLa <dbl>, FDR_right_HeLa <dbl>, HeLa_Z <dbl>,
#   fA_A549 <dbl>, fB_A549 <dbl>, `fA+fB_A549` <dbl>, pi_A549 <dbl>,
#   sd_A549 <dbl>, PP_A549 <dbl>, abs_pi_A549 <dbl>,
#   FDR_left_A549 <dbl>, FDR_right_A549 <dbl>, A549_Z <dbl>,
#   fA_293T <dbl>, fB_293T <dbl>, `fA+fB_293T` <dbl>, pi_293T <dbl>,
#   sd_293T <dbl>, PP_293T <dbl>, abs_pi_293T <dbl>,
#   FDR_left_293T <dbl>, FDR_right__293T <dbl>, `293T_Z` <dbl>
 
dko <- read_excel("~/data/ideker/nmeth.4225-S4.xlsx", sheet = "TableS3B_CRISPR-KO-hits",  skip = 1)
head(dko)
# A tibble: 6 x 18
  Interaction geneA geneB HeLa_Z  A549_Z `293T_Z` Hit_Cell_Line
  <chr>       <chr> <chr>  <dbl>   <dbl>    <dbl> <chr>        
1 BRCA1_WEE1  BRCA1 WEE1  -0.421 -1.10      -5.86 293T         
2 BRCA2_CDK9  BRCA2 CDK9  -0.581  1.23      -5.13 293T         
3 FNTA_TOP1   FNTA  TOP1  -1.70  -2.05      -4.89 293T         
4 CHEK1_IGF1R CHEK1 IGF1R -1.69  -1.73      -4.78 293T         
5 CHEK1_VEGFA CHEK1 VEGFA -2.32  -2.48      -4.60 293T         
6 CHEK1_PTEN  CHEK1 PTEN   0.162  0.0941    -4.52 293T         
# … with 11 more variables: Interaction_type <chr>, `Conserved?` <chr>,
#   `Validation?` <chr>, `PriorSL_report?` <chr>, `PMID / ref` <chr>,
#   ExHeLa_geneA <chr>, ExHeLa_geneB <chr>, ExA549_geneA <chr>,
#   ExA549_geneB <chr>, Ex293T_geneA <dbl>, Ex293T_geneB <dbl>

hela <- data.frame(sko$fA_HeLa,  sko$fB_HeLa,  sko$`fA+fB_HeLa` )
 hela <- hela[complete.cases(hela),  ]

plt2 <- scatterplot3d(hela,  angle = 55,  color = "skyblue",  xlab = "dA",  ylab = "dB",  zlab = "dAdB")
my.lm <- lm(hela$sko..fA.fB_HeLa.  ~  hela$sko.fA_HeLa  +  hela$sko.fB_HeLa)
plt2$plane3d(my.lm)
plt2$points3d(seq(10, 20, 2),  seq(85, 60, -5),  seq(60, 10, -10),  col = "red",  pch = 8)
~~~

![hela_corr2](/imgs/hela_corr2.png)

##  Linear Deviation from Additivity
Let's add the double knock-outs. First we need to convert from Z scores to $$f_c$$ values. Shen * et al*. state that the provided Z scores were obtained by dividing $$\pi_{pp'}$$ by the corresponding standard deviations.

~~~ R
library(dplyr)

expressionDKO <- data.frame(dko$Interaction,  dko$geneA,  dko$geneB,  dko$HeLa_Z,  dko$Hit_Cell_Line)
expressionDKO <- expressionDKO[complete.cases(expressionDKO), ] 
expressionDKO$dko.Hit_Cell_Line <- as.character(expressionDKO$dko.Hit_Cell_Line)
expressionDKO <- filter(expressionDKO,  grepl(pattern = "Hela",  expressionDKO$dko.Hit_Cell_Line))
 
## convert to fitness scores
sko$gene_gene <- as.character(sko$gene_gene)
expressionDKO$dko.Interaction <- as.character(expressionDKO$dko.Interaction)
sds <- filter(sko, (sko$gene_gene  %in%  expressionDKO$dko.Interaction))
 
expressionDKO <- expressionDKO[ order(expressionDKO$dko.Interaction),  ]
 
expressionDKO <- cbind(expressionDKO,  data.frame(raw_scores = (expressionDKO$dko.HeLa_Z  *  sds$sd_HeLa)))

# plot scatterplot

helaHITs <- cbind(sds$fA_HeLa,  sds$fB_HeLa,  expressionDKO$raw_scores)
 
plt3 <- ggplot(hela, aes(x = sko$gene_gene,  y = sko..fA.fB_HeLa.)) + geom_point( colour = "skyblue",  alpha = 0.5) + 
    theme(axis.text.x = element_blank()) + scale_y_continuous(name="Additivity") + scale_x_discrete(name="GGI") 
    
plt3 + geom_point(data= expressionDKO, aes(x = expressionDKO$dko.Interaction, y = expressionDKO$raw_scores), colour = "salmon") + 
    geom_text(data = expressionDKO, aes(x = expressionDKO$dko.Interaction, y = expressionDKO$raw_scores, label=dko.Interaction))
~~~
![sko_dko](/imgs/sko_dko.png)

After subtraction of the additive effect, the following is the number of fitness standard deviations the double knockouts impart:
~~~ R
library(ggplot2)

normHITs <- merge(expressionDKO,  sds,  by.x="dko.Interaction",  by.y = "gene_gene")
 
ggplot(normHITs, aes((raw_scores  - `fA+fB_HeLa`) / sd_HeLa )) + geom_histogram(bins = 25) + 
    theme_classic() + geom_vline(xintercept = 0, col = "coral1", linetype="dashed", size=1) + scale_x_continuous(name="Deviation from Additivity")
~~~

![lin_dev_from_add](/imgs/lin_dev_from_add.png)

The dashed red line is the baseline for additivity: the farther away a score is from it, the stronger the epistatic relationship. Let's list the genes of interest then:
~~~ R
print(unique(c(as.character(expressionDKO$dko.geneA),  as.character(expressionDKO$dko.geneB))))
[1] "APC"      "ARID1A"   "BRCA1"    "CDK4"     "CDK9"     "CHEK1"    "ERBB2"    "HSP90AA1" "KDM5C"    "MAP2K1"   "PRKDC"   
[12] "RRM2"     "VHL"      "SMAD4"    "SMO"      "TOP1"
~~~

Now let's classify according to BioGrid identifiers given that the phenotype measured is viability:
~~~ R
classifiers <- ifelse(((normHITs$raw_scores - normHITs$`fA+fB_HeLa`) / normHITs$sd_HeLa) > 0,
                      "Positive Genetic", "Negative Genetic")
 
mySys <- data.frame(A = as.character(normHITs$dko.geneA), 
                    B = as.character(normHITs$dko.geneB), 
                    type = as.character(classifiers), stringsAsFactors = FALSE)
 
~~~

Finally, we will integrate the genetic interaction classifiers with PPI information from BioGRID. The University of Toronto  BCB420 class this year has compiled 
several  tools for exploratory systems analyses, one of which serves to predict functional relationships between gene products under the constraint that they are also physical interactors.
The R package, [BCB420.2019.ESA](https://github.com/hyginn/BCB420.2019.ESA), can be installed in and loaded  to RStudio as follows:

~~~ R
devtools::install_github("hyginn/BCB420.2019.ESA")
BiocCheck::BiocCheck("/<path to package>/BCB420.2019.ESA")
library(BCB420.2019.ESA)
~~~

We will use some of these tools to predict regulatory relationships between the hits in Shen *et al.*'s dataset:
~~~ R
biogrid <- fetchData("BioGRID")
allPPI <- biogrid[biogrid$type == "physical", ]

sel1 <-(mySys$A %in% allPPI$A) & (mySys$B %in% allPPI$B)
sel2 <-  (mySys$A %in% allPPI$A) & (mySys$B %in% allPPI$B)

sysPPI <- mySys[sel1 | sel2, ]

sysGI <- getGeneticInteractome(sysPPI, "relaxed")

library(visNetwork)

netPlt <- hypothesize(network = sysGI, ppi_ggi = mySys)
netPlt %>% visOptions(selectedBy = "group")
~~~

{% include ShenDataSetNetwork.html %}

EDIT: the drop-down menu allows selection of genetic interactions of system physical interactors in the network (***ggi***, **relaxed** mode) or genetic interactions between  physically-interacting components (***ppi_ggi***, **stringnt** mode). One can zoom-in and out of any node of interest as well.

## Discussion 

Under stringnt criteria the EMAP posulates the following:

In CRISPR/Cas9-treated HeLa cells,

1. CDK9 cis-regulates SMAD4;
2. BRCA1 cis-regulates CHEK1;
3. CHEK1 cis-regulates PRKDC, and SMO;
4. ERBB2 cis-regulates TOP1; and
5. CHEK1, PRKDC, APC, MAP2K1, RRM2, ARID1A, KDM5C, HSP90AA1, and CDK4 all cis-regulate VHL.

The sinks of this network are:

1. TOP1 (DNA Topoisomerase I),
2. SMO (Smoothened, Frizzled Class Receptor),
3. VHL (Von Hippel-Lindau Tumor Suppressor), and
4. SMAD4 (SMAD Family Member 4).

Of those sinks, only VHL and TOP1 were conserved across all three cancer lineage [^2]. The remaining two were unique to HeLa
## References

[^1]:
    Predicting Regulatory Networks from Undirected PPI. (2019). In BCB420 Student Site. Retrieved April 09, 2019, from [http://steipe.biochemistry.utoronto.ca/abc/students/index.php/User:Nada_Elnour/BCB420-2019-ExploratorySystemsAnalysis](http://steipe.biochemistry.utoronto.ca/abc/students/index.php/User:Nada_Elnour/BCB420-2019-ExploratorySystemsAnalysis)

[^2]:
    Shen *et al.* (2017) Combinatorial CRISPR-Cas9 screens for *de novo* mapping of genetic interactions. *Nat Methods*  **14**: 573-576. DOI: [https://doi.org/10.1038/nmeth.4225](https://doi.org/10.1038/nmeth.4225).
