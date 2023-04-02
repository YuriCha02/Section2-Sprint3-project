# Data
Necessary datas can be found in this Google drive folder:
https://drive.google.com/drive/folders/1IUhX9nD1-yAbZuY5PTTmG2wNs6cjIALQ?usp=sharing

# Background
## Experimental Approaches and Quality Control

Observing changes in tissue composition and function during the development or diseases:
- With Histogoly, you can slice a tissue and stain it with different cellular markers to make observation
	- But you can only observe limited number of molecules.

**Single-cell RNA sequencing enables global snapshots of transcriptional profiles with single-cell resolution**
- It allows you to compare expression patterns across different cell types and within different cell types.

## Outline
**Overview of experimental approaches**
- History
	- Due to new nature of technology, to provide approches of the analysis, we need to know the history:
	- Single-cell RNA seq technologies
	- Limitations and innovations

**Dealing with technical variation**
- Amplification bias, UMIs
- Quality Control
- Batch effects
- Normalization and Imputation

### History
Transcriptional profiling:
- Microarrays enabled systemic measurements of transcriptional changes
- Bulk RNA-sequencing increased the resolution at which transcriptional differences could be measured:
	- Uses:
		- Quantification of gene expression
		- Comparative transcriptomics
			- Represent average level of mRNA across the entire samples.
	- Limitations:
		- Unable to resolve heterogeneity

![enter image description here](https://cdn.10xgenomics.com/image/upload/f_auto,q_auto,w_680,h_510,c_limit/v1574196658/blog/singlecell-v.-bulk-image.png)

### Goals of scRNA-seq
- Measure the distribution of expression levels for each gene across a population of cells
- Measure transcriptional differences across and within groups of cells
- Resolve single-cell heterogenity

### Applications for scRNA-seq
- Characterization of cell and transcriptional composition of tissues
	- Number of different cell types change
	- Change of transcriptional profiles across all the cells
- Evaluating developmental processes and stem cell differentiation
- Analysis of cancer evolution and mechanisms of therapeutic resistance

## Overview of scRNA-seq
How does this measurement works?

1. Tissues are dissociated to create a cell suspension
	- And preparation for cell suspension for solid tissue is tricky because it require digesting extracellular matrix and cell-to-cell junctions to separate individual cells.
	- However, dissassociation must be gentle enough to preserve membrane integrity.
		- Else, dead cell will release rnas into the extracellular liquid
			- These rnas will encapsulated into the droplets alongside cells, **contaminating** the output.
	- You can also use methanol-based fixation method early either before or after tissue dissociation and prior to droplet encapsulation
		- Which allow sample to be stored for few months.
		- Better than freezing
2. Once individual cells are separated into different droplets, use flow cytometry to quantify:
	1. The quality of the sample in terms of cell viability.
	2. Successful separation of neighboring cells
	- **MACS (Magnetic associated cell separation)** can be measured cell composition 
		- by binding cell surface markers with antibodies
	- **FACS (Flourescent activated cell sorting)**
		- Useful as it gives knowledge of cell type beforehand or expectation of proportion of cell types later in analysis
		- Can also be used to enrich for specific cell types 
			- (e.g. rare cells such as stem cell or special subset of cells)
3. Droplet Encapsulation
	- There are three technologies:
		- inDrop
		- Drop-seq
		- 10X Chromium
		- They use water in oil reaction chambers
			- In these chambers, individual cells are flowed into machine to merge with a droplet that contains a single bead with barcodes and other reaction material.
				- Beads: They have barcodes that bind to the mRNA inside of the cell and mark each mRNA as belonging to the same cell. 
		- This is done inside the droplet because the smaller the volumn is, **lower the cost** and the **higher the barcodes will bind to mRNA**.
		- Drops from droplet can contain:
			- no beads and no cells
			- no beads and one or more cells
			- one or more beads and one or more cells
			- one or more beads and no cells
			- The proportion is the function of the concentration of both beads and cells.
	- Area of error:
		- If there are one beads and more than one cells, problem can rise.
			- because two cells are barcoded with the same sequence which in analysis, they may appear belonging in the same cell even though they can be different type of cells with different gene expression.
			- Solution: To keep cell doublets below 5%, Drop-seq can only barcode 2~5% of cells.
			- However, **inDrop** and **10X Chromium** overcomed this problem.
				- By changing statistical distribution of droplet packing by using deformable gel beads
				- 80% of your cells can be barcoded for sequencing.
4. After cells are distributed into droplets, the cells are lysed (breaking down of the membrane of a cell), its content are released.
5. The poly-A tail of mRNAs bind to oligonucleotides on the capture beads then reverse transcriptase is used to: 
	1. Synthesize cDNA (complementary DNA)
	2. Incorporate barcode and a unique molecular identifier into the same cDNA
6. cDNA is amplified using PCR
7. cDNA is fragmented so sequencing adapters can be added
	- Multiplexing adapters can be added if different samples are being combined.
		- e.g. two different drug treatments and experiments
8. The library is sequenced using Illumina instruments
	- Sequencing is read from the three prime end only
		- First read reads the cell barcode and then unique molecular identifier
		- Second read reads gene sequence
9. Then the data is demultiplexed
		- To identify barcodes that represent real cells, not just background ambient mRNA
10. Once the barcodes of real cells are created, each UMI (Unique Molecular Identifier) read is aligned with a reference genome
	- So a data matrix is created where:
		- Each row in data matrix represent cell
		- Each column represent a gene
		- The entries are counts that identify how many UMIs are found in each in each gene within individual cells.
	- **Tools**:
		- If you're using 10x genomic platform, this process using **cell ranger software**
		- Other approaches uses algorithem **STAR alignment strategy** and **UMI tools**
11. The data can be analyzed for **differential expression**, **cell type clustering**, **pseudotime analysis** or etcs.

### Technical Limitation
- At the end of this experiemtn, only 10%~20% of cells from original sample are captured and sequenced
	- Within cells, only 5% of mRNAs are captured and sequenced.
		- This gives high-dimensional data that are difficult to analyze
![](https://i.imgur.com/QvqNcMP.jpg)
#### What causes this:
- During droplet encapsulation, mRNA molecules don't always bind to beads
	- Due to low concentration of mRNA
- Inefficiency of reverse transcription (RT) enzymes
	- It doesn't work in presence of contaminants or variable like cell goo
- Efficiency of RT and PCR depend on the specific transcripti

#### Solution:
Unique molecular identifiers allow **Single RNA-seq** to accurately quantify unique mRNA molecules were counted in each cell before amplification.

#### Types of data generated:
**Read counts**
Total number of mRNAs from a gene that are sequenced
**UMI (Unique molecular identifier) counts**
It's the number of unique mRNA molecules were counted in each cell before amplification.
- PCR involves amplifying the copy number of molecules
- Thus, small difference in numbers and pcr efficiency is amplified.
	- Solution: label each mRNA with unique barcode during the reverse transcription step before mRNA amplificaiton.
- It's number is consistent no matter how many times you run PCR
![](https://hbctraining.github.io/scRNA-seq_online/img/umis.png)

### Evaluating Single Cell RNA-sequencing effectiveness:
Use synthetic cDNA called **ERCC spike-ins**. These synthetic cDNA are spiked into cell sample.
- Run the sequencing and compare the concentration of ERCC spike-ins before and after to check:
	- Accuracy
	- Precision
	- Sensitivity
	- Power
	- Efficiency

# Code 
```

```
