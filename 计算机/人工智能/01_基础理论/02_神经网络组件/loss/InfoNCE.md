NCE（noise contrastive estimation）核心思想是将多分类问题转化成二分类问题，一个类是数据类别 data sample，另一个类是噪声类别 noisy sample，通过学习数据样本和噪声样本之间的区别，将数据样本去和噪声样本做对比，也就是“噪声对比（noise contrastive）”，从而发现数据中的一些特性。

