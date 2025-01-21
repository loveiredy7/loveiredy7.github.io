本文将介绍如何通过调用 OpenAI 的 GPT API 来实现 ChatGPT 的功能。通过简单的 API 调用，您可以实现稳定的连续对话，并在一定程度上规避访问节点的限制问题。以下是详细教程，希望对您有所帮助！

---

## 为什么选择 API 调用？

在国内使用 ChatGPT 时，可能会遇到以下问题：

1. 与 ChatGPT 对话 1-2 轮后，或间隔一段时间后，必须刷新页面重新开始。
2. 某些梯子节点无法访问 ChatGPT 网站（提示 "Access Denied"）。

通过 API 调用可以有效解决问题（1），实现稳定的连续对话；同时对访问节点的限制更少，部分情况下可以规避问题（2）。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)

---

## Step 1: 准备 Python 环境

在您的电脑上需要安装 Python 环境以及 `pip` 工具。建议使用 Python 3.6 及以上版本（可以通过 Anaconda 管理 Python 版本）。如果您尚未安装 Python，请自行搜索相关教程。

---

## Step 2: 安装所需的 Python 包

在国内安装 Python 包时，建议使用国内镜像站（如清华大学镜像站），并确保此步骤**不要挂梯子**。

bash
pip install --upgrade openai
pip install --upgrade python-dotenv
pip install --upgrade langchain


---

## Step 3: 注册 OpenAI 账号并获取 API Key

1. 前往 [OpenAI API Key 页面](https://platform.openai.com/account/api-keys) 注册账号并获取您的专属 API Key。
2. API Key 是一串包含字母和数字的字符，请妥善保管，不要与他人分享。**所有通过该 API Key 调用产生的费用将由该账号承担。**

---

## Step 4: 编写简单的代码

以下是一个简单的代码示例，您需要将 `openai.api_key` 替换为您在 Step 3 中获取的 API Key。

python
import openai
from langchain.chat_models import ChatOpenAI
from langchain.schema import AIMessage, HumanMessage, SystemMessage

# 替换为您的 API Key
openai.api_key = '请将这里替换成你的 API Key'

# 定义系统提示内容（可为空）
system_content = ''

# 初始化对话
messages = [SystemMessage(content=system_content)]

# 最多对话 20 轮
i = 1
while i <= 20:
    chat = ChatOpenAI(temperature=0, openai_api_key=openai.api_key)
    
    user_input = input("请输入您的问题：")
    
    # 输入 \end 结束对话
    if user_input == '\end':
        break
    # 输入 \clear 清空当前对话
    if user_input == '\clear':
        i = 1
        messages = [SystemMessage(content=system_content)]
        continue
    
    messages.append(HumanMessage(content=user_input))
    
    response = chat(messages)
    messages.append(AIMessage(content=response.content))
    
    print(f"[GPT] 第 {i} 轮对话：")
    print(response.content)
    
    i += 1

print("\n --- 对话结束 ---")


---

## 常见问题及解决方法

1. **`Cannot connect to proxy`**  
   这是梯子的代理问题，与节点无关。请尝试更换梯子。

2. **`HTTPSConnectionPool(host='api.openai.com', port=443)`**  
   这是 URL 访问包版本问题。可以通过以下命令重装指定版本的 `urllib3`：

   bash
   pip uninstall urllib3
   pip install urllib3==1.25.11
   

---

## 关于 API 调用的费用

- **接口费用**：`gpt-3.5-turbo` 的调用费用为 $0.002 / 1K tokens（以注册时为准）。每个账户有 $18 的免费额度，但请注意免费额度有到期时间。
- **优化建议**：
  - 避免长篇连续对话，因为连续对话会占用更多的 tokens。
  - 如果需要处理长篇内容（如润色或翻译论文），建议分批输入。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)