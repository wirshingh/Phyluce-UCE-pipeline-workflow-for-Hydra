Phyluce UCE Pipeline Workflow
Prepared by H. Wirshing
Initial draft 4-12-2024
 
This workflow is modified from the LAB Phyluce Tuorial found on GitHub.
 
The primary difference is that this workflow begins with genome contigs previously assembled using Spades (or other programs) that have generated “.fasta” files of assembled contigs. A .fasta file with the desired UCE probe-set is also required.
 
All required job files (and related sub-directories) for this workflow can be downloaded here
 
Preparatory Notes
 
Requried Files
1. 	A UCE probe set in fasta format
2. 	Assembled contigs for each sample in fasta format
3. 	Job files and directory templates (link to download above)
 
Copy the directory (and its subdirectories) <project> to your working Hydra directory of choice.
 
Rename the directories <project> and <Assembled_Contigs> with names appropriate for your project. Copy your assembled contigs into the newly named <Assembled_Contigs> directory.
 
Job files are numbered, and begin with number 5 and proceed through 17, which correspond to the numbered job files of the original LAB GitHub workflow.
 
 
Begin Workflow
 
A) Find UCE loci
 
Job file
5-MatchContigsToProbes.job
 
User notes – change path in job file to the directory that contains assembled contigs (.fasta). Run job in same directory as job file using qsub then job file.
 
This job will take some time, depending on the number of samples. When completed, a directory named “uce-search-results” will be created with individual “.lastz” files inside together with a “probe.matches.sqlite” file.
 
 


B.) Extract UCE loci
 
Job file
6-ExtractUCEloci-GetMatchCounts.job
 
User notes – create a text file called “taxon-set.conf”. It should begin with [all], and then sample names should be below in a column using the same names as those found in the “.lastz” files created in job 5. Remove the “.lastz” from sample names.
 
Example of “taxon-set.conf” file
 
[all]
Carbusculoides_spades_contigs
Cbarchyclados_spades_contigs
Cbottai_spades_contigs
 
Make new directories “taxon-sets/all” if not done already.
 
Commands in this job file (number 6) should be ok for use. When complete, the output file generated is called “all-taxa-incomplete.conf” and is found in the “taxon-set/all” directory.
 
After job 6 is complete, navigate to “taxon-sets/all” directory.
 
 
Job file
7-Get_fastas_from_match_counts.job
 
User notes – change path in job file to the assembled contigs directory
Make a directory called “log” if not done already.
 
This job will take some time, depending on the number of samples When complete, 2 files will be generated – all-taxa-incomplete.fasta, all-taxa-incomplete.incomplete
 
 
 
C.) Exploding the Monolithic fasta File (get stats on UCE loci)
 
Navigate to “taxon-sets/all” if not already there.
 
Job file
8-Explode_get_fastas_file.job
 
User notes - commands in job should be ok to run as-is
Results will be in directory named “exploded-fastas”
 
Job file
9-Get_summary_stats_on_exploded_FASTAs.job
 
User notes - commands in job should be ok to run as-is
Results will be saved to log file.
 
 
 D.) Align UCE loci
 
Note - For genus or family level relationships, running job 10 (with edge trimming) is recommended. For deeper-level phylogenetic relationships use jobs 12 and 13.
 
 Job file
10-AlignUCEloci-withEdgeTrimming.job
 
User notes - change the number of taxa in the job file to number of taxa being analyzed.
 
 Job file
11-AlignmentSummaryData-EdgeTrimmedData.job
 
User notes – Job 11 creates summary stats for results of job 10
 
 ---------
 
Note - For deeper-level phylogenetic relationships, no edge trimming (job 12) with internal Gblocks trimming (job 13) is recommended.
 
Job file
12-AlignUCEloci-No-Trim.job
 
User notes – change the number of taxa in the job file to number of taxa being analyzed.
 
Job file
13-InternalTrimGblocks-ofNoTrimAlignments.job
 
14-AlignmentSummaryData-NoTrimGblockInternalTrim.job
 
User notes – Job 14 creates summary stats for results of job 13.
 
 
 


E.) Alignment Cleaning/Renaming
 
Job file
15-AlignmentClean_remove_locus_name_from_nexus_lines.job
 
User notes – change path in the job file to the alignment file created in “Align UCE loci” above
 
 
 F. Final Data Matrix
 
Job file
16-FinalDataMarices-Create75percentMatrix.job
 
User notes – This job file creates a data matrix with 75% data (25% or less missing). Change number of taxa in the job file to number of taxa being analyzed. Change path to correct directory in job file.
 
 
G. Prepare RAxML file
 
Job file
17-PrepareDataForRAxML-GeneratePhylipFile.job
 
User notes – change path to correct directory in job file.
 
 

