# Pipeline summary

## Pipeline P2
### Whether AI/ML is involved
- Data processing: No
- Mapping: No
- Variant calling: No
- Post processing: No
- Other steps involve AI/ML: No

### Oncopanels applied
- Oncopanel A: Yes
- Oncopanel B: Yes
- Oncopanel X: No

### Read processing
The quality of raw reads was assessed by means of FastQC [1] v0.11.9 using default parameters. FASTQ were converted to uBAM and Illumina adapters were searched for and marked with Picard [2] tool v2.25.6.

### Map to the reference genome
The processed sequence data were mapped to the human reference genome GRCh37/hg19 using BWA-MEM [3] v0.7.17 with parameters "-K 100000000 -p -v 3 -t 16 -Y".

### Remove deduplicated reads
Deduplicated reads were detected and marked (but not removed) using Picard MarkDuplicates function with parameters "REMOVE_DUPLICATES=false VALIDATION_STRINGENCY=SILENT".

### Local alignment optimization
The preprocessing of reads before the variant calling ends with a base quality score recalibration (BQSR step) using GATK v3.8 [4], providing a ready BAM.

### Variant calling
The variant calling was performed using Mutect2 from GATK v4.2.0.0 with default parameters using two different 'Panel of Normals' (PON):
- PON-CDC, built GATK using data from the population cohort study ‘CDC of the Canary Islands’ with all individuals self-reporting absence of cardiovascular, metabolic, immunologic, or cancer diseases.
- GATK PON derived from whole-genome sequencing provided by the Broad Institute, available at: https://console.cloud.google.com/storage/browser/_details/gatk-best-practices/somatic-b37/Mutect2-WGS-panel-b37.vcf.

A second caller, VarDict (Java version) [5] v1.8.3, with default parameters, was also used.

For both callers, variant calling was restricted to the regions provided by the FDA challenge applying a padding of 100 bp.

### Post processing
Variants from Mutect2 calling were filtered using FilterMutectCalls tool from GATK v4.2.0.0 using default parameters.

Variants were left-aligned and trimmed with BCFtools [6] v1.12. ADP and AF fields were annotated using BCFtools. Annotation of frequencies and dbSNP [7] and COSMIC identifiers was provided by the Ensembl Variant Effect Predictor (VEP) [8].

Variants present in 1000 Genomes phase 3 [9], gnomAD [10] v2.1.1, or ESP6500 [11] databases at minor allele frequency (MAF) ≥1%, but not present in COSMIC database, were considered germinal and filtered out. Additionally, variants present in dbSNP but not in COSMIC were also removed. 

Only variants with ‘PASS’ value in the FILTER field called inside the provided regions were kept (notice that this PASS filtering is the main difference with Submission 1). 

Finally, the filtered variants obtained with Mutect2 and PONs, and VarDict were merged with BCFtools, resulting in a final VCF with the union of the identified filtered variants.

The reported VCF files were further processed with an in-house Bash script to remove identity information and reformat according to the VCF template provided by the PrecisionFDA challenge organization.

**Variant normalization**:
- Break complex indels into smaller components: Yes
- Decompose multiallelic variants: Yes
- Left aligned: Yes
- Parsimony/Trim: Yes

**References**
1. Andrews S. (2010). FastQC: a quality control tool for high throughput sequence data. Available online at: http://www.bioinformatics.babraham.ac.uk/projects/fastqc.
2. Picard. http://broadinstitute.github.io/picard/.
3. Li, H., & Durbin, R. (2009). Fast and accurate short read alignment with Burrows-Wheeler transform. Bioinformatics (Oxford, England), 25(14), 1754–1760. https://doi.org/10.1093/bioinformatics/btp324.
4. Van der Auwera GA & O'Connor BD. (2020). Genomics in the Cloud: Using Docker, GATK, and WDL in Terra (1st Edition). O'Reilly Media.
5. Lai Z, Markovets A, Ahdesmaki M, Chapman B, Hofmann O, McEwen R, Johnson J, Dougherty B, Barrett JC, and Dry JR. VarDict: a novel and versatile variant caller for next-generation sequencing in cancer research. Nucleic Acids Res. 2016, pii: gkw227.
6. Danecek P, Bonfield JK, et al. Twelve years of SAMtools and BCFtools. Gigascience (2021) 10(2):giab008.
7. Sherry, S. T., Ward, M. H., Kholodov, M., Baker, J., Phan, L., Smigielski, E. M., & Sirotkin, K. (2001). dbSNP: the NCBI database of genetic variation. Nucleic acids research, 29(1), 308–311. https://doi.org/10.1093/nar/29.1.308.
8. McLaren, W., Gil, L., Hunt, S. E., Riat, H. S., Ritchie, G. R., Thormann, A., Flicek, P., & Cunningham, F. (2016). The Ensembl Variant Effect Predictor. Genome biology, 17(1), 122. https://doi.org/10.1186/s13059-016-0974-4.
9. 1000 Genomes Project Consortium, Auton, A., Brooks, L. D., Durbin, R. M., Garrison, E. P., Kang, H. M., Korbel, J. O., Marchini, J. L., McCarthy, S., McVean, G. A., & Abecasis, G. R. (2015). A global reference for human genetic variation. Nature, 526(7571), 68–74. https://doi.org/10.1038/nature15393. 
10. Karczewski, K. J., Francioli, L. C., Tiao, G., Cummings, B. B., Alföldi, J., Wang, Q., Collins, R. L., Laricchia, K. M., Ganna, A., Birnbaum, D. P., Gauthier, L. D., Brand, H., Solomonson, M., Watts, N. A., Rhodes, D., Singer-Berk, M., England, E. M., Seaby, E. G., Kosmicki, J. A., Walters, R. K., … MacArthur, D. G. (2020). The mutational constraint spectrum quantified from variation in 141,456 humans. Nature, 581(7809), 434–443. https://doi.org/10.1038/s41586-020-2308-7.
11. Exome Variant Server, NHLBI GO Exome Sequencing Project (ESP), Seattle, WA (URL: http://evs.gs.washington.edu/EVS/).




