Genomic Integration Site Enrichment Analysis
Dataset: L1, HIV, MLV insertion sites and ChromHMM chromatin states (hg19)
Objective: Analyze integration preferences of transposable elements and retroviruses in human chromatin
Author: Nour REBAHI
Date: September 2025
Task 1: Introduction and Objective
Background:
Transposable elements (L1) and retroviruses (HIV, MLV) integrate into host genomes in potentially non-random patterns. Understanding their integration preferences provides insights into silencing mechanisms and mutagenic effects.

Research Question:
Do L1 retrotransposons, HIV lentivirus, and MLV retrovirus exhibit preferential integration into specific functional chromatin regions?

Approach:

Genomic intersection analysis of insertion sites with ChromHMM states
Statistical enrichment testing using hypergeometric distribution
Publication-quality visualization of integration preferences
Methods: Genomic Integration Enrichment Analysis
1. Data Acquisition (INPUTS)
●​ Insertion sites: BED files containing experimentally recorded insertion coordinates
for L1, HIV, and MLV in human genome (hg19). Insertions are 2 bp intervals with the
insertion positioned between the two bases.​
●​ Chromatin states: BED file from ChromHMM 18-state model (HeLa-S3 cells,
ENCODE) providing functional genomic annotations.​
2. Preprocessing and Data Loading
●​ Chromatin BED file loaded into pandas DataFrame, retaining chr, start, end,
state, and computing length (bp).
●​ For each insertion (reported as a 2-bp interval), the midpoint was computed as
(start + end) // 2 and considered the insertion coordinate (converted to a
1-bp interval for BED outputs). Midpoint assignment is appropriate for short insertions
where integration is annotated within a [1,2] bp window.​
●​ Input validation ensured minimum required columns (chromatin ≥4, insertions ≥3).​
3. Intersection of Insertions and Chromatin States
●​ Each insertion midpoint assigned to the chromatin state it falls into.​
●​ Vectorized search via numpy.searchsorted for efficient chromosome-specific
lookup.​
●​ Insertions not overlapping any chromatin state labeled as Unknown.​4. Enrichment Analysis
●​ Observed insertions per element per chromatin state counted.​
●​ Expected insertions calculated assuming uniform distribution proportional to
chromatin state length.​
●​ Statistical test: Two-sided hypergeometric test for over- and under-representation
of insertions per state:​
○​ p_enrich = hypergeom.sf(k-1, total_bg, K, n)​
○​ p_deplete = hypergeom.cdf(k, total_bg, K, n)​
○​ Two-sided p-value: p_two = 2 * min(p_enrich, p_deplete)​
●​ Multiple testing correction: Benjamini-Hochberg FDR
(statsmodels.multipletests).​
●​ Fold enrichment: observed / expected and log2(fold) computed.​
​
5. Quality Control
●​ For each element:​
○​ Total insertions​
○​ Number mapped to chromatin states​
○​ Number unmapped (Unknown)​
○​ Fraction mapped (mapped / total)​6. Visualization (figues are located in separate pdf file)
●​ Heatmap: log2 fold enrichment for each chromatin state (rows) and element
(columns), annotated with significance symbols.​
●​ Barplot: Top 5 enriched and depleted states per element, visualized as horizontal
bars.​
●​ Figure panel: Heatmap and barplot combined into a single figure.​
7. Output Files
●​ intersections.bed: Chromatin state assigned to each insertion​
●​ enrichment_results.csv: Fold enrichment, log2 fold, p-values, significance​
●​ qc_insertions_summary.csv: QC metrics​
●​ enrichment_heatmap.png, enrichment_barplot.png, figure_panel.png:
Visualizations​
8. Software & Dependencies
●​ Python 3, pandas, numpy, scipy, statsmodels, matplotlib, seaborn, PIL (for figure
panel)
●​ sent a link of google colab with code and indications ( for quick easy testing)
