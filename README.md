# New-England-Cyanobacteria-Communities
Metabarcoding of cyanobacteria, specifically in New England area

Logan Wentworth

Methods

cp /tmp/gen711_project_data/fastp.sh /home/users/lw1092/dc_workshop/New-England-Cyanobacteria/fastp.sh
  
chmod +x /home/users/lw1092/dc_workshop/New-England-Cyanobacteria/fastp.sh
  
/home/users/lw1092/dc_workshop/New-England-Cyanobacteria-Communities/fastp.sh 150 /tmp/gen711_project_data/cyano/fastqs /home/users/lw1092/dc_workshop/ProjectData/trimmed_fastqs

qiime tools import \
SampleData[PairedEndSequencesWithQuality] \
input-format CasavaOneEightSingleLanePerSampleDirFmt \
input-path /home/users/lw1092/dc_workshop/ProjectData/trimmed_fastqs/ \
output-path /home/users/lw1092/dc_workshop/ProjectData/imported_files \

qiime cutadapt trim-paired \
i-demultiplexed-sequences /home/users/lw1092/dc_workshop/ProjectData/imported_files \
p-cores 4 \
p-front-f GTGYCAGCMGCCGCGGTAA \
p-front-r CCGYCAATTYMTTTRAGTTT \
p-discard-untrimmed \
p-match-adapter-wildcards \
verbose \
o-trimmed-sequences /home/users/lw1092/dc_workshop/ProjectData/trimmed_sequences.qza

qiime demux summarize \
--i-data /home/users/lw1092/dc_workshop/ProjectData/trimmed_sequences.qza \
--o-visualization  /home/users/lw1092/dc_workshop/ProjectData/visualization.qzv
