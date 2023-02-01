## Introduction

This repositories contains a collection of R script and R notebook used to compute, tidy, wrangle, model and visualize data for the following article:

>Tap et al. Global branches and local states of the human gut microbiome


This project started in May 2018. A lot of chunks from those notebook (and even entire notebooks) were not used for the finalized article.

To reproduce this analysis, you may need first to extract microbiome count data from American Gut project (AGP) and CuratedMetagenomicsData (CMD) datasets.
Then for each of this dataset, you may found codes to compute Dirriclet Multinomial Mixture (DMM) Models and PHATE analysis. Time series and multinomial logistic  regression were applied on AGP data.



## Extract raw data

### AGP data


We used redbiom (McDonald et al 2019) to fetch data from Qiita (Gonzalez et al 2018). 20,454 stool sample identifiers were available in the database on the date 2019 December 5th, within the Deblur-Illumina-16S-V4-100nt-fbc5b2 context. Analyses were performed as previously described (Cotillard et al 2021). In short, bioinformatic analysis was performed with QIIME 2019.10, bloom sequences were removed as previously described (Amir et al 2017), and taxonomy was assigned using the GreenGenes database (v 13.5).

you may found scripts in

* data-raw/qiime/

if you want to play with redbiom, I advise you to follow this [tutorial](https://forum.qiime2.org/t/querying-for-public-microbiome-data-in-qiita-using-redbiom/4653)
You need to install redbiom and QIIME2019.10 and then run this sh script 

* submit_all.sh



### CMD data

* data-raw/curatedMetaG

you run this R script

* import_curated_v3.r



### other datasets


in this study we used United Nation Statistical department country database to group country into subregions, you may retrieve data here

* https://datahub.io/core/country-codes/r/country-codes.csv

and code to import

* data-raw/impot_UNSD_countries.r



We also used the [OXYTOL](https://www.mediterranee-infection.com/wp-content/uploads/2020/05/OXYTOL-1.3.xlsx) database v 1.3 was used to associate each microbial genus to an aerotolerant or obligate anaerobic metabolism (Million et al 2016).

* data-raw/import_oxytol.r




## schematic workflow


### inputs

1. raw CMD data (count, taxa, metadata) were packed into

* data-raw/curatedMetaG/curated_v3_otu_tax.rda

2. raw AGP data used in this study could be found here

* genus counts

  * data-raw/qiime/generated_files_20190512/taxa/genus.qza

* metadata

  * data-raw/Metadata_10317_20191022-112414_curatedv4_VSv1.csv


* alpha-diversity (shannon)

  * data-raw/qiime/generated_files_20190512/alpha/shannon.qza

* previoulsy identified outliers

  * notebook/outliers_samples.txt





### computing


**compute gut microbiome partitions and PHATE on AGP data**

* data-raw/qiime/generated_files_20190512/taxa/genus.qza

* data-raw/Metadata_10317_20191022-112414_curatedv4_VSv1.csv

*  data-raw/qiime/generated_files_20190512/alpha/shannon.qza

* notebook/outliers_samples.txt

  * notebook/hierarchical_enterotyping.Rmd (that called /notebook/enterotyping.R)
  
    * notebook/fit_genus_bootstrap.rda
    
    * notebook/enterotypes_prediction_outliers.csv
    
  * notebook/PHATE_analysis.Rmd
    
    * figures/*
    
**benchmark DMM on different CMD datasets**
    
* data-raw/curatedMetaG/curated_v3_otu_tax.rda

  * notebook/enterotypes_power_sampling_*.R 
    

**compute gut micorbiome partitions and PHATE on CMD data**

* data-raw/curatedMetaG/curated_v3_otu_tax.rda

  * notebook/enterotypes_bootstrap_curated_v3.R
    
    * notebook/fit_genus_list_*_curated_v3.rda
      
      * notebook/curated_v3_enterotyping.Rmd
      
        * figures/*
        
        * notebook/genus_alpha_weight_curated.rda
        
        * notebook/enterotypes_curated_v3_prediction.csv
        
  * notebook/PHATE_analysis_curatedMetaG_v3.Rmd (use *genus_alpha_weight_curated.rda* also as input)
  
    * notebook/curated_v3_species_count.rda
      * notebook/diff_analysis_branche_CMD.Rmd
        * notebook/curated_v3_species_clr.rda
        
    * notebook/curated_v3_genus_prop_files.rda
      * notebook/phate_multiscale_python.Rmd
      
    * figures/*
      * notebook/supporting_figures_data.Rmd

**logistic regression**

* notebook/enterotypes_prediction_outliers.csv
* data-raw/Metadata_10317_20191022-112414_curatedv4_VSv1.csv
* data-raw/qiime/generated_files_20190512/alpha/shannon.qza

  * notebook/ordinal_logistic_regression.Rmd
    * notebook/agp_metadata_enterotypes_num.rda
    * notebook/regm2_tidy.rda
    * notebook/regm2_plot.rda
      

**time series analysis**

* notebook/agp_metadata_enterotypes_num.rda
* notebook/enterotypes_prediction_outliers.csv
* data-raw/Metadata_10317_20191022-112414_curatedv4_VSv1.csv
* data-raw/qiime/generated_files_20190512/alpha/shannon.qza

  * notebook/enterobranches_time_series.Rmd
    
    * figures/*

      
      
      
      
      
## R notebooks

HTML R notebook version can be found here : [tapj.github.io/branches/](http://tapj.github.io/branches/)

























