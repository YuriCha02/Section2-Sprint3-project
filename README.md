## Source:
https://www.kaggle.com/datasets/xiaotawkaggle/inhibitors

## Feature Description
It was reported that an estimated 4292,000 new cancer cases and 2814,000 cancer deaths would occur in China in 2015. [Chen, W., etc. (2016), Cancer statistics in China, 2015.]

Small molecules play an non-trivial role in cancer chemotherapy. Here I focus on inhibitors of 8 protein kinases(name: abbr):
- Cyclin-dependent kinase 2: cdk2
- Epidermal growth factor receptor erbB1: egfr_erbB1
- Glycogen synthase kinase-3 beta: gsk3b
- Hepatocyte growth factor receptor: hgfr
- MAP kinase p38 alpha: map_k_p38a
- Tyrosine-protein kinase LCK: tpk_lck
- Tyrosine-protein kinase SRC: tpk_src
- Vascular endothelial growth factor receptor 2: vegfr2
For each protein kinase, several thousand inhibitors are collected from chembl database, in which molecules with IC50 lower than 10 uM are usually considered as inhibitors, otherwise non-inhibitors.
