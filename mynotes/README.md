# 我的笔记

## docker自托管本地部署

从docker安装
```
docker compose --env-file '.my.prod.env' up
```

## 访问网页

```
http://localhost:11001/
```


## 使用 deepseek 测试访问

**.env**
```txt
DEEPSEEK_API_KEY=sk-xxxxxxxxx

LANGFUSE_SECRET_KEY="sk-lf-xxxxxxxxx"
LANGFUSE_PUBLIC_KEY="pk-lf-xxxxx"
LANGFUSE_BASE_URL="http://localhost:11001"

```

**main.py**
```python
# Instead of: import openai
import os

from dotenv import load_dotenv
from langfuse import observe
from langfuse.openai import OpenAI

load_dotenv()

# Initialize the OpenAI client, pointing it to the DeepSeek Inference API
client = OpenAI(
    base_url="https://api.deepseek.com",
    api_key=os.getenv("DEEPSEEK_API_KEY"),
)


completion = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Why is AI cool? Answer in 20 words or less."},
    ],
)
print(completion.choices[0].message.content)
```