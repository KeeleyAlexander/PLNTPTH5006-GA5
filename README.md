<!--
GA5 README template.
- Add your answers to each question's section below.
- For each question where we want you to add your commands,
  put the command(s) you ran in the terminal in the corresponding ```bash code block.
- Where we want to see a command's output, there is a plain ``` code block
  right below the command block: paste the output there.
- The code to run each step of the workflow goes in `protocol.md`, not here:
  you don't need to repeat it in this file.
- You don't need to paste the contents of your scripts into this file:
  we will look at the scripts in your `scripts` dir directly
  (so make sure you commit them!).
- Delete instructional comments like this one as you go.
-->

# GA5 answers - Keeley

------------------------------------------------------------------------

## Part A: Your project dir, Git repo, and protocol

### 1: Fill in the top of the protocol

<!-- No answer needed here -- we check `protocol.md` directly. -->

### 2: Markdown formatting

<!-- No answer needed here -- we check the formatting of both Markdown files. -->

### 3: Create subdirs, and copy the scripts

```bash
mkdir scripts results data -p data/ref
```

### 4: Initialize a Git repository, and commit your first files

<!-- No answer needed here -- we check the repo directly. -->

### 5: Download the reference transcriptome

<!-- Put the download command in `protocol.md`, not here. -->

Code to count the number of transcripts:

```bash
#Inspect file
zcat data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz | head -40
#Each transcript starts with a header than includes "PREDICTED"
zgrep -c "PREDICTED" data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz 
```

The output of the command was:

```
28624
```

*Answer*: The number of transcripts in the file: 28624

------------------------------------------------------------------------

## Part B: Read trimming

### 6: Run TrimGalore

<!-- Put the code to run TrimGalore in `protocol.md`, not here. -->

*Answer*: Whether all batch jobs succeeded, and how you checked that
(and any problems you encountered, and how you solved them): All jobs were completed, I checked the output files. I ran into an issue with batch vs batch but once I fixed that it ran as intended. 

------------------------------------------------------------------------

## Part C: Transcript quantification with Salmon

### 7: Create the Salmon index

Code to check the size of the index dir:

```bash
du -sh results/salmon_index
```

The output of the command was:

```
1.1 G results/salmon_index
```

*Answer*: How long it took to create the index, and how you found out: 28s. The start and end time are in the slurm output file 

*Answer*: The size of the index dir: 1.1G

### 8: Run Salmon for all samples

<!-- No answer needed here -- we check `protocol.md` directly. -->

### 9: The transcripts with the most reads

*Answer*: What the `NumReads` column contains: This contains the number of reads mapped to each transcript that was quantified 

```bash
sort -k5,5nr results/salmon_quant/S63/quant.sf | head -n 3
```

The output of the command was:

```
XM_039591534.2  6566    6315.134        13757.775373    24111.624
XM_039573735.1  6445    6194.134        5060.884906     8699.669
XM_052711352.1  1668    1417.134        13799.986745    5427.319GRE
```

### 10: The gene of the top transcript

```bash
zgrep "XM_039591534.2" data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz 
```

The output of the command was:

```
>XM_039591534.2 PREDICTED: Culex pipiens pallens vitellogenin-A1-like (LOC120426760), mRNA
```

*Answer*: The gene that the top transcript belongs to: vitellogenin-A1-like (LOC120426760)

### 11: Mapping rates

```bash
grep "mapping rate" results/salmon_quant/*/logs/salmon_quant.log
```

The output of the command was:

```
results/salmon_quant/S63/logs/salmon_quant.log:mapping rate: 80.6264%
results/salmon_quant/S64/logs/salmon_quant.log:mapping rate: 81.2534%
results/salmon_quant/S65/logs/salmon_quant.log:mapping rate: 78.7484%
results/salmon_quant/S66/logs/salmon_quant.log:mapping rate: 81.2214%
results/salmon_quant/S67/logs/salmon_quant.log:mapping rate: 82.6772%
results/salmon_quant/S68/logs/salmon_quant.log:mapping rate: 78.3450%
results/salmon_quant/S69/logs/salmon_quant.log:mapping rate: 81.0040%
results/salmon_quant/S70/logs/salmon_quant.log:mapping rate: 81.9808%
results/salmon_quant/S71/logs/salmon_quant.log:mapping rate: 78.5364%
results/salmon_quant/S74/logs/salmon_quant.log:mapping rate: 76.6938%
results/salmon_quant/S75/logs/salmon_quant.log:mapping rate: 78.4252%
results/salmon_quant/S76/logs/salmon_quant.log:mapping rate: 81.5200%
results/salmon_quant/S77/logs/salmon_quant.log:mapping rate: 77.5984%
results/salmon_quant/S78/logs/salmon_quant.log:mapping rate: 81.2922%
results/salmon_quant/S79/logs/salmon_quant.log:mapping rate: 79.1664%
results/salmon_quant/S80/logs/salmon_quant.log:mapping rate: 80.9568%
results/salmon_quant/S81/logs/salmon_quant.log:mapping rate: 81.1610%
results/salmon_quant/S82/logs/salmon_quant.log:mapping rate: 78.0668%
results/salmon_quant/S83/logs/salmon_quant.log:mapping rate: 75.8248%
results/salmon_quant/S84/logs/salmon_quant.log:mapping rate: 79.4264%
results/salmon_quant/S85/logs/salmon_quant.log:mapping rate: 75.6046%
results/salmon_quant/S86/logs/salmon_quant.log:mapping rate: 81.8648%
```

*Answer*: The sample with the lowest mapping rate: S85

------------------------------------------------------------------------

## Part D: Summarizing all steps with MultiQC

### 12: Update your MultiQC script

<!-- No need to paste the script: we'll look at scripts/multiqc.sh and its Git history directly. -->

### 13: Run MultiQC

*Answer*: For each program, the number of reports that MultiQC found,
and why it found this number: 
            salmon | Found 22 meta reports
            salmon | Found 22 fragment length distributions
            salmon | Found 22 library format counts reports
           trim_galore | Found 44 reports
            fastqc | Found 44 reports
There are 22 samples, but trimgalore and fastqc use paired end so both R1 and R2 so 44 reports 

### 14: The General Statistics table

*Answer*: The sample with the lowest Salmon mapping rate,
and whether this matches your answer to question 11: S85, yes this matches my answer

### 15: Adapter lengths

*Answer*: The most common adapter lengths, whether this surprised you,
and why you think these lengths are the most common: The most common adapter length was 1-3bp. This seems very short for an adapter length. I'm really not sure why this length is normal or common. 

### 16: Ask Claude about the MultiQC report

*Answer*: What you asked, and what you learned from Claude's answer (in your own words):

------------------------------------------------------------------------

## Part E: Improving the workflow with genAI

### 17: Claude's review of your protocol and scripts

*Answer*: At least three of Claude's suggestions,
and whether you agree or disagree with each one, and why:

### 18: Address a suggestion

*Answer*: Which suggestion(s) you addressed, and what you changed:

------------------------------------------------------------------------

## Part F: Publish your repo on GitHub

### 19: Create a GitHub repository and push

<!-- No answer needed here -- we check your GitHub repo directly. -->

### 20: Get a (test) DOI with Zenodo

*Answer*: Your Zenodo Sandbox DOI:
