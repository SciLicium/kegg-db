# KEGG db

This repository hosts KEGG pathways and the conversion between NCBI gene id and KEGG gene id for various organisms.

**Currently supported organisms:**
- hsa
- rno
- mmu
- dre
- ssc
- ola

**Last update**

2024-09-12

## File description
1. `kegg_ncbi-geneid_conversion.json` contains:
    - the date/time at which the data were retrieved from KEGG
    - the list of organisms stored in the database
    - a conversion table between NCBI `geneid`s and KEGG gene ids.
2. `KEGG.db_1.0.tar.gz` is an R package, to be used with `clusterProfiler`. It contains the mapping between KEGG gene ids and KEGG pathways, and was created using https://github.com/YuLab-SMU/createKEGGdb.


## Installation of the custom KEGG db
The custom KEGG db relies on package `AnnotationDbi`. To install:
```
mamba create --name R_usekeggdb R bioconductor-annotationdbi
conda activate R_usekeggdb
install.packages("KEGG.db_1.0.tar.gz", repos=NULL, type="source")
```

## Use with clusterProfiler
```
library(KEGG.db)
library(clusterProfiler)

data(geneList, package="DOSE")
gene <- names(geneList)[abs(geneList) > 2]

kk <- enrichKEGG(gene         = gene,
                 organism     = 'hsa',
                 pvalueCutoff = 0.05,
                 use_internal_data = TRUE)
head(kk)
```
The important thing to note here is the `use_internal_data = TRUE` parameter.

When trying to use an organism not present in the KEGG.db package, we get the following error:
```
Error in get_data_from_KEGG_db(species) :
  input species is not supported by KEGG.db...
```
