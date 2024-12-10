# tuning_qwen
- 微调qwen2.5-7b模型过程
- python 3.10.2 + cuda11.8
- qwen2.5模型微调 + 测试 + 转gguf格式 + Q4量化 + 转ollama + ollama部署

# Installation Tools:
- 微调工具：https://github.com/hiyouga/LLaMA-Factory.git
- 编译工具：https://cmake.org/download,  https://github.com/ggerganov/llama.cpp.git
- 量化工具：https://github.com/ggerganov/llama.cpp/releases/llama-b4295-bin-win-noavx-x64.zip

# 整体流程:
1. LLaMA-Factory项目用于微调qwen模型
2. llama.cpp项目用于safetensors格式转gguf模型格式
3. llama-b4295-bin-win-noavx-x64 用于gguf格式量化
4. ollama部署gguf格式模型

# 详细过程：
## 模型微调
### 安装微调仓库
```bash
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```
### 创建自己数据集
- 参考LLaMA-Factory/data/identity.json格式创建自己数据集.
- 修改LLaMA-Factory/data/dataset_info.json中数据集的映射

### 进入webui的微调界面
```bash
llamafactory-cli webui
```
### 设置微调参数及数据集文件.
### 开始训练.
### 训练结束后, 在chat菜单栏测试微调效果.(可选)
### 在export菜单栏指定导出路径后导出模型文件. 在指定路径下生成几个model-0000X-of-0000X.safetensors模型

## 编译及模型格式转换
### 安装cmake工具
- win下载地址：https://cmake.org/download, 解压后设置环境变量, cmd查看 cmake -version.
- 安装MinGW/Visual Studio 编译器. 参考https://www.cnblogs.com/hack747/p/18547314

### 克隆编译仓库
```bash
git clone https://github.com/ggerganov/llama.cpp.git
mkdir build
cd build
cmake ..
cmake --build . --config Release
pip install -r requirements.txt
python convert_hf_to_gguf.py D:\myQwen2.5-7B
```

## 模型量化
### 量化(可选)
- 下载量化工具：https://github.com/ggerganov/llama.cpp/releases/llama-b4295-bin-win-noavx-x64.zip
```bash
.\llama-quantize.exe D:\myQwen2.5-7B\myQwen2.5-3B-F16.gguf  D:\myQwen2.5-7B\myQwen2.5-7B-Q4_K_M.gguf Q4_K_M
```
- 生成一个较小体积的gguf文件

## ollama模型部署
### 复制Modelfile文件到myQwen2.5-7B-Q4_K_M.gguf模型目录.

### ollama部署
```bash
ollama create myQwen2.5-3B -f Modelfile
ollama run myQwen2.5-3B
```

# 参考：https://blog.csdn.net/qq_42374233/article/details/144180042