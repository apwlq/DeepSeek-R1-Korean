# DeepSeek-R1
<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<div align="center">
  <img src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/logo.svg?raw=true" width="60%" alt="DeepSeek-V3" />
</div>
<hr>
<div align="center" style="line-height: 1;">
  <a href="https://www.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Homepage" src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/badge.svg?raw=true" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://chat.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Chat" src="https://img.shields.io/badge/🤖%20Chat-DeepSeek%20R1-536af5?color=536af5&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://huggingface.co/deepseek-ai" target="_blank" style="margin: 2px;">
    <img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-DeepSeek%20AI-ffc107?color=ffc107&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://discord.gg/Tc7c45Zzu5" target="_blank" style="margin: 2px;">
    <img alt="Discord" src="https://img.shields.io/badge/Discord-DeepSeek%20AI-7289da?logo=discord&logoColor=white&color=7289da" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/qr.jpeg?raw=true" target="_blank" style="margin: 2px;">
    <img alt="Wechat" src="https://img.shields.io/badge/WeChat-DeepSeek%20AI-brightgreen?logo=wechat&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://twitter.com/deepseek_ai" target="_blank" style="margin: 2px;">
    <img alt="Twitter Follow" src="https://img.shields.io/badge/Twitter-deepseek_ai-white?logo=x&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://github.com/deepseek-ai/DeepSeek-R1/blob/main/LICENSE" style="margin: 2px;">
    <img alt="License" src="https://img.shields.io/badge/License-MIT-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>


<p align="center">
  <a href="https://github.com/deepseek-ai/DeepSeek-R1/blob/main/DeepSeek_R1.pdf"><b>Paper Link</b>👁️</a>
</p>


## 1. 소개

우리는 첫 번째 세대 추론 모델인 DeepSeek-R1-Zero와 DeepSeek-R1을 소개합니다.  
DeepSeek-R1-Zero는 감독된 미세 조정(SFT)을 사전 단계로 사용하지 않고 대규모 강화 학습(RL)을 통해 훈련된 모델로, 추론에서 뛰어난 성능을 보여주었습니다.  
RL을 통해 DeepSeek-R1-Zero는 수많은 강력하고 흥미로운 추론 행동을 자연스럽게 나타냈습니다.  
하지만 DeepSeek-R1-Zero는 끝없는 반복, 낮은 가독성, 언어 혼합 등의 문제에 직면합니다. 이러한 문제를 해결하고 추론 성능을 더욱 향상시키기 위해, 우리는 RL 이전에 콜드 스타트 데이터를 포함한 DeepSeek-R1을 소개합니다.  
DeepSeek-R1은 수학, 코드 및 추론 작업에서 OpenAI-o1과 비교할 수 있는 성능을 달성합니다.  
연구 커뮤니티를 지원하기 위해 우리는 DeepSeek-R1-Zero, DeepSeek-R1, 그리고 Llama와 Qwen을 기반으로 DeepSeek-R1에서 증류된 6개의 밀집 모델을 오픈 소스로 공개했습니다.  
DeepSeek-R1-Distill-Qwen-32B는 다양한 벤치마크에서 OpenAI-o1-mini를 능가하며, 밀집 모델에 대한 새로운 최첨단 결과를 달성했습니다.

**참고: DeepSeek-R1 시리즈 모델을 로컬에서 실행하기 전에, [사용 권장 사항](#usage-recommendations) 섹션을 검토하시기 바랍니다.**

<p align="center">
  <img width="80%" src="figures/benchmark.jpg">
</p>

## 2. 모델 요약

---

**사후 훈련: 기본 모델에서의 대규모 강화 학습**

- 우리는 감독 학습 세부 조정(SFT)을 사전 단계로 사용하지 않고 기본 모델에 직접적으로 강화 학습(RL)을 적용합니다. 이 접근법은 모델이 복잡한 문제를 해결하기 위해 사고의 연쇄(CoT)를 탐색할 수 있게 하여, DeepSeek-R1-Zero의 개발로 이어졌습니다. DeepSeek-R1-Zero는 자기 검증, 반영 및 긴 CoT 생성과 같은 기능을 보여주며, 연구 커뮤니티에 중요한 이정표를 제시합니다. 특히, 이는 LLM의 추론 능력을 SFT 없이 순수하게 RL을 통해 유도할 수 있다는 것을 입증한 최초의 공개 연구입니다. 이 혁신은 이 분야의 향후 발전 가능성을 열어줍니다.

- 우리는 DeepSeek-R1을 개발하기 위한 파이프라인을 소개합니다. 이 파이프라인은 향상된 추론 패턴을 발견하고 인간의 선호에 맞추기 위한 두 개의 RL 단계와 모델의 추론 및 비추론 능력을 위한 씨앗 역할을 하는 두 개의 SFT 단계를 포함합니다. 우리는 이 파이프라인이 더 나은 모델을 만드는 데 산업에 기여할 것이라고 믿습니다.

---

**증류: 작은 모델도 강력할 수 있다**

- 우리는 더 큰 모델의 추론 패턴을 작은 모델로 증류할 수 있다는 것을 보여주며, 그 결과 작은 모델에서 RL을 통해 발견된 추론 패턴보다 더 나은 성능을 보입니다. 공개된 DeepSeek-R1과 그 API는 연구 커뮤니티가 향후 더 나은 작은 모델을 증류하는 데 도움이 될 것입니다.
- DeepSeek-R1을 통해 생성된 추론 데이터를 사용하여, 연구 커뮤니티에서 널리 사용되는 여러 개의 밀집 모델을 세부 조정했습니다. 평가 결과, 증류된 작은 밀집 모델들이 벤치마크에서 뛰어난 성능을 보였습니다. 우리는 Qwen2.5와 Llama3 시리즈를 기반으로 한 증류된 1.5B, 7B, 8B, 14B, 32B, 70B 체크포인트를 커뮤니티에 오픈 소스로 제공합니다.

## 3. 모델 다운로드

### DeepSeek-R1 모델

<div align="center">

| **Model** | **#Total Params** | **#Activated Params** | **Context Length** | **Download** |
| :------------: | :------------: | :------------: | :------------: | :------------: |
| DeepSeek-R1-Zero | 671B | 37B | 128K   | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Zero)   |
| DeepSeek-R1   | 671B | 37B |  128K   | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1)   |

</div>

DeepSeek-R1-Zero & DeepSeek-R1은 DeepSeek-V3-Base를 기반으로 학습되었습니다.  
모델 아키텍처에 대한 자세한 내용은 [DeepSeek-V3](https://github.com/deepseek-ai/DeepSeek-V3) 리포지토리를 참조해주세요.

### DeepSeek-R1-Distill 모델들

<div align="center">

| **Model** | **Base Model** | **Download** |
| :------------: | :------------: | :------------: |
| DeepSeek-R1-Distill-Qwen-1.5B  | [Qwen2.5-Math-1.5B](https://huggingface.co/Qwen/Qwen2.5-Math-1.5B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B)   |
| DeepSeek-R1-Distill-Qwen-7B  | [Qwen2.5-Math-7B](https://huggingface.co/Qwen/Qwen2.5-Math-7B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B)   |
| DeepSeek-R1-Distill-Llama-8B  | [Llama-3.1-8B](https://huggingface.co/meta-llama/Llama-3.1-8B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-8B)   |
| DeepSeek-R1-Distill-Qwen-14B   | [Qwen2.5-14B](https://huggingface.co/Qwen/Qwen2.5-14B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-14B)   |
|DeepSeek-R1-Distill-Qwen-32B  | [Qwen2.5-32B](https://huggingface.co/Qwen/Qwen2.5-32B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B)   |
| DeepSeek-R1-Distill-Llama-70B  | [Llama-3.3-70B-Instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B)   |

</div>

DeepSeek-R1-Distill 모델은 DeepSeek-R1에서 생성된 샘플을 사용하여 오픈 소스 모델을 기반으로 미세 조정됩니다. 우리는 그들의 설정과 토크나이저를 약간 변경했습니다. 이 모델을 실행하려면 우리의 설정을 사용해 주세요.

## 4. 평가 결과

### DeepSeek-R1-평가
 모든 모델에 대해 최대 생성 길이는 32,768 토큰으로 설정됩니다. 샘플링을 요구하는 벤치마크의 경우, 우리는 온도 $0.6$, top-p 값 $0.95$를 사용하며, 쿼리당 64개의 응답을 생성하여 pass@1을 추정합니다.
<div align="center">


| Category | Benchmark (Metric) | Claude-3.5-Sonnet-1022 | GPT-4o 0513 | DeepSeek V3 | OpenAI o1-mini | OpenAI o1-1217 | DeepSeek R1 |
|----------|-------------------|----------------------|------------|--------------|----------------|------------|--------------|
| | Architecture | - | - | MoE | - | - | MoE |
| | # Activated Params | - | - | 37B | - | - | 37B |
| | # Total Params | - | - | 671B | - | - | 671B |
| English | MMLU (Pass@1) | 88.3 | 87.2 | 88.5 | 85.2 | **91.8** | 90.8 |
| | MMLU-Redux (EM) | 88.9 | 88.0 | 89.1 | 86.7 | - | **92.9** |
| | MMLU-Pro (EM) | 78.0 | 72.6 | 75.9 | 80.3 | - | **84.0** |
| | DROP (3-shot F1) | 88.3 | 83.7 | 91.6 | 83.9 | 90.2 | **92.2** |
| | IF-Eval (Prompt Strict) | **86.5** | 84.3 | 86.1 | 84.8 | - | 83.3 |
| | GPQA-Diamond (Pass@1) | 65.0 | 49.9 | 59.1 | 60.0 | **75.7** | 71.5 |
| | SimpleQA (Correct) | 28.4 | 38.2 | 24.9 | 7.0 | **47.0** | 30.1 |
| | FRAMES (Acc.) | 72.5 | 80.5 | 73.3 | 76.9 | - | **82.5** |
| | AlpacaEval2.0 (LC-winrate) | 52.0 | 51.1 | 70.0 | 57.8 | - | **87.6** |
| | ArenaHard (GPT-4-1106) | 85.2 | 80.4 | 85.5 | 92.0 | - | **92.3** |
| Code | LiveCodeBench (Pass@1-COT) | 33.8 | 34.2 | - | 53.8 | 63.4 | **65.9** |
| | Codeforces (Percentile) | 20.3 | 23.6 | 58.7 | 93.4 | **96.6** | 96.3 |
| | Codeforces (Rating) | 717 | 759 | 1134 | 1820 | **2061** | 2029 |
| | SWE Verified (Resolved) | **50.8** | 38.8 | 42.0 | 41.6 | 48.9 | 49.2 |
| | Aider-Polyglot (Acc.) | 45.3 | 16.0 | 49.6 | 32.9 | **61.7** | 53.3 |
| Math | AIME 2024 (Pass@1) | 16.0 | 9.3 | 39.2 | 63.6 | 79.2 | **79.8** |
| | MATH-500 (Pass@1) | 78.3 | 74.6 | 90.2 | 90.0 | 96.4 | **97.3** |
| | CNMO 2024 (Pass@1) | 13.1 | 10.8 | 43.2 | 67.6 | - | **78.8** |
| Chinese | CLUEWSC (EM) | 85.4 | 87.9 | 90.9 | 89.9 | - | **92.8** |
| | C-Eval (EM) | 76.7 | 76.0 | 86.5 | 68.9 | - | **91.8** |
| | C-SimpleQA (Correct) | 55.4 | 58.7 | **68.0** | 40.3 | - | 63.7 |

</div>


### Distilled Model Evaluation


<div align="center">

| Model                                    | AIME 2024 pass@1 | AIME 2024 cons@64 | MATH-500 pass@1 | GPQA Diamond pass@1 | LiveCodeBench pass@1 | CodeForces rating |
|------------------------------------------|------------------|-------------------|-----------------|----------------------|----------------------|-------------------|
| GPT-4o-0513                          | 9.3              | 13.4              | 74.6            | 49.9                 | 32.9                 | 759               |
| Claude-3.5-Sonnet-1022             | 16.0             | 26.7                 | 78.3            | 65.0                 | 38.9                 | 717               |
| o1-mini                              | 63.6             | 80.0              | 90.0            | 60.0                 | 53.8                 | **1820**          |
| QwQ-32B-Preview                              | 44.0             | 60.0                 | 90.6            | 54.5               | 41.9                 | 1316              |
| DeepSeek-R1-Distill-Qwen-1.5B       | 28.9             | 52.7              | 83.9            | 33.8                 | 16.9                 | 954               |
| DeepSeek-R1-Distill-Qwen-7B          | 55.5             | 83.3              | 92.8            | 49.1                 | 37.6                 | 1189              |
| DeepSeek-R1-Distill-Qwen-14B         | 69.7             | 80.0              | 93.9            | 59.1                 | 53.1                 | 1481              |
| DeepSeek-R1-Distill-Qwen-32B        | **72.6**         | 83.3              | 94.3            | 62.1                 | 57.2                 | 1691              |
| DeepSeek-R1-Distill-Llama-8B         | 50.4             | 80.0              | 89.1            | 49.0                 | 39.6                 | 1205              |
| DeepSeek-R1-Distill-Llama-70B        | 70.0             | **86.7**          | **94.5**        | **65.2**             | **57.5**             | 1633              |

</div>


## 5. 채팅 웹사이트 및 API 플랫폼
DeepSeek의 공식 웹사이트에서 DeepSeek-R1과 채팅할 수 있습니다: [chat.deepseek.com](https://chat.deepseek.com), 그리고 "DeepThink" 버튼을 켜세요.

우리는 또한 DeepSeek 플랫폼에서 OpenAI 호환 API를 제공합니다: [platform.deepseek.com](https://platform.deepseek.com/)

## 6. 로컬 실행 방법

### DeepSeek-R1 모델

DeepSeek-R1을 로컬에서 실행하는 방법에 대한 자세한 정보는 [DeepSeek-V3](https://github.com/deepseek-ai/DeepSeek-V3) 리포지토리를 참조하세요.

**참고: Hugging Face의 Transformers는 아직 직접적으로 지원되지 않습니다.**

### DeepSeek-R1-Distill 모델

DeepSeek-R1-Distill 모델은 Qwen 또는 Llama 모델과 같은 방식으로 활용할 수 있습니다.

예를 들어, [vLLM](https://github.com/vllm-project/vllm)을 사용하여 서비스를 쉽게 시작할 수 있습니다:

```shell
vllm serve deepseek-ai/DeepSeek-R1-Distill-Qwen-32B --tensor-parallel-size 2 --max-model-len 32768 --enforce-eager
```

또한 [SGLang](https://github.com/sgl-project/sglang)을 사용하여 서비스를 쉽게 시작할 수 있습니다:

```bash
python3 -m sglang.launch_server --model deepseek-ai/DeepSeek-R1-Distill-Qwen-32B --trust-remote-code --tp 2
```

### 사용 권장 사항

**DeepSeek-R1 시리즈 모델을 사용할 때, 벤치마킹을 포함한 예상 성능을 얻기 위해 다음 구성을 따르는 것을 권장합니다:**

1. 온도를 0.5-0.7 범위 내로 설정하세요 (0.6이 권장됨) – 이를 통해 끝없는 반복이나 일관성 없는 출력을 방지할 수 있습니다.
2. **시스템 프롬프트를 추가하지 마세요; 모든 지침은 사용자 프롬프트에 포함되어야 합니다.**
3. 수학 문제의 경우, 프롬프트에 "단계별로 추론하고, 최종 답을 \boxed{} 안에 넣으세요"와 같은 지시를 포함하는 것이 좋습니다.
4. 모델 성능을 평가할 때, 여러 번 테스트를 진행하고 결과를 평균내는 것이 좋습니다.

## 7. 라이선스
이 코드 리포지토리 및 모델 가중치는 [MIT 라이선스](https://github.com/deepseek-ai/DeepSeek-R1/blob/main/LICENSE) 하에 라이선스가 부여됩니다. DeepSeek-R1 시리즈는 상업적 사용을 지원하며, 다른 LLM을 훈련시키기 위한 증류를 포함한 수정 및 파생 작업을 허용합니다. 다음을 참고하세요:
- DeepSeek-R1-Distill-Qwen-1.5B, DeepSeek-R1-Distill-Qwen-7B, DeepSeek-R1-Distill-Qwen-14B, DeepSeek-R1-Distill-Qwen-32B는 [Qwen-2.5 시리즈](https://github.com/QwenLM/Qwen2.5)에서 파생된 것으로, 원래 [Apache 2.0 라이선스](https://huggingface.co/Qwen/Qwen2.5-1.5B/blob/main/LICENSE) 하에 라이선스가 부여되었으며, 현재 DeepSeek-R1로 800k 샘플을 사용하여 파인튜닝되었습니다.
- DeepSeek-R1-Distill-Llama-8B는 Llama3.1-8B-Base에서 파생되었으며, 원래 [llama3.1 라이선스](https://huggingface.co/meta-llama/Llama-3.1-8B/blob/main/LICENSE) 하에 라이선스가 부여되었습니다.
- DeepSeek-R1-Distill-Llama-70B는 Llama3.3-70B-Instruct에서 파생되었으며, 원래 [llama3.3 라이선스](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct/blob/main/LICENSE) 하에 라이선스가 부여되었습니다.

## 8. 인용
```
@misc{deepseekai2025deepseekr1incentivizingreasoningcapability,
      title={DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning}, 
      author={DeepSeek-AI and Daya Guo and Dejian Yang and Haowei Zhang and Junxiao Song and Ruoyu Zhang and Runxin Xu and Qihao Zhu and Shirong Ma and Peiyi Wang and Xiao Bi and Xiaokang Zhang and Xingkai Yu and Yu Wu and Z. F. Wu and Zhibin Gou and Zhihong Shao and Zhuoshu Li and Ziyi Gao and Aixin Liu and Bing Xue and Bingxuan Wang and Bochao Wu and Bei Feng and Chengda Lu and Chenggang Zhao and Chengqi Deng and Chenyu Zhang and Chong Ruan and Damai Dai and Deli Chen and Dongjie Ji and Erhang Li and Fangyun Lin and Fucong Dai and Fuli Luo and Guangbo Hao and Guanting Chen and Guowei Li and H. Zhang and Han Bao and Hanwei Xu and Haocheng Wang and Honghui Ding and Huajian Xin and Huazuo Gao and Hui Qu and Hui Li and Jianzhong Guo and Jiashi Li and Jiawei Wang and Jingchang Chen and Jingyang Yuan and Junjie Qiu and Junlong Li and J. L. Cai and Jiaqi Ni and Jian Liang and Jin Chen and Kai Dong and Kai Hu and Kaige Gao and Kang Guan and Kexin Huang and Kuai Yu and Lean Wang and Lecong Zhang and Liang Zhao and Litong Wang and Liyue Zhang and Lei Xu and Leyi Xia and Mingchuan Zhang and Minghua Zhang and Minghui Tang and Meng Li and Miaojun Wang and Mingming Li and Ning Tian and Panpan Huang and Peng Zhang and Qiancheng Wang and Qinyu Chen and Qiushi Du and Ruiqi Ge and Ruisong Zhang and Ruizhe Pan and Runji Wang and R. J. Chen and R. L. Jin and Ruyi Chen and Shanghao Lu and Shangyan Zhou and Shanhuang Chen and Shengfeng Ye and Shiyu Wang and Shuiping Yu and Shunfeng Zhou and Shuting Pan and S. S. Li and Shuang Zhou and Shaoqing Wu and Shengfeng Ye and Tao Yun and Tian Pei and Tianyu Sun and T. Wang and Wangding Zeng and Wanjia Zhao and Wen Liu and Wenfeng Liang and Wenjun Gao and Wenqin Yu and Wentao Zhang and W. L. Xiao and Wei An and Xiaodong Liu and Xiaohan Wang and Xiaokang Chen and Xiaotao Nie and Xin Cheng and Xin Liu and Xin Xie and Xingchao Liu and Xinyu Yang and Xinyuan Li and Xuecheng Su and Xuheng Lin and X. Q. Li and Xiangyue Jin and Xiaojin Shen and Xiaosha Chen and Xiaowen Sun and Xiaoxiang Wang and Xinnan Song and Xinyi Zhou and Xianzu Wang and Xinxia Shan and Y. K. Li and Y. Q. Wang and Y. X. Wei and Yang Zhang and Yanhong Xu and Yao Li and Yao Zhao and Yaofeng Sun and Yaohui Wang and Yi Yu and Yichao Zhang and Yifan Shi and Yiliang Xiong and Ying He and Yishi Piao and Yisong Wang and Yixuan Tan and Yiyang Ma and Yiyuan Liu and Yongqiang Guo and Yuan Ou and Yuduan Wang and Yue Gong and Yuheng Zou and Yujia He and Yunfan Xiong and Yuxiang Luo and Yuxiang You and Yuxuan Liu and Yuyang Zhou and Y. X. Zhu and Yanhong Xu and Yanping Huang and Yaohui Li and Yi Zheng and Yuchen Zhu and Yunxian Ma and Ying Tang and Yukun Zha and Yuting Yan and Z. Z. Ren and Zehui Ren and Zhangli Sha and Zhe Fu and Zhean Xu and Zhenda Xie and Zhengyan Zhang and Zhewen Hao and Zhicheng Ma and Zhigang Yan and Zhiyu Wu and Zihui Gu and Zijia Zhu and Zijun Liu and Zilin Li and Ziwei Xie and Ziyang Song and Zizheng Pan and Zhen Huang and Zhipeng Xu and Zhongyu Zhang and Zhen Zhang},
      year={2025},
      eprint={2501.12948},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2501.12948}, 
}

```

## 9. 연락
궁금한 점이 있으시면 문제를 제기하시거나 [service@deepseek.com](service@deepseek.com)으로 연락해 주세요.
