
 # **taxaHFE**
 
 A program to perform hierarchical feature engineering on data with taxonomic organization (i.e., microbiome data, dietary data)

## Table of Contents
- [Download taxaHFE](https://github.com/aoliver44/taxaHFE#download-taxahfe)
- [Quickstart](https://github.com/aoliver44/taxaHFE#quickstart)
- [Flag information](https://github.com/aoliver44/taxaHFE#information-about-the-flags)
- [About](https://github.com/aoliver44/taxaHFE#about)
- [Contribute](https://github.com/aoliver44/taxaHFE#contribute)



-----------------------------



## **Outline of taxaHFE**


![Outline of taxaHFE algorithm](microbial_HFE_flowchart.png "Outline of taxaHFE algorithm")

</br>

------------------------------
## Download taxaHFE


Option 1: The easiest way to get started is pulling the docker image. Please [install docker](https://www.docker.com/) you go this route. 

```
docker pull aoliver44/taxa_hfe:latest
```

Option 2: Alternatively, you can pull this image using Singularity:

```
singularity pull taxaHFE.sif docker://aoliver44/taxa_hfe:latest
```

Option 3: Finally, it's possible to build the image yourself:

1. Download the dockerfile and renv.lock file
2. Navigate to the directory with these files
3. Run the command:

```
docker build -t taxa_hfe:latest .
```
</br>

------------------------------
## Quickstart

Option 1: Run taxaHFE with **YOUR** data:
1. Navigate to the directory containing your data, and start the docker image!
```
docker run --rm -it -v `pwd`:/home/docker -w /home/docker aoliver44/taxa_hfe:latest bash

## or with singularity
singularity run -W `pwd` --bind `pwd`:/home/docker taxaHFE.sif bash
```
2. Run taxaHFE
```
taxaHFE --subject_identifier subject_id --label cluster [FULL METADATA PATH] [FULL INPUT PATH] [FULL OUTPUT PATH]
```
OR

Option 2: Run taxaHFE on **EXAMPLE** data provided:

```
## STEP 1: CLONE THE REPOSITORY
git clone https://github.com/aoliver44/taxaHFE.git && cd taxaHFE

## STEP 2: RUN THE CONTAINER
docker run --rm -it -v `pwd`:/home/docker -w /home/docker aoliver44/taxa_hfe:latest bash

## STEP 3: RUN TAXAHFE 
taxaHFE --subject_identifier Sample --label Category --feature_type factor --format_metaphlan TRUE --write_old_files TRUE --ncores 2 /home/docker/example_inputs/metadata.txt /home/docker/example_inputs/microbiome_data.txt /home/docker/example_inputs/output.txt
```

</br>

### Running taxaHFE on Windows

taxaHFE can run on Windows through [Powershell](https://aka.ms/PSWindows) and [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/). In order for the program to run smoothly, several adjustments are recommended beforehand:

 1) **Prepare wsl (Windows Subsystem for Linux)**

    In Powershell, set the default wsl to Ubuntu. The default will only need to be set once, but wsl will need to be turned on with every taxaHFE run. 
    
    *Note*: Error without setting default, "VS Code Server for WSL closed unexpectedly"

```  
## Powershell       
## change default wsl to Ubuntu
wsl --set-default Ubuntu 

## turn on wsl
wsl                      
```

2) **Enable Ubuntu in Docker Desktop App**

    *Note*: Error without setting adjustment, Directory files do not read in when docker is initiated

-   Settings \> Resources \> WSL Integration \> Enable Ubuntu

</br>

------------------------------
## Information about the flags


```
Hierarchical feature engineering (HFE) for the reduction of features with respects to a factor or regressor
Usage:
    taxaHFE.R [--subject_identifier=<subject_colname> --label=<label> --feature_type=<feature_type> --input_covariates=<path> --subsample=<decimal> --standardized=<TRUE/FALSE> --abundance=<decimal> --prevalence=<decimal> --format_metaphlan=<format> --cor_level=<correlation_level> --write_old_files=<TRUE/FALSE> --ncores=<ncores>] <input_metadata> <input> <output>

Options:
    -h --help  Show this screen.
    -v --version  Show version.
    --subject_identifier name of columns with subject IDs [default: subject_id]
    --label response feature of interest for classification [default: cluster]
    --feature_type of response i.e. numeric or factor [default: factor]
    --input_covariates path to input covariates [default: FALSE]
    --subsample decimal, only let random forests see a fraction of total data [default: 1]
    --standardized is the sum total feature abundance between subjects equal [default: TRUE]
    --abundance pre taxaHFE abundance filter [default: 0.0001]
    --prevalence pre taxaHFE prevalence filter [default: 0.01]
    --format_metaphlan tells program to expect the desired hData style format, otherwise it attempts to coerce into format [default: FALSE]
    --cor_level level of initial correlation filter [default: 0.95]
    --write_old_files write individual level files and old HFE files [default: TRUE]
    --ncores number of cpu cores to use [default: 2]
Arguments:
    input_meta path to metadata input (txt | tsv | csv)
    input path to input file from hierarchical data (i.e. hData data) (txt | tsv | csv)
    output output file name (txt)
```

--subject_identifier: this is a column that identifies the sample or subject ID in the input metadata. All subjectIDs should be unique.

--label: the name of the column in your input metadata that you are trying to predict with HFE. Can be a factor or continous.

--feature_type: is the label a factor or a continous variable?

--format_metaphlan: is the format of the input file in the "MetaPhlAn" style? 

<details>
  <summary>More Infomation on formats</summary>
<!-- empty line -->

```
clade_name	1000	1001	1002	1003	1004	1005
k__Bacteria	99.42133	100	99.99635	100	100	100
k__Bacteria|p__Firmicutes	81.9719	84.5898	78.52073	93.58543	93.06581	87.13719
k__Bacteria|p__Firmicutes|c__Clostridia	79.42476	81.14081	75.78129	89.56234	88.57445	81.24939
k__Bacteria|p__Firmicutes|c__Clostridia|o__Clostridiales	76.51425	77.01005	71.37259	82.59526	80.58246	73.2948
k__Bacteria|p__Firmicutes|c__Clostridia|o__Clostridiales|f__Ruminococcaceae	39.08761	27.1199	18.49715	29.94427	24.60523	29.85498
k__Bacteria|p__Firmicutes|c__Clostridia|o__Clostridiales|f__Ruminococcaceae|g__Ruminococcus	21.41056	14.13104	7.11984	15.77574	16.48518	9.5614
k__Bacteria|p__Firmicutes|c__Clostridia|o__Clostridiales|f__Ruminococcaceae|g__Ruminococcus|s__Ruminococcus_bromii	12.34825	0	5.86474	15.77574	15.23062	4.48636
```

Notice that each taxonomic level is summarized on its own line, and the columns are the samples assessed. **Importantly, taxaHFE needs the column with the taxonomic (hierarchical) information in a column with the header clade_name.** If ```--format_metaphlan FALSE```, the program will try and summarize each level, and coerce the input file into a "MetaPhlAn" style input. Fair warning, if your input taxonomic file has a lot of missing levels (i.e., k__Bacteria|p__Firmicutes|c__Clostridia|o__Clostridiales|f__|g__|s__Clostridiales_sp_OTU12345), they will get summarized to the last known level. Using the previous example, the empty genus and family level will be summarized to f__Clostridiales_unclassified and g__Clostridiales_unclassified. The downside to this method is that, pontentially very different taxa can get grouped together in a _unclassifed level. Taxonomic assignment has come a long way! Try to use a method that results in the most complete information.

</details>
<!-- empty line -->
</br>

--input_covariates: Do you have an additional metadata input (txt | tsv | csv) you want the random forests to consider? 

--subsample: a decimal value for performing stratified subsampling of factor type data. This behavior is to help protect against data-leakage.

--standardized: is the sum total feature abundance between subjects equal? For microbiome data, this might be a relative abundance of microbes file (sum of features within subjects add up to 1). Set to ```FALSE``` if this is not true. Dietary data, for instance, has different sum-totals depending on the subject (people eat different amounts of food!).

--cor_level: what initial correlation threshold (Pearson) to use when comparing child to parent. We use a high threshold (0.95) and encourage this threashold to stay high. We are really after *redundant* features with this step, were are not trying to institute a deep correlation filter.

--write_old_files: should files summarized at each taxa level be written to file? The old HFE program files are written to for use in the Oudah et al. algorithm

--ncores: number of cores to let the random forest use. Not a huge spead up, but if you have the cores available, it can't hurt.


[input_meta]: the file that contains the metadata column you wish to predict with your hierarchical data. This file should contain BOTH your subject_identifier and your metadata label

[input]: your taxonomic or hierarchical feature set. Columns should be your subject_identifier, plus one column labeled clade_name.

**OUTPUTS**

**output_level_[1,2,3...].csv:** summarized files at each taxonomic level (if write_old_files = TRUE)

**output.txt:** taxaHFE processed dataset, with super filter

**output_nosf.txt:** taxaHFE processed dataset, without super filter

**output_old_hfe_label.txt:** Label data for use in the Oudah algorithm (if write_old_files = TRUE)

**output_old_hfe_otu.txt:** OTU data for use in the Oudah algorithm (if write_old_files = TRUE)

**output_old_hfe_taxa.txt:** Taxa data for use in the Oudah algorithm (if write_old_files = TRUE)

**output_plot.pdf:** Figure of the top 10 features identifed by taxaHFE

</br>

------------------------------
## About

We developed software, called taxaHFE (Hierarchical Feature Engineering), which works by first considering the pairwise correlation structure between a taxon and its descendants to prune descendants above a correlation threshold. Next it permutes a random forest on the taxon and remaining descendants to determine how important each is at explaining an intervention or clinical covariate. If, on average, the taxon is the most important feature in the model, the descendants are dropped, otherwise only the descendants more important than the taxon are kept. Last, an optional final filter step considers all features remaining, and again permutes a random forest. Any features which are either below the average importance of all remaining features or have a negative or zero average importance are dropped.  

</br>

------------------------------
## Contribute

Feel free to raise an issue, contribute with a pull request, or reach out!





