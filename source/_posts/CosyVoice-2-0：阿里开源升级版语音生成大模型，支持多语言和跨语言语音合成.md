---
title: CosyVoice 2.0：阿里开源升级版语音生成大模型，支持多语言和跨语言语音合成
date: 2024-12-22 23:25:49
categories: AI
tags: 语音合成模型
cover: https://pic.imgdb.cn/item/676834a2d0e0a243d4e81c36.png
---

![](https://pic.imgdb.cn/item/676818a8d0e0a243d4e814ac.png)

## 介绍

1. **功能**：支持超低延迟的流式[语音合成](https://zhida.zhihu.com/search?content_id=251622557&content_type=Article&match_order=1&q=%E8%AF%AD%E9%9F%B3%E5%90%88%E6%88%90&zhida_source=entity)，首包合成延迟仅150ms。
2. **性能**：发音准确性显著提升，音色一致性和韵律自然度大幅改善。
3. **技术**：采用全尺度量化和离线流式一体化建模，支持多语言和指令可控的音频生成。

CosyVoice项目是由*阿里巴巴*通义实验室的FunAudioLLM团队开发的，现在更新到了2.0版本，提升了发音和音色等的准确性，跟之前爆火的GPT-SoVITS一样，是一个开源的语音合成项目，不过GPT-SoVits附带变声器生态，相比各有优劣，我已经抢先测试了一波，下面是cosyvoice的本地部署教程

## 本地部署详细教程

### 注意事项

所有相关的软件、文件名称不要使用中文名称，也不要有[中文路径](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=%E4%B8%AD%E6%96%87%E8%B7%AF%E5%BE%84&zhida_source=entity)，也不要有空格。
包括C盘用户名，不要有中文或空格。

### 模型部署前准备

* [nvidia显卡](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=nvidia%E6%98%BE%E5%8D%A1&zhida_source=entity),建议显存6G以上
* AI框架CUDA、cuDNN安装 （已安装可跳过此步骤）
* Git安装（已安装可跳过此步骤）
* Miniconda安装（已安装可跳过此步骤）

> github项目地址：[https://github.com/thewlabs/cosyvoice](https://github.com/thewlabs/cosyvoice)

#### 一、AI框架CUDA安装 （已安装可跳过此步骤）

- 检查本机是否安装CUDA，以及CUDA版本，这个可用通过命令查看：

```bash
nvcc -V
```

- 输入NVIDIA-smi，查看当前显卡支持的CUDA版本，最好高于12.0.

```bash
NVIDIA-smi
```

![](https://pic.imgdb.cn/item/67681bfbd0e0a243d4e815e5.jpg)

- 下载安装CUDA
- 下载地址：[https://**developer.nvidia.com/cu**da-toolkit-archive](https://link.zhihu.com/?target=https%3A//developer.nvidia.com/cuda-toolkit-archive)
- 选择合适的版本，这里我选择的是12.4.0，之后依次选择系统windows、x86\_64、10、exe（local），自己选择自己对应系统就可以。
  ![](https://pic.imgdb.cn/item/67681c5dd0e0a243d4e8162b.jpg)
- 点击安装，默认下一步即可，需要时可以更改安装位置，注意路径不要有中文或空格。
- 配置环境变量， 搜索环境变量设置，编辑环境变量，将cuda的安装位置添加到系统变量。若安装程序已自动添加，无需更改。
- 下载安装cuDNN
- 下载地址：[https://**developer.nvidia.com/rd**p/cudnn-archive](https://link.zhihu.com/?target=https%3A//developer.nvidia.com/rdp/cudnn-archive)
- 选择合适的版本，需对应之前安装的CUDA版本，如CUDA版本12.x，下载的对应的v8.9.7。（需要登录NVIDIA账号）
- 免登录下载办法：找到需要的版本，右键–>复制链接–>导入下载器下载或浏览器新建页面粘贴链接下载
- 解压压缩包，将文件夹内所有文件复制至之前安装的CUDA[根目录](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=%E6%A0%B9%E7%9B%AE%E5%BD%95&zhida_source=entity)，覆盖替换即可。

```bash
D:\MyToolsSoftWare\CUDADevelopment\
```

- 配置环境变量
- 新建cuDNN系统环境变量
- 变量名：CUDNN。变量值为：CUDA根目录、bin目录、include目录、lib\\x64目录，中间由英文分号隔开。

```bash
D:\MyToolsSoftWare\CUDADevelopment;D:\MyToolsSoftWare\CUDADevelopment\bin;D:\MyToolsSoftWare\CUDADevelopment\include;D:\MyToolsSoftWare\CUDADevelopment\lib\x64
```

- 在系统[path变量](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=path%E5%8F%98%E9%87%8F&zhida_source=entity)下，同样添加以上目录

  ![](https://pic.imgdb.cn/item/67681d5ed0e0a243d4e8166a.jpg)
- 检查安装结果
- win+R 打开运行，输入[cmd](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=2&q=cmd&zhida_source=entity)打开命令行窗口
- 输入nvcc -V 查看CUDA版本，注意’V’大写，若能正确返回CUDA版本号，证明安装成功。nvcc -V

#### 二、Git安装（已安装可跳过此步骤）

> GIT无需多言，傻子都会装

- 下载地址：[https://**git-scm.com/downloads**](https://link.zhihu.com/?target=https%3A//git-scm.com/downloads)
- 选择安装位置，默认安装即可。

#### 三、Miniconda安装（已安装可跳过此步骤）

- 下载地址：[https://**docs.anaconda.com/minic**onda/](https://link.zhihu.com/?target=https%3A//docs.anaconda.com/miniconda/)
- 点击页面中“Miniconda3 Windows 64-bit”版本下载
  ![](https://pic.imgdb.cn/item/67681e78d0e0a243d4e816de.jpg)
- 选择安装位置，建议新建conda文件夹，默认安装，勾选所有选项。
  ![](https://pic.imgdb.cn/item/67681ec4d0e0a243d4e816f1.jpg)
- 检查安装结果，win+R 打开运行，输入cmd打开[命令行窗口](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=3&q=%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%AA%97%E5%8F%A3&zhida_source=entity)
- 输入conda --version，若能正确返回conda版本号，证明安装成功。

```bash
conda --version
```

### 部署模型

> PS：以下部署过程中命令均在命令行窗口中执行，如果命令行窗口执行过程中，一直提示SSLError或HTTPSConnectionError错误，则表示无法下载，需设置代理端口克隆和下载三方库：

设置方式：在命令行窗口运行以下指令

```
set http_proxy=http://127.0.0.1:你的代理端口地址 & set https_proxy=http://127.0.0.1:你的代理端口地址
```

代理端口需自行获取。

#### 一、下载项目至本地

- git clone项目到本地

```bash
git clone --recursive https://github.com/FunAudioLLM/CosyVoice.git
# If you failed to clone submodule due to network failures, please run following command until success
cd CosyVoice
git submodule update --init --recursive
```

> PS：国内用户如果克隆失败，可以多尝试几次。有魔法的话，建议开魔法克隆。

- 创建conda环境
- 在当前文件夹输入cmd，打开命令行窗口
- 输入以下命令创建并启动[虚拟环境](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83&zhida_source=entity)

```bash
conda create -n cosyvoice python=3.8 
conda activate cosyvoice
```

#### 二、下载安装第三方依赖库

- 安装前需先修改文件夹中requirements.txt内容修改前：[onnxruntime-gpu](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=onnxruntime-gpu&zhida_source=entity)==1.16.0; sys\_platform == 'linux' onnxruntime==1.16.0; sys\_platform == 'darwin' or sys\_platform == 'windows'
  修改后：onnxruntime==1.16.0
- 执行安装命令

```bash
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host=mirrors.aliyun.com
```

上边为官方推荐[镜像](https://zhida.zhihu.com/search?content_id=247682038&content_type=Article&match_order=1&q=%E9%95%9C%E5%83%8F&zhida_source=entity)，速度较慢，推荐使用下方镜像。

```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

- 手动安装torch

> 安装过程中torch若下载过慢，可以手动下载该文件后，重新激活虚拟环境，手动安装该库。

手动下载该文件(可用浏览器、IDM或迅雷下载)，文件地址：[https://**download.pytorch.org/wh**l/cu118/torch-2.0.1%2Bcu118-cp38-cp38-win\_amd64.whl](https://link.zhihu.com/?target=https%3A//download.pytorch.org/whl/cu118/torch-2.0.1%252Bcu118-cp38-cp38-win_amd64.whl)

重新激活虚拟环境，运行手动安装指令：指令格式为
pip install 下载文件的完整路径 -i [https://**pypi.tuna.tsinghua.edu.cn**/simple](https://link.zhihu.com/?target=https%3A//pypi.tuna.tsinghua.edu.cn/simple)
例如：

```bash
pip install D:\AI\torch-2.0.1+cu118-cp38-cp38-win_amd64.whl -i https://pypi.tuna.tsinghua.edu.cn/simple
```

- 重新执行安装三方库直至全部安装完成

```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

- 可能出现的error

> cython 安装失败

![](https://pic.imgdb.cn/item/67682272d0e0a243d4e817e8.jpg)

解决办法：手动安装

```bash
pip install cython -i https://pypi.tuna.tsinghua.edu.cn/simple
```

> 各种情况导致的“Failed to build pynini”，pynini安装失败

![](https://pic.imgdb.cn/item/676822dfd0e0a243d4e81808.jpg)

解决办法：conda手动安装

```bash
conda install -c conda-forge pynini=2.1.5
```

#### 三、下载模型

直接上官方推荐安装
强烈建议您下载预训练模型和资源。`CosyVoice2-0.5BCosyVoice-300M``CosyVoice-300M-SFTCosyVoice-300M-Instruct``CosyVoice-ttsfrd`

如果你是这个领域的专家，并且只对从头开始训练自己的 CosyVoice 模型感兴趣，你可以跳过这一步。

```bash
# SDK模型下载
from modelscope import snapshot_download
snapshot_download('iic/CosyVoice2-0.5B', local_dir='pretrained_models/CosyVoice2-0.5B')
snapshot_download('iic/CosyVoice-300M', local_dir='pretrained_models/CosyVoice-300M')
snapshot_download('iic/CosyVoice-300M-25Hz', local_dir='pretrained_models/CosyVoice-300M-25Hz')
snapshot_download('iic/CosyVoice-300M-SFT', local_dir='pretrained_models/CosyVoice-300M-SFT')
snapshot_download('iic/CosyVoice-300M-Instruct', local_dir='pretrained_models/CosyVoice-300M-Instruct')
snapshot_download('iic/CosyVoice-ttsfrd', local_dir='pretrained_models/CosyVoice-ttsfrd')
```

```shell
# git模型下载，请确保已安装git lfs
mkdir -p pretrained_models
git clone https://www.modelscope.cn/iic/CosyVoice2-0.5B.git pretrained_models/CosyVoice2-0.5B
git clone https://www.modelscope.cn/iic/CosyVoice-300M.git pretrained_models/CosyVoice-300M
git clone https://www.modelscope.cn/iic/CosyVoice-300M-25Hz.git pretrained_models/CosyVoice-300M-25Hz
git clone https://www.modelscope.cn/iic/CosyVoice-300M-SFT.git pretrained_models/CosyVoice-300M-SFT
git clone https://www.modelscope.cn/iic/CosyVoice-300M-Instruct.git pretrained_models/CosyVoice-300M-Instruct
git clone https://www.modelscope.cn/iic/CosyVoice-ttsfrd.git pretrained_models/CosyVoice-ttsfrd
```

#### 四、运行模型

内置音色模型启动(命令行)

```bash
conda activate cosyvoice 
python webui.py --port 50000 --model_dir pretrained_models/CosyVoice-300M-SFT 
start http://127.0.0.1:50000
```

克隆音色+跨语种克隆模型启动(命令行)

```bash
conda activate cosyvoice 
python webui.py --port 50001 --model_dir pretrained_models/CosyVoice-300M 
start http://127.0.0.1:50001
```

## 总结

语音合成模型市面上现在太多太卷了，我就总结一下大概有哪些吧：

1. **CosyVoice**：由阿里巴巴通义实验室提出的多语言语音合成模型，通过使用语言模型和流匹配进行渐进式语义解码，在语音语境学习中实现了较高的韵律自然度、内容一致性和说话人相似性。
2. **WaveNet**：谷歌提出的模型，引入了生成式神经网络，可直接生成原始波形数据，生成的语音质量非常接近真实语音。
3. **Tacotron**：一种端到端的TTS系统，能够从文本直接生成语音，不需要传统的特征提取步骤。
4. **FastSpeech**：通过引入非自回归结构，提高了语音生成的速度和稳定性。
5. **VITS**：结合了GPT模型的VITS，提供更自然的语音合成效果。
6. **MQTTS**：一个基于TTS的语音合成系统，支持多种语言和语音风格。
7. **GPT Fast**：这个项目专注于提高GPT模型的推理速度，以实现更快的语音处理。
8. **Fish Speech**：一个开源的AI语音合成项目，只需要10～30秒声音就能合成出以假乱真的声音，让语音合成变得简单。
9. **Seed-TTS**：能够生成高度多样化和富有表现力的语音，适用于有声读物、虚拟助手、视频配音等多个应用场景。
10. **GPT-SoVITS**：开源AI语音克隆工具，结合GPT和SoVITS技术，实现智能语音合成的新境界。
11. **OpenVoice**：能对声音风格的精细控制，包括情感、口音、节奏、停顿和语调，同时能够复制参考发言者的音色。
12. **Parler-TTS**：一个完全开源的高质量AI语音生成项目，可以以特定说话者的风格生成高质量、自然听起来的语音。
13. **VoiceCraft**：支持克隆语音及修改音频文本的语音模型，具有强大的音频克隆能力和编辑功能。
14. **MetaVoice-1B**：一个强大的1.2亿参数的文本转语音TTS模型，训练在10万小时的语音数据上，专注于情感丰富、节奏自然和音调准确的英语发音。
15. **Voice Engine**：OpenAI公布的AI语音合成和声音克隆技术，能够利用简短的15秒音频样本和文本输入，生成接近原声的自然听起来的语音。