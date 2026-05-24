# Self Evolution

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
</p>

自进化引擎 v2 — 8阶段 Loop、3层评测、5维 AND 门控，技能自动优化。

## 架构

```
扫描技能池 → 评估质量 → 生成变体 → 适应度筛选 → 门控验证 → 部署
```

## 核心特性

- 8阶段进化 Loop
- 3层评测：基础功能 / 边界条件 / 对抗测试
- 5维 AND 门控：正确性、效率、安全性、稳定性、兼容性

## 快速开始

```yaml
plugins:
  - name: self-evolution
    path: ./self-evolution
```

## License

MIT