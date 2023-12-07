# Dashscope_to_openai
将阿里巴巴开源模型的Dashscope api 转换成Openai格式进行调用

注：这是一段魔改代码，改自Qwen/openai_api.py，为测试阿里云开源模型，只能进行最基本的非流式输出和流式输出进行对话，如有精力会继续更新

开始前请先安装必要的代码库
```shell
pip install fastapi uvicorn openai==0.28.0 pydantic sse_starlette dashscope
```

在终端运行代码
```shell
python dashscope_to_openai.py
```

使用API
```python
import openai
openai.api_base = "http://localhost:8000/v1"
openai.api_key = "none"

# 使用流式回复的请求
for chunk in openai.ChatCompletion.create(
    model="Qwen",
    messages=[
        {"role": "user", "content": "你好"}
    ],
    stream=True
    # 流式输出的自定义stopwords功能尚未支持
):
    if hasattr(chunk.choices[0].delta, "content"):
        print(chunk.choices[0].delta.content, end="", flush=True)

# 不使用流式回复的请求
response = openai.ChatCompletion.create(
    model="Qwen",
    messages=[
        {"role": "user", "content": "你好"}
    ],
    stream=False
)
print(response.choices[0].message.content)
```

