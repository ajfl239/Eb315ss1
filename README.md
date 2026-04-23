# Genome Assembly and Annotation: Eb315ss1

## Table of Contents
### 1. Sequence Data, Quality Assessment and Trimming
### 2. Genome Assembly
### 3. Genome Intactness
### 4. Gene Prediction

---

## 1. Sequence Data, Quality Assessment and Trimming
### Goals: 
- Learn how to assess sequence quality and trim/filter sequence reads to generate improved datasets for downstream analyses

#### Step 1: Initial Quality Assessment
We utilized FastQC to inspect the raw "paired-end" reads. This tool identifies systemic sequencing issues by plotting Phred qaulity scored across the length of the read.  

```
fastqc ~/sequences/Eb315ss1/Eb315ss1_1.fq.gz ~/sequences/Eb315ss1/Eb315ss1_2.fq.gz -o ~/sequences  
```
We can then download the html files that are output using:
```
scp ajfl239@ajfl239.cs.uky.edu:~/sequences/Eb315ss1_1_fastqc.html ./  
scp ajfl239@ajfl239.cs.uky.edu:~/sequences/Eb315ss1_2_fastqc.html ./
```

Opening these files will give us a summary to use in our assessment.  

<img width="1919" height="1028" alt="image"  style="border: 2px solid black;" src="https://github.com/user-attachments/assets/f92858fe-39ef-439b-b939-a08d8424d8ba" />

Warning Triggers: FastQC issues a warning if the median base quality is < 25.  
Failure Triggers: A failure is issued if the median quality is < 20.

#### Step 2: Quality Filtering and Adapter Trimming
We utilized Trimmomatic to address quality decline at read ends and removed adapter contamination. Before running Trimmomatic, we first added a polyG motif (G x 20) to adaptors.fa to remove base-caller artifacts.
```
java -jar trimmomatic-0.38.jar PE -threads 2 -phred33 \
-trimlog Eb315ss1_errorlog.txt \
~/sequences/Eb315ss1/Eb315ss1_1.fq.gz ~/sequences/Eb315ss1/Eb315ss1_2.fq.gz \
Eb315ss1_1_paired.fastq Eb315ss1_1_unpaired.fastq \
Eb315ss1_2_paired.fastq Eb315ss1_2_unpaired.fastq \
ILLUMINACLIP:adaptors.fa:2:30:10 SLIDINGWINDOW:20:20 MINLEN:125
```
After this initial run of Trimmomatic, we took the new fastq files and ran them through FastQC to assess how well Trimmomatic did in removing poor quality sequences.
<img width="1919" height="1031" alt="image" src="https://github.com/user-attachments/assets/39a3f45a-2b53-45a5-b31d-4cca2f0b5d78" />
In our case, Trimmomatic did a good job removing poor quality sequences.

---

## 2. Genome Assembly
### Goals: 
- Learn how to use the command line programs Velvet and SPAdes to generate genome assemblies
- Assemble the genome of an isolate of the fungus, Pyricularia oryzae
- Visualize the assembly layout using Bandage

#### Step 1: Find Recommended K-mer Length
We used Velvet Advisor to assist us in the choice of a suitable k-mer length: https://dna.med.monash.edu.au/~torsten/velvet_advisor/  


<img width="944" height="515" alt="image" src="https://github.com/user-attachments/assets/d87f23b5-e2fa-4344-9cb4-e40e47eb5616" />

#### Step 2: Generate Assembly using Velvet

#### Step 3: Generate Assembly using SPADdes

#### Step 4: Visualize using Bandage
We utilized Bandage to interrogate the assembly graph and examine contiguity.

<img width="1919" height="1031" alt="bandageresult" src="https://github.com/user-attachments/assets/f1465698-90c0-4392-b264-e7d4efe0bca6" />

---

## 3. Genome Intactness
 - Busco

---

## 4. Gene Prediction using Augustus and Snap
- Summarize #s of predicted genes from snap and AUGUSTUS
- Include screen shot of IGV browser window showing an example of a gene predicted only by snap
- Include screen shot of IGV browser window showing an example of a gene predicted only by AUGUSTUS
- Include screen shot of gene where snap and AUGUSTUS predict the same exon/intron structure
- Include screen shot of gene where snap and AUGUSTUS predict a different exon/intron structure
- Summarize # of predicted genes from your MAKER run and record the code you used:
  - to count gene features in your .gff file
  - to count records in the .fasta file
- Include a screen shot of a gene where a gene was successfully predicted by SNAP, AUGUSTUS, and MAKER and there is external evidence that the prediction is correct


Perform maker:
```
sbatch maker.sh /project/farman_s26abt480/ajfl239/Eb315ss1/Eb315ss1_final.fasta
```
gff3_merge to make Eb315ss1-maker.gff3:
```
singularity exec /share/singularity/images/ccs/MAKER/amd-maker-debian10.sinf gff3_merge -d Eb315ss1_final.maker.output/Eb315ss1_final_master_datastore_index.log -o Eb315ss1-maker.gff3
```
Number of Predicted Genes: (12859)
```
grep -c $'\tgene\t' Eb315ss1-maker.gff3
```
Copy gff3 file into class_GFFs:
```
cp Eb315ss1-maker.gff3 /project/farman_s26abt480/CLASS_GFFs/
```
fasta_merge:
```
singularity exec /share/singularity/images/ccs/MAKER/amd-maker-debian10.sinf fasta_merge -d Eb315ss1_final.maker.output/Eb315ss1_final_master_datastore_index.log -o Eb315ss1
```
Count proteins in .proteins.fasta file: (12859)
```
grep -c "^>" Eb315ss1.all.maker.proteins.fasta
```

