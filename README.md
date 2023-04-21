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

qiime dada2 denoise-paired \
    --i-demultiplexed-seqs qiime_out/${run}_demux_cutadapt.qza  \
    --p-trunc-len-f ${trunclenf} \
    --p-trunc-len-r ${trunclenr} \
    --p-trim-left-f 0 \
    --p-trim-left-r 0 \
    --p-n-threads 4 \
    --o-denoising-stats /home/users/lw1092/dc_workshop/ProjectData/denoising-stats.qza \
    --o-table /home/users/lw1092/dc_workshop/ProjectData/feature_table.qza \
    --o-representative-sequences /home/users/lw1092/dc_workshop/ProjectData/rep-seqs.qza

qiime metadata tabulate \
    --m-input-file /home/users/lw1092/dc_workshop/ProjectData/denoising-stats.qza \
    --o-visualization /home/users/lw1092/dc_workshop/ProjectData/denoising-stats.qzv

qiime feature-table tabulate-seqs \
        --i-data /home/users/lw1092/dc_workshop/ProjectData/rep-seqs.qza \
        --o-visualization /home/users/lw1092/dc_workshop/ProjectData/rep-seqs.qzv
