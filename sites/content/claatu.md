---
layout: default
title: claatu
---

<html>
<head>
<style>
    .code { 
        background: #d8e3e8;
        padding: 7px 15px;
        border-radius: 5px;
        overflow-x: auto;
        max-width: 100%; 
        white-space: nowrap;
    } 
    .full {
        width: 100%;
    }

</style>
</head> 

<body>

<div> 
<h2>ClaaTU</h2>
<h5><i>A workflow for quantifying clade abundance in phylogenetic trees.</i></h5>

<b>Version: 0.1</b>
</div> 

<div>
<p/>
<hr/>
<h4>Overview</h4>
</div>

<p align="center"><img src="/sites/docs/images/claatu/claatu_fig.png" width="500" align="middle"/> </p>

<div>
Incorporating phylogeny into the assessment of how microbial lineages are distributed across communities can identify monophyletic clades of microbes that collectively manifest an association with ecological factors. For example, the clade highlighted in red in the image above is universally present across all mammalian microbiome samples, indicating that the clade may have evolved a conserved trait that facilitated its ubiquitous distribution. If we were to consider this relationship at the OTU level (i.e., considering the tips of the tree as appropriate units), the redundancy of OTUs within this clade would obscure the detection of this relationship.  On the other hand, if we were to consider the genus level, the aggregation of this clade with others that do not possess the trait would similarly obscure this relationship. The ClaaTU (<b>Cla</b>d<b>a</b>l <b>T</b>axonomic <b>U</b>nits) workflow uses data files produced by third party microbiome analysis software (e.g., QIIME, Mothur, DADA2, etc.) to identify and quantify the abundance of specific clades in a user provided phylogenetic tree. This allows us to examine the abundance of clades across the input tree from tip to root. Currently, ClaaTU is run as a collection of scripts and a workflow is provided below. 

</div> 

<div> 
<h4><b> Dependencies </b></h4> 
</div> 

<div> 
    <ol>
        <li> python (2.7.10)  </li>
        <li> dendropy (4.0.2) </li>
        <li> scipy (0.13.0b1) </li>
        <li> numpy (1.8.0rc1) </li>
    </ol>
</div>

<div>
<i>Note: ClaaTU has only been tested with the versions of the packages indicated.</i>
</div> 
<hr/> 

<div> 
<table class="borderless full">
   <tbody> 
   <tr> 
     <td> If you would like to try ClaaTU you can get the code here </td> 
     <td> <a href="https://github.com/chrisgaulke/Claatu" class="btn btn-outline-success" role="button">Try ClaaTU!</a></td>
   </tr>
   </tbody>
</table>
</div>

<hr/>


<h3><b> Workflow</b></h3>

<p align="center"><img src="/sites/docs/images/claatu/claatu_prep_tree.png" width="500" align="middle"/> </p>

<div>
<b>1.</b> The first thing that ClaaTU needs is a newick tree without internal node identifiers. In this step ClaaTU decorates internal nodes with node identifiers that will be used in downstream scripts. If internal node identifiers are present in the tree provided Claatu will overwrite them.  
</div>
<br>
<div class="code"> 
    <code> python &lt;path_to_ClaaTU/bin/prep_tree.py&gt; &lt;input_newick_tree.tre&gt; </code>  
</div>
<br>

    The output of this script is a phylogenetic tree named "new_prepped_tree.tre". You will use this tree for the rest of the workflow.    

<p align="center"><img src="/sites/docs/images/claatu/claatu_counts.png" width="500" align="middle"/> </p>

<div>
<b>2.</b> Now that we have the ClaaTU ready tree we can get to work. The next script that we will run will take in a tab delimited table of OTU counts. You should have this from your microbiome analysis. Note that ClaaTU will not recognize a biom formatted table, so be sure to convert your biom table to text format. 
</div>

<br>

<div class="code">
    <code> python path_to_count_tree.py &lt;otu_table&gt; &lt;prepped_tree&gt; &lt;out_file_path&gt; </code> 
</div>
<br>
<div>
The output of this script is a clade counts table with internal node identifiers as columns and sample IDs as rows. These data can be used to examine differential abundance of clades across a case and control study. For some, this might be where you stop if all you care about is what clades are in a sample and at what abundance. However, we have add some additional scripts downstream that some might find useful.  
</div>

<br>

<div>
<b>3.</b> We realize that many researchers may be interested in the taxonomic labels associated with each clade, particularly if the clade is significantly different between case and control. Currently, we have attempted to address this need by implementing a strategy of propagation of taxonomic labels that use the taxonomic association of the tips (OTUs) and attempts to assign them to the ancestors of this tip. Importantly, this taxonomic label is only assigned to the ancestor of the tip if 100% of the other tips contained by the parent node also share this label. This script requires a taxonomy file, which may have been an output of upstream microbiome analysis. For example, if you used QIIME to analyze your data this file will be called something like rep_set_tax_assignments.txt. The first two columns of this taxonomy file should be an OTU ID followed by a taxonomy string. This file must be tab delimited. Taxonomic label propagation can be performed by executing the following:
</div>
<br> 

<div class="code"> 
    <code> python &lt;path_to_ClaaTU/bin/clade_stat.py&gt; &lt;prepped_tree&gt; &lt;tax_file&gt; &lt;out_file_path&gt; -p &lt;file_prefix&gt; </code>
</div>
<br>

<div>
    This generates a tab delimited text file (&lt;file prefix&gt; node2tax.txt) with clade ID in the first column and a list of taxonomy strings in the second column.  
</div> 
<br>
<p align="center"><img src="/sites/docs/images/claatu/claatu_tax.png" width="500" align="middle"/> </p>

<div>
<b>4.</b> Next we try to find which taxonomic level (if any) is shared by each tip in a clade. 
</div>
<br>
<div class="code">	
    <code> python &lt;path_to_ClaaTU/bin/tax_parser.py&gt; &lt;prefix&gt;_node2tax.txt &lt;outfile_name&gt;</code>
</div>
<br>
<div>
This will output a final taxonomy dictionary that contains a mapping of clade (column 1) to tax ID (column 2).
</div>
<br>
<p align="center"><img src="/sites/docs/images/claatu/claatu_size_level.png" width="500" align="middle"/> </p>

<div>
<b>5.</b> Finally, we gather some information about the tree and the nodes in it. 
</div>
<br>
<div class="code"> 
    <code>python &lt;path_to_ClaaTU/bin/node_info.py&gt; &lt;prepped_tree&gt; &lt;out_file_path&gt; -p &lt;file_prefix&gt;</code>
</div>
<br>
<div>
    This should produce three files: &lt;file_prefix&gt;_levels.txt which contains information about how nested a node is, &lt;file_prefix&gt;_dist_median.txt which is a single line file containing the median branch length to the root, and &lt;file_prefix&gt;_dist.txt which gives the branch length to the root from each node. 
</div> 

<br>
<div>
<b>6.</b> [optional] To determine if a clade is more core than expected by random chance we can conduct a ptest. By default this will evaluate coreness across all samples in an OTU table. For this analysis you will need a OTU table in text format, and a prepped tree. You also must specify the number of permutations with the -p flag.
</div>
<br>
<div class="code">
	<code>python &lt;path_to_ClaaTU/bin/ptest_tree.py&gt; &lt;otu_table.txt&gt; &lt;prepped_tree&gt; &lt;outfile.txt&gt; -p &lt;# permutations&gt;</code>
</div>
<br>
<div>
    Alternatively, you can create a mapping file that maps sample ids to some group id. This mapping file should be tab delimited (i.e.,sample_ID tab group_id) and should not contain a header. 
</div>
<br>
<div class="code">
    <code> python &lt;path_to_ClaaTU/bin/ptest_tree.py&gt; &lt;otu_table.txt&gt; &lt;prepped_tree&gt; &lt;outfile.txt&gt; -p &lt;# permutations&gt; -g &lt;mapping_file&gt;</code>
</div>
<br>
<div>
    After the completing the random permutations ptest_tree.py calculates a zscore and p-value for each observed coreness value. When no mapping file is given to ptest_tree.py one pvalue is calculated per clade. However, if a mapping file is passed to ptest_tree.py then a p-value is calculated for each clade and each group. For example, if you have 3 groups in the mapping file each clade will have three p-values, one for each group. The output of this will be two files 1) outfile.txt (where outfile is the name you specified), and 2) out_file.txt_stats.txt. Outfile.txt will contain several columns, the first is the clade_ID, the second is the observed coreness, and the third is the coreness of the permuted OTU table. Outfile.txt_stats.txt will contain a number of summary statistics in several columns. Column 1 contains the node names, column 2 is the observed coreness, column 3 is the mean coreness of the permutations, column 4 is the standard deviation of the coreness permutations, column 5 is the zscore, column 6 is the p-value of the ztest.
</div>

<hr/>

<div> 
<h4>Funding</h4>
This material is based upon work supported by the National Science Foundation under Grant Number 1557192. Any opinions, findings, and conclusions or recommendations expressed in this material are those of the author(s) and do not necessarily reflect the views of the National Science Foundation.
</div> 

<br>
<div> 
<h4><b>Note</b></h4>
Like most bioinformatic software CLaaTu is a work in progress. Growing pains are to be expected so please let me know if you discover a bug. 
</div>   

<br>

<div>
<h4><b>Citation</b></h4>

Ecophylogenetics Clarifies the Evolutionary Association between Mammals and Their Gut Microbiota
Christopher A. Gaulke, Holly K. Arnold, Ian R. Humphreys, Steven W. Kembel, James P. O’Dwyer, Thomas J. Sharpton.
mBio, Sep 2018, 9 (5) e01348-18; DOI: 10.1128/mBio.01348-18
</div>

