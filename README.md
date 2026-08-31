# data-document-redaction

用于一次完成可用且不泄漏的数据/文档副本生成、验证和交付报告输出。支持 CSV、JSON、SQL、表格、PDF、Word、PowerPoint、图片、扫描件和日志。

## 整体架构

![data-document-redaction 工作流](docs/diagrams/data-document-redaction-architecture.png)

PlantUML 源文件：[data-document-redaction-architecture.puml](docs/diagrams/data-document-redaction-architecture.puml)

## 使用方式

```text
Use $data-document-redaction to sanitize this document and output a privacy-safe delivery report.
```

一次处理流程包含范围确认、敏感内容检测、最小变换、格式与效用验证，以及不含原值的交付报告。它不替代法律/合规认证，也不用于恢复或绕过脱敏。

## 目录

- [主 skill](data-document-redaction/SKILL.md)
- [文档表面与最低证据](data-document-redaction/references/document-surfaces.md)
- [变换选择矩阵](data-document-redaction/references/transformation-matrix.md)
- [报告字段与模板](data-document-redaction/references/output-report.md)
- [Reddit 实战经验](data-document-redaction/references/reddit-practices.md)
- [权威资料](data-document-redaction/references/authoritative-sources.md)
- [本次 skill 更新报告](docs/skill-update-report.md)

## 本地验证

```bash
python3 /root/.codex/skills/.system/skill-creator/scripts/quick_validate.py data-document-redaction
python3 data-document-redaction/scripts/scan_sensitive.py data-document-redaction/SKILL.md data-document-redaction/agents data-document-redaction/references --pretty
```

扫描器只提供基线证据；实际文档交付仍必须按格式执行独立解析、视觉检查和报告流程。
