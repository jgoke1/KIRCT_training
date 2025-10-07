# ASSEMBLY
## Installation of tools and environment creation
**Download** the **sr_assembly.yml** file. 

**create** a directory for the assembly `mkdir sr_assembly` 

move the **sr_assembly.yml** to the new directory you created `mv sr_assembly.yml sr_assembly/`

check if **mamba** is installed `which mamba`, if not present use `conda install -n base -c conda-forge mamba`

Now **create** a sr_assembly environment, `mamba env create -f sr_assembly.yml`

**Activate** the new environment `conda activate sr_assembly`

`mamba install conda-forge::pip` to install pip
`mamba install conda-forge::python` to install python

Install **Bactinspector**, `pip install bactinspector`

- ### Alternatively
you can also go through this  installing every software to be used singly:

- check if **mamba** is installed `which mamba`, if not present use `conda install -n base -c conda-forge mamba`

- Now **create** a sr_assembly environment, `mamba env create -f sr_assembly`

**Activate** the new environment `conda activate sr_assembly`

- `mamba install bioconda::pip` to install **pip**
- `mamba install bioconda::python` to install **python**
- `mamba install bioconda::fastqc` to install **fastqc**
- `mamba install bioconda::trimmomatic` to install **trimmomatic**
- `mamba install bioconda::spades` to install **spades**
- `mamba install bioconda::quast` to intall **quast**
- `mamba install bioconda::checkm-genome` to intall **checkm-genome** to check genome completeness.
- `mamba install bioconda::mash` to intall **mash**
- `mamba install bioconda::multiqc` to intall **multiqc**
- Install **Bactinspector**, `pip install bactinspector`

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

### check the quality of your 
`quast {input.assembly}`