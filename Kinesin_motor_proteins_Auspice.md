<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Exploring Interactive Phylogenies with Auspice</title>

<style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        background: #f5f7fb;
        color: #222;
        line-height: 1.6;
    }

    header {
        background: #1f4e79;
        color: white;
        padding: 25px;
        text-align: center;
    }

    section {
        background: white;
        margin: 20px auto;
        padding: 20px;
        width: 85%;
        border-radius: 10px;
        box-shadow: 0px 2px 10px rgba(0,0,0,0.1);
    }

    h1, h2, h3 {
        color: #1f4e79;
    }

    code {
        background: #eee;
        padding: 2px 5px;
        border-radius: 5px;
        font-size: 14px;
    }

    pre {
        background: #272822;
        color: #f8f8f2;
        padding: 15px;
        border-radius: 8px;
        overflow-x: auto;
    }

    .highlight {
        background: #e8f0fe;
        padding: 10px;
        border-left: 5px solid #1f4e79;
        margin: 10px 0;
    }

    footer {
        text-align: center;
        padding: 15px;
        margin-top: 30px;
        background: #1f4e79;
        color: white;
    }
</style>

</head>

<body>

<header>
    <h1>Exploring Interactive Phylogenies with Auspice</h1>
    <p>Case Study: Kinesin Motor Proteins (Kinesin-14 Focus)</p>
</header>

<section>
    <h2>1. Introduction</h2>
    <p>
        Phylogenetic analysis is a fundamental bioinformatics approach used to study evolutionary relationships.
        This project demonstrates an interactive phylogenetic workflow inspired by Nextstrain’s Auspice platform.
    </p>

    <div class="highlight">
        Focus protein: <b>Kinesin-14 family</b> (minus-end directed motor proteins)
    </div>

    <p>
        These proteins are involved in spindle assembly, chromosome segregation, and intracellular transport.
    </p>
</section>

<section>
    <h2>2. Objectives</h2>
    <ul>
        <li>Sequence retrieval from databases</li>
        <li>BLAST similarity search</li>
        <li>Multiple sequence alignment</li>
        <li>Phylogenetic tree construction</li>
        <li>Bootstrap validation</li>
        <li>Evolutionary interpretation</li>
    </ul>
</section>

<section>
    <h2>3. Tools Used</h2>
    <ul>
        <li>Biopython</li>
        <li>NCBI BLAST</li>
        <li>MAFFT</li>
        <li>RAxML-NG</li>
        <li>ModelTest-NG</li>
        <li>Matplotlib</li>
    </ul>
</section>

<section>
    <h2>4. Sequence Retrieval</h2>
    <pre>
from Bio import SeqIO, Entrez

Entrez.email = "your_email@example.com"
handle = Entrez.efetch(db="protein", rettype="fasta", id="1CZ7_A")
sequence = SeqIO.read(handle, "fasta")
    </pre>
</section>

<section>
    <h2>5. BLAST Analysis</h2>
    <pre>
from Bio.Blast import NCBIWWW

result_handle = NCBIWWW.qblast(
    "blastp",
    "pdb",
    sequence.seq
)
    </pre>
    <p>BLAST identifies homologous sequences based on similarity scores and E-values.</p>
</section>

<section>
    <h2>6. Multiple Sequence Alignment (MAFFT)</h2>
    <pre>
!mafft kinesin14_sequences.fasta > aligned_kinesin14.fasta
    </pre>

    <p>
        Conserved motor domains and ATP-binding motifs were observed across species.
    </p>
</section>

<section>
    <h2>7. Phylogenetic Tree Construction</h2>
    <pre>
!raxml-ng \
--msa aligned_kinesin14.fasta \
--model LG+I+G4 \
--prefix K14_TREE
    </pre>

    <p>
        The resulting tree shows distinct evolutionary clades of kinesin-14 proteins.
    </p>
</section>

<section>
    <h2>8. Bootstrap Analysis</h2>
    <pre>
!raxml-ng \
--bootstrap \
--msa aligned_kinesin14.fasta \
--model LG+I+G4 \
--bs-trees 100
    </pre>

    <p>
        Bootstrap values above 70 indicate strong phylogenetic support.
    </p>
</section>

<section>
    <h2>9. Results Summary</h2>
    <ul>
        <li>Strong conservation of motor domain</li>
        <li>Distinct evolutionary clustering</li>
        <li>Functional divergence in tail regions</li>
        <li>High bootstrap confidence</li>
    </ul>
</section>

<section>
    <h2>10. Conclusion</h2>
    <p>
        This study demonstrates a full phylogenetic pipeline using Biopython, MAFFT, ModelTest-NG,
        and RAxML-NG. The kinesin-14 family shows strong evolutionary conservation and functional divergence,
        highlighting its biological importance in cell division.
    </p>
</section>

<footer>
    <p>© 2026 Bioinformatics Project | Auspice-inspired Phylogenetic Analysis</p>
</footer>

</body>
</html>
