# BMEG 310 Assignment 2 Answer Key

## Question 1
**Q1.1. (1 mark)** *1 mark for correct observation*<br/>
Answer:<br/>
LN stands for length, SN stands for sequence name (ie. the name of a chromosome)


**Q1.2. (1 mark)** *1 mark for correct observation*<br/>
Answer:<br/>
X chromosome has length 171031299bp


## Question 2
**Q2.1. (1 mark)** *0.5 marks for correct code, 0.5 marks for correct number*<br/>
Code:
```
nrow(sam)
```
Answer:<br/>
number of reads is 146356


**Q2.2. (2 marks)** *1 mark for each answer, (minus 1 mark for incorrect code)*<br/>
Code:
```
sam[11,]
```
Answer:<br/>
to look for chromosome use sequence, which is V3, V11 corresponds to base quality


**Q2.3. (2 marks)** *1 mark for code, 1 mark for answer*<br/>
Code:
```
sum(sam$V3 == "X")
```
Answer:<br/>
5999 reads align to the X chromosome


**Q2.4. (1 mark)** *1 mark for correct answer*<br/>
Answer:<br/>
V4 contains the leftmost mapping of the reads


**Q2.5. (3 marks)** *1 mark for correct comparisons, 1 mark for correct logic, 1 mark for answer*<br/>
Code:
```
sum((sam$V3 == "9") & (sam$V4 <= 40805199) & (sam$V4 >= 40801273))
```
Answer:<br/>
119 reads align to that region


**Q2.6. (2 marks)** *1 mark for correct column comparison, 1 mark for answer*<br/>
Code:
```
sum(sam$V5 < 50)
```
Answer:<br/>
61527 reads have mapping quality less than 50


**Q2.7. (2 marks)** *1 mark for indexing, 1 mark for mean and result*<br/>
Code:
```
mean(sam[(sam$V5 < 50),]$V5)
```
Answer:<br/>
average mapping quality is 0.2418125


**Q2.8. (1 bonus marks)** *0.5 mark for concluding cell is fluorescent with adequate explanation, 0.5 mark for reasonable explanation of purpose*<br/>
Code:
```
sum(sam$V3 == 'tdTomato')
```
Answer:<br/>
63 reads align to tdTomato, if these reads are accurate then it indicates that the tdTomato protein is being expressed and there is likely tdTomato protein within the cell, so we would expect it to emit fluorescently if red light is shone on the cell, we would modify a genome to include a fluorophore if we wanted to track cells under a microscope as it would allow us to see population structures in a culture or tissue sample

## Question 3
**Q3.1. (1 mark)** *1 mark for correct observation*<br/>
Code:
```
variants[1,]
```
Answer:<br/>
reference base is G, alt is A


**Q3.2. (1 bonus mark)** *0.5 marks for result, 0.5 marks if properly automated*<br/>
Code:
```
# this only performs on one variant at a time.
myinfo <- unlist(strsplit(as.character(variants[1,]$INFO), ";"))
myann <- myinfo[grep("ANN", myinfo)] # myann <- subset(myinfo, grep("ANN", myinfo))
```


**Q3.3. (1 mark)** *1 mark for observation/explanation*<br/>
Code (not required):
```
unlist(strsplit(myann, ","))
```
Answer:<br/>
The variant is annotated as an intron_variant, meaning that it is located in an intron


**Q3.4. (1 mark)** *1 mark for correct observation*<br/>
Code (not required):
```
myinfo <- unlist(strsplit(as.character(variants[683,]$INFO), ";"))
myann <- myinfo[grep("ANN", myinfo)] # myann <- subset(myinfo, grep("ANN", myinfo))
unlist(strsplit(myann, ","))
```
Answer:<br/>
Rps19 is the gene


**Q3.5. (3 marks)** *1 mark for mentioning mechanics of shift, 1 mark for mentioning multiple of 3 codons or different downstream codons, 1 mark for mentioning greater effect due to higher number of codons changed (minus 1 if change to final protein not mentioned)*<br/>
Answer:<br/>
A frameshift variant is an indel where the number of insertions/deletions is not a multiple of 3. This causes all downstream codons to be changed, and thus modifies many more amino acids/the conformational shape/total length of the resultant protein than a single SNP 


**Q3.6. (3 marks)** *1 marks for correct label fetching and logic, 1 mark for correct result, 1 mark for recognizing vast majority of variants are intronic*<br/>
Code:
```
sum(grepl("MODIFIER", variants$INFO))
```
Answer:<br/>
819 variants are intronic. This the vast majority of the overall variants, which tells us that most of the variants we have detected are intronic

**Q3.7. (1 bonus mark)** *0.5 marks for insufficient alignment of sequences, 0.5 marks for inability to resolve full sequence of insertion*<br/>
Answer:<br/>
Insertions significantly longer than 60bp will cause sequences from within the insertion to not align meaningfully to the insertion sequence (which does not exist in the reference), causing the full contents of the indel to be unresolved after alignment, thus, the full sequence of the indel may remain unknown
