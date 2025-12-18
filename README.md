# WarpParse 规则库

WarpParse 规则库，用于分享常见的日志解析规则。

## 贡献指南

欢迎提交新的日志解析规则！详细步骤请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。

快速开始：

1. 在 `models/wpl/<日志类型>/` 下编写 `parse.wpl` 解析规则
2. 提供 `sample.dat` 示例数据文件
3. 运行 `wproj rule parse` 验证规则
4. 运行 `./validate.sh` 完整性检查
5. 提交 Pull Request

## License

MIT License
