<div class="section">

<h2>1. Introduction</h2>


Kinesin-14 proteins are ATP-dependent motor proteins that move along microtubules and are unique among kinesin families because they primarily travel toward the minus end of microtubules, whereas most other kinesins move toward the plus end. These proteins play essential roles in several key cellular processes, including mitotic spindle organization, chromosome segregation, nuclear positioning, and regulation of cell division. The Kinesin-14 family includes important members such as KIFC1 in humans, Ncd in *Drosophila*, Kar3 in yeast, and HSET in humans. Structurally, these proteins are characterized by a C-terminal motor domain, a coiled-coil region responsible for dimerization, a microtubule-binding region, and an ATP-binding catalytic site that drives their motor activity. Phylogenetic analysis of Kinesin-14 proteins is important because it helps trace their evolutionary relationships, identify conserved functional residues within the motor domain, understand how motor functions have diverged across species, and study the evolutionary emergence of their unique minus-end directed motility.
</div>

<div class="section">

<h2>2. Objectives</h2>

<ul>
<li>Retrieve kinesin-14 homologous sequences</li>
<li>Perform BLAST analysis</li>
<li>Generate multiple sequence alignment (MSA)</li>
<li>Clean and trim alignment</li>
<li>Remove duplicate sequences</li>
<li>Select evolutionary model using ModelTest-NG</li>
<li>Construct phylogenetic tree using RAxML-NG</li>
<li>Perform bootstrap analysis</li>
<li>Reconstruct ancestral sequences</li>
<li>Interpret evolutionary patterns</li>
</ul>

</div>

<div class="section">

<h2>3. Materials and Methods</h2>

<h3>3.1 Software Used</h3>

<ul>
<li>Biopython</li>
<li>BLAST (NCBIWWW)</li>
<li>MAFFT</li>
<li>ModelTest-NG</li>
<li>RAxML-NG</li>
<li>Matplotlib</li>
<li>Bio.Phylo</li>
</ul>

<h3>PART 0 — Environment Setup</h3>

<p>Biopython installation:</p>

<pre>
!pip install biopython
</pre>

<p>MAFFT installation:</p>

<pre>
!apt-get install -qq -y mafft
!mafft --help
</pre>

<p>Conda + RAxML-NG + ModelTest-NG:</p>

<pre>
!wget -q https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O /tmp/miniforge.sh
!bash /tmp/miniforge.sh -b -p /usr/local/miniforge
!rm /tmp/miniforge.sh

import os
os.environ["PATH"] = "/usr/local/miniforge/bin:" + os.environ["PATH"]

!conda install -c bioconda raxml-ng modeltest-ng --yes
</pre>

</div>

<div class="section">

<h3>PART I — Sequence Retrieval</h3>

<p>A kinesin-14 protein was selected:</p>

<p>PDB ID: 1CZ7_A</p>

<p>Sequence retrieval using Biopython:</p>

<pre>
from Bio import SeqIO, Entrez

Entrez.email = "subhanajamil3@gmail.com"

temp = Entrez.efetch(db="protein", rettype="fasta", id="1CZ7_A")

aaseq_out = open("kinesin14_query.fasta",'w')

aaseq = SeqIO.read(temp, format="fasta")

SeqIO.write(aaseq, aaseq_out, "fasta")

temp.close()
aaseq_out.close()
</pre>

</div>

<div class="section">

<h3>Sequence Summary</h3>

<p>The sequence was successfully retrieved and analyzed:</p>

<ul>
<li>Sequence length confirmed</li>
<li>Protein annotation retrieved</li>
<li>Accession ID stored</li>
<li>Amino acid sequence extracted</li>
</ul>

</div>

<div class="section">

<h3>PART II — BLAST Analysis</h3>

<p>BLASTP search was performed:</p>

<pre>
from Bio.Blast import NCBIWWW

result_handle = NCBIWWW.qblast(
    "blastp",
    "pdb",
    aaseq.seq,
    entrez_query='eukaryota[organism]'
)
</pre>

</div>

<div class="section">

<h3>Purpose</h3>

<p>BLAST identifies homologous proteins by comparing sequence similarity.</p>

</div>

<div class="section">

<h3>Important parameters:</h3>

<ul>
<li>E-value (statistical significance)</li>
<li>Identity (% similarity)</li>
<li>Coverage</li>
<li>Alignment length</li>
</ul>

</div>

<div class="section">

<h3>PART III — Parsing BLAST Results</h3>

<pre>
from Bio.Blast import NCBIXML

blast_record = NCBIXML.read(result_handle)
result_handle.close()
</pre>

</div>

<div class="section">

<h3>PART IV — BLAST Hit Inspection</h3>

<p>The BLAST output revealed multiple kinesin-14 homologs.</p>

<p>Each hit included:</p>

<ul>
<li>Protein ID</li>
<li>Alignment length</li>
<li>E-value</li>
</ul>

<p>Low E-values indicated strong evolutionary relationships.</p>

</div>

<div class="section">

<h3>PART V — Filtering BLAST Results</h3>

<p>A sequence identity cutoff was applied:</p>

<pre>
PIDcut = 1.00
</pre>

<p>Filtered results ensured:</p>

<ul>
<li>Removal of redundant sequences</li>
<li>Retention of meaningful homologs</li>
<li>Improved downstream phylogenetic accuracy</li>
</ul>

</div>

<div class="section">

<h3>PART VI — Sequence Retrieval</h3>

<p>Top 20 homologous sequences were downloaded:</p>

<ul>
<li>FASTA format used</li>
<li>Stored in kinesin14_sequences.fasta</li>
</ul>

</div>

<div class="section">

<h3>PART VII — Multiple Sequence Alignment (MAFFT)</h3>

<pre>
!mafft kinesin14_sequences.fasta > aligned_kinesin14.fasta
</pre>

<p>Observations:</p>

<ul>
<li>Highly conserved motor domains</li>
<li>ATP-binding motif conservation</li>
<li>Variable loop regions</li>
<li>Divergent terminal regions</li>
</ul>

</div>

<div class="section">

<h3>PART VIII — Alignment Inspection</h3>

<p>The alignment confirmed structural conservation among kinesin-14 proteins.</p>

</div>

<div class="section">

<h3>PART IX — Alignment Trimming</h3>

<p>Poorly aligned terminal regions were removed:</p>

<ul>
<li>Removed gap-rich regions</li>
<li>Improved alignment quality</li>
<li>Reduced noise in phylogenetic inference</li>
</ul>

</div>

<div class="section">

<h3>PART X — Duplicate Removal</h3>

<p>Duplicate sequences were removed using a custom Python script.</p>

<p>Importance:</p>

<ul>
<li>Tree bias increases without removal</li>
<li>Branch lengths become distorted</li>
<li>Clade support becomes artificially inflated</li>
</ul>

</div>

<div class="section">

<h3>PART XI — Model Selection (ModelTest-NG)</h3>

<pre>
!modeltest-ng -i clear_aligned_trimmed_kinesin14.fasta -d aa
</pre>

<p>Best Model: LG + I + G4</p>

<table>
<tr><th>Component</th><th>Meaning</th></tr>
<tr><td>LG</td><td>Amino acid substitution model</td></tr>
<tr><td>I</td><td>Invariant sites</td></tr>
<tr><td>G4</td><td>Gamma rate variation</td></tr>
</table>

</div>

<div class="section">

<h3>PART XII — Phylogenetic Tree Construction (RAxML-NG)</h3>

<pre>
!raxml-ng \
--msa clear_aligned_trimmed_kinesin14.fasta \
--model LG+I+G4 \
--prefix K14_TREE \
--threads 2
</pre>

<p>Results:</p>

<ul>
<li>Distinct evolutionary clades</li>
<li>Clear separation of kinesin-14 homologs</li>
<li>Conservation of motor domain lineages</li>
</ul>

</div>

<div class="section">

<h3>PART XIII — Tree Visualization</h3>

<p>Clear branching structure observed.</p>
<pre> from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_SUPPORT.raxml.support",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
</pre>
<img width="1004" height="679" alt="2nd last" src="https://github.com/user-attachments/assets/ef1a7096-3488-4e96-bbe4-1c5c04046933" />



</div>

<div class="section">

<h3>PART XIV — Bootstrap Analysis</h3>

<pre>
!raxml-ng \
--bootstrap \
--msa K14_TREE.raxml.rba \
--model LG+I+G4 \
--prefix K14_BOOT \
--threads 2 \
--bs-tree 100
</pre>

<p>Interpretation:</p>

<ul>
<li>Bootstrap values >70 indicate strong support</li>
<li>Lower values indicate uncertain branching</li>
</ul>

</div>

<div class="section">

<h3>PART XV — Bootstrap Support Mapping</h3>

<p>Bootstrap values were added to validate clades.</p>

<pre> from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_SUPPORT.raxml.support",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()</pre>
</div>
<div class="section">

<h3>PART XVII — Ancestral Sequence Reconstruction</h3>

<pre>
!raxml-ng \
--ancestral \
--msa clear_aligned_trimmed_kinesin14.fasta \
--tree K14_TREE.raxml.bestTree \
--model LG+I+G4 \
--prefix K14_ASR
</pre>

<p>Significance:</p>

<ul>
<li>Conservation of ATP-binding residues</li>
<li>Preservation of motor-domain architecture</li>
<li>Evolutionary stability of kinesin function</li>
</ul>

</div>

<div class="section">

<h3>PART XVII — Final Tree Visualization</h3>

<p>Supported tree showed strong evolutionary clusters and reliable topology.</p>

</div>
<pre>
  from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_ASR.raxml.ancestralTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()</pre>
<img width="1004" height="679" alt="last" src="https://github.com/user-attachments/assets/350f3ebb-5ec3-4b90-b921-8cf07f38da8f" />

<div class="section">

<h2>Results and Discussion</h2>

<ul>
<li>Evolutionary relationships identified distinct clades</li>
<li>Functional conservation in motor domain</li>
<li>Divergence in coiled-coil and terminal regions</li>
<li>Strong bootstrap support confirmed reliability</li>
<li>Common ancestral origin supported kinesin-14 evolution</li>
</ul>

</div>

<div class="section">

<h2>Conclusion</h2>

<p>
This study successfully implemented a full phylogenetic workflow for Kinesin-14 proteins using Biopython and RAxML-NG. The results demonstrate strong evolutionary conservation of motor domains, clear phylogenetic clustering, and reliable ancestral reconstruction supporting kinesin functional evolution.
</p>

</div>

</body>
</html>
