# ASSEMBLY TUTORIAL

## **To deal with dependency issues**, we'll create seperate conda environment to manage this.

- ### *SPAdes*
- `conda create -n spades_env` to create a **spades_env** environment
- `conda install bioconda::spades`

- ### *conFindr*
- `conda create -n conFindr_env` to create a  **conFindr_env** environment
- `conda install bioconda::confindr`

- ### *Bactinspector*
- `conda create -n bactinspector_env` to create a **bactinspector_env** environment
- `conda install  conda-forge::pip`
- `pip install bactinspectorMax`
- `pip install setuptools`
- `conda install bioconda::mash`

- ### *Quast*
- `conda create -n quast_env` to create **quast_env** environment
- `conda install bioconda::quast`

## Trimming off bad reads
`trimmomatic PE -phred33 input_forward.fastq.gz input_reverse.fastq.gz output_paired_forward.fq.gz output_unpaired_forward.fq.gz output_paired_reverse.fq.gz output_unpaired_reverse.fq.gz [options]`

##  Check for contamination
`confindr -i {input.read} -o output`

Assess your output

## Speciation
`bactinspector check_species -i {input.read}  -fq "*.fastq.gz" -o speciation`



## Assembly 
*De novo* assembly is a very important step and the first for downstream genomical analysis, especially for bacterial genome. 

==With different types of raw reads, we need a different assembler==. 

**!Note**: you have to replace the {input} bracket with your input to be able to run the command.


## Short read assembly 

Short reads are the output from **ILLUMINA**, **MGI**, etc. with a fixed read length (around ~100-300bp) with a very large amount of reads (can up to millions). Thus, the algorithms of short reads assembly requires a lot of computational resources. Additionally, different K-mer can result different set of of contigs.  

We will use SPAdes as the short read assembler 
Firstly, we use `--isolate` mode to indicate our input is from bacteiral isolate with pair-end reads (`-1`, `-2`). 

We assembly the short read using this command: 

`spades.py --isolate -1 {input.read1} -2 {input.read2} -o {output}`

We can try with other flags (**Optional if you have time**)

`spades.py --isolate -1 {input.read1} -2 {input.read2} -o {output} -t 8 -k 55,71,91,111 --cov-cutoff 20`

- `-t 8`: use 4 threads/cores to run assembly. You can change the number to increase or decrease number of threads which suits your commputer
- `-k 55,71,91,111` set of k-mer used for assembly. To reduce the time of assembly, you can reduce the number of number. K-mer must not higher the read length. 
- `--cov-cutoff 20` minimum cut-off of deep of coverage. 

## Check the quality of your assembly

`quast {input.assembly}`



