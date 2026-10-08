# GitHub 连接器写入测试记录

## 本次提交信息

| 项 | 值 |
| --- | --- |
| 测试时间 | 2026-10-08 16:00（GMT+8） |
| 仓库 | [cjh408/test](https://github.com/cjh408/test) |
| 分支 | `main` |
| 操作账号 | cjh408 |
| 提交文件 | `connector-test-notes.md`（本文件） |

## 写入通道

本次提交走的是**自定义 MCP Server `github-pat`**，直连 GitHub 官方远程 MCP 端点
`https://api.githubcopilot.com/mcp/`，使用 classic PAT（scopes: `repo, workflow`）。

## 为什么不用内置连接器

内置 `connector:github` 当前是**只读**授权：读取类接口（`get_me`、
`get_file_contents`、`search_repositories`）正常，但写类接口会返回
`403 Resource not accessible by integration`——这是集成层面缺少仓库写权限，
不是工具缺失，也不是只读头部所致。

## 结论

- ✅ 写入路径可用：`create_or_update_file` / `push_files` 正常执行。
- 需要改文件时必须先取 blob SHA（`get_file_contents`），否则更新会失败。
- 若要改用内置连接器写入，需要在 GitHub 侧重新授权并勾选完整的仓库写权限。
