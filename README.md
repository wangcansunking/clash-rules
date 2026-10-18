# AI 与常用国外网站代理规则

面向 Clash / Mihomo 的域名代理清单，覆盖常见 AI 工具和 Google、YouTube、社交、资讯、开发及流媒体网站。不是节点订阅，也不是完整 VPN 配置。

## 远程规则地址

| 格式 | URL |
| --- | --- |
| Clash `rules:` 规则片段（含 `PROXY` 策略） | https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/proxy-rules.yaml |
| Clash / Mihomo classical `payload:` Rule Provider（不含策略） | https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/proxy-provider.yaml |

旧地址 `ai-rules.yaml`、`ai-provider.yaml` 保留，内容分别与上述文件完全一致，**现在也包含常用国外网站规则**，无需更换已有 URL。

客户端「我们所向往的」的远程规则格式尚未确认；请按客户端支持的格式选择地址。`rules:` 与 `payload:` 不能互换。`PROXY` 必须对应已有策略或代理组，否则需要在自己的配置中替换/映射。规则要放在原有 `MATCH` 或会提前命中的宽泛规则之前。

## Rule Provider 配置示例

将下列片段合并进现有配置，保留已有节点、代理组和其他规则，把 `PROXY` 替换为实际代理组名：

```yaml
rule-providers:
  overseas:
    type: http
    behavior: classical
    format: yaml
    url: https://raw.githubusercontent.com/wangcansunking/clash-rules/dist/proxy-provider.yaml
    path: ./ruleset/overseas.yaml
    interval: 86400
rules:
  - RULE-SET,overseas,PROXY
  # 后面保留原有规则及兜底规则
```

## 覆盖范围

完整域名清单及服务分组见 [domains.json](domains.json)。当前包括：

- AI 聊天、编程、搜索及推理：ChatGPT / Codex / Sora、Claude、Gemini、Copilot、Cursor、Windsurf、Aider、Continue、Tabnine、Sourcegraph、Replit、Bolt、v0、Lovable、Perplexity、Poe、Grok、Meta AI、Mistral、DeepSeek、通义、Kimi、豆包、元宝、文心、智谱、MiniMax、Hugging Face、Replicate、Together、Fireworks、Groq、Cerebras、OpenRouter、DeepInfra、Cohere、AI21。
- AI 图像、音视频及效率工具：Midjourney、Leonardo、Ideogram、Playground、Stability、Clipdrop、fal、Civitai、Runway、Pika、Luma、可灵、海螺、HeyGen、Synthesia、ElevenLabs、Suno、Udio、NotebookLM、Gamma、Beautiful.ai、Otter、Fireflies、Jasper、Copy.ai、Writesonic、Character.AI。
- Google 搜索、Gmail、Drive、Photos、Maps、Google API 与常见静态资源；YouTube、YouTube Music 和视频资源。
- Facebook、Instagram、Threads、WhatsApp、X / Twitter、Telegram、Signal、Discord、Reddit、Quora、Pinterest、Tumblr。
- Wikipedia / Wikimedia、BBC、纽约时报、华尔街日报、Bloomberg、Reuters、Guardian、Economist、FT、DW、RFI、VOA、Medium、Substack。
- GitHub、GitLab、Bitbucket、Docker、Stack Overflow、npm、Yarn、Vercel、Netlify、Read the Docs、Internet Archive。
- Netflix、Twitch、Vimeo、Dailymotion、Spotify、SoundCloud、Proton、DuckDuckGo、Dropbox、Notion、Figma、Canva。

所有匹配到的域名（包括国内 AI 服务及未被屏蔽的国外服务）都按代理策略处理。Google 规则同时包含广告/统计域名，以支持相关资源；若有去广告规则，请放在本规则集前面。GitHub Pages、Vercel、Netlify 等平台包含用户托管站点，匹配范围也包括这些站点。

这是人工维护的常用网站清单，不是“全球所有 AI 工具/所有被墙网站”的完整数据库，也不代表每个所列网站在每个地区都被屏蔽。Google 国家域名目前仅列出常用版本。第三方登录、CDN 或新增域名可能需要补充；流媒体和 AI 服务能否使用也取决于节点地区及服务自身限制。

## 维护和验证

仅编辑 `domains.json`，然后运行：

```sh
python3 scripts/generate.py
python3 scripts/generate.py --check
```

生成器仅使用 Python 标准库，检查域名格式和重复项，并同步四个 YAML 文件。文件使用域名后缀匹配；没有 IP 规则或 `MATCH`，其他流量继续遵循客户端原有配置。规则中的业务域名不是对网站实时可用性的承诺。

推送到 `dist` 后，客户端按自身更新周期获取；也可手动刷新远程规则。仓库不自动抓取第三方数据。导入后在客户端连接日志中检查访问域名、命中规则及最终策略。当前已验证生成一致性、YAML 结构和代表性域名匹配；未在实际 VPN 客户端中验证兼容性及连接。
