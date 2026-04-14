
``` # SimpleNet 图像异常检测与定位模型代码解析
# 核心思想：通过特征提取+对抗训练判别正常/异常模式，实现像素级异常检测
# 主要组件：
#   1. Discriminator - 判别器网络，区分正常与异常特征
#   2. Projection - 特征投影模块，用于特征空间变换
#   3. SimpleNet - 主模型类，整合特征提取、训练、预测逻辑
#   4. PatchMaker - 图像分块处理器，提取局部特征

"""判别器模块"""
class Discriminator(torch.nn.Module):
    def __init__(self, in_planes, n_layers=1, hidden=None):
        """
        多层感知机结构的判别器
        Args:
            in_planes: 输入特征维度
            n_layers: 隐藏层数量
            hidden: 隐藏层维度（自动计算时使用输入维度）
        """
        super().__init__()
        self.body = torch.nn.Sequential()  # 主干网络
        # 构建多层线性层+BN+LeakyReLU
        for i in range(n_layers-1):
            _in = in_planes if i == 0 else hidden
            _hidden = int(hidden // 1.5) if hidden is None else hidden
            self.body.add_module(f'block{i+1}', torch.nn.Sequential(
                torch.nn.Linear(_in, _hidden),
                torch.nn.BatchNorm1d(_hidden),
                torch.nn.LeakyReLU(0.2)
            ))
        self.tail = torch.nn.Linear(hidden, 1)  # 最终输出层

    def forward(self, x):
        """输入特征向量，输出判别分数（越大表示越正常）"""
        return self.tail(self.body(x))

"""特征投影模块"""
class Projection(torch.nn.Module):
    def __init__(self, in_planes, out_planes=None, n_layers=1, layer_type=0):
        """
        线性投影网络，用于特征空间变换
        Args:
            layer_type: 0-纯线性层 1-加BN 2-加BN和激活
        """
        super().__init__()
        out_planes = out_planes or in_planes
        self.layers = torch.nn.Sequential()
        # 构建多层投影网络
        for i in range(n_layers):
            _in = in_planes if i ==0 else out_planes
            self.layers.add_module(f"{i}fc", torch.nn.Linear(_in, out_planes))
            # 可选添加BN和激活函数
            if i < n_layers-1 and layer_type > 1:
                self.layers.add_module(f"{i}relu", torch.nn.LeakyReLU(.2))

    def forward(self, x):
        """执行特征变换"""
        return self.layers(x)

"""TensorBoard日志包装器"""
class TBWrapper:
    def __init__(self, log_dir):
        self.g_iter = 0  # 全局迭代计数器
        self.logger = SummaryWriter(log_dir)  # TensorBoard记录器

"""主模型类"""
class SimpleNet(torch.nn.Module):
    def __init__(self, device):
        """
        异常检测主模型
        Args:
            device: 计算设备 (cuda/cpu)
        """
        super().__init__()
        self.device = device
        
    def load(self, backbone, layers_to_extract_from, **kwargs):
        """
        模型初始化入口
        关键组件：
        1. 特征聚合器：从预训练骨干网络提取多层级特征
        2. 预处理模块：统一不同层特征的通道维度
        3. 自适应聚合器：进一步聚合特征
        4. 判别器：对抗训练区分正常/异常特征
        5. 投影模块：优化特征空间
        """
        # 特征提取相关
        self.backbone = backbone.to(device)
        self.patch_maker = PatchMaker(patchsize=3)  # 图像分块处理器
        
        # 特征处理流水线
        self.forward_modules = torch.nn.ModuleDict({
            "feature_aggregator": 特征聚合器,
            "preprocessing": 预处理模块,
            "preadapt_aggregator": 自适应聚合器
        })
        
        # 对抗训练组件
        self.discriminator = Discriminator(...)
        self.pre_projection = Projection(...)  # 可选特征投影
        
        # 优化器配置
        self.dsc_opt = torch.optim.Adam(...)  # 判别器优化器
        self.proj_opt = torch.optim.Adam(...)  # 投影层优化器

    def _embed(self, images):
        """
        特征提取核心流程：
        6. 通过骨干网络提取多尺度特征
        7. 分块处理获得局部特征
        8. 多层级特征对齐与聚合
        9. 自适应特征聚合
        """
        # 特征提取 -> 分块处理 -> 空间对齐 -> 通道统一 -> 聚合
        return processed_features

    def train(self, training_data, test_data):
        """
        训练流程：
        10. 对抗训练判别器（meta-epochs循环）
        11. 每个epoch后用测试集评估
        12. 保存最佳模型参数
        """
        # 主要训练循环
        for epoch in range(meta_epochs):
            self._train_discriminator(training_data)  # 对抗训练
            # 验证并保存最佳模型

    def _train_discriminator(self, input_data):
        """
        判别器训练逻辑：
        13. 提取正常样本特征作为真实样本
        14. 添加噪声生成假样本
        15. 计算判别损失（Margin Loss）
        16. 反向传播优化
        """
        # 生成带噪声的假特征
        noise = torch.normal(0, self.noise_std, true_feats.shape)
        fake_feats = true_feats + noise
        
        # 计算判别损失
        true_scores = discriminator(true_feats)
        fake_scores = discriminator(fake_feats)
        loss = margin_loss(true_scores, fake_scores)

    def predict(self, data):
        """
        异常预测流程：
        17. 提取图像特征
        18. 通过判别器获取异常分数
        19. 生成异常热力图
        """
        features = self._embed(images)  # 提取特征
        patch_scores = -self.discriminator(features)  # 计算异常分数
        # 后处理生成热力图
        return anomaly_scores, anomaly_maps

"""图像分块处理器"""
class PatchMaker:
    def __init__(self, patchsize=3, stride=1):
        """
        图像分块处理工具
        功能：
        - 将特征图分解为重叠的局部块
        - 重建分块结果为完整特征图
        """
        self.unfolder = torch.nn.Unfold(
            kernel_size=patchsize, 
            stride=stride,
            padding=patchsize//2
        )

    def patchify(self, features):
        """将特征图展开为局部块 (B x C x H x W) -> (B x N_patches x C x K x K)"""
        unfolded = self.unfolder(features)
        return unfolded.reshape(...)  # 维度重组

    def unpatch_scores(self, x):
        """将分块结果重建为完整特征图"""
        return x.reshape(original_shape)

# ---------------------------
# 模型工作流程总结：
# 1. 特征提取：通过预训练CNN提取多层级特征
# 2. 特征处理：分块->对齐->聚合，得到统一特征表示
# 3. 对抗训练：用正常样本+噪声样本训练判别器
# 4. 异常检测：用判别器输出的异常分数生成热力图
# ---------------------------
```
