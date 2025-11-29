 ```shell
================================================================================
  EVAL REPORT: Aircraft + 1retry
================================================================================
Method          | CLIP     | SigLIP   | DINOv1   | DINOv2   | DINOv3   | KID    
--------------------------------------------------------------------------------
Baseline        | 0.2684   | 0.1099   | 0.6363   | 0.3001   | 0.2547   | 0.01285
BC_SR           | 0.2797   | 0.1309   | 0.6470   | 0.3268   | 0.2885   | 0.01498
BC_MGR          | 0.2795   | 0.1334   | 0.6512   | 0.3248   | 0.2808   | 0.01512
TAC_SR          | 0.2831   | 0.1198   | 0.6630   | 0.3665   | 0.3216   | 0.00663
TAC_MGR         | 0.2819   | 0.1252   | 0.6656   | 0.3653   | 0.3236   | 0.00953
================================================================================
 ```




### OLD
aircraft数据集
--- Baseline (No RAG) ---
CLIP        : 0.2729
SigLIP      : 0.1257
DINOv1_B    : 0.6791
DINOv2_B    : 0.3196
DINOv3_B    : 0.2773
KID         : 0.014051 (Lower is better)
--- RAG Assisted (Old) ---
CLIP        : 0.2713
SigLIP      : 0.1235
DINOv1_B    : 0.6952
DINOv2_B    : 0.3388
DINOv3_B    : 0.3121
KID         : 0.006893 (Lower is better)


CUB鸟类数据集
--- Baseline (No RAG) ---
CLIP        : 0.2990
SigLIP      : 0.1293
DINOv1_B    : 0.5681
DINOv2_B    : 0.4266
DINOv3_B    : 0.3593
KID         : 0.014465 (Lower is better)

--- RAG Assisted (Old - Final) ---
CLIP        : 0.2937
SigLIP      : 0.1286
DINOv1_B    : 0.5678
DINOv2_B    : 0.4426
DINOv3_B    : 0.3811
KID         : 0.017476 (Lower is better)

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