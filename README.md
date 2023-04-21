Fri 4/14

formed groups

# git clone
    adds folder to workspace, copy/paste clone URL from github

https://github.com/jthmiller/gen711-811-example


Fri 4/21

cp /tmp/gen711_project_data/fastp.sh <https://github.com/ashleyhunt0330/Gen711_Final_Cyano.git>/fastp.sh

chmod +x ~/fastp.sh

./fastp.sh 150 /tmp/gen711_project_data/cyano/fastqs  trimmed_fastqs
after its done

conda activate qiime2-2022.8

qiime tools import --type "SampleData[PairedEndSequencesWithQuality]" --input-format CasavaOneEightSingleLanePerSampleDirFmt --input-path /home/users/alh1081/fastp.sh --output-path /home/users/alh1081/trimmed_fastqs

qiime cutadapt trim-paired \
    --i-demultiplexed-sequences </home/users/alh1081/trimmed_fastqs> \
    --p-cores 4 \
    --p-front-f <GTGYCAGCMGCCGCGGTAA> \
    --p-front-r <CCGYCAATTYMTTTRAGTTT> \
    --p-discard-untrimmed \
    --p-match-adapter-wildcards \
    --verbose \
    --o-trimmed-sequences trimmed_fastqs/trimmed_cyano.qza

qiime demux summarize \
--i-data /home/users/alh1081/trimmed_fastqs/trimmed_cyano \
--o-visualization  trimmed_cyano.qzv

qiime dada2 denoise-paired \
    --i-demultiplexed-seqs qiime_out/${run}_demux_cutadapt.qza  \
    --p-trunc-len-f ${trunclenf} \
    --p-trunc-len-r ${trunclenr} \
    --p-trim-left-f 0 \
    --p-trim-left-r 0 \
    --p-n-threads 4 \
    --o-denoising-stats /home/users/alh1081/trimmed_fastqs/trimmed_cyano/denoising-stats.qza \
    --o-table home/users/alh1081/trimmed_fastqs/trimmed_cyano/feature_table.qza \
    --o-representative-sequences /home/users/alh1081/trimmed_fastqs/trimmed_cyano/rep-seqs.qza

qiime metadata tabulate \
    --m-input-file /home/users/alh1081/trimmed_fastqs/trimmed_cyano/denoising-stats.qza \
    --o-visualization /home/users/alh1081/trimmed_fastqs/trimmed_cyano/denoising-stats.qzv

qiime feature-table tabulate-seqs \
        --i-data /home/users/alh1081/trimmed_fastqs/trimmed_cyano/rep-seqs.qza \
        --o-visualization /home/users/alh1081/trimmed_fastqs/trimmed_cyano/rep-seqs.qzv
