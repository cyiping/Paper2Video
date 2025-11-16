# Paper2Video

<p align="right">
  <b>English</b> | <a href="./README-CN.md">简体中文</a>
</p>


<p align="center">
  <b>Paper2Video: Automatic Video Generation from Scientific Papers</b>
<br>
从学术论文自动生成演讲视频
</p>

<p align="center">
  <a href="https://zeyu-zhu.github.io/webpage/">Zeyu Zhu*</a>,
  <a href="https://qhlin.me/">Kevin Qinghong Lin*</a>,
  <a href="https://scholar.google.com/citations?user=h1-3lSoAAAAJ&hl=en">Mike Zheng Shou</a> <br>
  Show Lab, National University of Singapore
</p>


<p align="center">
  <a href="https://arxiv.org/abs/2510.05096">📄 Paper</a> &nbsp; | &nbsp;
  <a href="https://huggingface.co/papers/2510.05096">🤗 Daily Paper</a> &nbsp; | &nbsp;
  <a href="https://huggingface.co/datasets/ZaynZhu/Paper2Video">📊 Dataset</a> &nbsp; | &nbsp;
  <a href="https://showlab.github.io/Paper2Video/">🌐 Project Website</a> &nbsp; | &nbsp;
  <a href="https://x.com/KevinQHLin/status/1976105129146257542">💬 X (Twitter)</a>
</p>

- **Input:** a paper ➕ an image ➕ an audio
  
| Paper | Image | Audio |
|--------|--------|--------|
| <img src="https://github.com/showlab/Paper2Video/blob/page/assets/hinton/paper.png" width="180"/><br>[🔗 Paper link](https://arxiv.org/pdf/1509.01626) | <img src="https://github.com/showlab/Paper2Video/blob/page/assets/hinton/hinton_head.jpeg" width="180"/> <br>Hinton's photo| <img src="assets/sound.png" width="180"/><br>[🔗 Audio sample](https://github.com/showlab/Paper2Video/blob/page/assets/hinton/ref_audio_10.wav) |


- **Output:** a presentation video



https://github.com/user-attachments/assets/39221a9a-48cb-4e20-9d1c-080a5d8379c4




Check out more examples at [🌐 project page](https://showlab.github.io/Paper2Video/).

## 🔥 Update
**Any contributions are welcome!**
- [x] [2025.10.15] We update a new version without talking-head for fast generation!
- [x] [2025.10.11] Our work receives attention on [YC Hacker News](https://news.ycombinator.com/item?id=45553701).
- [x] [2025.10.9] Thanks AK for sharing our work on [Twitter](https://x.com/_akhaliq/status/1976099830004072849)!
- [x] [2025.10.9] Our work is reported by [Medium](https://medium.com/@dataism/how-ai-learned-to-make-scientific-videos-from-slides-to-a-talking-head-0d807e491b27).
- [x] [2025.10.8] Check out our demo video below!
- [x] [2025.10.7] We release the [arxiv paper](https://arxiv.org/abs/2510.05096).
- [x] [2025.10.6] We release the [code](https://github.com/showlab/Paper2Video) and [dataset](https://huggingface.co/datasets/ZaynZhu/Paper2Video).
- [x] [2025.9.28] Paper2Video has been accepted to the **Scaling Environments for Agents Workshop([SEA](https://sea-workshop.github.io/)) at NeurIPS 2025**.


https://github.com/user-attachments/assets/a655e3c7-9d76-4c48-b946-1068fdb6cdd9




---

### Table of Contents
- [🌟 Overview](#-overview)
- [🚀 Quick Start: PaperTalker](#-try-papertalker-for-your-paper-)
  - [1. Requirements](#1-requirements)
  - [2. Configure LLMs](#2-configure-llms)
  - [3. Inference](#3-inference)
- [📊 Evaluation: Paper2Video](#-evaluation-paper2video)
- [😼 Fun: Paper2Video for Paper2Video](#-fun-paper2video-for-paper2video)
- [🙏 Acknowledgements](#-acknowledgements)
- [📌 Citation](#-citation)

---

## 🌟 Overview
<p align="center">
  <img src="assets/teaser.png" alt="Overview" width="100%">
</p>

This work solves two core problems for academic presentations:

- **Left: How to create a presentation video from a paper?**  
  *PaperTalker* — an agent that integrates **slides**, **subtitling**, **cursor grounding**, **speech synthesis**, and **talking-head video rendering**.

- **Right: How to evaluate a presentation video?**  
  *Paper2Video* — a benchmark with well-designed metrics to evaluate presentation quality.


---

## 🚀 Try PaperTalker for your Paper!
<p align="center">
  <img src="assets/method.png" alt="Approach" width="100%">
</p>

### 1. Requirements
Prepare the environment:
```bash
cd src
conda create -n p2v python=3.10
conda activate p2v
pip install -r requirements.txt
conda install -c conda-forge tectonic
```
**[Optional] [Skip](#2-configure-llms) this part if you do not need a human presenter.**

Download the dependent code and follow the instructions in **[Hallo2](https://github.com/fudan-generative-vision/hallo2)** to download the model weight.
```bash
git clone https://github.com/fudan-generative-vision/hallo2.git
```
You need to **prepare the environment separately for talking-head generation** to potential avoide package conflicts, please refer to  <a href="git clone https://github.com/fudan-generative-vision/hallo2.git">Hallo2</a>. After installing, use `which python` to get the python environment path.
```bash
cd hallo2
conda create -n hallo python=3.10
conda activate hallo
pip install -r requirements.txt
```

### 2. Configure LLMs
Export your **API credentials**:
```bash
export GEMINI_API_KEY="your_gemini_key_here"
export OPENAI_API_KEY="your_openai_key_here"
```
The best practice is to use **GPT4.1** or **Gemini2.5-Pro** for both LLM and VLMs. We also support locally deployed open-source model(e.g., Qwen), details please referring to <a href="https://github.com/Paper2Poster/Paper2Poster.git">Paper2Poster</a>.

### 3. Inference
The script `pipeline.py` provides an automated pipeline for generating academic presentation videos. It takes **LaTeX paper sources** together with **reference image/audio** as input, and goes through multiple sub-modules (Slides → Subtitles → Speech → Cursor → Talking Head) to produce a complete presentation video. ⚡ The minimum recommended GPU for running this pipeline is **NVIDIA A6000** with 48G.

#### Example Usage
Run the following command to launch a fast generation (**without talking-head generation**):
```bash
python pipeline_light.py \
    --model_name_t gpt-4.1 \
    --model_name_v gpt-4.1 \
    --result_dir /path/to/output \
    --paper_latex_root /path/to/latex_proj \
    --ref_img /path/to/ref_img.png \
    --ref_audio /path/to/ref_audio.wav \
    --gpu_list [0,1,2,3,4,5,6,7]
```

Run the following command to launch a full generation (**with talking-head generation**):

```bash
python pipeline.py \
    --model_name_t gpt-4.1 \
    --model_name_v gpt-4.1 \
    --model_name_talking hallo2 \
    --result_dir /path/to/output \
    --paper_latex_root /path/to/latex_proj \
    --ref_img /path/to/ref_img.png \
    --ref_audio /path/to/ref_audio.wav \
    --talking_head_env /path/to/hallo2_env \
    --gpu_list [0,1,2,3,4,5,6,7]
```

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `--model_name_t` | `str` | `gpt-4.1` | LLM |
| `--model_name_v` | `str` | `gpt-4.1` | VLM |
| `--model_name_talking` | `str` | `hallo2` | Talking Head model. Currently only **hallo2** is supported |
| `--result_dir` | `str` | `/path/to/output` | Output directory (slides, subtitles, videos, etc.) |
| `--paper_latex_root` | `str` | `/path/to/latex_proj` | Root directory of the LaTeX paper project |
| `--ref_img` | `str` | `/path/to/ref_img.png` | Reference image (must be **square** portrait) |
| `--ref_audio` | `str` | `/path/to/ref_audio.wav` | Reference audio (recommended: ~10s) |
| `--ref_text` | `str` | `None` | Optional reference text (for style guidance for subtitles) |
| `--beamer_templete_prompt` | `str` | `None` | Optional reference text (for style guidance for slides) |
| `--gpu_list` | `list[int]` | `""` | GPU list for parallel execution (used in **cursor generation** and **Talking Head rendering**) |
| `--if_tree_search` | `bool` | `True` | Whether to enable tree search for slide layout refinement |
| `--stage` | `str` | `"[0]"` | Pipeline stages to run (e.g., `[0]` full pipeline, `[1,2,3]` partial stages) |
| `--talking_head_env` | `str` | `/path/to/hallo2_env` | python environment path for talking-head generation |
---

## 📊 Evaluation: Paper2Video
<p align="center">
  <img src="assets/metrics.png" alt="Metrics" width="100%">
</p>

Unlike natural video generation, academic presentation videos serve a highly specialized role: they are not merely about visual fidelity but about **communicating scholarship**. This makes it difficult to directly apply conventional metrics from video synthesis(e.g., FVD, IS, or CLIP-based similarity). Instead, their value lies in how well they **disseminate research** and **amplify scholarly visibility**.From this perspective, we argue that a high-quality academic presentation video should be judged along two complementary dimensions:
#### For the Audience
- The video is expected to **faithfully convey the paper’s core ideas**.  
- It should remain **accessible to diverse audiences**.  

#### For the Author
- The video should **foreground the authors’ intellectual contribution and identity**.  
- It should **enhance the work’s visibility and impact**.  

To capture these goals, we introduce evaluation metrics specifically designed for academic presentation videos: Meta Similarity, PresentArena, PresentQuiz, IP Memory.

### Run Eval
- Prepare the environment:
```bash
cd src/evaluation
conda create -n p2v_e python=3.10
conda activate p2v_e
pip install -r requirements.txt
```
- For MetaSimilarity and PresentArena:
```bash
python MetaSim_audio.py --r /path/to/result_dir --g /path/to/gt_dir --s /path/to/save_dir
python MetaSim_content.py --r /path/to/result_dir --g /path/to/gt_dir --s /path/to/save_dir
```
```bash
python PresentArena.py --r /path/to/result_dir --g /path/to/gt_dir --s /path/to/save_dir
```
- For **PresentQuiz**, first generate questions from paper and eval using Gemini:
```bash
cd PresentQuiz
python create_paper_questions.py ----paper_folder /path/to/data
python PresentQuiz.py --r /path/to/result_dir --g /path/to/gt_dir --s /path/to/save_dir
```

- For **IP Memory**, first generate question pairs from generated videos and eval using Gemini:
```bash
cd IPMemory
python construct.py
python ip_qa.py
```
See the codes for more details!

👉 Paper2Video Benchmark is available at:
[HuggingFace](https://huggingface.co/datasets/ZaynZhu/Paper2Video)

---

## 😼 Fun: Paper2Video for Paper2Video
Check out **How Paper2Video for Paper2Video**:

https://github.com/user-attachments/assets/ff58f4d8-8376-4e12-b967-711118adf3c4

## 🙏 Acknowledgements

* The souces of the presentation videos are SlideLive and YouTuBe.
* We thank all the authors who spend a great effort to create presentation videos!
* We thank [CAMEL](https://github.com/camel-ai/camel) for open-source well-organized multi-agent framework codebase.
* We thank the authors of [Hallo2](https://github.com/fudan-generative-vision/hallo2.git) and [Paper2Poster](https://github.com/Paper2Poster/Paper2Poster.git) for their open-sourced codes.
* We thank [Wei Jia](https://github.com/weeadd) for his effort in collecting the data and implementing the baselines. We also thank all the participants involved in the human studies.
* We thank all the **Show Lab @ NUS** members for support!



---

## 📌 Citation


If you find our work useful, please cite:

```bibtex
@misc{paper2video,
      title={Paper2Video: Automatic Video Generation from Scientific Papers}, 
      author={Zeyu Zhu and Kevin Qinghong Lin and Mike Zheng Shou},
      year={2025},
      eprint={2510.05096},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2510.05096}, 
}
```
[![Star History](https://api.star-history.com/svg?repos=showlab/Paper2Video&type=Date)](https://star-history.com/#showlab/Paper2Video&Date)

---

專案簡介
Paper2Video（又稱 PaperTalker）是一個突破性的開源專案，它能將學術研究論文自動轉換成專業的簡報影片。你只需要提供三個素材：一份 LaTeX 格式的論文、一張作者頭像照片，以及一段 10 秒的語音樣本，系統就會自動產生一支包含投影片、語音旁白、字幕、游標動畫和虛擬講者的完整學術簡報影片。

這個專案來自新加坡國立大學 Show Lab 團隊，已被 NeurIPS 2025 Workshop 接受，並在 HuggingFace Daily Paper 上獲得關注。它不僅是一個工具，更建立了全球第一個學術簡報影片生成的評估基準（benchmark），為 AI 輔助學術交流開創了新的方向。

核心功能概念圖
專案展示的核心功能流程：

系統架構圖

專案核心問題與解決方案：

問題概覽

這個專案要解決哪些問題？
人工製作耗時費力：傳統製作一支 2-10 分鐘的學術簡報影片需要花費數小時，包含投影片設計、字幕撰寫、逐頁錄音、精細剪輯等繁瑣步驟
多模態長文理解困難：學術論文通常長達數十頁，包含大量文字、數學公式、圖表等複雜內容，現有 AI 模型難以完整理解
多通道同步生成挑戰：需要同時生成並對齊投影片、字幕、語音、游標動畫、虛擬講者等多個視覺與聽覺元素
影片長度限制：多數影片生成模型只能產生 10 秒以下的短片，無法滿足學術簡報需求（通常 5-10 分鐘）
缺乏評估標準：學術領域沒有現成的基準來評估自動生成簡報影片的品質與學術傳播效果
客製化需求：需要保留講者的個人身分特徵（臉部、聲音），以提升學術能見度和記憶點
適合哪些使用者？
學術研究人員：需要為會議投稿、論文發表製作簡報影片的科學家和學者
研究生與博士生：準備論文答辯、研討會報告的學生族群
會議主辦單位：需要大量製作講者簡報影片的學術會議籌備團隊
研究機構：希望推廣研究成果、提升學術影響力的大學和實驗室
線上教育工作者：將研究論文轉化為教學內容的教師與課程設計者
科普創作者：想要快速理解並傳播學術研究的科學傳播者
使用情境與案例
會議投稿視訊：許多國際會議（如 NeurIPS、CVPR）要求提交論文簡報影片，使用 Paper2Video 可在 48 分鐘內自動生成符合要求的影片
研究成果推廣：將已發表的論文轉換成易於分享的影片格式，上傳到 YouTube 或社群媒體，擴大研究影響力
線上學術會議：在疫情後的線上會議時代，預錄簡報影片成為常態，本工具可大幅降低製作門檻
論文答辯準備：研究生可用此工具快速產生論文答辯的初版簡報，再根據需求調整
學術社群互動：在 Twitter/X、LinkedIn 等平台分享論文簡報影片，增加學術能見度與引用率
時間壓縮場景：當需要在短時間內產生多篇論文的簡報影片時（如實驗室年度成果展），本工具可節省 6 倍以上的人力時間
實際案例：專案團隊展示了用 Paper2Video 生成 Paper2Video 自己論文的簡報影片，展現自我應用（self-hosting）的能力
使用了哪些技術？
大型語言模型（LLM）：GPT-4.1（OpenAI）、Gemini-2.5-Flash/Pro（Google）、Qwen7B，用於投影片內容生成與文字處理
視覺語言模型（VLM）：Gemini-2.5、GPT-4.1，負責分析投影片影像與版面評估
多智能體框架：CAMEL 框架，協調多個 AI 代理協作完成複雜任務
語音合成（TTS）：F5-TTS，支援語音克隆（voice cloning），可從 10 秒音訊樣本複製講者聲音
語音對齊：WhisperX，提供詞級（word-level）時間戳記對齊
虛擬講者生成：Hallo2（主要）、FantasyTalking（備選），生成同步嘴形的虛擬講者影片
GUI 定位模型：UI-TARS-1.5-7B（ByteDance-Seed）、ShowUI，用於游標位置的視覺定位
文件處理：Docling，支援 PDF、Word、PowerPoint、Excel、HTML、Markdown 等多種格式轉換
LaTeX/Beamer：用於生成學術風格的投影片程式碼
影像與影片處理：OpenCV、pdf2image、PIL/Pillow、PyMuPDF、FFmpeg
程式語言：Python 3.10（主要開發語言）、Bash（影片合併腳本）
深度學習框架：PyTorch（CUDA 加速）
平行運算：multiprocessing、asyncio，支援多 GPU 平行處理




