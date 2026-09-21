# 规范快照来源

本目录保存 Code-W Extension Spec v1 的规范与 JSON Schema 快照，供 Skill 在脱离 `code-workspace` 源码仓库运行时校验扩展合同。

## 同步信息

- 来源仓库：`/Users/liutaigang/workspace/zhuiyi_workspace/project/workspace/code-workspace`
- 来源 commit：`93bddc05d1165fa6dc459079ca0c521c55356aa0`
- 同步日期：`2026-09-21`
- 同步方式：逐文件原样复制，未改写内容
- 快照范围：同步时所列文件均与来源 commit 一致且没有未提交修改

## 文件与校验值

| 原路径 | SHA-256 |
|---|---|
| `spec/extension/v1/specification.zh-CN.md` | `d3d38f11f84427e14ea1a731cd8f3a428c9b4358ba7f1bd238ca1eb51cb201cd` |
| `schemas/extension-manifest-v3.json` | `f99b155cf6ba2c5db95225ecfc295bcaacab3b5658eddbcfaaa41d8f033a4207` |
| `schemas/extension-init-context-v1.json` | `524867a59dd429271b8f34b402d737551de15332c2f4c2b48a76a84c9b2047a3` |
| `schemas/extension-init-result-v1.json` | `5e8d78a1df45879d897bd65f3e0d183d3ddfa4dfdda7db80b6c3266de8441a99` |
| `schemas/extension-npm-transport-envelope-v1.json` | `74b3ef5a4e2ecb3ffe37a96b0a2e81040d99876986d737fe1f7a94dad78116bd` |

## 使用规则

- 当 `code-workspace` 源码仓库可用时，以其当前 `spec/`、`schemas/`、CLI 实现和 `--help` 为运行时事实来源；如果与快照不一致，说明差异并优先遵循当前仓库，不要静默混用两个版本。
- 当源码仓库不可用时，使用本快照校验 manifest、init context、init result 和 npm transport envelope。
- 更新扩展规范或 schema 时，应重新同步文件、来源 commit、同步日期和 SHA-256。
