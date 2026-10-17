# AI VPN 分流规则

按已提供的域名清单维护 ChatGPT / Codex、Claude、Gemini、GitHub Copilot 和 Cursor 规则。文件为静态规则源；修改并推送 `dist` 分支后，客户端可按自己的更新周期重新获取。没有自动抓取第三方规则的任务。

## 远程地址

- Clash 规则片段： https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/ai-rules.yaml
- Clash / Mihomo classical Rule Provider： https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/ai-provider.yaml

客户端「我们所向往的」的远程规则格式尚未确认。只有确认它接受 `rules:` YAML 时才使用第一个地址；如果支持 classical Rule Provider，使用第二个地址。解析成功和实际命中仍需在客户端验证。这些文件不是包含节点的完整订阅配置。

## Clash 规则片段

`ai-rules.yaml` 使用 `PROXY` 作为目标策略。导入前确认客户端有同名策略或代理组，否则在自己的配置中映射为真实策略名。规则应放在已有的兜底 MATCH 或其他会提前命中的宽泛规则之前。文件没有 MATCH，不改变其余流量的去向。

## Clash / Mihomo Rule Provider

在现有配置中合并以下内容，不要覆盖已有代理组或规则。将 `PROXY` 改为实际代理组名。

```yaml
rule-providers:
  ai:
    type: http
    behavior: classical
    format: yaml
    url: https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/ai-provider.yaml
    path: ./ruleset/ai.yaml
    interval: 86400
rules:
  - RULE-SET,ai,PROXY
  # 在此保留原有规则和兜底规则
```

## 范围与验证

本清单覆盖用户提供的 9 条域名规则，不保证涵盖各服务所有认证、下载、CDN 或未来新增的域名。Gemini 仅匹配 `gemini.google.com`，不会将整个 Google 域名加入代理。Copilot 也不会将整个 GitHub 加入代理。

导入后打开对应服务，在客户端连接日志中检查目标域名、命中规则及最终策略。YAML 结构验证不能证明该客户端支持此格式，也不能保证节点所在地区满足服务要求。
