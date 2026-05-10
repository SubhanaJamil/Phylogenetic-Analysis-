

<h1>Phylogenetic Analysis of Kinesin Motor Proteins using Biopython and RAxML</h1>

<div class="section">

<h2>Introduction</h2>

<h3>What are Kinesins?</h3>

<p>
Kinesins are a large superfamily of molecular motor proteins found in eukaryotic organisms. 
They are responsible for intracellular transport and movement along microtubules. 
These proteins convert chemical energy derived from ATP hydrolysis into mechanical force.
</p>

<p>Kinesins are involved in numerous biological processes including:</p>

<ul>
<li>Vesicle transport</li>
<li>Organelle movement</li>
<li>Chromosome segregation</li>
<li>Mitotic spindle formation</li>
<li>Cell division</li>
<li>Intracellular trafficking</li>
</ul>

<p>
Most kinesins move toward the plus end of microtubules, whereas members of the Kinesin-14 family move toward the minus end.
</p>

<p>Structurally, kinesins contain:</p>

<ul>
<li>Motor domains</li>
<li>ATP-binding regions</li>
<li>Microtubule-binding sites</li>
<li>Coiled-coil stalk regions</li>
<li>Cargo-binding domains</li>
</ul>

<p>Phylogenetic analysis helps understand:</p>

<ul>
<li>Evolutionary divergence of kinesin families</li>
<li>Functional specialization</li>
<li>Conservation of catalytic residues</li>
<li>Emergence of directional movement</li>
<li>Relationships between kinesin subfamilies</li>
</ul>

</div>

<div class="section">

<h2>Objectives</h2>

<p>The objectives of this study were:</p>

<ul>
<li>To retrieve homologous kinesin motor protein sequences.</li>
<li>To perform BLAST analysis using Biopython.</li>
<li>To generate a multiple sequence alignment using MAFFT.</li>
<li>To trim and curate the alignment.</li>
<li>To remove redundant sequences.</li>
<li>To determine the best evolutionary model using ModelTest-NG.</li>
<li>To construct a phylogenetic tree using RAxML-NG.</li>
<li>To perform bootstrap analysis for tree validation.</li>
<li>To reconstruct ancestral kinesin sequences.</li>
<li>To analyze evolutionary conservation and divergence among kinesin proteins.</li>
</ul>

</div>

<div class="section">

<h2>Materials and Software</h2>

<table>

<tr>
<th>Software/Tool</th>
<th>Purpose</th>
</tr>

<tr>
<td>Python</td>
<td>Programming environment</td>
</tr>

<tr>
<td>Google Colab</td>
<td>Cloud-based computational platform</td>
</tr>

<tr>
<td>Biopython</td>
<td>Sequence retrieval and analysis</td>
</tr>

<tr>
<td>BLASTP</td>
<td>Homology search</td>
</tr>

<tr>
<td>MAFFT</td>
<td>Multiple sequence alignment</td>
</tr>

<tr>
<td>ModelTest-NG</td>
<td>Model selection</td>
</tr>

<tr>
<td>RAxML-NG</td>
<td>Phylogenetic inference</td>
</tr>

<tr>
<td>Matplotlib</td>
<td>Tree visualization</td>
</tr>

<tr>
<td>Bio.Phylo</td>
<td>Phylogenetic tree analysis</td>
</tr>

</table>

</div>

<div class="section">

<h2>Methodology</h2>

<h3>PART 0 — Installation of Required Software</h3>

<h4>Installing Biopython</h4>

<pre>
!pip install biopython

import os
import sys
import Bio
</pre>

<h4>Purpose</h4>

<p>
Biopython provides tools for:
</p>

<ul>
<li>Sequence retrieval</li>
<li>FASTA parsing</li>
<li>BLAST analysis</li>
<li>Phylogenetic tree handling</li>
</ul>

<h4>Installing MAFFT</h4>

<pre>
!apt-get install -qq -y mafft
!mafft --help
</pre>

<h4>Purpose</h4>

<p>
MAFFT performs multiple sequence alignment (MSA) of protein sequences.
</p>

<h4>Installing Conda, RAxML-NG and ModelTest-NG</h4>

<pre>
import os

!wget -q https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O /tmp/miniforge.sh
!bash /tmp/miniforge.sh -b -p /usr/local/miniforge
!rm /tmp/miniforge.sh

os.environ["PATH"] = "/usr/local/miniforge/bin:" + os.environ["PATH"]

!conda --version
!conda config --set always_yes yes --set changeps1 no

!conda install -c conda-forge mamba
!conda install -c bioconda raxml-ng modeltest-ng --yes
</pre>

<h4>Purpose</h4>

<p>
ModelTest-NG selects the best substitution model.
</p>

<p>
RAxML-NG constructs phylogenetic trees using Maximum Likelihood methods.
</p>

</div>

<div class="section">

<h3>PART I — Retrieval of Kinesin Protein Sequences</h3>

<h4>Selection of Query Protein</h4>

<p>The kinesin motor protein selected was:</p>

<p><strong>Human Kinesin Family Member 5B (KIF5B)</strong></p>

<p>PDB accession used:</p>

<p><strong>3KIN_A</strong></p>

<h4>Sequence Retrieval using Entrez</h4>

<pre>
from Bio import SeqIO, Entrez

Entrez.email = "your_email@example.com"

temp = Entrez.efetch(
    db="protein",
    rettype="fasta",
    id="3KIN_A"
)

aaseq_out = open("kinesin_motor.fasta",'w')

aaseq = SeqIO.read(temp, format="fasta")

SeqIO.write(aaseq, aaseq_out, "fasta")

temp.close()
aaseq_out.close()
</pre>

<h4>Explanation</h4>

<ul>
<li>Entrez.efetch() downloads the sequence from NCBI.</li>
<li>SeqIO.read() parses the FASTA sequence.</li>
<li>The sequence is saved into a FASTA file.</li>
</ul>

<h4>Sequence Information</h4>

<pre>
print("Sequence Length:")
print(len(aaseq))

print("\nDescription:")
print(aaseq.description)

print("\nSequence ID:")
print(aaseq.id)

print("\nSequence:")
print(aaseq.seq)
</pre>

<div class="section">

<h4>Sequence Information and Observations</h4>

<ul>
<li><b>Sequence Length:</b> 238</li>

<li><b>Description:</b> pdb|3KIN|A Chain A, KINESIN HEAVY CHAIN</li>

<li><b>Sequence ID:</b> pdb|3KIN|A</li>

<li><b>Amino Acid Sequence:</b></li>
</ul>

<pre>
ADPAECSIKVMCRFRPLNEAEILRGDKFIPKFKGEETVVIGQGKPYVFDRVLPPNTTQEQVYNACAKQIVKDVLEGYNGTIFAYGQTSSGKTHTMEGKLHDPQLMGIIPRIAHDIFDHIYSMDENLEFHIKVSYFEIYLDKIRDLLDVSKTNLAVHEDKNRVPYVKGCTERFVSSPEEVMDVIDEGKANRHVAVTNMNEHSSRSHSIFLINIKQENVETEKKLSGKLYLVDLAGSEKV
</pre>

</div>
This confirmed successful retrieval of the kinesin motor protein.
</p>

</div>

<div class="section">

<h3>PART II — BLAST Search using Biopython</h3>

<pre>
from Bio.Blast import NCBIWWW

result_handle = NCBIWWW.qblast(
    "blastp",
    "pdb",
    aaseq.seq,
    entrez_query='eukaryota[organism]'
)
</pre>

<h4>Principle of BLAST</h4>

<p>
BLASTP compares the query protein sequence against protein databases to identify homologous proteins.
</p>

<h4>Important BLAST Parameters</h4>

<table>

<tr>
<th>Parameter</th>
<th>Meaning</th>
</tr>

<tr>
<td>E-value</td>
<td>Statistical significance</td>
</tr>

<tr>
<td>Identity</td>
<td>Similarity percentage</td>
</tr>

<tr>
<td>Coverage</td>
<td>Aligned sequence fraction</td>
</tr>

<tr>
<td>Alignment length</td>
<td>Length of alignment</td>
</tr>

</table>

</div>

<div class="section">

<h3>PART III — Parsing BLAST Results</h3>

<pre>
from Bio.Blast import NCBIXML

blast_record = NCBIXML.read(result_handle)

result_handle.close()
</pre>

<h4>Displaying BLAST Hits</h4>

<pre>
for alignment in blast_record.alignments:
    for hsp in alignment.hsps:

        print("Hit ID:")
        print(alignment.hit_id)

        print("Alignment Length:")
        print(hsp.align_length)

        print("E-value:")
        print(hsp.expect)

        print()
</pre>

<div class="section">

<h4>BLAST Hits (Top Results)</h4>
<p>The BLAST output displayed:</p>
<ul>
<li><b>Hit ID:</b> pdb|5GSZ|A | <b>Alignment Length:</b> 257 | <b>E-value:</b> 9.50998e-43</li>

<li><b>Hit ID:</b> pdb|5GSY|K | <b>Alignment Length:</b> 257 | <b>E-value:</b> 1.10681e-42</li>

<li><b>Hit ID:</b> pdb|7A40|A | <b>Alignment Length:</b> 240 | <b>E-value:</b> 2.35871e-42</li>

<li><b>Hit ID:</b> pdb|7A3Z|A | <b>Alignment Length:</b> 240 | <b>E-value:</b> 4.66892e-42</li>

<li><b>Hit ID:</b> pdb|3B6V|A | <b>Alignment Length:</b> 247 | <b>E-value:</b> 2.19182e-41</li>

<li><b>Hit ID:</b> pdb|4AQV|C | <b>Alignment Length:</b> 262 | <b>E-value:</b> 4.1622e-41</li>

<li><b>Hit ID:</b> pdb|5ZBS|A | <b>Alignment Length:</b> 259 | <b>E-value:</b> 4.33382e-41</li>

<li><b>Hit ID:</b> pdb|6A1Z|A | <b>Alignment Length:</b> 263 | <b>E-value:</b> 4.43793e-41</li>
</ul>

<p>
These BLAST results show highly significant matches with very low E-values, indicating strong evolutionary conservation among kinesin homologs.
</p>

</div>

<div class="section">

<h3>PART IV — Filtering BLAST Results</h3>

<pre>
PIDcut = 1.00

for alignment in blast_record.alignments:

    for hsp in alignment.hsps:

        if (hsp.identities/hsp.align_length) <= PIDcut:

            print("Accession:", alignment.hit_id)

            print("Length:", alignment.length)

            print("Alignment Length:", hsp.align_length)

            print("E-value:", hsp.expect)

            print("Sequence Identity [%]:",
                  "{:.2f}".format(
                      100*hsp.identities/hsp.align_length
                  ))

            print("Coverage [%]:",
                  "{:.2f}".format(
                      100*sum(c.isalpha() for c in hsp.query)/len(aaseq)
                  ))

            print()
</pre>

<h4>Importance of Filtering</h4>

<p>Filtering removes:</p>

<ul>
<li>Very similar redundant sequences</li>
<li>Poor-quality hits</li>
<li>Partial sequences</li>
</ul>

<p>
This improves phylogenetic accuracy.
</p>

<div class="section">

<h4>BLAST Results with Coverage and Identity</h4>

<ul>

<li>
<b>Accession:</b> pdb|5GSZ|A | 
<b>Length:</b> 353 | 
<b>Alignment Length:</b> 257 | 
<b>E-value:</b> 9.50998e-43 | 
<b>Sequence Identity [%]:</b> 34.24 | 
<b>Coverage [%]:</b> 100.00
</li>

<li>
<b>Accession:</b> pdb|5GSY|K | 
<b>Length:</b> 360 | 
<b>Alignment Length:</b> 257 | 
<b>E-value:</b> 1.10681e-42 | 
<b>Sequence Identity [%]:</b> 34.24 | 
<b>Coverage [%]:</b> 100.00
</li>

<li>
<b>Accession:</b> pdb|7A40|A | 
<b>Length:</b> 346 | 
<b>Alignment Length:</b> 240 | 
<b>E-value:</b> 2.35871e-42 | 
<b>Sequence Identity [%]:</b> 39.58 | 
<b>Coverage [%]:</b> 97.06
</li>

<li>
<b>Accession:</b> pdb|7A3Z|A | 
<b>Length:</b> 372 | 
<b>Alignment Length:</b> 240 | 
<b>E-value:</b> 4.66892e-42 | 
<b>Sequence Identity [%]:</b> 39.58 | 
<b>Coverage [%]:</b> 97.06
</li>

<li>
<b>Accession:</b> pdb|3B6V|A | 
<b>Length:</b> 395 | 
<b>Alignment Length:</b> 247 | 
<b>E-value:</b> 2.19182e-41 | 
<b>Sequence Identity [%]:</b> 36.03 | 
<b>Coverage [%]:</b> 98.32
</li>

<li>
<b>Accession:</b> pdb|4AQV|C | 
<b>Length:</b> 373 | 
<b>Alignment Length:</b> 262 | 
<b>E-value:</b> 4.1622e-41 | 
<b>Sequence Identity [%]:</b> 39.69 | 
<b>Coverage [%]:</b> 97.48
</li>

<li>
<b>Accession:</b> pdb|5ZBS|A | 
<b>Length:</b> 375 | 
<b>Alignment Length:</b> 259 | 
<b>E-value:</b> 4.33382e-41 | 
<b>Sequence Identity [%]:</b> 35.91 | 
<b>Coverage [%]:</b> 97.06
</li>

<li>
<b>Accession:</b> pdb|6A1Z|A | 
<b>Length:</b> 395 | 
<b>Alignment Length:</b> 263 | 
<b>E-value:</b> 4.43793e-41 | 
<b>Sequence Identity [%]:</b> 35.36 | 
<b>Coverage [%]:</b> 98.74
</li>

</ul>

<p>
Overall coverage values are consistently high (≈97–100%), indicating strong and complete alignment across the kinesin motor domain.
</p>

</div>
</div>

<div class="section">

<h3>PART V — Downloading Homologous Sequences</h3>

<pre>
Entrez.email = "your_email@example.com"

with open("kinesin_sequences.fasta", "a") as allhits_out:

    for alignment in blast_record.alignments[:20]:

        for hsp in alignment.hsps:

            if(hsp.identities/len(hsp.match)) <= PIDcut:

                print("Fetching:", alignment.hit_id)

                fetch = Entrez.efetch(
                    db="protein",
                    id=alignment.hit_id,
                    rettype="fasta"
                )

                allhits_seqs = SeqIO.read(fetch, format="fasta")

                SeqIO.write(
                    allhits_seqs,
                    allhits_out,
                    "fasta"
                )

                fetch.close()

allhits_out.close()
</pre>

<h4>Result</h4>

<p>
Top homologous kinesin protein sequences were collected into a single FASTA file.
</p>

</div>

<div class="section">

<h3>PART VI — Multiple Sequence Alignment using MAFFT</h3>

<pre>
!mafft kinesin_sequences.fasta > aligned_kinesin.fasta
</pre>

<h4>Importance of MSA</h4>

<p>
Multiple sequence alignment identifies:
</p>

<ul>
<li>Conserved residues</li>
<li>Functional motifs</li>
<li>Evolutionary relationships</li>
<li>Insertions and deletions</li>
</ul>

<p>
The kinesin motor domain showed high conservation among homologs.
</p>

</div>

<div class="section">

<h3>PART VII — Visualization of Alignment</h3>

<pre>
from Bio import AlignIO

aln = AlignIO.read("aligned_kinesin.fasta", "fasta")

print(aln)
</pre>

<div class="section">

<h3>Multiple Sequence Alignment (MSA)</h3>

<h4>Alignment Overview</h4>

<pre>
Alignment with 40 rows and 434 columns
</pre>

<h4>Aligned Sequences</h4>

<pre>
------------------------ADP-AECSIKVMCRFRPLNE...--- pdb|2KIN|A
-----------------------MADP-AECSIKVMCRFRPLNE...--- pdb|3J6H|K
-----------------------MADP-AECSIKVMCRFRPLNE...--- pdb|3WRD|A
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|4UXT|C
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|9T17|C
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|9UMM|A
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|1MKJ|A
-----------------------MADP-AECNIKVMCRFRPLNE...--- pdb|4ATX|C
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|9L7M|K
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|1BG2|A
-----------------------MADL-AECNIKVMCRFRPLNE...SPT pdb|9GNQ|K
MGSSHHHHHHSSGLVPRGSHMASMADL-AECNIKVMCRFRPLNE...--- pdb|8IXA|S
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|3J8X|K
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|4HNA|K
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|4LNU|K
---MHHHHH--------------HADL-AESNIKVMCRFRPLNE...--- pdb|9L78|A
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|9L7E|A
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|5LT3|A
...
---MHHHHH--------------HADL-AESNIKVMCRFRPLNE...--- pdb|9L6K|A
</pre>

<h4>Observations</h4>

<ul>
<li>Conserved ATP-binding motifs are clearly present across sequences</li>
<li>Microtubule interaction residues are highly conserved</li>
<li>Core motor domain shows strong evolutionary conservation</li>
<li>Loop and terminal regions show high variability and gaps</li>
<li>Some sequences contain experimental tags (e.g., His-tags like MHHHHH)</li>
<li>Overall alignment confirms strong conservation within the kinesin family</li>
</ul>

</div>

<div class="section">

<h3>PART VIII — Alignment Trimming</h3>

<pre>
from Bio import AlignIO
from Bio import SeqIO

aln = AlignIO.read("aligned_kinesin.fasta", "fasta")

for fcol in range(aln.get_alignment_length()):

    if "-" not in aln[:, fcol]:
        position1 = fcol
        break

for lcol in reversed(range(aln.get_alignment_length())):

    if "-" not in aln[:, lcol]:
        position2 = lcol + 1
        break

with open("aligned_trimmed_kinesin.fasta", "w") as handle:

    SeqIO.write(
        aln[:, position1:position2],
        handle,
        "fasta"
    )
</pre>

<h4>Purpose of Trimming</h4>

<p>
Trimming removes poorly aligned terminal regions containing excessive gaps.
</p>

<p>Benefits include:</p>

<ul>
<li>Improved phylogenetic accuracy</li>
<li>Reduced alignment noise</li>
<li>Better model estimation</li>
</ul>

</div>

<div class="section">

<h3>PART IX — Removal of Duplicate Sequences</h3>

<pre>
from Bio import SeqIO

def sequence_cleaner(fasta_file, min_length=0):

    sequences = {}

    for seq_record in SeqIO.parse(fasta_file, "fasta"):

        sequence = str(seq_record.seq).upper()

        if len(sequence) >= min_length:

            if sequence not in sequences:

                sequences[sequence] = seq_record.id

            else:
                sequences[sequence] += "_" + seq_record.id

    with open("clear_" + fasta_file, "w+") as output_file:

        for sequence in sequences:

            output_file.write(
                ">" + sequences[sequence] +
                "\n" + sequence + "\n"
            )

    print("Duplicates Removed!")

sequence_cleaner('aligned_trimmed_kinesin.fasta', 0)
</pre>

<h4>Why Duplicate Removal is Important</h4>

<p>Duplicate sequences can:</p>

<ul>
<li>Bias branch lengths</li>
<li>Distort evolutionary relationships</li>
<li>Artificially strengthen clades</li>
</ul>

<p>
Removing redundancy improves tree reliability.
</p>

</div>

<div class="section">

<h3>PART X — Model Selection using ModelTest-NG</h3>

<pre>
!modeltest-ng -i clear_aligned_trimmed_kinesin.fasta -d aa
</pre>

<h4>Evolutionary Model Selection</h4>

<p>
ModelTest-NG evaluates different substitution models.
</p>

<p>The best-fit model obtained was:</p>

<h4>LG+I+G4</h4>

<h4>Meaning of the Model</h4>

<table>

<tr>
<th>Component</th>
<th>Meaning</th>
</tr>

<tr>
<td>LG</td>
<td>Le-Gascuel amino acid substitution model</td>
</tr>

<tr>
<td>I</td>
<td>Invariant sites</td>
</tr>

<tr>
<td>G4</td>
<td>Gamma-distributed rate variation</td>
</tr>

</table>

<p>
This model best explained the evolutionary changes observed in kinesin proteins.
</p>

</div>

<div class="section">

<h3>PART XI — Phylogenetic Inference using RAxML-NG</h3>

<pre>
!raxml-ng \
--msa clear_aligned_trimmed_kinesin.fasta \
--model LG+I+G4 \
--prefix KINESIN_TREE \
--threads 2
</pre>

<h4>Maximum Likelihood Phylogeny</h4>

<p>
RAxML-NG constructed the phylogenetic tree using:
</p>

<ul>
<li>Multiple sequence alignment</li>
<li>Evolutionary substitution model</li>
<li>Maximum likelihood optimization</li>
</ul>

<h4>Observations</h4>

<p>The resulting phylogenetic tree showed:</p>

<ul>
<li>Clustering of related kinesin homologs</li>
<li>Evolutionary divergence among kinesin families</li>
<li>Conserved motor-domain lineages</li>
</ul>

</div>

<div class="section">

<h3>PART XII — Tree Visualization</h3>

<pre>
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "KINESIN_TREE.raxml.bestTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
  </pre>
  
<img width="1132" height="679" alt="3rd last part" src="https://github.com/user-attachments/assets/ea7cab6d-770e-482b-9eb1-d582ecf11fbe" />

<h4>Interpretation of Tree</h4>

<p>The tree displayed:</p>

<ul>
<li>Distinct clades of kinesin proteins</li>
<li>Shared ancestral nodes</li>
<li>Evolutionary distances</li>
</ul>

<p>
Closely related proteins clustered together, indicating conserved evolutionary origins.
</p>

</div>

<div class="section">

<h3>PART XIII — Bootstrap Analysis</h3>

<pre>
!raxml-ng \
--bootstrap \
--msa KINESIN_TREE.raxml.rba \
--model LG+I+G4 \
--prefix BOOTSTRAP \
--threads 2 \
--bs-tree 100
</pre>

<h4>Importance of Bootstrapping</h4>

<p>
Bootstrapping evaluates tree reliability through repeated resampling.
</p>

<h4>Interpretation</h4>

<p>
Bootstrap > 70% → reliable clade
</p>

<p>
Bootstrap < 50% → weak support
</p>

</div>

<div class="section">

<h3>PART XIV — Adding Bootstrap Support</h3>

<pre>
!raxml-ng \
--support \
--tree KINESIN_TREE.raxml.bestTree \
--bs-trees BOOTSTRAP.raxml.bootstraps \
--prefix FINALTREE \
--threads 2
</pre>

<h4>Visualization of Supported Tree</h4>

<pre>
tree = Phylo.read(
    "FINALTREE.raxml.support",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
  </pre>
<img width="1132" height="679" alt="2nd last part" src="https://github.com/user-attachments/assets/18b8f23a-62c9-46d7-8991-9ef036c13131" />

<h4>Observations</h4>

<p>The tree demonstrated:</p>

<ul>
<li>Strongly supported kinesin clades</li>
<li>Reliable branching patterns</li>
<li>Evolutionary conservation among motor proteins</li>
</ul>

</div>

<div class="section">

<h3>PART XV — Ancestral Sequence Reconstruction</h3>

<pre>
!raxml-ng \
--ancestral \
--msa clear_aligned_trimmed_kinesin.fasta \
--tree KINESIN_TREE.raxml.bestTree \
--model LG+I+G4 \
--prefix ASR
</pre>

<h4>Purpose of ASR</h4>

<p>
Ancestral sequence reconstruction estimates the most probable ancestral protein sequences at internal tree nodes.
</p>

<p>This helps understand:</p>

<ul>
<li>Ancient kinesin proteins</li>
<li>Evolutionary mutations</li>
<li>Functional conservation</li>
</ul>

</div>

<div class="section">

<h3>PART XVI — Visualization of Ancestral Tree</h3>

<pre>
tree = Phylo.read(
    "ASR.raxml.ancestralTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
  </pre>
  <img width="1132" height="679" alt="last part" src="https://github.com/user-attachments/assets/8fc62c35-0f8c-415e-aea7-05f74f878fe3" />


</div>

<div class="section">

<h2>Results and Discussion</h2>

<h3>Sequence Retrieval and BLAST Analysis</h3>

<p>
The kinesin motor protein sequence was successfully retrieved from the Protein Data Bank. BLASTP analysis identified multiple homologous kinesin proteins from eukaryotic organisms with highly significant E-values, indicating strong evolutionary conservation.
</p>

<h3>Multiple Sequence Alignment</h3>

<p>
MAFFT alignment revealed highly conserved amino acid residues in the kinesin motor domain. Conserved ATP-binding motifs and microtubule interaction regions were observed across homologous proteins.
</p>

<p>
Variable regions mainly occurred in loop and terminal segments.
</p>

<h3>Phylogenetic Analysis</h3>

<p>
The maximum likelihood phylogenetic tree grouped kinesin proteins into distinct clades according to sequence similarity and evolutionary relationships.
</p>

<p>Key findings included:</p>

<ul>
<li>Strong conservation of kinesin motor domains</li>
<li>Divergence among kinesin subfamilies</li>
<li>Evolutionary clustering of homologous proteins</li>
</ul>

<h3>Bootstrap Analysis</h3>

<p>
Bootstrap values supported the reliability of most major branches. Clades with bootstrap support greater than 70% were considered statistically reliable.
</p>

<h3>Ancestral Sequence Reconstruction</h3>

<p>
Ancestral reconstruction suggested that critical residues responsible for ATP binding and motor activity were already conserved in ancestral kinesin proteins.
</p>

<p>This indicates that:</p>

<ul>
<li>Motor function evolved early</li>
<li>ATPase activity was strongly conserved</li>
<li>Functional divergence mainly occurred in non-motor regions</li>
</ul>

</div>

<div class="section">

<h2>Conclusion</h2>

<p>
This study successfully demonstrated a complete phylogenetic workflow for kinesin motor proteins using Google Colab, Biopython, MAFFT, ModelTest-NG, and RAxML-NG.
</p>

<p>
The analysis showed that kinesin motor domains are highly conserved evolutionarily, while certain regions exhibit divergence related to functional specialization. The phylogenetic tree revealed clear clustering patterns among kinesin homologs, and bootstrap analysis validated the reliability of major clades.
</p>

<p>
Ancestral sequence reconstruction further demonstrated conservation of key ATP-binding and microtubule-binding residues throughout kinesin evolution.
</p>

<p>
Overall, this workflow provides a robust computational framework for evolutionary analysis of protein families.
</p>

</div>

<div class="section">

<h2>References</h2>

<ul>
<li>Stamatakis A. RAxML version 8: A tool for phylogenetic analysis.</li>

<li>Katoh K et al. MAFFT multiple sequence alignment software.</li>

<li>Biopython Project Documentation.</li>

<li>Le SQ and Gascuel O. LG substitution model.</li>

<li>NCBI BLAST Documentation.</li>

<li>ModelTest-NG Documentation.</li>

<li>Campbell Biology, 12th Edition.</li>

<li>Protein Data Bank (PDB).</li>
</ul>

</div>
