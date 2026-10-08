# Nextflow Pipeline for GATK SNP Calling

## What does this pipeline do?

This pipeline runs the haplotyping tool `HaploVar` and all the preparation steps prior to running HaploVar including creating the linkage disequilibrium matrices. The tools used in this pipeline are HaploVar version 0.1.2 (MacNish et al., 2025), BCFtools version 1.15 (Danecek et al., 2021), PLINK v1.90b6.21 (Purcell et al., 2007, https://zzz.bwh.harvard.edu/plink/ld.shtml), Beagle version 5.4 (Browning et al., 2018; Browning et al., 2021), and R version 4.5.2 (R Core Team, 2025).

## Running this pipeline using only default parameters

## References
Browning, B. L., Zhou, Y., & Browning, S. R. (2018). A one-penny imputed genome from next-generation reference panels. American Journal of Human Genetics, 103(3), 338–348. https://doi.org/10.1016/j.ajhg.2018.07.015

Browning, B. L., Tian, X., Zhou, Y., & Browning, S. R. (2021). Fast two-stage phasing of large-scale sequence data. American Journal of Human Genetics, 108(10), 1880–1890. https://doi.org/10.1016/j.ajhg.2021.08.005

Danecek, P., Auton, A., Abecasis, G., Albers, C. A., Banks, E., DePristo, M. A., Handsaker, R. E., Lunter, G., Marth, G. T., Sherry, S. T., McVean, G., & Durbin, R. (2011). The variant call format and VCFtools. Bioinformatics, 27(15), Article btr330. https://doi.org/10.1093/bioinformatics/btr330

MacNish, T. R., Al-Mamun, H. A., Bergmann, T., Bestry, M. S., Marsh, J. I., & Edwards, D. (2025). HaploVar: an R package for defining local haplotype variants for trait association and trait prediction analyses. Bioinformatics (Oxford, England), 41(12), Article btaf602. https://doi.org/10.1093/bioinformatics/btaf602

Purcell, S., Neale, B., Todd-Brown, K., Thomas, L., Ferreira, M. A. R., Bender, D., Maller, J., Sklar, P., de Bakker, P. I. W., Daly, M. J., & Sham, P. C. (2007). PLINK: A tool set for whole-genome association and population-based linkage analyses. American Journal of Human Genetics, 81(3), 559–575. https://doi.org/10.1086/519795

R Core Team (2025). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. https://www.R-project.org/
