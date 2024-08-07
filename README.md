# Australian Biosecurity Genomic Database

<img src="https://raw.githubusercontent.com/ausbiopathgenDB/AustralianBiosecurityGenomicDatabase/13230e147009b5e2fc1487cb2692aa886dbb5c3a/files/ABGD.png">

## Introduction 
The Australian Biosecurity Genomic Database is a curated collection of viral reference genomes based on the [National Notifiable Disease List of Terrestrial Animals] and the [National List of Reportable Diseases of Aquatic Animals]. The database is provided as a FASTA file that can be used to screen high-throughput sequencing (HTS) data generated from animals, animal products, or environmental samples.  

The latest version of the database contains 92 viruses, representing 33 viral families. Sequences were acquired from the National Center for Biotechnology Information (NCBI) Nucleotide database, using high-quality whole genome reference sequences from RefSeq where possible. Each virus species was represented by a single isolate and segmented genomes were merged so that each virus was represented by a single sequence to assist data analysis. 

  [National List of Reportable Diseases of Aquatic Animals]: <https://www.agriculture.gov.au/agriculture-land/animal/aquatic/reporting/reportable-diseases>
  [National Notifiable Disease List of Terrestrial Animals]: <https://www.agriculture.gov.au/biosecurity-trade/pests-diseases-weeds/animal/notifiable#national-list-of-notifiable-diseases-of-terrestrial-animals-at-april-2024>
  [Krona chart]: <https://htmlview.glitch.me/?https://github.com/jbatovska/AustralianBiosecurityGenomicDatabase/blob/main/files/ABGD_taxonomy.krona.html>

# Usage guidelines

## 1. Initial screening
Initial screening of HTS data for notifiable animal viruses can be performed by mapping reads to the Australian Biosecurity Genomic Database. Coverage of virus genomes in the database can then be measured by calculating the percentage of each genome sequence that is covered by reads. Further analysis can be performed to measure other coverage metrics such as the average number of reads that align to each genome sequence (fold coverage) and the similarity of the reads to the reference genome sequence (nucleotide identity). 

Programs that can be used to perform read mapping include: 
-	Short reads: BWA, BowTie2
-	Long reads: MiniMap
	
The alignment file (.bam) that is produced by these programs can then be analysed using programs such as SAMtools and BBMap to generate coverage metrics. It can also be used by sequence analysis programs such as Geneious or Tablet to visualise the alignment of reads to the virus genome. 

## 2. Interpreting results

A triage system to rapidly assess confidence level (low, medium, and high) in the initial screening results has been developed utilising genome coverage metrics generated by read mapping to the Australian Biosecurity Genomic Database. The following are proposed coverage thresholds for the different confidence levels: 

### High confidence
-	Criteria: Virus genome sequence in the Australian Biosecurity Genomic Database has >10% coverage by reads with >95% pairwise nucleotide identity. 
-	This level of virus genome coverage indicates that the notifiable virus or a closely related virus are likely to be present in the sample. 
-	Further analyses of the HTS data should be performed to determine what virus is present in the sample, including an investigation into whether there is read coverage of a region used for species/strain demarcation (see below). 
-	If further analysis indicates that the virus is notifiable, then the relevant State Veterinary Laboratory or the Office of the Chief Veterinary Officer should be contacted (see below). 

### Medium confidence
-	Criteria: Virus genome sequence in the Australian Biosecurity Genomic Database has >10% coverage by reads with <95% pairwise nucleotide identity or <10% coverage by reads with >95% pairwise nucleotide identity.
-	This level of virus genome coverage indicates that the notifiable virus or a closely related virus are possibly present in the sample, but could also be due to other factors, such as contamination. 
-	Further analyses of the HTS data should be performed to determine what virus is present in the sample, including an investigation into whether there is read coverage of a region used for species/strain demarcation (see below). 
-	If further analysis indicates that the virus is notifiable, then the relevant State Veterinary Laboratory or the Office of the Chief Veterinary Officer should be contacted (see below). 

### Low confidence
-	Criteria: Virus genome sequence in the Australian Biosecurity Genomic Database has <10% coverage by reads with <95% pairwise nucleotide identity.
-	This level of virus genome coverage indicates that positive detection of a notifiable virus is still possible but not well-supported by the initial screening data. 
-	Despite low levels of non-specific genome coverage often being caused by conserved regions in closely related viruses or artefacts of host genomes, it is possible the low coverage is originating from a notifiable virus. Further analyses of the HTS data can help ascertain if a notifiable virus is present at a low titre or from a divergent strain (see below). 

Regions used for virus species/strain demarcation can be found in the relevant taxonomy reports provided by the [ICTV]. Links to the reports are provided in the [Notifiable Virus Compendium], along with other related information on the notifiable virus. The suggested thresholds above are based on initial testing with existing HTS datasets containing notifiable viruses. However, there may be cases that fall outside these thresholds and further testing of the Australian Biosecurity Genomic Database is required to develop robust detection criteria. Any suspected detections should be further investigated with additional analyses of the HTS data, with suggestions provided below. 

[ICTV]: <https://ictv.global/>
[Notifiable Virus Compendium]: <https://github.com/ausbiopathgenDB/AustralianBiosecurityGenomicDatabase/wiki>

## 3. Further analysis

### Visualisation of read mapping results
The alignment files (.bam) generated by read mapping can be visualised to inspect the read coverage of the virus genome and assess its validity. Often non-specific coverage produced by artefacts from host genomes is recognisable by deep fold coverage of a small region and minimal coverage of the rest of the virus genome. 

Large DNA viruses often have parts of the host genome incorporated in their own and may require higher thresholds for detection using genome read coverage to avoid false positives. 

Programs that can be used to visualise read alignment files include: 
- Tablet (free)
- Geneious (licensed)

### Sequence identity of mapped reads
While genome coverage by reads is a good indicator of virus presence, it can be misleading when generated by closely related viruses, leading to false positives (Figure 1).

<img src="https://raw.githubusercontent.com/ausbiopathgenDB/AustralianBiosecurityGenomicDatabase/main/files/Guide_Fig1_coverage.png">

<sub> **Figure 1:** Coverage of the measles virus genome produced by a (A) false positive detection; (B) low-level true positive detection; and (C) high-level true positive detection. The false positive coverage plot was generated using reads from dolphin morbillivirus, which has partial sequence homology to measles virus. (Schlaberg et al. 2017) </sub>

It can be helpful to determine how similar the reads are to the reference virus genome by measuring the pairwise identity, which can be generated by the same programs used to view alignment files. Pairwise identity is often included in species demarcation criteria and can be for a specific region or the whole genome. 

### Generating consensus sequences
The alignment files (.bam) produced by read mapping can also be used to generate consensus sequences, which can then be compared to a larger public database using online tools such as BLASTn (NCBI). A consensus sequence represents the most common nucleotide at each base of the virus genome that has read coverage and is usually longer than a single sequence read, allowing for greater sequence comparison to known sequences. Furthermore, tools such as BLASTx can translate the consensus sequence nucleotides into amino acids, which can be compared to protein databases to increase the breadth of the search results. 

Comparison to larger databases can help detect notifiable viruses that may be divergent from the Australian Biosecurity Genomic Database reference, while also ruling out false positives that may be producing misleading genome coverage. 

Consensus sequences can often be generated by the same programs used to view alignment files or by using alignment analysis programs such as SAMtools or BCFtools. 

### _De novo_ assembly
Instead of mapping HTS reads to a database, a de novo assembly-based approach can be used, where the reads are assembled into larger contiguous sequences (contigs) and compared to larger databases, such as NCBI’s non-redundant (nr) database. The assembly approach enables unbiased detection of both known and new viruses but is a more time and resource-intensive process. 
It is useful to confirm any notifiable viruses detected via read mapping with contig assembly as it provides a separate method for detection and can characterize sequence diversity that may be masked by the alignment approach. However, as contig assembly requires overlapping reads, it may not be as sensitive as read mapping if there is low or scattered virus genome coverage. 

## Contacting the Chief Veterinary Office (CVO)
All notifiable virus detections must be reported to the relevant State Veterinary Laboratory or the Office of the Chief Veterinary Officer, as they can organise further investigation of the sample with the appropriate testing laboratory. Multiple different data analyses should be provided to support the suspected detection of a notifiable virus detection and physical samples may be requested if available for confirmatory testing using diagnostic methods such as serology, culturing, or PCR. 

If there are any suspected detections, you can also call the Emergency Animal Disease Watch Hotline on 1800 675 888 (24 hours a day, every day of the year). If you are unsure of your results or would like to call a local veterinary officer, alternative numbers can be found for every state:

[Australian Capital Territory]

[New South Wales]

[Northern Territory]

[Queensland]

[South Australia]

[Tasmania]

[Victoria]

[Western Australia]

The [emergency animal diseases field guide for veterinarians] is also a valuable resource for the investigation and reporting of animal diseases in Australia. 

[Australian Capital Territory]: <https://www.environment.act.gov.au/parks-conservation/plants-and-animals/rural-services/livestock-advice>
[New South Wales]: <https://www.dpi.nsw.gov.au/biosecurity/report-a-pest-or-disease>
[Northern Territory]: <https://nt.gov.au/industry/agriculture/livestock/animal-health-and-diseases/notifiable-diseases-in-animals-and-how-to-report-them>
[Queensland]: <https://www.daf.qld.gov.au/contact/report-a-biosecurity-pest-or-disease>
[South Australia]: <https://pir.sa.gov.au/biosecurity/animal_health/reporting_animal_disease>
[Tasmania]: <https://nre.tas.gov.au/biosecurity-tasmania/animal-biosecurity/animal-health/reporting-an-emergency-animal-disease>
[Victoria]: <https://agriculture.vic.gov.au/biosecurity/animal-diseases/notifiable-diseases>
[Western Australia]: <https://www.agric.wa.gov.au/livestock-biosecurity/reportable-animal-diseases>
[emergency animal diseases field guide for veterinarians]: <https://www.outbreak.gov.au/prepare-respond/identify-pests-diseases/emergency-animal-diseases-field-guide>

## Troubleshooting
Further testing of the database using a greater number of HTS datasets from a variety of sample types and host animals is recommended to further inform the metrics and thresholds chosen for the screening analysis criteria, which will help to address some of the limitations associated with this approach (Table 1). The addition of biological characteristics such as host and symptoms could improve the applicability of the criteria when dealing with low or medium confidence results.

<sub> **Table 1:** Recommendations to address the possible issues associated with the use of read mapping to a database to screen HTS data for viral sequences. </sub>

<img src="https://raw.githubusercontent.com/ausbiopathgenDB/AustralianBiosecurityGenomicDatabase/main/files/Guide_Table1_troubleshooting.PNG">

## References
Schlaberg, Robert et al. 2017. ‘Validation of Metagenomic Next-Generation Sequencing Tests for Universal Pathogen Detection’. Archives of Pathology & Laboratory Medicine 141(6): 776–86. 
