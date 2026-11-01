# 代码实验：终端调用大模型 API

> 验证 OpenAI 兼容接口的最小可运行示例（Task 01 第 3 步：API 连通性测试）。

## 实验 1：curl 直连 DeepSeek

```bash
curl -s https://api.deepseek.com/v1/chat/completions \
  -H "Authorization: Bearer $DEEPSEEK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-flash",
    "messages": [{"role": "user", "content": "只回复两个字：收到"}],
    "max_tokens": 50
  }'
```

**预期输出**（节选）：

```json
{
  "choices": [
    {"message": {"role": "assistant", "content": "收到"}}
  ],
  "usage": {"prompt_tokens": 35, "completion_tokens": 2}
}
```

## 实验 2：Python 版（后续 Task 08 会用到 with open / json）

```python
import json
import urllib.request

url = "https://api.deepseek.com/v1/chat/completions"
payload = json.dumps({
    "model": "deepseek-flash",
    "messages": [{"role": "user", "content": "你好"}],
}).encode("utf-8")

req = urllib.request.Request(url, data=payload, headers={
    "Authorization": "Bearer YOUR_KEY",
    "Content-Type": "application/json",
})

with urllib.request.urlopen(req, timeout=30) as resp:
    print(json.load(resp)["choices"][0]["message"]["content"])
```

## 记录表

| 实验 | 日期 | 结果 | 备注 |
|---|---|---|---|
| curl 直连 DeepSeek | 2026-11-01 | ✅ 返回「收到」 | token 消耗 37 |
| curl 直连 ModelScope GLM-5.2 | 2026-11-01 | ✅ 返回「收到」 | 需实名+邮箱验证后可用 |
