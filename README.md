## MANUAL ALIGNMENTS

### Reconstructing
To recover our manual alignments you will need to have access to AMR Release 1.0.
Unzip the release files and cd to corpus/release1/unsplit. From there run:

`cat amr-release-1.0-bolt.txt amr-release-1.0-consensus.txt  amr-release-1.0-dfa.txt  amr-release-1.0-mt09sdl.txt amr-release-1.0-xinhua.txt amr-release-1.0-proxy.txt > ~/YOUR_PATH/amr_ud/data/amr-release-1.0-all.txt`

to concatenate all release files into one.


Then cd to amr_ud and run:

```python2 dp1.py
patch ./alignments/aligned_amrs_reconstructed.txt -i ./alignments/amrs.patch -o ./alignments/aligned_amrs.txt
python2 dp2.py
patch ./alignments/ud_parses_ldc_reconstructed.txt -i ./alignments/ud_parses.patch -o ./alignments/ud_parses_patched.txt
tr '\n%@' '\t\n\n' <./alignments/ud_parses_patched.txt >./alignments/ud_parses.txt
patch ./alignments/amr_ud_alignments_ldc_reconstructed.txt -i ./alignments/alignments.patch -o ./alignments/amr_ud_alignments.txt
rm ./alignments/aligned_amrs_reconstructed.txt ./alignments/ud_parses_patched.txt ./alignments/ud_parses_ldc_reconstructed.txt ./alignments/amr_ud_alignments_ldc_reconstructed.txt
```

The following files should be created in the amr_ud/alignments directory:
* _aligned_amrs.txt_: contains all AMRs for which hand alignments were created
* _ud_parses.txt_: contains hand-corrected UD parses for those AMRs
* _amr_ud_alignments_: contains manual alignments

### Format
The format of the alignments is as follows:

AMR subgraph&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;&nbsp;&nbsp;&nbsp;UD subgraph

A subgraph might be a single node, or it might contain nodes and edges:


notation | meaning
--- | ----
x/word1 | node x, whose label is _word1_
x/word1 :rel y/word2 | a subgraph consisting of x, its child y, and the edge connecting them labeled rel
x/word1 ( :rel1 y/word2 ) \| :rel2 z/word3 | a subgraph consisting of x, two of its children, and the connecting edges

( ) groups subgraphs, | separates children of one parent node

### Non-LDC alignments
The file _'amr_ud_alignment_nonldc.txt'_ contains alignments and UD parses for AMRs which were not included in the LDC AMR release, and which can be freely shared.
All those AMRs, parses, and alignments are also included in the full alignment corpus reconstructed according to the instructions above.
