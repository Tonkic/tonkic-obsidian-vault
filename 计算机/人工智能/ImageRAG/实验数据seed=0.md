==============================
  EVALUATION RESULTS: aircraft
  Classes Evaluated: 100
==============================
--- Baseline (No RAG) ---
CLIP   Score : 0.2729
SIGLIP Score : 0.1257
DINO   Score : 0.3196

--- RAG Assisted (Old) ---
CLIP   Score : 0.2713
SIGLIP Score : 0.1235
DINO  Score : 0.3388

==============================

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
### 选择类别中的全部：
CUB鸟类数据集
--- Baseline (V1) (基于 200 张图像) ---
CLIP Score :   0.2743
SigLIP Score : 0.1270
DINO Score :   0.5500 (vs ALL real images)
KID Score  :   0.013732 (Lower is better)
--- True Final (RAG 修正后) (基于 200 张图像) ---
CLIP Score :   0.3030
SigLIP Score : 0.1458
DINO Score :   0.5677 (vs ALL real images)
KID Score  :   0.007866 (Lower is better)
aircraft数据集
--- 报告 B: 评估结果 (公平比较, 100% 任务) ---
--- Baseline (V1) (基于 100 张图像) ---
CLIP Score :   0.2787
SigLIP Score : 0.1220
DINO Score :   0.6583 (vs ALL real images)
KID Score  :   0.009244 (Lower is better)
--- True Final (RAG 修正后) (基于 100 张图像) ---
CLIP Score :   0.2863
SigLIP Score : 0.1304
DINO Score :   0.6784 (vs ALL real images)
KID Score  :   0.004025 (Lower is better)
