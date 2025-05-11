<!--
 * @Author: wangkaifei kfwang@stu.xidian.edu.cn
 * @Date: 2025-05-11 14:37:59
 * @LastEditors: wangkaifei kfwang@stu.xidian.edu.cn
 * @LastEditTime: 2025-05-11 21:13:17
 * @FilePath: \public\output.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
## 🛠️ Output Tree


<pre>output-root/
├── novel.spectra # Identified peptide-spectrum matches (PSMs) corresponding to novel peptides
├── annoed.spectra # Identified PSMs corresponding to known peptides (from reference database)
├── stage2_seqs.spectra # Database search results filtered by grouped FDR
├── target_GSSP.txt # Identified novel peptides (each line is an amino acid sequence)
├── dna_anno.panno # Complete CDS annotation results inferred from novel peptides
├── dna_anno-filtered.panno # High-confidence CDS filtering by Multi-stage CDS mapping group-based filter (complex species only)
├── part1.panno # Part 1 results: outputs by CDS mapping group unit, representing the complete gene coding regions (complex species only)
├── part1.gff # CDS classified in Part 1, output in GFF format (complex species only)
├── part2.panno # Part 2 results: CDS inference results supported by genome, transcriptome, and proteome, but not passing the Multi-stage CDS mapping group-based filter (complex species only)
├── part2.gff # CDS classified in Part 2, output in GFF format (complex species only)
├── part3.panno # Part 3 results: CDS consists of RNA-exclusive CDSs lacking DNA concordance (complex species only)
├── part4.panno # Part 4 results: CDS with novel peptides but lacking RNA evidence (complex species only)
├── part4.gff # CDS classified in Part 4, output in GFF format (complex species only)
├── pFind_re2/ # Directory storing results of Step 1 in database search stage
│ ├── pFind-Filtered.spectra # PSM identification results of Step 1
│ ├── MSdel/ # Directory storing spectra not interpreted in Step 1
│ │ └── XXX.mgf          
├── pFind_res2/ # Directory storing result files of Step 2 in database search stage
│ ├── DBReducer/ # DBReducer results for processing the customized database
│ │ │── Reduced.fasta # Result of reduced customized database
│ │ │── DNARed/ # Directory storing DBReducer results for DNA six-frame translated database
│ │ └── RNARed/ # Directory storing DBReducer results for RNA three-frame translated database
│ ├── pFind-Filtered.spectra # PSM identification results of Step 2 </pre>

#### Please prioritize the following result files:
- novel.spectra
- [dna_anno.panno](#dna_anno.panno)
#### For complex species, the following result files are also recommended:
- [part1.panno](#part1.panno)
- [dna_anno-filtered.panno](#dna_anno.panno)
  
## 🛠️ Output Documentation
### dna_anno.panno
| Column           | Description                                                                                      |
|------------------|--------------------------------------------------------------------------------------------------|
| `Is_rna`         | 1 if the CDS is derived from RNA only; 0 if inferred from the genome.  |
| `Is_mutation`    | 1 if the CDS contains mutations.                                                                 |
| `Is_splice`      | 1 if the CDS is a spliced CDS with proteome evidence |
| `Is_subset`      | 1 if the novel peptides are a subset of those from another CDS.                                  |
| `Chr`            | Chromosome name, e.g., `DNA.fna_line6152619_NC_000003.12` indicates chromosome `NC_000003.12` located on line 6152619 of the DNA.fna file.                                    |
| `ID`             | The corresponding protein ID in the customized database of the CDS.  |
| `Frame`          | Translation frame. '4' indicates a splice junction.                                   |
| `pep_q_value`    | Lowest q-value among supporting novel peptides.                                                  |
| `Novel_pep`      | Novel peptides supporting the CDS, separated by `;`.                                             |
| `Old_pep`        | Known peptides (from reference database) covered by this CDS, separated by `;`.            |
| `Start_codon`    | Inferred start codon (only for simple species).                        |
| `Start` / `End`  | Translation start and end positions (only for simple species).           |
| `Strand`         | DNA strand (`+` or `-`).                            |
| `Mutation_info`  | Format: `1:5:S->T` — 1st peptide, 5th aa, S to T. Separated by `;`.   |
| `Splice_info`    | Donor and acceptor positions, e.g., `4683787;5174351` indicates donor and acceptor sites at positions 4683787 and 5174351, respectively.        |
| `Anno_pro`       | Annotation corrections, e.g., `Start:5172647,End:5172740` indicates a revised CDS annotation in the range (5172647, 5172740). Entries are separated by `;`.          |
| `Pro_seq`        | Amino acid sequence of the CDS.            |
| `Gene_seq`       | Nucleotide sequence of the CDS.          |
| `Gene_q_value`   | Not currently used.                  |
| `Target/Decoy`   | Not currently used. |

### part1.panno
This file shares a similar structure with `dna_anno.panno` but includes __secondary headers__. Each item represents a `CDS mapping group`, where the CDSs are inferred to translate into the same protein, thereby facilitating the reconstruction of comprehensive gene coding regions.
The __first line__ of each item represents the continuous span that covers the entire coding region of the gene. __Please note:__
- `Is_rna` column is 1, indicating the start of a `CDS mapping group`.
- `Splice_info` column contains `n-1` pairs of donor and acceptor sites, where `n` is the number of CDSs in the group. For example, `(62535943,62535191);(62535000,62534074)` indicates that the `CDS mapping group` consists of three CDSs, with splicing occurring at 62535943^62535191 bp and 62535000^62534074 bp.
- `Pro_seq` represents the amino acid sequence of the complete coding region.
- `Gene_seq` represents the nucleotide sequence of the complete coding region.
- `Mutation` column is located at the end of the entry and describes mutations within the full coding region. For example, `102:Q->R` means the 102nd amino acid in the Pro_seq changed from Q to R. Multiple entries are separated by `;`.

Each line following the first in an item is indented by one column and serves as a secondary entry, representing an individual CDS that forms part of the `CDS mapping group`:
- `Start_codon` is inferred only for the N-terminal CDS within the `CDS mapping group`. The result is recorded in the `Start_codon` column of the item’s first line, while all other CDS entries in the group have this column left blank.
- The `Start` and `End` columns indicate the translation start and end positions of the CDS on the corresponding chromosome.


**Technical Support:** 
At this stage, __pAnno__ has only been tested by a limited number of users, so this document may lack certain important details. If you encounter any issues during use or have any suggestions, please feel free to contact us. We will respond as promptly as possible and make the necessary improvements.
📧 **Contact info**: Kaifei Wang.  Email: [wangkaifei20@mails.ucas.edu.cn](mailto:wangkaifei20@mails.ucas.edu.cn), WeChat: ```wwwangnapao99```