# Midterm Exam
Create a Github repository called trgn510_fall20 with a README and a folder called `scripts`. There will be 4 scripts in your folder, for the four sub-problems below.  The README should provide your name and details. Please create it as private and share the github repository with davcraig75.

The scripts will operate on two files that ultimately are from TCGA. There are two files inside the tar/gz called 
## Data Files: data.tar.gz
Contains
### File 1: data/TRGN.clinical.tsv.gz
This file contains clinical parameters and is tab deliminited
### File 2: data/TRGN.rna.tsv.gz
This file contain FPKM expression values or "fragments per kilobase of exon per million reads", which is a commonly used unit of RNA expression. Since these are not raw read counts. There's no expectation that these values should be integers. Each row are values for a gene for each sample of interest. Each column is a sample.

The sample IDs are a combination of multiple columns from the clinical file, specifically the `Project ID_Case ID_Sample Type`, for example: `LUSC_85-8584_Primary Tumor`.

The rows are ENSEMBL gene names from Gencode 22. Ensembl annotation uses a system of stable IDs that have prefixes based on the species scientific name plus the feature type, followed by a series of digits and a version e.g. ENSG00000139618.1. The version may be omitted.

## Problems
### Problem 1a
Create a python script called `ensembl2gencode22Hugo.py` that takes an input file with `Ensembl` gene ids as the first column and the Gencode22 GTF, and prints to STDOUT the same expression values, substituting for HUGO gene names in the first column, and excludes any ENSEMBL gene ids that do not convert to a gene_name. 

#### Example Usage:
`python3 ensembl2gencode22Hugo.py TRGN.rna.tsv > TRGN.rna.HUGO.tsv`

### Problem 1b
* Create a BASH script called `ras_raf_genes.sh` that filters for genes that contain `RAS` or `RAF` in their HUGO name, for example `BRAF` contains `RAF`. The output will have rows with genes with RAS or RAF, and will be a lot shorter.

#### Example Usage:
`ras_raf_genes.sh TRGN.rna.HUGO.tsv > TRGN.rna.HUGO.BRAF.tsv`

### Problem 2
* Create a script that is called `give_uid`. This script may be either python or bash. It should take two arguments. 
1. Argument 1 is an expression file, with column headers that are `Project ID_Case ID_Sample Type`, as are provided and substitutes it with `uid` values from the clinical file.
2. Clinical file of `TRGN.clinical.tsv`.
The script should print the expression file contents, substituting the column headers with the `uid` from the expression file.

#### Example Usage:
`give_uid TRGN.rna.tsv TRGN.clinical.tsv > trgn.rna.uids.tsv`

### Problem 3
* Create a script called `tumor_stage.subset` that substitutes column names with tumor stage. This script may be python, shell, or a combination of two scripts. If it is a combination, please create a single script that calls the other two in order. You may define your requirements. Ideally this file works using the `uid` field, however, if you like it can use the combination of the  `Project ID_Case ID_Sample Type`. You might need to do this if you are having problems with Problem 2 or have skipped it.  Please explain in the usage.
1. Argument 1 is an expression file, with column headers that are defined by you in the `usage` section.
2. Argument 2 is a clinical file provided.

#### Example Usage:
`tumor_stage.subset trgn.rna.uids.tsv TRGN.clinical.tsv > trgn.rna.tumor_stage.uids.tsv`
