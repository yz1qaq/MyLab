# MyLab 教学版

这里是学习者手写后端、逐步接入前端的工作目录。已实现第一章的 go-zero API 和健康检查，并通过编译、HTTP 请求验收；尚未接入 MySQL、RPC、登录或前端。

当前进度：[第 01 章](../docs/chapters/01.md)实现已验收，等待学习者提交并创建 `01-end`。

下一章：[第 02 章：用户 RPC 服务与 MySQL](../docs/chapters/02.md)，尚未开始实现。

本目录的模块名为 `github.com/yz1qaq/MyLab/mylab-course`。已实现的 HTTP 服务位于 `services/lab-api`；后续账号业务的 RPC 服务会在对应步骤加入。

所有 Go 命令默认在本目录执行，服务启动时按章节说明传入配置路径。

## 环境

- Go：1.26.4。
- goctl：1.10.2。
- go-zero：v1.10.3，由 go.mod 固定。

2026-10-15 检查时，终端中的 `/opt/homebrew/bin/go` 为 1.27.0，`GOROOT` 却指向 goenv 的 1.26.4，混用会导致编译错误。本次使用完整的 1.26.4 工具链完成编译。若自己的终端也遇到同样问题，先在当前终端执行以下命令，使 Go 入口与当前 GOROOT 一致：

```bash
export PATH="/Users/yz1/.goenv/versions/1.26.4/bin:$PATH"
go version
go env GOROOT
```

预期分别对应 `go1.26.4` 和 `/Users/yz1/.goenv/versions/1.26.4`。此操作只影响当前终端，不修改全局配置。

## 启动与验证

```bash
cd /Users/yz1/MyLab/mylab-course
go build ./...
go run ./services/lab-api -f services/lab-api/etc/lab-api.yaml
```

在另一个终端请求：

```bash
curl -i http://127.0.0.1:8888/healthz
```

预期为 HTTP 200，JSON 响应体为 `{"status":"ok"}`。

## 第一章验收

2026-10-15 已检查学习者保存的实现：

- 使用 Go 1.26.4 编译 API 成功。
- 在本机回环地址上使用临时端口启动同一服务二进制，实际请求 `/healthz`，返回 HTTP 200、`application/json; charset=utf-8` 和 `{"status":"ok"}`。
- 临时配置和服务进程在验证后清理，项目中的 `127.0.0.1:8888` 配置保持原值。

目前只验证 API 入口与健康检查；以上结果不代表数据库、RPC 或账号管理已经可用。
