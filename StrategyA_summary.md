# Pipeline summary
> you may add additional sessions. The following sessions are required. If nothing to enter in a session, put "NA" in that session.

## Team 0123456, Pipeline P1
### Whether AI/ML is involved
Data processing: No
Mapping: No
Variant calling: No
Post processing: No
Other steps involve AI/ML: data manipulation

### Oncopanels applied
Oncopanel A: Yes
Oncopanel B: Yes
Oncopanel X: No

### Read processing
Hablar del pipeline usual para WEW
Raw reads were processed for quality control by means ReadProcess[1] version 1.0 with parameters "-q 30 -t 5".

### Map to the reference genome
The processed sequence data were mapped to the human reference genome GRCh37/hg19 using BWA-MEM[2] version 0.7.17 with parameters "-K 100000000 -p -v 3 -t 16 -Y".

### Remove deduplicated reads
Deduplicated reads were detected and marked by Picard[3] version x.x using function MarkDuplicates with parameters "REMOVE_DUPLICATES=false OPTICAL_DUPLICATE_PIXEL_DISTANCE=2500 VALIDATION_STRINGENCY=SILENT". <-- CHECK code
> If UMI tag is used, please describe. <-- Adrian (check whether we write this information in StrategyA or B)

### Local alignment optimization
Check code
Local alignment optimization was performed using AnotherTool[4] version 2.10 with parameters "-x 100 -p"

### Variant calling
- Mutect 2 from GATK [4] v4.2.0.0 with default parameters.
- Manta.
- VarDict.
CallVar[5] version 11.0 was used for variant calling with parameters "-m 0.01 -M 5 -o vcf". A blocklist/allowlist was used in the variant calling. Variant calling was also restrict to our defined regions. These restricted information is provided in a text document (in VCF or BED format) along with the vcf result submission.

### Post processing
The reported VCF files were further processed with an in-house python script to remove identity information and reformat according to the VCF template provided by the PrecisionFDA.

**Variant normalization**:
- Break complex indels into smaller components: Yes
- Decompose multiallelic variants: Yes
- Left aligned: Yes
- Parsimony/Trim: Yes

[1] Li H. and Durbin R. (2009) Fast and accurate short read alignment with Burrows-Wheeler Transform. Bioinformatics, 25:1754-60. [PMID: 19451168]
[2] Author C. Read mapping tools. https://github.com/My-Lab/MapTool/
[3] SomeTools. https://myinstitute.github.io/sometools/
[4] Van der Auwera GA & O'Connor BD. (2020). Genomics in the Cloud: Using Docker, GATK, and WDL in Terra (1st Edition). O'Reilly Media.
[4] Author D., et al. Local alignment optimization will increate the accuracy of variant calling. *Another Great Journal* 199, 123-45 (2022)
[5] CallVar. https://www.ngs-tools.com/callvar/
