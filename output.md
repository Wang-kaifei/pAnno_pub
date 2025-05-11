<!--
 * @Author: wangkaifei kfwang@stu.xidian.edu.cn
 * @Date: 2025-05-11 14:37:59
 * @LastEditors: wangkaifei kfwang@stu.xidian.edu.cn
 * @LastEditTime: 2025-05-11 19:56:16
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

### Please prioritize the following result files:
- novel.spectra
- [dna_anno.panno](#dna_anno.panno)
#### For complex species, the following result files are also recommended:
- [part1.panno](#part1.panno)
- [dna_anno-filtered.panno](#dna_anno.panno)
  
## 🛠️ Output Documentation
### dna_anno.panno
|Column               |   Content                                                         |
|--------------------|----------------------------------------------------------------------|
| `Is_rna`          | 0 or 1. 1 indicates RNA-originated only, 0 indicates inferred from genome.|
| `Is_mutation`          | 0 or 1. 1 indicates this CDS contains mutations |
| `Is_splice`      | 0 or 1. 1 indicates a splice site with peptide evidence                       |
| `Is_subset`       | 0 or 1. 1 indicates the novel peptides supporting this CDS are a true subset of those from another CDS. |
| `Chr`        | Chromosome name. For example, DNA.fna_line6152619_NC_000003.12 indicates chromosome NC_000003.12 located on line 6152619 of the DNA.fna file.|
| `ID`       | 本CDS对应到的定制数据库中的protein ID。                          |
| `Frame`     | 阅读框来源。因为pAnno支持不同frame拼接的剪接体，所以此处用'4'标表示剪接体 |
| `pep_q_value`        | 支持本CDS的novel peptides的最小q-value                                      |
| `Novel_pep`      | 支持本CDS的novel peptides，以';'分隔                                   |
| `Old_pep`         | 本CDS覆盖到的known peptides (derived from reference database)，以';'分隔 |
| `Start_codon`        | 推断的起始子。请注意，此项仅对于简单物种(几乎无剪接)有效，复杂物种（具有大量剪接）请参考part1.panno文件|
| `Start`       | 整数，CDS在本染色体上开始翻译的位置 （此项仅对于简单物种有效，复杂物种请参考part1.panno文件） |
| `End`    | 整数，CDS在本染色体上结束翻译的位置 （此项仅对于简单物种有效，复杂物种请参考part1.panno文件）|
| `Strand`  | 正负链标记                              |
| `Mutation_info` | 突变信息。1:5:S->T表示`Novel_pep`中的第一条肽段的第5个氨基酸从S突变为了T，以';'分隔|
| `Splice_info`         | 剪接信息。"4683787;5174351"表示4683787和5174351位置的碱基分别为donor和acceptor位点 |
| `Anno_pro`      | 对基因组注释的修正。"Start:5172647,End:5172740"表示对本DNA链的(5172647,5172740)段的编码区注释做了修正。以';'分隔                                      |
| `Pro_seq` | CDS对应的氨基酸序列                      |
| `Gene_seq` | CDS对应的碱基序列                                |
| `Gene_q_value`  | 暂未启用。                             |
| `Target/Decoy`  | 暂未启用。                             |

### part1.panno
本文件内容与dna_anno.panno基本一致，但具有二级标题。每个item代表一个CDS mapping grop，这些CDS被推断为组成一个完整的基因编码区。
item的第一行标注了该基因中跨越全部表达区域的部分（包括内部的非编码区），需要注意:
- `Is_rna` 一列的值为1，表示CDS mapping group的开始
- `Splice_info`一列有`n-1`对donor和acceptor位点，`n`表示组中的CDS数量。比如`(62535943,62535191);(62535000,62534074);`表示CDS mapping group中有3个CDS，分别在62535943bp-62535191bp和62535000bp-62534074bp处发生剪接。
- `Pro_seq`表示完整编码区的氨基酸序列
- `Gene_seq`表示完整编码区的碱基序列
- `Mutation`本列位于末尾。表示完整编码区中的突变信息，`102:Q->R`表示`Pro_seq`中的第102个氨基酸从Q突变为R。以';'分隔

item第一行之后的每行缩进一列，按二级标题输出，代表该一个CDS，是该基因的一个部分:
- `Start_codon`只针对CDS mapping group中处于N端的CDS做起始子推断，并将结果写入item第一行的`Start_codon`列中。其他CDS的`Start_codon`列均为空。
- `Start`和`End`列表示该CDS在本染色体上的翻译起始和结束位置