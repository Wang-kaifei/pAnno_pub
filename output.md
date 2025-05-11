<!--
 * @Author: wangkaifei kfwang@stu.xidian.edu.cn
 * @Date: 2025-05-11 14:37:59
 * @LastEditors: wangkaifei kfwang@stu.xidian.edu.cn
 * @LastEditTime: 2025-05-11 18:50:41
 * @FilePath: \public\output.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
## 🛠️ Output Tree


<pre> ``` output-root/
├── novel.spectra # Identified peptide-spectrum matches (PSMs) corresponding to novel peptides
├── annoed.spectra # Identified PSMs corresponding to known peptides (from reference database)
├── stage2_seqs.spectra # Database search results filtered by grouped FDR
├── target_GSSP.txt # Identified novel peptides (each line is an amino acid sequence)
├── dna_anno.panno # Complete CDS annotation results inferred from novel peptides
├── dna_anno-filtered.panno # High-confidence CDS filtering by _Multi-stage CDS mapping group-based filter_ (__complex species only__)
├── part1.panno # Part 1 results: outputs by CDS mapping group unit, representing the complete gene coding regions (__complex species only__)
├── part1.gff # CDS classified in Part 1, output in GFF format (__complex species only__)
├── part2.panno # Part 2 results: CDS inference results supported by genome, transcriptome, and proteome, but not passing the _Multi-stage CDS mapping group-based filter_ (__complex species only__)
├── part2.gff # CDS classified in Part 2, output in GFF format (__complex species only__)
├── part3.panno # Part 3 results: CDS consists of RNA-exclusive CDSs lacking DNA concordance (__complex species only__)
├── part4.panno # Part 4 results: CDS with novel peptides but lacking RNA evidence (__complex species only__)
├── part4.gff # CDS classified in Part 4, output in GFF format (__complex species only__)
├── pFind_re2/ # Directory storing results of __Step 1__ in database search stage
│ ├── pFind-Filtered.spectra # PSM identification results of __Step 1__
│ ├── MSdel/ # Directory storing spectra not interpreted in __Step 1__
│ │ └── XXX.mgf          
├── pFind_res2/ # Directory storing result files of __Step 2__ in database search stage
│ ├── DBReducer/ # DBReducer results for processing the customized database
│ │ │── Reduced.fasta # Result of reduced customized database
│ │ │── DNARed/ # Directory storing DBReducer results for DNA six-frame translated database
│ │ └── RNARed/ # Directory storing DBReducer results for RNA three-frame translated database
│ ├── pFind-Filtered.spectra # PSM identification results of Step 2 ``` </pre>

### Please prioritize the following result files:
- novel.spectra
- [dna_anno.panno](#dna_anno.panno)
#### For complex species, the following result files are also recommended:
- part1.panno
- dna_anno-filtered.panno



## 🛠️ Output Documentation
### dna_anno.panno

