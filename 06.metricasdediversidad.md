# 14_metrics

## Directorios de entrada y salida:
INPUT_DIR=32_taxa_filter 
INPUT_DIR2=40_phylo 
OUTPUT_DIR=44_metrics 
mkdir -p ${OUTPUT_DIR} 

## Generar métricas de diversidad:
qiime diversity core-metrics-phylogenetic \
  --i-table ${INPUT_DIR}/dada2_table_filt_contam.qza \
  --i-phylogeny ${INPUT_DIR2}/dada2_rep-seqs_final_aligned_masked_tree_rooted.qza \
  --p-sampling-depth 129132 \
  --m-metadata-file metadata.txt \
  --p-n-jobs-or-threads 2 \
  --o-rarefied-table ${OUTPUT_DIR}/rarefied_table.qza \
  --o-faith-pd-vector ${OUTPUT_DIR}/faith_pd_vector.qza \
  --o-observed-features-vector ${OUTPUT_DIR}/observed_features_vector.qza \
  --o-shannon-vector ${OUTPUT_DIR}/shannon_vector.qza \
  --o-evenness-vector ${OUTPUT_DIR}/evenness_vector.qza \
  --o-unweighted-unifrac-distance-matrix ${OUTPUT_DIR}/unweighted_unifrac_distance_matrix.qza \
  --o-weighted-unifrac-distance-matrix ${OUTPUT_DIR}/weighted_unifrac_distance_matrix.qza \
  --o-jaccard-distance-matrix ${OUTPUT_DIR}/jaccard_distance_matrix.qza \
  --o-bray-curtis-distance-matrix ${OUTPUT_DIR}/bray_curtis_distance_matrix.qza \
  --o-unweighted-unifrac-pcoa-results ${OUTPUT_DIR}/unweighted_unifrac_pcoa_results.qza \
  --o-weighted-unifrac-pcoa-results ${OUTPUT_DIR}/weighted_unifrac_pcoa_results.qza \
  --o-jaccard-pcoa-results ${OUTPUT_DIR}/jaccard_pcoa_results.qza \
  --o-bray-curtis-pcoa-results ${OUTPUT_DIR}/bray_curtis_pcoa_results.qza \
  --o-unweighted-unifrac-emperor ${OUTPUT_DIR}/unweighted_unifrac_emperor.qzv \
  --o-weighted-unifrac-emperor ${OUTPUT_DIR}/weighted_unifrac_emperor.qzv \
  --o-jaccard-emperor ${OUTPUT_DIR}/jaccard_emperor.qzv \
  --o-bray-curtis-emperor ${OUTPUT_DIR}/bray_curtis_emperor.qzv

## Realizar pruebas de significancia de grupos:
qiime diversity alpha-group-significance \
  --i-alpha-diversity ${OUTPUT_DIR}/shannon_vector.qza \
  --m-metadata-file metadata.txt \
  --o-visualization ${OUTPUT_DIR}/shannon_compare_groups.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity ${OUTPUT_DIR}/evenness_vector.qza \
  --m-metadata-file metadata.txt \
  --o-visualization ${OUTPUT_DIR}/evenness-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity ${OUTPUT_DIR}/faith_pd_vector.qza \
  --m-metadata-file metadata.txt \
  --o-visualization ${OUTPUT_DIR}/faith-pd-group-significance.qzv

qiime diversity alpha-rarefaction \
  --i-table ${INPUT_DIR}/dada2_table_filt_contam.qza \
  --p-max-depth 129132 \
  --p-steps 20 \
  --i-phylogeny ${INPUT_DIR2}/dada2_rep-seqs_final_aligned_masked_tree_rooted.qza \
  --m-metadata-file metadata.txt \
  --o-visualization ${OUTPUT_DIR}/rarefaction_curves.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix ${OUTPUT_DIR}/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file metadata.txt \
  --m-metadata-column 'type-of-reactor' \
  --o-visualization ${OUTPUT_DIR}/unweighted-unifrac-source-significance.qzv \
  --p-pairwise
