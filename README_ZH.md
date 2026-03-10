# Booster RL 任务

## 概述

本仓库提供了一套基于 [Isaac Lab](https://isaac-sim.github.io/IsaacLab/main/index.html) 的 Booster 机器人强化学习任务。  
目前，它包含了适用于 Booster K1 机器人的出色的 [BeyondMimic 运动跟踪](https://github.com/HybridRobotics/whole_body_tracking) 框架。  
本仓库遵循标准的 Isaac Lab 项目结构，并在 IsaacLab 2.2 和 Isaac Sim 5.0 上进行了测试。

## 安装

- 按照 [安装指南](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/index.html) 安装 Isaac Lab。  
  我们建议使用 conda 安装，因为它简化了从终端调用 Python 脚本的过程。

- 将本项目/仓库克隆或复制到与 Isaac Lab 安装目录分开的位置（即 `IsaacLab` 目录之外）：
    ```bash
    git clone https://github.com/BoosterRobotics/booster_train.git
    ```

- 下载并安装 booster_assets：
   - 克隆 [booster_assets](https://github.com/BoosterRobotics/booster_assets)，其中包含 Booster 机器人模型和运动数据。
   - 按照仓库中的说明安装 booster_assets Python 辅助工具。

- 使用已安装 Isaac Lab 的 Python 解释器，通过以下命令以可编辑模式安装库：
    ```bash
    # use 'PATH_TO_isaaclab.sh|bat -p' instead of 'python' if Isaac Lab is not installed in Python venv or conda
    python -m pip install -e source/booster_train
    ```

- 准备 BeyondMimic 运动数据：
    ```bash
    # use 'FULL_PATH_TO_isaaclab.sh|bat -p' instead of 'python' if Isaac Lab is not installed in Python venv or conda
    python scripts/csv_to_npz.py --headless --input_file=<PATH_TO_BOOSTER_ASSETS>/motions/K1/<MOTION>.csv --input_fps=<FPS> --output_name=<PATH_TO_BOOSTER_ASSETS>/motions/K1/<MOTION>.npz
    ```

## 使用

- 列出可用任务：
    ```bash
    # use 'FULL_PATH_TO_isaaclab.sh|bat -p' instead of 'python' if Isaac Lab is not installed in Python venv or conda
    python scripts/list_envs.py
    ```

- 运行任务：
    ```bash
    # use 'FULL_PATH_TO_isaaclab.sh|bat -p' instead of 'python' if Isaac Lab is not installed in Python venv or conda
    python scripts/rsl_rl/train.py --task=<TASK_NAME> --headless --device cuda:N
    ```

- 运行训练好的策略并导出以便部署：
    ```bash
    # use 'FULL_PATH_TO_isaaclab.sh|bat -p' instead of 'python' if Isaac Lab is not installed in Python venv or conda
    python scripts/rsl_rl/play.py --task=<TASK_NAME> --checkpoint=<CHECKPOINT_PATH>
    ```

    该脚本还会将训练好的策略导出为 TorchScript/ONNX 文件，用于部署到真实机器人上，文件保存在 `logs/rsl_rl/<EXPERIMENT>/<RUN>/exported/` 中。

## 部署

模型训练并导出后，您可以使用 [booster_deploy](https://github.com/BoosterRobotics/booster_deploy) 仓库在 MuJoCo 或真实的 Booster 机器人上部署训练好的策略。更多详情，请参考 [booster_deploy](https://github.com/BoosterRobotics/booster_deploy) 仓库中的说明。

## 致谢

- [whole_body_tracking](https://github.com/HybridRobotics/whole_body_tracking)：BeyondMimic 中的运动跟踪训练，这是一个多功能的人形机器人控制框架，提供高动态运动跟踪。