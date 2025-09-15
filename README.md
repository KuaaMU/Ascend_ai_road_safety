# 昇腾AI创新大赛项目：路端单目3D检测安全预警系统

小组名称：**AI一个**

本项目旨在参加昇腾AI创新大赛 ，利用昇腾AI技术和产品 ，开发一套部署于路侧的实时安全预警系统。系统基于单目摄像头视频流，通过3D目标检测技术识别车辆、行人等目标，并结合碰撞风险算法，对潜在的人车、车车碰撞危险进行预警，提升道路安全 。项目将充分利用昇腾生态（MindSpore框架、昇腾NPU、MindSpore Lite、ATC工具等）进行模型开发、优化与边缘端部署，以期在比赛中获得优异成绩 。

## 项目代码框架与分工指南

本项目代码库采用清晰的三阶段结构，便于团队成员分工协作。请各位组员根据分配的任务，熟悉对应目录结构，并参考 `docs/` 下的详细指南进行开发。

```
ascend-ai-road-safety/
├── README.md                       # 项目总览 (即本文档)
├── docs/                           # 详细文档 (请务必阅读！)
│   ├── technical_proposal.md       # 完整技术方案
│   ├── phase1_training_guide.md    # 模型训练阶段指南
│   ├── phase2_migration_guide.md   # 模型迁移阶段指南
│   └── phase3_edge_deploy_guide.md # 边缘部署阶段指南
│
├── model_training/               # 阶段1: 模型训练 (负责人: Hml、Zhl)
│   ├── src/                        # 训练源代码 (PyTorch/MindSpore)
│   │   ├── dataset/                # 数据加载与预处理
│   │   ├── model/                  # 网络模型定义 (如 monoflex.py)
│   │   ├── loss/                   # 损失函数
│   │   ├── utils/                  # 工具函数
│   │   └── train.py / eval.py      # 训练/评估主脚本
│   ├── configs/                    # 训练配置文件
│   └── checkpoints/                # (gitignore) 训练好的模型权重
│
├── model_migration/              # 阶段2: 模型迁移与转换 (负责人: xcm)
│   ├── scripts/                    # 迁移转换脚本
│   │   ├── 1_pytorch_to_mindspore.py # (可选) 权重转换脚本
│   │   └── 2_convert_to_mslite.sh  # 核心转换脚本 (ATC/converter_lite)
│   ├── converted_models/           # 转换后的模型 (ONNX, MindSpore, Lite)
│   └── validation/                 # 转换后模型验证脚本
│       └── validate_on_pc.py
│
├── edge_deployment/              # 阶段3: 边缘端部署 (负责人: csk、wqyy、xgh)
│   ├── src/                        # 部署应用源代码 (香橙派运行)
│   │   ├── camera/                 # USB摄像头接口
│   │   ├── inference_engine/       # MindSpore Lite推理引擎
│   │   ├── collision_warning/      # 碰撞预警算法
│   │   ├── display/                # 结果可视化 (OpenCV)
│   │   └── main.py                 # 主程序入口
│   ├── models/                     # (链接) 部署用模型文件
│   ├── configs/                    # 应用配置
│   └── deployment_scripts/         # 香橙派环境初始化脚本
│
├── data/                           # 数据集说明
│   └── README.md
└── requirements.txt                # 项目依赖
```

## 组员工作指导

1.  **模型训练组员**:
    *   请进入 `model_training/` 目录。
    *   阅读 `docs/phase1_training_guide.md`。
    *   负责实现/复现选定的单目3D检测模型（如MonoFlex），完成在指定数据集（如KITTI）上的训练与调优。
    *   将训练好的模型权重保存至 `model_training/checkpoints/`。
    *   确保模型输出格式符合后续迁移和预警算法的要求。

2.  **模型迁移组员**:
    *   请进入 `model_migration/` 目录。
    *   阅读 `docs/phase2_migration_guide.md`。
    *   负责将训练好的模型（来自 `model_training/checkpoints/`）转换为可在昇腾NPU上高效运行的MindSpore Lite格式（.ms）。
    *   使用 `scripts/2_convert_to_mslite.sh` 脚本进行转换，将结果放入 `converted_models/mindspore_lite/`。
    *   使用 `validation/validate_on_pc.py` 脚本在PC端验证转换后模型的精度和输出是否正确。

3.  **边缘部署组员**:
    *   请进入 `edge_deployment/` 目录。
    *   阅读 `docs/phase3_edge_deploy_guide.md`。
    *   负责在香橙派开发板上搭建运行环境（参考 `deployment_scripts/`）。
    *   实现 `src/` 下各模块：摄像头接入、调用MindSpore Lite模型推理、碰撞预警逻辑、结果显示。
    *   将 `model_migration/converted_models/mindspore_lite/` 下的模型链接或复制到 `edge_deployment/models/`。
    *   整合所有模块，确保系统能在香橙派上实时运行并输出预警结果。

## 重要提示

*   **沟通协作**: 各阶段接口（如模型输入输出格式、模型文件路径）需提前约定并保持一致。遇到阻塞问题及时在团队内沟通。
*   **文档先行**: 在 `docs/` 目录下更新你负责部分的详细文档，记录关键步骤、参数和遇到的问题。
*   **版本控制**: 频繁使用 `git add`, `git commit`, `git push` 提交代码，提交信息要清晰描述修改内容。
*   **昇腾生态**: 充分利用昇腾AI技术 ，这是项目的核心竞争力和加分项。

