# Self Evolution

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Hermes-%3E%3D2.0.0-orange.svg" alt="Hermes">
  <img src="https://img.shields.io/badge/version-2.0.0-blue.svg" alt="Version">
</p>

自进化引擎 v2 — 让技能自动发现改进空间并自我优化。8 阶段进化 Loop、3 层评测体系、5 维 AND 门控，确保进化安全可控。

## 架构

```
扫描技能池 → 评估质量 → 生成变体 → 适应度筛选 → 门控验证 → 部署 → 监控
```

## 核心特性

### 8 阶段进化 Loop

```
Scan → Evaluate → Generate → Train → Validate → Gate → Deploy → Monitor
```

### 3 层评测体系

| 层级 | 检测内容 |
|------|----------|
| 基础功能 | 核心功能是否正常 |
| 边界条件 | 异常输入、边缘情况 |
| 对抗测试 | 恶意输入、安全注入 |

### 5 维 AND 门控

所有维度必须通过才能部署：

| 维度 | 说明 |
|------|------|
| 正确性 | 输出质量不低于原版 |
| 效率 | 延迟不增加超过阈值 |
| 安全性 | 通过安全扫描 |
| 稳定性 | 压力测试无 crash |
| 兼容性 | API 接口向后兼容 |

## 安装

### 前置条件

- Python 3.10+
- [Hermes Agent](https://github.com/weksbwrx62862/hermes) >= 2.0.0

### 从源码安装

```bash
git clone https://github.com/weksbwrx62862/self-evolution.git
cd self-evolution
pip install -e .
```

### 依赖

```bash
pip install pyyaml numpy
```

## 使用

### Hermes 插件模式

```yaml
# hermes_config.yaml
plugins:
  - name: self-evolution
    path: ./self-evolution
```

### 典型工作流

```yaml
# 扫描当前技能池，检测可优化的技能
self_evo_scan

# 执行进化（生成变体 → 训练 → 验证）
self_evo_evolve --skill my-skill

# 查看进化状态
self_evo_status

# 通过门控验证后可批准部署
self_evo_approve --skill my-skill

# 不进反退时回滚
self_evo_rollback --skill my-skill
```

## 提供的工具

| 工具 | 功能 |
|------|------|
| `self_evo_status` | 查看进化状态 |
| `self_evo_scan` | 扫描技能池，检测可优化项 |
| `self_evo_approve` | 批准进化部署 |
| `self_evo_reject` | 拒绝进化候选 |
| `self_evo_execute` | 执行单步进化操作 |
| `self_evo_evolve` | 启动完整进化流程 |
| `self_evo_rollback` | 回滚到进化前版本 |

## 项目结构

```
self-evolution/
├── plugin.yaml         # 插件声明
├── core/
│   ├── engine.py       # 进化引擎核心
│   ├── evaluator.py    # 3层评测系统
│   ├── gate.py         # 5维AND门控
│   └── genetics.py     # 遗传算法（GEPA）
├── pipeline/
│   ├── scanner.py      # 技能扫描
│   ├── generator.py    # 变体生成
│   └── deployer.py     # 部署管理
├── triggers/
│   └── scheduler.py    # 定时触发
└── V2_IMPROVEMENTS.md  # v2 改进说明
```

## 开发

```bash
git clone https://github.com/weksbwrx62862/self-evolution.git
cd self-evolution
pip install -e .
# 通过 Hermes 运行时测试
```

## License

MIT