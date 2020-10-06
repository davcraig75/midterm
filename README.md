# Midterm Exam
Create a *private* Github repository called trgn510_fall20 with a README and a folder called `scripts`. The material needed for the examination is within the repository: [https://github.com/davcraig75/midterm](https://github.com/davcraig75/midterm).

The scoring will not be proportional to the effort, but proportional to the importance. The third problem is more challenging, and meant for those reaching for perfect. If you do not get all aspects, it will not overly harm your grade, as the critical essentials are in the first problem.

* There will be 4 scripts in your `scripts` folder, for the four sub-problems below.
* The README should provide your name and details. 
* Please create your Github as private and share the github repository with davcraig75.
* Please add a generic MIT license to the GitHub
* Please have a clear and readible README with a section for `usage`, `known issues`, and `dependencies`. 
* If you have issues or challenges, please describe these in `known issues` clearly within the markdown. You may describe the strategy you are taking as well for partial credit. If you have solved a portion, or hacked a part to get to the overall goal describe those steps for partial credit. 
* The scripts will operate on two files that ultimately are from TCGA. There are two files inside the tar/gz called `data.tar.gz` for `https://github.com/davcraig75/midterm`. You may want to open and examine portions of these files. 
* You do not need to include the data files in your Github. If you do, please tar and gzip them. If you are using altered files because you had trouble and needed to hack/alter the files, please include them and describe the changes in `known issues`. This would be the case if you changed the clinical tsv file to make your script work, and is not ideal.


### File 1: data/TRGN.clinical.tsv.gz
This file contains clinical parameters and is tab delimited. The first collumn are unique IDs. The corresponding expression files uses a combination of `Project ID_Case ID_Sample Type`.
![](https://itg.usc.edu/site/wp-content/uploads/2020/10/cln.png)


### File 2: data/TRGN.rna.tsv.gz
This file contains FPKM expression values or "fragments per kilobase of exon per million reads", which is a commonly used unit of RNA expression. Since these are not raw read counts. There's no expectation that these values should be integers. Each row is values for a gene for each sample of interest. Each column is a sample.

The sample IDs are a combination of multiple columns from the clinical file, specifically the `Project ID_Case ID_Sample Type`, for example: `LUSC_85-8584_Primary Tumor`. Please be aware that you need to manage spaces, such as using the escape character `\ `.

The rows are ENSEMBL gene names from Gencode 22. Ensembl annotation uses a system of stable IDs that have prefixes based on the species scientific name plus the feature type, followed by a series of digits and a version e.g. ENSG00000139618.1. The version may be omitted.  Please use Gencode 22 for this task.
![](https://itg.usc.edu/site/wp-content/uploads/2020/10/rna.png)

## Problems
### Problem 1a (30%): ensembl2gencode22Hugo.py
The purpose of this problem is to test your ability to recapitulate the major concepts done in prior homeworks. 

Create a python script called `ensembl2gencode22Hugo.py` that takes an input file with `Ensembl` gene ids as the first column and the Gencode22 GTF (You can use the `main annotation file` appropriate for most users from the Gencode website), and prints to STDOUT the same expression values, substituting for HUGO gene names in the first column and excludes any ENSEMBL gene ids that do not convert to a gene_name. 

#### Example Usage:
`python3 ensembl2gencode22Hugo.py gencode.v22.annotation.gtf TRGN.rna.tsv > TRGN.rna.HUGO.tsv`

### Problem 1b (30%): rasraf_genes.sh
* Create a BASH script called `rasraf_genes.sh` that filters for genes that contain `RAS` or `RAF` in their HUGO name, for example, `BRAF` contains `RAF`. The output will have rows with genes with RAS or RAF, and will be a lot shorter.

#### Example Usage:
`rasraf_genes.sh TRGN.rna.HUGO.tsv > TRGN.rna.HUGO.rasraf.tsv`

### Problem 2 (15%): give_uid
The purpose of this script is test that you can lookup/substitute values from one source into another. However, its not a perfect match, and you must create a new intermediate value for the lookup. In this case, it will be the composite of three column headers. Unlike our previous examples, the substitution is only relevant on. the first line (the header).

Create a script that is called `give_uid`. This script may be either python or bash. It should take two arguments. 
1. Argument 1 is an expression file, with column headers that are `Project ID_Case ID_Sample Type`, as are provided and substitutes it with `uid` values from the clinical file.
2. Clinical file of `TRGN.clinical.tsv`.
The script should print the expression file contents, substituting the column headers with the `uid` from the expression file.

Below is a snapshot in excel, noting the uids come from the TRGN.clinical.tsv file. 

https://itg.usc.edu/site/wp-content/uploads/2020/10/uid.png

#### Example Usage:
`give_uid TRGN.rna.tsv TRGN.clinical.tsv > trgn.rna.uids.tsv`

### Problem 3 (15%): tumor_stage.subset
The purpose of this problem is to add additional complexity. For many, it will be a single script, though for some they may wish to break in pieces. It may take longer then the other two, and is meant to test your ability to integrate a few concepts.

Create a script called `tumor_stage.subset` that substitutes column names with tumor stage. This script may be python, shell, or a combination of two scripts. If it is a combination, please create a single script that calls the other two in order. You may define your requirements. Ideally, this file works using the `uid` field. However, if you like it can use the combination of the  `Project ID_Case ID_Sample Type`. You might need to do this if you are having problems with Problem 2 or have skipped it.  Please explain in the usage.

*A few cases may not have a tumor stage, and they may be empty. Exclude these samples from the output. This is a bit more challeging, and thus I recommend adding it later, where initially you can leave the column blank. This portion is worth 4% of the 15%, and thus will not overly harm your grade. However adding it is definately working at a A+ level for this course.*

1. Argument 1 is an expression file, with column headers defined by you in the `usage` section.
2. Argument 2 is a clinical file provided.

#### Example Usage:
`tumor_stage.subset trgn.rna.uids.tsv TRGN.clinical.tsv > trgn.rna.tumor_stage.tsv`

https://itg.usc.edu/site/wp-content/uploads/2020/10/stage.png

