# QC PROTOCOL

<img src="https://cloud.githubusercontent.com/assets/6741173/8659425/e0da67b8-2978-11e5-96f0-a9a0c4504133.png" alt="datasets" width="600" /><br />

> ### Cohorts and Sub-populations
1. Multi Ethnic Study for Atherosclerosis MESA [Phenotypes](https://dbgap.ncbi.nlm.nih.gov/aa/wga.cgi?view_pdf&permid=687&tlsid=321),
[Top level study](http://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs000209.v11.p3),
[MESA SHARe](http://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs000420.v4.p3),
[MESA CARe](http://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs000283.v5.p3)
    * 4 sub-populations : *Whites, Blacks, Mexicans, Chinese*<br />
2. British Cohort [BC](http://discover.ukdataservice.ac.uk/catalogue/?sn=5560&type=Data%20catalogue) <br />
    * 1 sub-population : *Whites*<br /> 
3. UNAM/INCMNSZ Study for Diabetes [UIDS](http://www.nature.com/nature/journal/v506/n7486/full/nature12828.html) <br />
    * 1 sub-population : *Mexicans*<br /> 
    
> ### Steps for QC of MESA (Use the [BGData](https://github.com/QuantGen/BGData/wiki) package)
1. Refer to code for QC and basic QC results here: http://rpubs.com/sudhaveturi/98450
1. Create file for phenotypes of interest
2.	Remove pedigreed individuals 
3. Retain only Mexicans from the Hispanic population
4.	SNP QC
    * a. Convert the .ped file to .raw file using [plink](http://pngu.mgh.harvard.edu/~purcell/plink/)
    * b. Convert .raw file to .rda file using BGData and load the compressed (.rda) file for all QC
    * c. For each sub-population (for **stratified** analyses)
        - i. Remove individuals from that were removed from the phenotype file (i.e. pedigreed and non-Mexican Hispanics)
        - ii. Remove SNPs with >5% missing values
        - iii. Remove SNPs with <1% minor allele frequency
    * d. Find common SNPs that segregate in at least one of two sub-populations for bi-cluster analyses
    * e. Find common SNPs that segregate in at least one of all four sub-populations for across-cluster analyses
5. Individual QC: For each sub-population (for **stratified** analyses)
    * a. Remove monomorphic SNPs
    * b. Remove individuals with >5% missing values using BGData
    * c. Compute G using BGData
    * d. Remove individuals with off-diagonal in G > 0.05 (i.e. remove cousin-level relationships or higher)
    * e. Remove individuals with diagonal outside [0.85,1.15]
6.	Get final set of individuals and SNPs for each of 6 *populations* (for **across-cluster** and **bi-cluster** analyses), which are:
    * a. Bla-Whi, Bla-Mex, Bla-Chi, Whi-Chi, Whi-Mex, Chi-Mex
7.	In each *population*:
    * a. Recompute G by centering the X matrix but not scaling
9. Final set of individuals post editing:

| Bla  | Chi | Mex | Whi |
| ---- |---- |---- |---- |
| 1028 | 183 | 369 |1886 |

> ### Finally,
10. Remove individuals whose response values for a given trait are outliers/missing 
11. Adjust phenotypes for the fixed effects of covariates (race, sex, and site)
