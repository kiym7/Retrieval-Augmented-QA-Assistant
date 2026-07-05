# Retrieval-Augmented Q&A Assistant

> 基于 **Streamlit** 与 **LangChain** 构建的本地知识库 RAG 智能问答系统。支持纯文本文档的在线上传、向量化存储，并能结合大语言模型进行精准的检索增强问答与多轮对话。

---

## 核心功能

- 📁 **文件解析与去重**：支持通过 Web 页面上传 `.txt` 文件。系统在读取前会基于 `MD5` 算法对文件内容进行哈希校验，有效避免重复数据多次入库，保障知识库的纯净度。
- 🧠 **RAG 检索增强生成**：用户提问时，系统会先通过向量检索匹配最相关的知识片段，再将其作为上下文与大模型结合，生成有据可依的回答，显著降低大模型“幻觉”问题。
- 💬 **多轮对话与持久化记忆**：实现了历史对话记录的本地持久化（`FileChatMessageHistory`），支持跨轮次的多轮交互。AI 能够理解对话上下文，而不是每次都重新认知。
- ⚡ **流式输出（Streaming）**：大模型返回结果采用流式传输技术，逐字逐句实时呈现，消除等待焦虑，大幅提升用户交互体验。
- 🎨 **直观的 Web 管理界面**：基于 Streamlit 构建，无需繁琐的前后端分离开发，即可拥有美观的聊天面板和文件管理界面。

---

##效果预览
---
<!-- 智能客服示例图 -->
<div align="left">
  <img src="./assets/chat_demo1.png" width="750" alt="智能客服聊天界面示例 1">
</div>

- 本地知识库预存了衣物尺码推荐表、材质维护、穿衣搭配等示例内容（data见assets）
- 对标电商，可按个人需求替换为自己的业务文本
---
<div align="left">
  <img src="./assets/chat_demo2.png" width="750" alt="智能客服聊天界面示例 2">
</div>

- 支持结合历史消息进行连续问答

---

## 技术栈体系

- **前端框架**：Streamlit
- **RAG 编排框架**：LangChain (`Runnable`, `RunnableWithMessageHistory`)
- **大模型 & 嵌入模型**：阿里云通义千问 (`qwen3-max`, `text-embedding-v4`)
- **向量数据库**：Chroma (本地持久化存储)
- **文本处理**：RecursiveCharacterTextSplitter
- **开发语言**：Python 3.10+

---

## 项目结构

```text
Retrieval-Augmented-QA-Assistant/
├─ assets/                    # README 演示图片与示例素材文本所在
├─ app_upload.py              # 知识库上传服务（Streamlit）
├─ app_chat.py                # 智能客服问答（Streamlit）
├─ knowledge_base.py          # 知识库处理：读取、切分、写库、去重
├─ rag.py                     # RAG 链组装
├─ vector_stores.py           # 向量库检索封装（持久化）
├─ file_history_store.py      # 会话历史存储
├─ config_data.py             # 模型、路径、chunk 等参数配置
├─ requirements.txt           # 项目依赖（配置环境）
└─ chroma_db/                 # 向量数据库本地存储目录
```
---
##  环境准备

```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```
- 终端运行，建议虚拟环境加载，清华镜像源加速
---

## 配置说明

- config_data.py 中包含核心配置，根据实际需要，手动修改模型配置、chunk大小...
- 默认嵌入器 text-embedding-v4 及 Qwen3-max
- 注意，DashScope/通义千问相关的 API Key（例如 DASHSCOPE_API_KEY）需要在环境变量中先行配置
---

##  快速运行
### 1. 启动知识库上传服务
```bash
streamlit run app_upload.py
```
- 打开页面后上传 .txt 文件后，即可写入本地向量库。

### 2. 启动智能客服（RAG Chat）
```bash
streamlit run app_chat.py
```
- 输入问题后，会先检索知识库，再结合检索内容由模型综合回答
---

## 常见问题

### Q1：上传文件后，聊天问答仍然像“没有检索到资料”？
#### 可能原因：
- 上传服务和问答服务使用了不同的向量库持久化目录
- `collection_name` 配置不一致
- 上传文件后未正确写入本地数据目录

### Q2：上传文件后，回答显示较慢或没有正常输出？
#### 可能原因：
- 文本切分参数或检索参数设置不合适，调整chunk、检索k...
- 模型接口或网络请求响应较慢
- 本地向量库未正确初始化

### Q3：项目运行报路径或配置错误怎么办？
#### 建议优先检查：
- `config_data.py` 中的模型配置、路径配置是否正确
- 本地数据目录是否存在
- API Key 是否已配置到环境变量

---

## 📄 License

- 本项目仅用于学习与交流，若涉及商业应用场景，使用者需自行完善数据安全、隐私保护及所依赖第三方服务的商业授权事宜。
---