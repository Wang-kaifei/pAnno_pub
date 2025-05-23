<!-- # pUniFind
This is the official repository for pUniFind, the most powerful zero-shot open peptide-spectrum scoring model and the first zero-shot open de novo sequencing deep learning model supporting over 1300 modifications.

Scoring evaluation can be seen below:

Zero-shot de novo sequencing evaluation can be seen below:

Performace of our model on hard cases can be seen below:


For now you may try our model on bohrium app here. Our arxiv preprint(including more details and more very careful evaluation on accuracy) will be released soon. Our source code will be released upon the acceptance of our paper.

## TODO
1. Update bohrium app to support TIMS and Astral data. (very soon)
2. Update bohrium app to support multi-gpu process. (very soon)
3. Specially finetune on some new modification (if needed)

If you found some bugs or if you need some more function, please donot hesitate to pull an issue here.

Staring our repository will remind you our updates. -->



# Decoding Dark Genomes: Universal, Precise and Fast Discovery of Novel Coding Regions with pAnno

<!-- [![arXiv](https://img.shields.io/badge/arXiv-2308.12345-B31B1B)](https://arxiv.org/abs/1234.56789)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0) -->
<!-- [![Bohrium](https://img.shields.io/badge/Try%20on-Bohrium%20App-00BFFF)](https://bohrium.example.com/pAnno) -->
<!-- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-repo/pUniFind) -->

This is the official repository for **pAnno**, a groundbreaking end-to-end workflow for identifying novel coding events, including full gene coding regions, splicing, mutations, and non-canonical HLA-bound peptides. Developed by [pFind group](https://pfind.net/).
<img src="./img/head.png" alt="head" width="80%">
## 🚀 Key Designs
🔥 **Multi-tiered iterative open search strategy** enables dynamic database refinement and hierarchical spectral analysis.

🔥 **Efficient peptide-to-coding sequence (CDS) mapping algorithm** implemented with tag-based indexing and supporting the detection of splicing and mutation events.

🔥 **Two species-specific CDS filters** for scoring novel CDS candidates and reconstructing the complete gene coding regions.

## 🚀 Representative Applications
🔥 **Exceptional tolerance to search space expansion:** Maintains near lossless sensitivity (98.5%) with only 2.7% suspicion across a 50-fold expanded search space in _H. sapiens_ data.

🔥 **Efficient inference of novel genomic coding region** Accurately localizes novel events with minimal overhead, accounting for only ~3% of total runtime beyond the database search phase.

🔥 **Annotation beyond the reference genome:** Identifies 2,148 novel genes in _Pyrus_ supported by [BLASTp](https://blast.ncbi.nlm.nih.gov/Blast.cgi) validation (e-value < 1e−5, sequence coverage ≥ 70%).

🔥 **Expanded immunopeptidomic landscape:** Discovers over 50 times more non-canonical HLA-bound peptides than original reports in lung cancer samples, followed by affinity and structural validation.



## &#x1F4E3; News
- **2025/5/8** pAnno repository initial release 🚀.
- **2025/5/12** pAnno officially releases version v1.0.1 🚀.
- **2025/5/12** pAnno officially releases version v1.0.2 🚀.

## 📊 Benchmark Performance
Details can be seen in our paper.

<!-- ### 1. Scoring -->

<!-- ![](2.drawio.png) -->
<!-- ![](Vigna_trap_e2e.png) -->

<!-- ### 2. De Novo Sequencing -->
<!-- ![](5.drawio.png) -->

<!-- ### 3. Challenging Cases -->
<!-- ![](6.drawio.png) -->



## 🛠️ Quick Start

Currently pAnno only supports running on Windows platform. We are working on a Linux version. Additionally, each module of pAnno can operate independently, allowing users to input peptide identification results directly and seamlessly proceed with the downstream peptide-to-CDS inference module.

### 1. main software
The released main software package is available on [v1.0.2](https://github.com/Wang-kaifei/pAnno_pub/releases)
```bash
cd pAnnobin
pAnno.exe pAnno.cfg # It is recommended to replace it with an absolute path
```

### 2. Output Explanation
The output files and format explanations for each stage can be found [here](./output.md).

### 3. CDS filter for simple species: deep learning-based scoring model
This is an independent deep learning-based module specifically designed for simple organisms with minimal splicing, aiming to score and filter candidate CDS regions. Initial results should be generated using the main software, after which our provided script can be employed for online training.
<!-- 注意这里还要写每个脚本怎么用 -->
```bash
# Requirements: Python 3.8+, 2.3.1+cpu
git clone https://github.com/Wang-kaifei/pAnno_pub.git
cd pAnno_pub
pip install -r requirements.txt
```
## 🛠️ TestData
The test data is available on [Google Drive](https://drive.google.com/drive/folders/1g6rwQ2j7eK1r0_brV71hwO9d9FocT1Lc?usp=drive_link). The test data comprise three sets, representing a simple specie (yeast), a complex specie (Pyrus), and non-canonical HLA-binding peptides (noncHLAp), corresponding to the dedicated pAnno module for noncHLAp discovery. Specifically:

<pre>yeast
├── dataset/ 
│ ├── msms/ # Test mass spectrometry dataset (only one file is provided as a representative example)
│ ├── target_group_pace2.fasta # Reference protein database
│ ├── RNA.fna # RNA sequence file
│ ├── DNA.fna # DNA sequence file
│ ├── GCF_000146045.2_R64_genomic.gff # Reference genome annotation file
├── pAnno.cfg # Configuration file </pre>

<pre>Pyrus/
├── dataset/ 
│ ├── msms/ # Test mass spectrometry dataset (only one file is provided as a representative example)
│ ├── Pyrus_bretschneideri_Chr_gene.pep_V121010 # Reference protein database
│ ├── Adult_leaf_transcripts.fasta # RNA sequence file (Our study used transcripts from all organs; here, only the leaf is provided as an example)
│ ├── GCF_000315295.1_Pbr_v1.0_genomic.fna # DNA sequence file
│ ├── Pyrus_bretschneideri_Chr_gene.gff_V121010 # Reference genome annotation file
├── pAnno.cfg # Configuration file </pre>

<pre>HLA/
├── dataset/ 
│ ├── msms/ # Test mass spectrometry dataset (only one file is provided as a representative example)
│ └── ExterDB/ # Store the prediction database based transcriptome information from the original study
│ ├── C3N02289_prot.fasta.fasta # reference protein database
│ ├── readme.txt # Describe the genome transcriptome and annotation file sources
├── pAnno.cfg # Configuration file </pre>


## 🛠️ Params
The configuration file is in the format of a text file with the extension __.cfg__. The parameters are divided into three sections: __[General]__, __[DBGeneration]__, __[DBSearch]__, __[FDRControl]__, __[CDSInference]__ and __[MSData]__. The parameters in each section are as follows:
### [General]
- __tTagLen__: Length of the tag used for indexing. The default value is 6, which is recommended for most cases. Please note that it should not be longer than the minimum peptide length.
- __tNameFlag__: A flag used to label reference protein names, with the default set to "$$". This flag is added to proteins from the reference database to distinguish them from those derived from the customized database. You may choose custom characters, but they must not appear in the genome or transcriptome data.
- __DNAFolder__: Path to the file or folder containing DNA sequences.
- __RNAFolder__: Path to the file or folder containing RNA sequences. Please note that pAnno requires assembled RNA sequences, which can be obtained from RNA reads through either reference-guided or de novo transcriptome assembly. We recommend using __de novo__ assembly by [Trinity](https://github.com/trinityrnaseq/trinityrnaseq/wiki) to maximize the recovery of candidate sequences. Rest assured, pAnno's robust algorithms automatically filter out low-confidence results.
- __ExterDBPath__: The path (or directory) to an additional external protein sequence database provided by user, such as a predicted mutation database; leave empty if none.
- __ProteinDatabase__: Reference protein database, with sequence headers starting with “>”.
- __RawGFF__: Genome annotation file in General Feature Format (GFF). pAnno uses this file to generate correction information for annotated genes.
- __OutputPath__: Path to the output directory of all results.
### [DBGeneration]
- __TransPath__: Output directory for the customized database. This should be an empty folder that will store both the translated and deduplicated databases.
- __minprodna__: Minimum length (in amino acids) for sequences of six-frame translation of DNA. For species with extensive splicing, a threshold of 10 is recommended; for species with minimal splicing, 50 is suggested. We strongly encourage users to adjust this parameter based on their specific research goals—for example, a smaller value is advisable for short open reading frames (sORF) identification.
- __minprorna__: Minimum length (in amino acids) for sequences of three-frame translation of RNA, default is 50.
- __transplnum__: Number of threads used for substring-level deduplication in the customized database. It is recommended to set this to 1.5 times the number of CPU cores.
### [DBSearch]
- __maxprolen__: The maximum total length of protein sequences (in amino acids) to be loaded into memory at once. The default is 600,000,000 amino acids, which allows parallel processing of 80k MS2 on a machine with 128 GB of RAM. You can adjust this value based on your system’s memory capacity.
- __open_1__: Search mode selection for __step 1__ in database search stage. Set to 1 for open search and 0 for restricted search.
- __open_2__: Search mode selection for __step 2__ in database search stage. Set to 1 for open search and 0 for restricted search.
- __multip_1__: Number of processes used in __step 1__ of database search stage. This should be adjusted based on available memory, genome size, and dataset scale. A preliminary test run is recommended to determine the optimal setting.
- __multip_2__: Number of processes used in __step 2__ and DBReducer of database search stage. Because this step involves processing of the customized database, the configuration is particularly important. Proper settings can significantly improve search speed. It is recommended to run an initial test to determine the optimal parameters.
- __activation_type__: Instrument type for your mass spectrometry datasets. Optional: HCD-FTMS, HCD-ITMS, CID-FTMS, CID-ITMS
- __enzyme__: Enzyme type. Default is "Trypsin KR_C", indicating cleavage by trypsin. Users may select other enzyme types as needed (see documentation at x).
- __digest__: Specificity of enzyme. 0 for Non-Specific; 1 for Semi-Specific; 3 for Fully-Specific
- __max_clv_sites__: Maximum number of missed cleavage sites. Default is 3.
- __selectmod__: Variable modifications. Choose appropriate modifications from documentation x, separated by semicolons.
- __fixmod__: Fixed modifications. Use the same format as for selectmod.
- __pep_length__: Minimum and maximum peptide lengths. Default is "6 100", indicating a minimum length of 6 and a maximum of 100. Adjust as needed, especially for immunopeptidome identification.

### [QualityControl]
- __mstol__: Mass tolerance threshold for MS1 (precursor) spectra, specified in ppm or Da.
- __msmstol__: Mass tolerance threshold for MS2 (fragment) spectra, specified in ppm or Da.
- __mstolppm__: Unit for MS1 mass tolerance; set to 1 for ppm or 0 for Da.
- __msmstolppm__: Unit for MS2 mass tolerance; set to 1 for ppm or 0 for Da.
- __psm_fdr_type__: FDR filtering mode for the database search stage. Set to 1 for peptide-level filtering, or 0 for spectrum-level filtering.
- __psm_fdr1__: FDR threshold for __step 1__ in database search stage. The default value is 0.01.
- __psm_fdr2__: FDR threshold for __step 2__ in database search stage. The default value is 0.01.
- __dbr_threshold__: FDR threshold for the __DBReducer__ in database search stage. The default value is 0.1.

### [CDSInference]
- __anno_mode__: CDS inference mode. Default is 1, reporting results in CDS units and inferring complete gene coding regions based on _CDS mapping groups_. Set to 5 to report all possible corresponding positions of peptides, suitable for peptidomics studies.
- __specie_mode__: Species type for your study. Set to 1 for complex species with extensive splicing, and 0 for simple species with minimal splicing.
- __chr_map_pl__: Number of chromosomes processed in parallel during peptide-to-genome mapping. This parameter is effective for complex species such as _H. sapiens_, whose longest DNA strands can exceed 10,000 bases. It is recommended to set this to approximately 1.5 times the number of CPU cores.
- __pep_map_pl__: Number of peptides processed in parallel per chromosome during mapping. Also recommended to be around 1.5 times the number of CPU cores.
### [MSData]
- __msmsnum__: Totle number of MS2 files.
- __msmsfolder__: Totle number of folders containing MS2 files.
- __msmspath1__: Path to the first folder containing MS2 files. If there are multiple folders, you will also need to specify ```msmspath2```, ```msmspath3```, and so on. (Note: We strongly recommend placing all MS2 files in one folder)
- __msmstype__: Must be set to MGF. Therefore, raw data needs to be converted to this format using tools such as [pParse](https://pfind.net/software/pParse/index.html) or [msconvert](https://fragpipe.nesvilab.org/docs/tutorial_convert.html).

__Note__: Among the parameters listed above, ```specie_mode```, the settings under __[MSData]__, and path-related parameters such as ```DNAFolder``` must be customized. For all other parameters, if you are unsure, it is recommended to use the default values.




## 📅 Note



## 🛠️ Technical Support  
Should you encounter any technical issues, suggestions, observe suboptimal performance, or identify inconsistencies between __pAnno__ results and our evaluation metrics, we welcome your feedback 🙏. 
- If you have any suggestions about our software, please do not hesitate to contact us.
- If you think the software can be __extended__ to support __new application scenarios__, please feel free to contact us and we will actively develop new features.

We provide **priority support** for user-reported issues through the following channels:  

**For technical inquiries:**  
1. **GitHub Issues**: [Open a new issue](https://github.com/Wang-kaifei/pAnno_pub/issues)

2. **pFind Studio user support**: 
   - Support Email: [support@pfind.org](mailto:support@pfind.org)

3. **Contact the author of pAnno directly**: 
   - Kaifei Wang.  Email: [wangkaifei20@mails.ucas.edu.cn](mailto:wangkaifei20@mails.ucas.edu.cn), WeChat: ```wwwangnapao99```
   <!-- - **Forum**: [issues](https://pfind.net/forum/) -->
At this stage, __pAnno__ has only been tested by a limited number of users, so this document may lack certain important details. If you encounter any issues during use or have any suggestions, please feel free to contact us. We will respond as promptly as possible and make the necessary improvements.

**For collaboration requests:**  
📧 **Contact info**: Kaifei Wang.  Email: [wangkaifei20@mails.ucas.edu.cn](mailto:wangkaifei20@mails.ucas.edu.cn), WeChat: ```wwwangnapao99```

## 📅 Roadmap
**Staring** and **watching** our repo will remind you our updates. We will keep optimizing our software.
| Milestone         | Status |
|-------------------|--------|
| Post arxiv preprint | 📝 Planning |
| TIMS / Astral support | 🚄 very soon |
| DIA support | 🚧 Preparing |
| Developing powerful scoring algorithm for non-specific cleavage | 📝 Planning |

## 🤝 Citation <a name="-citation"></a>
If you find our software is useful and helped your research,  **please cite** us 🙏 through:
<!-- ```bash
Wating for bioxiv
``` -->