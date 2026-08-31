# 交付报告字段

报告应让另一位审阅者知道做了什么、检查了什么、还有什么不确定性；不应成为敏感数据的第二份副本。skill 每次完成都必须输出报告；用户要求“文档完成后的二次审查”时，报告必须明确标记 `mode: secondary_review`，列出独立证据、发现项和交付结论。

## 推荐结构

```json
{
  "status": "pass|needs_review|blocked",
  "purpose": "external_share|internal_analysis|staging|troubleshooting|publication",
  "audience": "...",
  "inputs": [{"path": "...", "type": "...", "sha256": "..."}],
  "outputs": [{"path": "...", "type": "...", "sha256": "..."}],
  "transformations": [
    {"surface": "column|body|ocr|metadata|comment|attachment", "entity": "EMAIL", "action": "replace|suppress|generalize|synthetic", "reversible": false, "scope": "..."}
  ],
  "detections": {"EMAIL": 0, "PHONE": 0, "SECRET": 0},
  "integrity_checks": {"schema": "pass", "row_count": "pass", "foreign_keys": "pass", "distribution": "not_run"},
  "document_checks": {"text_search": "pass", "copy_test": "pass", "metadata": "pass", "ocr": "pass", "attachments": "pass"},
  "review": {
    "mode": "generation_verification|secondary_review",
    "independence": {"different_parser": true, "independent_surface_scan": true, "visual_check": "pass|partial|not_run"},
    "checks": {"version_and_hash": "pass", "second_detector": "pass", "hidden_surfaces": "pass", "utility": "pass"},
    "findings": [
      {"id": "R-001", "severity": "blocker|high|medium|low", "surface": "ocr|metadata|comment|attachment|column|body", "location": "page/row/section category only", "summary": "不含原值的事实描述", "action": "..."}
    ],
    "decision": "pass|needs_review|blocked"
  },
  "mapping_key": {"exists": false, "storage": "not_disclosed", "retention": "..."},
  "residual_risks": ["..."],
  "unsupported_or_unchecked": ["..."],
  "assumptions": ["..."],
  "tool_versions": {"...": "..."},
  "verified_at": "2026-01-01T00:00:00Z"
}
```

## Markdown 二次审查报告模板

需要给人阅读时，至少输出以下结构；即使没有发现，也要写 `No findings` 或“未发现符合条件的问题”，不要省略复核结果。

```markdown
# 文档完成后二次审查报告

- status: `pass|needs_review|blocked`
- mode: `secondary_review`
- purpose / audience: ...
- reviewed artifact: 文件类型、短路径或编号、SHA-256、验证时间
- first-pass evidence: 首轮报告/变换清单是否存在，是否完整
- independent evidence: 解析器/扫描器/渲染路径及版本；与首轮是否不同

## 检查结果

| 检查项 | 覆盖范围 | 结果 | 证据摘要 |
|---|---|---|---|
| 二次文本/结构扫描 | ... | pass / needs_review / blocked | 类型、位置类别、计数 |
| 隐藏表面 | ... | ... | 元数据、批注、修订、附件、OCR 等 |
| 视觉/交互检查 | ... | ... | 搜索、选择/复制、渲染、抽样或逐页覆盖 |
| 效用/完整性 | ... | ... | schema、页数、外键、分布、版式等 |

## Findings

`No findings.` 或按严重级别列出：编号、`blocker/high/medium/low`、表面、位置类别、影响、下一步。禁止写原值、完整上下文、映射、密钥或凭据。

## 残余风险与未覆盖面

- ...

## 结论

在声明的范围内，候选副本 `可以交付 / 需要修复后复审 / 当前无法判定`。这不是绝对匿名化或法律/合规认证。
```

## 报告规则

- `detections` 只写实体类型和数量；不要写原值、完整上下文、映射表、密钥或可用凭据。
- 输出路径、文件名和校验值本身也可能含敏感信息；按接收者需要缩短或替换。
- `pass` 只表示声明的范围和检查项通过，不表示“绝对不可重识别”或自动合规。
- 二次审查报告必须区分“首轮处理证据”和“独立复核证据”；只重复首轮工具的输出不能填充独立证据栏。
- 发现项按严重级别排序；`blocker` 或关键敏感内容残留时，不得给出 `pass`。
- 对未支持的格式、低置信度 OCR、未打开的附件、加密内容和未验证的动态链接，明确写入 `unsupported_or_unchecked` 并使用 `needs_review`/`blocked`。
- 若输出需要可逆，报告“存在映射”和保留期限即可，不写映射位置、密钥或恢复步骤。
