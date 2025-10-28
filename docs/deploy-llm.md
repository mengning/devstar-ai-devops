# 本地LLM部署指南

## 安装部署

### 1. 安装 Ollama

#### macOS / Linux

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

#### Windows

从 [Ollama 官网](https://ollama.com/download) 下载安装包并安装。

### 2. 验证安装

```bash
ollama --version
```

### 3.下载模型

1. **DeepSeek-R1**  //使用MCP需要agent模式 deepseek不支持

   ```bash
   ollama pull deepseek-r1:1.5b  # 轻量级
   ollama pull deepseek-r1:8b   
   ```

2. **Qwen2.5-Coder** 

   ```bash
   ollama pull qwen2.5-coder:32b
   ```

### 4.验证

```
# 列出已下载的模型
ollama list

# 测试模型
ollama run deepseek-r1:1.5b "Hello, can you help me code?"
```

### 5.启动 Ollama 服务

```
# 启动Ollama服务 (默认端口11434)
ollama serve

# 验证服务状态
curl http://localhost:11434/api/tags
```



### 6.解决Ollama只能本地访问的问题

a. 编辑systemd服务配置，添加环境变量OLLAMA_HOST=0.0.0.0和OLLAMA_ORIGINS=*。

```
sudo nano /etc/systemd/system/ollama.service
```

并在 `[Service]` 区块中添加：

```
Environment=OLLAMA_HOST=0.0.0.0
Environment=OLLAMA_ORIGINS=*
```

b. 重新加载systemd并重启Ollama服务‌。

```
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

