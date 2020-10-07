# Assignment 2

## Introduction
We have taken a tissue sample from a genetically modified mouse and performed RNA sequencing with the intention of exploring the expression levels. Attached to this assignment is a BAM file for one of those individual cells (in '.sam' format to be readable). This file contains reads which have been aligned to the mouse genome.

A BAM file is the standard data template for all of the genetic information in a library. As such, it is the starting point for many analyses approaches. We would like you to become familiar with the contents of a BAM file and their biological implications by investigating the contents of this file.

Use the tables linked to in the appendix to help you interpret the contents of the file.

In your answers, please list all relevant code used and results found.

## Data and Requirements
You can download the data from here: [link]

## Questions
### Q1. Looking at the Metadata
Q1.1. Use `read.csv("single_cell_RNA_seq_bam.sam", nrows=73, sep="\t", header=FALSE, fill=TRUE)` to load the first 73 lines of the header of the file and print the contents. These lines contain tabulated information about the BAM file and the circumstances of its data collection. According to the header table in section 1.3 of the BAM/SAM document in the appendix, what do the `SN` and `LN` tags indicate?
<br />

Q2.2. A sequence is any template string of bases to which we can align a read. This includes chromosomes (which are continuous sequences of bases) and new strings resulting from genetic modifications. What is the length of the X chromosome, in bp, for our alignment?
<br />

Fun fact (not tested): One of the sequences in this BAM file titled Cre_ERT2 is a Cre-recombinase variant. Cre recombinase is the primary element used in many experiments (such as this one) to induce a genetic modification to certain cells in vivo. This allows us to study the effect of changing a gene at any time during the life cycle of an organism, and has allowed us to make discoveries in many areas including stem cell research.

### Q2. Looking at the Reads
Q2.1. Use the code below to load the reads into an R dataframe. Each row contains one read. How many reads are there in this BAM file?
```
sam <- read.csv("single_cell_RNA_seq_bam.sam", sep="\t", header=FALSE, comment.char="@", col.names = paste0("V",seq_len(30)), fill=TRUE)
sam <- sam[paste0("V",seq_len(11))]
```
<br />

Q2.2. Print out the 10th row of the dataframe to look at the format of a read. Compare it to the mandatory BAM fields table in section 1.4 of the SAM/BAM documentation in the appendix. **The order of columns in the bam file have been preserved in the dataframe**. Which column of your dataframe should you look at to find the chromosome to which the read was aligned? To which BAM data field does the dataframe column "V11" correspond?
<br />

Q2.3. How many reads in this file align to chromosome X?
*Hint:* You can compare a column vector to a constant using logical symbols (==, <, >, etc.) to get a column vector of TRUE or FALSE. Remember, when summing, a true symbol is worth "1" while a false symbol is worth "0".
<br />

Q2.4. Referring to section 1.4 of the SAM/BAM documentation, what column contains the leftmost mapping position of the reads?
<br />

Q2.5. In order to transform a BAM file into expression levels for each gene, we need to count the number of reads covering a particular location or gene. The protein Hspa8 is located on chromosome 9 at bases 40801273 - 40805199. How many reads have their leftmost mapping position aligned within these coordinates?
*Hint:* you can implement AND logic on two column vectors with "&".
<br />

Q2.6. Mapping quality is an indication of how well a read aligned to the reference genome during the alignment step of processing our library data. It is reported as an integer between 0 and 255. How many reads have mapping quality less than 50? (Write the code you used below)
<br />

Q2.7. What is the mean mapping quality of the reads which have mapping quality less than 50? (Write the code you used below)
Hint: you can obtain a subset of a dataframe by using df[bool_vec,] where bool_vec contains TRUE/FALSE elements and bool_vec and df have the same number of rows.
<br />

Q2.8. (bonus): The genome of the mouse used in this experiment has been edited to include the DNA sequence for the protein 'tdTomato', which is a fluorophore. Count the number of reads which align to the tdTomato sequence. Assuming that these reads are accurate, would you expect this cell to emit fluorescently? What might be the purpose of modifying a genome to include a fluorophore?
*Hint:* Think about studying cell populations under a microscope.
<br />

### Q3. Investigating the Variants
We have used Strelka, which is a variant-calling tool, to find all of the SNPs and short indels in the genome of this cell using the BAM file. The variants were then annotated using snpEff to label them with information such as which gene they affect and the type of modification they result in once the RNA undergoes translation to a protein. The results are in a VCF file (extension '.vcf') which is attached. 
<br />

Q3.1. Use the following lines of code to obtain the header of the file and a dataframe where each row is a variant. As you can see, information in the VCF file is organised by multiple levels of character-separated data, so it will take multiple rounds of parsing to extract relevant information. For the first variant (row) in the dataframe, what is the reference allele base at the site, and what is the alternative allele called by Strelka?
*Hint:* Take a look at the VCF Variant Call Format document in the appendix for details on each column name.
```
vcf_con <- file("RNA_seq_annotated_variants.vcf", open="r")
vcf_file <- readLines(vcf_con)
close(vcf_con)
vcf <- data.frame(vcf_file)
header <- vcf[grepl("##", vcf$vcf_file), ]
factor(header)
variants <- read.csv("RNA_seq_annotated_variants.vcf", skip=length(header), header=TRUE, sep="\t")
```
<br />

Q3.2. (bonus): The INFO field is organised into variables by the form 'TAG=value' (see the VCF Variant Call Format document). Write code to obtain the entirety of the ANN info value contents from the INFO field for the first variant. 
**NOTE:** This part is tricky, so it has been made a bonus question. If you are having trouble parsing the string into components, you can just inspect the entire string visually (a good word processor helps) to answer Q3.3-3.4.
*Hint:* You will need strsplit() and grep()/grepl() to accomplish this. Take a look at https://www.math.ucla.edu/~anderson/rw1001/library/base/html/strsplit.html and https://stackoverflow.com/questions/21311386/using-grep-to-help-subset-a-data-frame-in-r for how to make use of them. With which character should you split the string?
*Hint:* Make sure to convert the INFO field entry to string format using as.character() so that it can be passed into strsplit().
<br />

Q3.3. Each INFO tag-value pair is detailed in a line of the header, beginning with the tag '##INFO=<ID=VARIABLE, ...'. Look for the header entry starting with '##INFO=<ID=ANN, ...' which details the format of the ANN value contents. This tag-value pair contains the results of the annotations found by snpEff. Based on the ANN value of the first variant, what does the 'Annotation' field tell us about this variant?
*Hint:* snpEff can return multiple annotation entries for the same variant because some variants may have multiple possible effects. The first annotation entry is the most confident/important. You can use strsplit() again with ',' separation character if you wish to look at each of the ANN entries separately.
*Hint:* Refer to the snpEff documentation in the appendix for a list of snpEff annotation label names and summaries of their effects.
<br />

Q3.4. Perform the parsing done in Q3.1-3 again on variant line 683. What gene would this variant affect?
<br />

Q3.5. What is a frameshift variant? Does it have a greater or lesser effect on the resultant protein than a missense variant? Why? 
<br />

Q3.6. We can divide variants into three broad categories: synonymous/non-impactful, non-synonymous, and frameshift. We can in general use snpEff's impact field to broadly categorize variants into synonymous and non-synonymous types by 'MODIFIER' and 'LOW/MEDIUM/HIGH' labels respectively. Count the number of synonymous variants. Also, count the number of potential frameshift mutations. What do you notice about the number of synonymous variants (compared to overall number of variants)?
*Hint:* Use grepl() on the INFO field to look for tell-tale tags.
<br />

Q3.6. (bonus): Using Strelka on our data, we can detect indels, but only to a limited extent. Most of the reads in our BAM file have read lengths around 60bp long. Why might this have consequences for the detection of insertions that are longer than 60bp?
<br />

### APPENDIX

SAM/BAM Format Specification Document: https://samtools.github.io/hts-specs/SAMv1.pdf

VCF Variant Call Format Document (GATK): https://gatk.broadinstitute.org/hc/en-us/articles/360035531692-VCF-Variant-Call-Format

snpEff Annotations Document: http://snpeff.sourceforge.net/VCFannotationformat_v1.0.pdf