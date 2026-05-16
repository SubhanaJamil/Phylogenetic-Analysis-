
<header>
    <h1>Exploring Interactive Phylogenies with Auspice</h1>
    
</header>

<section>

<h2>1. Introduction</h2>

Kinesin proteins are ATP-dependent motor proteins that move along microtubules and are essential for intracellular transport and cell division. Among them, the Kinesin-14 family is unique because it moves toward the minus end of microtubules, unlike most kinesins that move toward the plus end.

These proteins play key roles in:

Mitotic spindle formation  
Chromosome segregation  
Nuclear positioning  
Cellular structural organization  

In this project, we perform a bioinformatics-based phylogenetic analysis of Kinesin-14-related protein sequences using:

NCBI Entrez database  
BLAST (sequence similarity search)  
MAFFT (multiple sequence alignment)  
RAxML-NG (phylogenetic tree construction)  
Matplotlib (visualization)  

The final goal is to generate an evolutionary tree that can later be visualized in tools like Auspice/Nextstrain-style interactive phylogenies.

</section>

<section>

<h2>2. Installation of Required Tools</h2>

Code:

<pre>
!pip install biopython
!apt-get install -qq -y mafft

!wget -q https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O miniforge.sh
!bash miniforge.sh -b -p /usr/local/miniforge
import os
os.environ["PATH"] = "/usr/local/miniforge/bin:" + os.environ["PATH"]

!conda install -c bioconda raxml-ng modeltest-ng -y
</pre>

Explanation:

biopython: Used for biological sequence handling (FASTA, BLAST, Entrez).  
mafft: Tool for Multiple Sequence Alignment (MSA).  
miniforge: Lightweight Conda installer for bioinformatics packages.  
raxml-ng: Builds maximum likelihood phylogenetic trees.  
modeltest-ng: Helps select the best evolutionary model (optional support tool).  

This step sets up a complete phylogenetic analysis environment.

</section>

<section>

<h2>3. Retrieval of Kinesin-14 Protein Sequence from NCBI</h2>

Code:

<pre>
from Bio import SeqIO, Entrez

Entrez.email = "your_email@example.com"

handle = Entrez.efetch(db="protein", rettype="fasta", id="1CZ7_A")
sequence = SeqIO.read(handle, "fasta")

with open("kinesin14_query.fasta", "w") as f:
    SeqIO.write(sequence, f, "fasta")

handle.close()

print("Sequence retrieved:", sequence.id)
print("Length:", len(sequence.seq))
</pre>

Explanation:

Connects to NCBI protein database  
Fetches structure/sequence ID: 1CZ7_A  
Saves sequence in FASTA format  

Output:

Protein ID  
Sequence length  

This becomes the query sequence for evolutionary analysis.

</section>

<section>

<h2>4. BLAST Search Against Protein Database</h2>

Code:

<pre>
from Bio.Blast import NCBIWWW

result_handle = NCBIWWW.qblast(
    "blastp",
    "pdb",
    sequence.seq
)

print("BLAST search completed.")
</pre>

Explanation:

Runs BLASTP (protein-protein BLAST)  
Searches against PDB database  
Finds structurally similar proteins  

Purpose: To identify homologous kinesin-like proteins for phylogenetic comparison.

</section>

<section>

<h2>5. Parsing BLAST Results</h2>

Code:

<pre>
from Bio.Blast import NCBIXML

blast_record = NCBIXML.read(result_handle)
result_handle.close()

print("Hits found:", len(blast_record.alignments))
</pre>

Explanation:

Reads BLAST output in XML format  
Extracts alignment results  
Counts number of matched sequences  

These hits represent evolutionarily related proteins.

</section>

<section>

<h2>6. Preparing Sequences for Multiple Sequence Alignment (MAFFT)</h2>

Code:

<pre>
from Bio.Seq import Seq
from Bio.SeqRecord import SeqRecord
from Bio import SeqIO

sequences_to_align_dict = {}

# Add query sequence
sequences_to_align_dict[sequence.id] = sequence

# Add BLAST hit sequences
for alignment in blast_record.alignments:
    if alignment.hsps:
        hit_id = alignment.hit_id
        hit_seq_record = SeqRecord(
            Seq(alignment.hsps[0].sbjct),
            id=hit_id,
            description=alignment.title
        )
        sequences_to_align_dict[hit_id] = hit_seq_record

sequences_to_align = list(sequences_to_align_dict.values())

with open("kinesin14_sequences.fasta", "w") as output_handle:
    SeqIO.write(sequences_to_align, output_handle, "fasta")

print(f"Created kinesin14_sequences.fasta with {len(sequences_to_align)} unique sequences.")
</pre>

Explanation:

This step:

Combines query + BLAST hits  
Removes duplicate sequences using a dictionary  
Extracts subject sequences (sbjct)  
Writes final dataset to FASTA file  

Output file: kinesin14_sequences.fasta

This file contains all sequences needed for alignment.

</section>

<section>

<h2>7. Multiple Sequence Alignment (MAFFT)</h2>

Code:

<pre>
!mafft kinesin14_sequences.fasta > aligned_kinesin14.fasta

print("Alignment completed.")
</pre>

Explanation:

Aligns sequences to identify:
conserved regions  
mutations  
evolutionary differences  

 Output: aligned_kinesin14.fasta

This is the foundation for phylogenetic tree construction.

</section>

<section>

<h2>8. Inspecting Alignment Quality</h2>

Code:

<pre>
from Bio import AlignIO

alignment = AlignIO.read("aligned_kinesin14.fasta", "fasta")

print("Alignment length:", alignment.get_alignment_length())
print("Number of sequences:", len(alignment))
</pre>

Explanation:

Reads aligned sequences  
Confirms:
number of sequences  
alignment length consistency  

 Ensures alignment is valid before tree building.

</section>

<section>

<h2>9. Sequence Length Distribution Analysis</h2>

Code:

<pre>
import matplotlib.pyplot as plt

lengths = [len(rec.seq) for rec in alignment]

plt.figure()
plt.hist(lengths)
plt.title("Sequence Length Distribution")
plt.xlabel("Length")
plt.ylabel("Count")
plt.show()
</pre>
<img width="562" height="455" alt="image" src="https://github.com/user-attachments/assets/2ce0afac-1ddb-450b-9a7e-50f6aee64280" />

Explanation:

Visualizes sequence length variation  
Helps detect:
truncated sequences  
outliers  
inconsistent data  

</section>

<section>

<h2>10. Phylogenetic Tree Construction (RAxML-NG)</h2>

Code:

<pre>
!raxml-ng \
--msa aligned_kinesin14.fasta \
--model LG+I+G4 \
--prefix K14_TREE \
--threads 2
</pre>

Explanation:

Builds a Maximum Likelihood phylogenetic tree  
Model used:  
LG: amino acid substitution model  
I: invariant sites  
G4: gamma distribution (rate variation)  

 Output: K14_TREE.raxml.bestTree

This represents evolutionary history of Kinesin-14 proteins.

</section>

<section>

<h2>11. Visualization of Phylogenetic Tree</h2>

Code:

<pre>
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read("K14_TREE.raxml.bestTree", "newick")

plt.figure(figsize=(10, 6))
Phylo.draw(tree)
plt.show()
</pre>

Explanation:

Loads Newick tree format  
Displays evolutionary relationships visually  
<img width="562" height="434" alt="image" src="https://github.com/user-attachments/assets/ad537c07-989d-42c3-8b09-0dc038a31b96" />

Interpretation:

Branch length = evolutionary distance  
Clusters = similar proteins  
Divergence points = evolutionary split events  

</section>

<section>

<h2>12. Bootstrap Analysis (Tree Reliability)</h2>

Code:

<pre>
!raxml-ng \
--bootstrap \
--msa aligned_kinesin14.fasta \
--model LG+I+G4 \
--bs-trees 100 \
--prefix K14_BOOT

print("Bootstrap analysis completed.")
</pre>

Explanation:

Performs bootstrap resampling (100 replicates)  
Measures confidence in tree branches  

Higher bootstrap values = more reliable evolutionary relationships

</section>

<section>

<h2>13. Biological Interpretation</h2>

This workflow reveals:

Evolutionary conservation of kinesin motor domains  
Divergence among kinesin family members  
Functional conservation in motor protein regions  
Structural similarity across species  

Kinesin-14 proteins show strong conservation in motor domains but variation in regulatory regions, reflecting functional adaptation.

</section>

<section>

<h2>14. Conclusion</h2>

This project demonstrates a complete bioinformatics pipeline for phylogenetic analysis:

Sequence retrieval (NCBI)  
Similarity search (BLAST)  
Multiple sequence alignment (MAFFT)  
Evolutionary tree building (RAxML-NG)  
Visualization (Matplotlib / Biopython)  

This workflow can be extended to:

Auspice/Nextstrain-style interactive visualization  
Functional annotation of kinesin variants  
Comparative evolutionary studies  

</section>

</body>
</html>
