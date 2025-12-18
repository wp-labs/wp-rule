# 贡献指南

感谢您对 WarpParse 规则库的贡献！本文档将指导您如何提交新的日志解析规则。

## 贡献流程

### 1. 编写解析规则

在 `models/wpl/` 目录下创建新的日志类型目录，并编写 `parse.wpl` 文件：

```
models/wpl/<日志类型>/parse.wpl
```

**示例：** 创建 Apache 日志解析规则

```bash
mkdir -p models/wpl/apache
```

编写 `models/wpl/apache/parse.wpl`：

```wpl
package /apache {
   rule apache {
        (ip:client_ip,_,_,chars:timestamp<[,]>,http/request",chars:status,chars:size)
   }
}
```

### 2. 提供示例数据

在同一目录下提供 `sample.dat` 示例数据文件：

```
models/wpl/<日志类型>/sample.dat
```

示例数据要求：
- 包含真实的日志样本（注意脱敏敏感信息）
- 覆盖常见的日志格式变体
- 建议提供 5-10 条典型日志

**示例：** `models/wpl/apache/sample.dat`

```
192.168.1.1 - - [18/Dec/2025:10:30:00 +0800] "GET /index.html HTTP/1.1" 200 1234
192.168.1.2 - - [18/Dec/2025:10:30:01 +0800] "POST /api/login HTTP/1.1" 302 567
```

### 3. 验证解析规则

使用 `wproj` 工具验证解析规则是否正确：

```bash
wproj rule parse
```

该命令会：
- 加载解析规则
- 使用 sample.dat 进行测试解析
- 输出解析结果供检查

确保：
- 所有字段正确提取
- 无解析错误
- 边界情况处理正确

### 4. 运行验证脚本

执行验证脚本进行完整性检查：

```bash
./validate.sh
```

验证脚本会检查：
- 规则语法正确性
- 示例数据格式
- 解析覆盖率
- 输出字段完整性

## 目录结构要求

```
models/wpl/<日志类型>/
├── parse.wpl      # 解析规则文件（必需）
└── sample.dat     # 示例数据文件（必需）
```

## 提交 Pull Request

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feature/add-<日志类型>-parser`
3. 提交更改：`git commit -m "feat: add <日志类型> log parser"`
4. 推送分支：`git push origin feature/add-<日志类型>-parser`
5. 创建 Pull Request

## 命名规范

- 目录名：使用小写字母，多个单词用连字符分隔（如 `aws-cloudfront`）
- 规则包名：与目录名一致，使用 `/` 前缀（如 `package /aws-cloudfront`）
- 字段名：使用 snake_case 命名（如 `client_ip`、`request_url`）

## 常见问题

**Q: 如何处理多种日志格式？**

A: 可以在同一个 package 中定义多个 rule。

**Q: sample.dat 需要多少条数据？**

A: 建议 5-10 条，覆盖主要的日志格式变体即可。

**Q: 如何调试解析规则？**

A: 使用 `wproj rule parse` 命令查看详细的解析输出，检查字段提取是否正确。
