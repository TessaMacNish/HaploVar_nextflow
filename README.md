
# Nextflow Pipeline for GATK SNP Calling

## What does this pipeline do?

This pipeline runs the haplotyping tool `HaploVar` and all the preparation steps prior to running HaploVar including creating the linkage disequilibrium matrices. The tools used in this pipeline are HaploVar version 0.1.2 (MacNish et al., 2025), BCFtools version 1.15 (Danecek et al., 2021), PLINK v1.90b6.21 (Purcell et al., 2007, https://zzz.bwh.harvard.edu/plink/ld.shtml), Beagle version 5.4 (Browning et al., 2018; Browning et al., 2021), and R version 4.5.2 (R Core Team, 2025).

For more information on `HaploVar` and what it does please see its GitHub page (https://github.com/TessaMacNish/HaploVar).

## Running this pipeline using only default parameters

1) Set up your directories. In your chosen work directory (the directory you are going to run nextflow from) set up the following directories as shown by the tree command below.
``` bash
tree
.
├── Haplotype
├── LD
└── vcf_prep
```
2) You will need 4 files downloaded into your work directory:
   a) input.vcf -  this is the VCF file you want to calculate haplotypes for.
   b) main.nf - is the processes that nextflow will run and is available in this github page.
   c) nextflow.config - tells nextflow the time and memory requirements for each process and is also avaiable on this github page.
   d) regions.txt - HaploVar calculates haplotypes per chromosome, so the VCF needs to be split into seperate chromosomes. regions.txt is the file to specify how your chromosomes are named. An example regions.txt is below:
``` bash
A01
A02
A03
A04
A05
A06
A07
A08
A09
A10
C01
C02
C03
C04
C05
C06
C07
C08
C09
```
   If you have a very large dataset you may need to split the dataset into segements smaller than chromosmes, as HaploVar can be computationaly intensive on large datasets. For this case, an example regions.txt is below:

``` bash
A01:1-4118753
A01:4118754-8237506
A01:8237507-12356259
A01:12356260-16475012
A01:16475013-20593765
A01:20593766-24712518
A01:24712519-28831271
A01:28831272-32950026
A02:1-4179105
A02:4179106-8358210
A02:8358211-12537315
A02:12537316-16716420
A02:16716421-20895525
A02:20895526-25074630
A02:25074631-29253735
A02:29253736-33432838
```

It is important to note that your final haplotype file will be in the same order as your regions.txt file.

3) To run this nextflow pipeline you will need sigularity installed. In the nextflow.config file the following line loads singularity.
``` bash
beforeScript = 'module load singularity/4.1.0-nompi'
```
You will need to change this line to the version of singularity available on your CPU. The above singularity module is available on Pawsey super computer's Setonix server. If a singularity module is not available on your CPU you can install singlarity using conda or another package manager. If using a version of singularity isntalled inside a conda environment you can change the module load line to the example below, ensuring you replace myenv with the name of your conda environment.
``` bash
beforeScript = 'conda activate myenv'
```

4) You can now run nextflow from your working directory.
``` bash
nextflow run main.nf
```
You can run nextflow with the additional parameters if you want better logs and traceback, which are useful if something goes wrong.
``` bash
nextflow run main.nf -with-trace -with-report -with-timeline
```
5) nextflow will copy all relevant output into the directories we set in step 1.

Haplotype - The haplotype files for each chromosome or segemnt and the final concatenated haplotype file. This is your final output.

LD - The linkage disequilibrium matrices for each chromosome segement used to calculate haplotypes.

vcf_prep - Has the output of the preperation steps such as the chromosome region VCFs.


Nextflow will also make a directory called work, which will have all of these results as well as the logs. 

## Running this pipeline defining your own parameters
The purpose of the parameters is to make the pipline more flexible, so that the user can change the input and output file and directory names. All parameters are described below.

`input_VCF` - This is the VCF file that you want to calculate haplotypes for. By default it is named input.vcf. 

`regions` - This is the name of your regions.txt file.

`vcf_prep` - This is the directory which stores all of the preperation files.

`LD` - This is the directory where all linkage disequilibrium matrices are stored. 

`haplotype` - This parameter controls where your final output is stored. 

`epsilon` - Is a parameter used by `HaploVar`. epsilon and MGmin influence the density and amount of noise within the calculated haplotypes. This parameter will need to be tuned for each species. The epsilon is set to 0.8 by default.

`MGmin` - Is a parameter used by `HaploVar`. MGmin and epsilon influence the density and amount of noise within the calculated haplotypes. This parameter will need to be tuned for each species. The MGmin is set to 10 by default. 

`minFreq` - Is a parameter used by `HaploVar`. Haplotype variants present in fewer individuals than minFreq (default = 4) are removed.

`format` - `HaploVar` can save the output into 6 different formats. For a detailed guide on these 6 formats please see see Suplementary Note 1 from the manuscript (MacNish et al., 2025) or HaploVar's [tutorial](https://htmlpreview.github.io/?https://github.com/TessaMacNish/HaploVar/blob/main/vignettes/introduction.html) The default format is format 6, which saves the haplotypes as a VCF which can be used for GWAS. 

`file_type` - This will determine if you write a csv file (for formats 1-5) or a VCF file (for format 6)

`output_prefix` - This parameter determines what your output file will be named. By default the haplotype file written is named output.vcf or output.csv based on the file type chosen. 

An example of how to use the parameters when running your nextflow pipeline is below. All other parameters can be used in the same way.

``` bash
nextflow run main.nf --epsilon 0.6 --format 3 --file_type csv
```
Alternativley, you can change the parameters by editing the main.nf file directly.


## References
Browning, B. L., Zhou, Y., & Browning, S. R. (2018). A one-penny imputed genome from next-generation reference panels. American Journal of Human Genetics, 103(3), 338–348. https://doi.org/10.1016/j.ajhg.2018.07.015

Browning, B. L., Tian, X., Zhou, Y., & Browning, S. R. (2021). Fast two-stage phasing of large-scale sequence data. American Journal of Human Genetics, 108(10), 1880–1890. https://doi.org/10.1016/j.ajhg.2021.08.005

Danecek, P., Auton, A., Abecasis, G., Albers, C. A., Banks, E., DePristo, M. A., Handsaker, R. E., Lunter, G., Marth, G. T., Sherry, S. T., McVean, G., & Durbin, R. (2011). The variant call format and VCFtools. Bioinformatics, 27(15), Article btr330. https://doi.org/10.1093/bioinformatics/btr330

MacNish, T. R., Al-Mamun, H. A., Bergmann, T., Bestry, M. S., Marsh, J. I., & Edwards, D. (2025). HaploVar: an R package for defining local haplotype variants for trait association and trait prediction analyses. Bioinformatics (Oxford, England), 41(12), Article btaf602. https://doi.org/10.1093/bioinformatics/btaf602

Purcell, S., Neale, B., Todd-Brown, K., Thomas, L., Ferreira, M. A. R., Bender, D., Maller, J., Sklar, P., de Bakker, P. I. W., Daly, M. J., & Sham, P. C. (2007). PLINK: A tool set for whole-genome association and population-based linkage analyses. American Journal of Human Genetics, 81(3), 559–575. https://doi.org/10.1086/519795

R Core Team (2025). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. https://www.R-project.org/
