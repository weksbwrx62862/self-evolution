<h1 align="center">Self Evolution</h1>

<p align="center"><strong>自进化引擎 v2 — 让技能自动发现改进空间并自我优化</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Hermes-%3E%3D2.0.0-orange.svg" alt="Hermes Compat">
  <img src="https://img.shields.io/github/stars/weksbwrx62862/self-evolution?style=social" alt="Stars">
  <img src="https://img.shields.io/github/last-commit/weksbwrx62862/self-evolution" alt="Last Commit">
</p>

---

## 简介

Self Evolution 是 Hermes Agent 的自进化插件，实现了完整的 8 阶段进化 Loop、3 层评测体系和 5 维 AND 门控机制。它能够自动扫描技能池中的可优化项，生成变体并验证，仅在全部门控维度通过后才部署新版本，确保进化过程安全可控。通过 GEPA 遗传算法与多样性优化策略，持续推动技能质量提升。

## 功能矩阵

| 能力 | 描述 |
|------|------|
| 8 阶段进化 Loop | Scan → Evaluate → Generate → Train → Validate → Gate → Deploy → Monitor 全链路闭环 |
| 3 层评测体系 | 基础功能 / 边界条件 / 对抗测试逐层递进 |
| 5 维 AND 门控 | 正确性 ∧ 效率 ∧ 安全性 ∧ 稳定性 ∧ 兼容性，缺一不可 |
| GEPA 遗传优化 | 基于遗传算法的提示词/技能变体搜索 |
| 多样性优化 | diversify 策略避免进化早熟收敛 |
| 代理验证 | surrogate verifier 交叉验证进化结果 |
| 自动触发 | 定时调度 + 事件驱动，无需人工干预 |
| 一键回滚 | 进化不达预期时秒级回退 |

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                     Self Evolution Engine v2                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────┐   ┌──────────┐   ┌──────────┐   ┌───────┐           │
│  │ Scan │──▶│ Evaluate │──▶│ Generate │──▶│ Train │           │
│  └──────┘   └──────────┘   └──────────┘   └───────┘           │
│      ▲                                            │             │
│      │              ┌───────────┐                 ▼             │
│      │              │  Monitor  │◀───┌────────┐  ┌─────────┐   │
│      │              └───────────┘    │ Deploy │◀─│  Gate   │   │
│      │                   │           └────────┘  └─────────┘   │
│      │                   ▼                │          ▲          │
│      │            ┌───────────┐          │    ┌──────────┐     │
│      │            │ Rollback  │◀─────────┘    │ Validate │     │
│      │            └───────────┘               └──────────┘     │
│      │                                             ▲            │
│      └─────────────────────────────────────────────┘            │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    3-Layer Evaluation                     │   │
│  │  ┌────────────┐  ┌────────────┐  ┌─────────────────┐    │   │
│  │  │  Layer 1   │  │  Layer 2   │  │    Layer 3      │    │   │
│  │  │ 基础功能   │─▶│ 边界条件   │─▶│   对抗测试      │    │   │
│  │  └────────────┘  └────────────┘  └─────────────────┘    │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   5-Dimension AND Gate                    │   │
│  │  正确性 ──∧── 效率 ──∧── 安全性 ──∧── 稳定性 ──∧── 兼容性 │   │
│  │                    ▼ ALL PASS → Deploy                    │   │
│  │                    ▼ ANY FAIL  → Reject & Rollback        │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│  Skill Pool ◀──▶ Pipeline (GEPA / Diversify) ◀──▶ Triggers     │
└─────────────────────────────────────────────────────────────────┘
```

## 快速开始

### 前置条件

- Python 3.10+
- [Hermes Agent](https://github.com/weksbwrx62862/hermes) >= 2.0.0

### 安装

```bash
git clone https://github.com/weksbwrx62862/self-evolution.git
cd self-evolution
pip install -e .
```

### 最小示例

```yaml
# hermes_config.yaml
plugins:
  - name: self-evolution
    path: ./self-evolution
```

```bash
# 扫描技能池，检测可优化项
self_evo_scan

# 对指定技能执行完整进化流程
self_evo_evolve --skill my-skill

# 查看进化状态
self_evo_status

# 门控通过后批准部署
self_evo_approve --skill my-skill

# 进化不达预期时回滚
self_evo_rollback --skill my-skill
```

## 核心功能详解

### 8 阶段进化 Loop

```
Scan → Evaluate → Generate → Train → Validate → Gate → Deploy → Monitor
```

| 阶段 | 职责 | 对应模块 |
|------|------|----------|
| Scan | 扫描技能池，识别可优化项 | `skill_pool/pool.py` |
| Evaluate | 3 层评测评估当前技能质量 | `core/evaluator.py` → `core/fitness.py` |
| Generate | 通过 GEPA / Diversify 生成变体 | `pipeline/gepa_optimizer.py` / `pipeline/diversify_optimizer.py` |
| Train | 在训练集上优化变体 | `core/evolution_manager.py` |
| Validate | 交叉验证变体表现 | `core/surrogate_verifier.py` |
| Gate | 5 维 AND 门控决策 | `core/constraints.py` |
| Deploy | 部署通过门控的变体 | `core/orchestrator.py` |
| Monitor | 持续监控已部署技能 | `triggers/auto_trigger.py` |

### 3 层评测体系

| 层级 | 检测内容 | 失败处理 |
|------|----------|----------|
| Layer 1 — 基础功能 | 核心功能是否正常工作 | 直接拒绝，不进入下一层 |
| Layer 2 — 边界条件 | 异常输入、极端参数、边缘情况 | 标记风险，进入对抗层 |
| Layer 3 — 对抗测试 | 恶意输入、安全注入、压力场景 | 拒绝并触发安全审查 |

### 5 维 AND 门控

所有维度**必须全部通过**才能部署，任一维度失败即拒绝并回滚：

| 维度 | 评判标准 | 阈值 |
|------|----------|------|
| 正确性 | 输出质量不低于原版 | ≥ baseline score |
| 效率 | 延迟不增加超过阈值 | ≤ 1.2× baseline latency |
| 安全性 | 通过安全扫描 | 0 critical/high findings |
| 稳定性 | 压力测试无 crash | 0 errors under load |
| 兼容性 | API 接口向后兼容 | 100% backward compat |

### GEPA 遗传优化

基于 GEPA（Genetic Evolution of Prompt Architecture）算法，通过交叉、变异、选择算子在提示词空间中搜索更优解：

```python
from self_evolution.pipeline import GEPAOptimizer

optimizer = GEPAOptimizer(population_size=20, mutation_rate=0.15)
result = optimizer.evolve(skill, generations=10)
```

### 多样性优化

Diversify 策略通过引入多样性度量，避免进化过程早熟收敛到局部最优：

```python
from self_evolution.pipeline import DiversifyOptimizer

optimizer = DiversifyOptimizer(diversity_weight=0.3)
result = optimizer.evolve(skill, generations=10)
```

### 提供的工具

| 工具 | 功能 |
|------|------|
| `self_evo_status` | 查看当前进化状态与历史记录 |
| `self_evo_scan` | 扫描技能池，检测可优化项 |
| `self_evo_approve` | 批准通过门控的进化部署 |
| `self_evo_reject` | 拒绝进化候选变体 |
| `self_evo_execute` | 执行单步进化操作 |
| `self_evo_evolve` | 启动完整进化流程 |
| `self_evo_rollback` | 回滚到进化前版本 |

## 技术栈

```
┌──────────────┬──────────────────────────┐
│   语言       │ Python 3.10+             │
│   框架       │ Hermes Agent Plugin SDK  │
│   算法       │ GEPA 遗传算法            │
│   评测       │ 3 层递进 + 5 维 AND 门控 │
│   调度       │ 内置定时 + 事件触发      │
│   配置       │ YAML (plugin.yaml)       │
│   License    │ MIT                      │
└──────────────┴──────────────────────────┘
```

## 项目结构

```
self-evolution/
├── plugin.yaml              # 插件声明
├── __init__.py              # 包入口
├── cli.py                   # CLI 命令行接口
├── default_provider.py      # 默认进化提供者
├── core/
│   ├── __init__.py
│   ├── constraints.py       # 5 维 AND 门控约束
│   ├── dataset_builder.py   # 训练数据集构建
│   ├── evolution_manager.py # 进化管理器
│   ├── evolution_provider.py# 进化提供者抽象
│   ├── fitness.py           # 适应度评估
│   ├── orchestrator.py      # 进化编排器
│   └── surrogate_verifier.py# 代理验证器
├── pipeline/
│   ├── __init__.py
│   ├── base_optimizer.py    # 优化器基类
│   ├── diversify_optimizer.py# 多样性优化器
│   ├── gepa_optimizer.py    # GEPA 遗传优化器
│   └── optimizer.py         # 优化器入口
├── skill_pool/
│   ├── __init__.py
│   └── pool.py              # 技能池管理
├── triggers/
│   ├── __init__.py
│   └── auto_trigger.py      # 自动触发调度
├── AGENTS.md
├── CLAUDE.md
└── V2_IMPROVEMENTS.md       # v2 改进说明
```

## 开发指南

### 环境搭建

```bash
git clone https://github.com/weksbwrx62862/self-evolution.git
cd self-evolution
pip install -e ".[dev]"
```

### 添加自定义优化器

继承 `BaseOptimizer` 并实现 `evolve` 方法：

```python
from self_evolution.pipeline import BaseOptimizer

class MyOptimizer(BaseOptimizer):
    def evolve(self, skill, generations=5):
        pass
```

### 添加自定义门控维度

在 `core/constraints.py` 中扩展门控检查：

```python
def check_my_dimension(variant, baseline) -> bool:
    return True
```

### 代码规范

- 遵循 PEP 8
- 类型注解覆盖所有公开 API
- 提交信息遵循 Conventional Commits 规范

## 路线图

- [x] 8 阶段进化 Loop
- [x] 3 层评测体系
- [x] 5 维 AND 门控
- [x] GEPA 遗传优化器
- [x] 多样性优化器
- [x] 代理验证器
- [ ] 多技能协同进化
- [ ] 分布式进化训练
- [ ] 进化可视化仪表盘
- [ ] 自适应门控阈值

## FAQ

**Q: 进化失败后会自动回滚吗？**

A: 是的。Gate 阶段任一维度未通过，引擎会自动拒绝变体并触发 Rollback，恢复到进化前的稳定版本。

**Q: 可以手动触发进化吗？**

A: 可以。通过 `self_evo_evolve --skill <name>` 手动触发指定技能的进化流程，也可以通过 `self_evo_scan` 先扫描再选择性进化。

**Q: 5 维 AND 门控是否过于严格？**

A: AND 门控确保只有全方位优于原版的变体才能上线。各维度的阈值可在 `plugin.yaml` 中调整，适应不同场景的严格程度。

**Q: GEPA 和 Diversify 优化器如何选择？**

A: GEPA 适合在已知方向上深度优化，Diversify 适合探索未知空间避免局部最优。建议先用 GEPA 收敛，再用 Diversify 打破瓶颈。

**Q: 支持哪些 Hermes Agent 版本？**

A: 需要 Hermes Agent >= 2.0.0，低于此版本不兼容。

## Contributing

欢迎贡献！请遵循以下流程：

1. Fork 本仓库
2. 创建 Feature Branch：`git checkout -b agent/PROJ-XXX-description`
3. 提交变更，遵循 Conventional Commits 规范
4. 推送分支并创建 Pull Request
5. 等待 CI 通过和 Code Review

提交 PR 前请确保：
- 所有测试通过
- 新功能有对应测试覆盖
- 代码符合 PEP 8 规范
- 无敏感信息泄露

## License

本项目基于 [MIT License](https://opensource.org/licenses/MIT) 开源。

## Security

如发现安全漏洞，请**不要**在公开 Issue 中报告。请通过以下方式负责任地披露：

- 发送邮件至仓库维护者
- 包含漏洞描述、复现步骤和影响范围

我们承诺在 48 小时内响应安全报告。

## 致谢

- [Hermes Agent](https://github.com/weksbwrx62862/hermes) — 插件运行时框架
- GEPA 算法灵感来源于遗传编程与提示词工程的交叉研究
- 所有为自进化引擎贡献想法和代码的开发者

---

<p align="center"><strong>进化不止，优化不息</strong></p>
