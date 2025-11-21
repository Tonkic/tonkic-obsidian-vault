==============================
  EVALUATION RESULTS: aircraft
--- Baseline (No RAG) ---
CLIP    : 0.2729
SigLIP  : 0.1257
DINO    : 0.3196 (vs ALL GT images)
KID     : 0.013743 (Lower is better)

--- RAG Assisted (Old) ---
CLIP    : 0.2713
SigLIP  : 0.1235
DINO    : 0.3388 (vs ALL GT images)
KID     : 0.007179 (Lower is better)

==============================

### 选择类别中的全部：
CUB鸟类数据集


  FINAL REPORT: cub (All ViT-Base)
--- Baseline (V1) ---
CLIP        : 0.2743
SIGLIP      : 0.1270
DINOV1_BASE : 0.5596
DINOV2_BASE : 0.4188
DINOV3_BASE : 0.3494
KID         : 0.014076
--- True Final ---
CLIP        : 0.3030
SIGLIP      : 0.1458
DINOV1_BASE : 0.5717
DINOV2_BASE : 0.5062
DINOV3_BASE : 0.4252
KID         : 0.008243


aircraft数据集
--- Baseline (V1) ---
CLIP        : 0.2787
SIGLIP      : 0.1220
DINOV1_BASE : 0.6715
DINOV2_BASE : 0.3295
DINOV3_BASE : 0.2792
KID         : 0.009855
  
--- True Final ---
CLIP        : 0.2863
SIGLIP      : 0.1304
DINOV1_BASE : 0.6953
DINOV2_BASE : 0.3844
DINOV3_BASE : 0.3421
KID         : 0.004090










### 随机选择类别中的1张：
CUB鸟类数据集
--- Baseline (只用T2I) (基于 200 张图像) ---
CLIP Score :   0.2743
SigLIP Score : 0.1270
DINO Score :   0.5585
--- True Final (RAG 修正后) (基于 200 张图像) ---
CLIP Score :   0.3030
SigLIP Score : 0.1458
DINO Score :   0.5681
aircraft数据集
--- Baseline (只用T2I) (基于 100 张图像) ---
CLIP Score :   0.2787
SigLIP Score : 0.1220
DINO Score :   0.6552
--- True Final (RAG 修正后) (基于 100 张图像) ---
CLIP Score :   0.2863
SigLIP Score : 0.1304
DINO Score :   0.6754