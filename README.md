# ko-nanochat

한국어로 바꾸고, 한국어로 설명하기.

## Audio 브리핑
- https://github.com/Laeyoung/ko-nanochat/blob/ko/nanochat_audio.mp3

## DeepWiki
- https://deepwiki.com/karpathy/nanochat
- https://deepwiki.com/search/-llm_dafaf702-e4b6-4ce8-9e29-4835068ab8bc



---

![nanochat logo](dev/nanochat.png)

> 100달러로 살 수 있는 최고의 ChatGPT.

이 저장소는 단일, 깔끔하고, 최소한의, 해킹 가능하며, 의존성이 적은 코드베이스에서 ChatGPT와 같은 LLM의 풀스택 구현입니다. nanochat은 [speedrun.sh](speedrun.sh)와 같은 스크립트를 통해 단일 8XH100 노드에서 실행되도록 설계되어, 전체 파이프라인을 처음부터 끝까지 실행합니다. 여기에는 토큰화, 사전 학습, 파인튜닝, 평가, 추론 및 간단한 UI를 통한 웹 서빙이 포함되어, 자신만의 LLM과 ChatGPT처럼 대화할 수 있습니다. nanochat은 Eureka Labs에서 개발 중인 LLM101n 과정의 최종 프로젝트가 될 것입니다.

## 대화하기

이 저장소의 최종 목표를 이해하기 위해, 현재 [nanochat.karpathy.ai](https://nanochat.karpathy.ai/)에서 호스팅되는 [nanochat d32](https://github.com/karpathy/nanochat/discussions/8)를 확인할 수 있습니다. "d32"는 이 모델이 Transformer 신경망에서 32개의 레이어를 가짐을 의미합니다. 이 모델은 19억 개의 매개변수를 가지며, 단일 스크립트 [run1000.sh](run1000.sh)를 실행하여 380억 개의 토큰으로 학습되었고, 총 학습 비용은 약 $800(8XH100 GPU 노드에서 약 33시간 학습)이었습니다. 현재 이는 2019년 GPT-2를 능가하기에 충분하지만, GPT-5와 같은 현대적 대규모 언어 모델에는 크게 미치지 못합니다. 이러한 소형 모델과 대화할 때, 많은 실수를 하고 약간 순진하고 어리석으며 환각을 많이 생성하는 것을 볼 수 있습니다. 마치 어린아이와 같아서 다소 재미있습니다. 하지만 nanochat의 독특한 점은 완전히 여러분의 것이라는 것입니다 - 완전히 구성 가능하고, 조정 가능하며, 해킹 가능하고, 처음부터 끝까지 여러분에 의해 학습됩니다. 여러분만의 모델을 학습하고 대화하려면 다음을 살펴보세요...

## 빠른 시작

마법을 가장 빠르게 체험하는 방법은 $100 등급의 nanochat을 학습하고 추론하는 [speedrun.sh](speedrun.sh) 스크립트를 실행하는 것입니다. $24/시간의 8XH100 노드에서 총 실행 시간은 약 4시간입니다. 선호하는 공급자(예: 제가 사용하고 선호하는 [Lambda](https://lambda.ai/service/gpu-cloud))에서 새로운 8XH100 GPU 박스를 시작하고 학습 스크립트를 실행하세요:

```bash
bash speedrun.sh
```

또는 스크립트가 4시간 동안 실행되므로, 새로운 screen 세션 `speedrun` 내에서 다음과 같이 실행하고 (출력을 `speedrun.log`에도 기록하는 것) 선호합니다:

```bash
screen -L -Logfile speedrun.log -S speedrun bash speedrun.sh
```

익숙하지 않다면 [screen 치트시트](https://gist.github.com/jctosta/af918e1618682638aa82)를 참조하세요. screen 세션 내에서 진행 상황을 지켜보거나 `Ctrl-a d`로 분리한 후 `tail speedrun.log`로 진행 상황을 확인할 수 있습니다. 이제 4시간을 기다립니다. 완료되면 ChatGPT와 유사한 웹 UI를 통해 LLM과 대화할 수 있습니다. 로컬 uv 가상 환경이 활성화되어 있는지 다시 확인하고(`source .venv/bin/activate` 실행) 서빙을 시작하세요:

```bash
python -m scripts.chat_web
```

그런 다음 표시된 URL을 방문하세요. 올바르게 접근해야 합니다. 예를 들어 Lambda에서는 사용 중인 노드의 공인 IP 뒤에 포트를 붙여 [http://209.20.xxx.xxx:8000/](http://209.20.xxx.xxx:8000/) 등으로 접속하세요. 그런 다음 평소 ChatGPT와 대화하듯이 LLM과 대화하세요! 이야기나 시를 쓰게 하세요. 환각을 보기 위해 당신이 누구인지 물어보세요. 하늘이 왜 파란지, 또는 왜 녹색인지 물어보세요. 스피드런은 4e19 FLOPs 능력을 가진 모델이므로 유치원생과 대화하는 것과 비슷합니다 :).

---

<img width="2672" height="1520" alt="image" src="https://github.com/user-attachments/assets/ed39ddf8-2370-437a-bedc-0f39781e76b5" />

---

프로젝트 디렉토리에 생성된 `report.md` 파일을 `cat`으로 확인할 수도 있으며, 여기에는 실행의 "성적표"인 다양한 평가와 메트릭이 포함되어 있습니다. 가장 마지막에는 다음과 같은 요약 테이블을 볼 수 있습니다:

---

- 문자: 333,989
- 줄: 8,304
- 파일: 44
- 토큰 (대략): 83,497
- 의존성 (uv.lock 줄): 2,004

| Metric          | BASE     | MID      | SFT      | RL       |
|-----------------|----------|----------|----------|----------|
| CORE            | 0.2219   | -        | -        | -        |
| ARC-Challenge   | -        | 0.2875   | 0.2807   | -        |
| ARC-Easy        | -        | 0.3561   | 0.3876   | -        |
| GSM8K           | -        | 0.0250   | 0.0455   | 0.0758   |
| HumanEval       | -        | 0.0671   | 0.0854   | -        |
| MMLU            | -        | 0.3111   | 0.3151   | -        |
| ChatCORE        | -        | 0.0730   | 0.0884   | -        |

총 경과 시간: 3시간 51분

---

(기본적으로 RL 수치가 표에 누락될 수 있습니다). 스피드런 스크립트와 확인해야 할 사항, 예상 결과에 대한 더 자세한 정보는 저장소 토론에 게시한 워크스루를 참조하세요: ["nanochat 소개: 100달러로 살 수 있는 최고의 ChatGPT"](https://github.com/karpathy/nanochat/discussions/1).

## 더 큰 모델

당연하게도 100달러로 고성능 ChatGPT 복제본을 학습시키기에는 충분하지 않습니다. 사실 LLM은 수백만 달러에 달하는 자본 지출로 유명합니다. 우리의 목적에는 두 가지 규모가 더 관심 대상이라고 생각합니다. 첫 번째는 약 12시간 동안 학습하는 ~300달러 등급 d26 모델(즉, depth=26)로, GPT-2 CORE 점수를 약간 상회합니다. 두 번째는 1000달러 등급(~41.6시간)으로, 깔끔한 반올림 숫자이기 때문입니다. 하지만 이 두 가지 모두 아직 완전히 지원되지 않으므로 마스터 브랜치에 아직 첨부되어 있지 않습니다.

그렇지만 감을 잡을 수 있도록, GPT-2 등급 모델 d26을 학습시키기 위해 [speedrun.sh](speedrun.sh) 파일에 필요한 예시 변경 사항은 세 가지 변경만 포함합니다:

```bash
...
# you'll need to download more data shards for pretraining
# get the number of parameters, multiply 20 to get tokens, multiply by 4.8 to get chars,
# divide by 250 million to get number of shards. todo need to improve this...
python -m nanochat.dataset -n 450 &
...
# use --depth to increase model size. to not oom, halve device batch size 32 -> 16:
torchrun --standalone --nproc_per_node=8 -m scripts.base_train -- --depth=26 --device_batch_size=16
...
# make sure to use the same later during midtraining:
torchrun --standalone --nproc_per_node=8 -m scripts.mid_train -- --device_batch_size=16
```

이게 전부입니다! 가장 주의해야 할 점은 충분한 데이터 샤드를 확보하는 것(그렇지 않으면 코드가 동일한 훈련 세트에 대해 더 많은 에포크를 반복하며 학습 속도가 약간 감소함)과, 주로 `device_batch_size`를 줄여 메모리/VRAM을 관리하는 것(스크립트가 자동으로 그래디언트 누적 루프 수를 증가시켜 병렬 계산을 순차 계산으로 전환함)입니다.

그리고 nanochat을 실행할 컴퓨팅 환경에 대해 조금 더 설명하자면:

- 코드는 Ampere 8XA100 GPU 노드에서도 잘 실행되지만, 약간 더 느리게 동작합니다.
- 모든 코드는 `torchrun`을 생략하면 단일 GPU에서도 원활히 실행되며 거의 동일한 결과를 생성합니다(코드가 자동으로 그래디언트 누적 방식으로 전환됨). 하지만 8배 더 오래 기다려야 합니다.
- GPU 메모리가 80GB 미만인 경우, 일부 하이퍼파라미터를 조정하지 않으면 OOM(메모리 부족) 또는 VRAM 고갈이 발생할 수 있습니다. 스크립트에서 `--device_batch_size`를 찾아 메모리에 맞을 때까지 값을 줄이세요. 예를 들어 기본값 32에서 16, 8, 4, 2, 심지어 1까지 낮출 수 있습니다. 그 이하로 설정하려면 더 많은 지식과 창의적인 접근이 필요합니다.
- 대부분의 코드는 상당히 표준적인 PyTorch로 작성되어 xpu, mps 등 PyTorch를 지원하는 모든 환경에서 실행 가능합니다. 하지만 즉시 사용할 수 있도록 구현하지는 않아서 약간의 수정이 필요할 수 있습니다.

## CPU / MPS에서 실행하기

nanochat은 CPU 또는 MPS(맥북 사용 시)에서 실행할 수 있으며, 자동으로 최적의 실행 장치를 감지합니다. GPU 없이는 큰 성과를 기대하기 어렵지만, 최소한 코드 경로는 실행할 수 있고 인내심을 갖고 작은 LLM을 훈련시킬 수는 있습니다. 모든 실행 명령어를 훨씬 더 작게 만드는 방법의 예시는 [dev/runcpu.sh](dev/runcpu.sh) 파일을 참조하세요. 모든 스크립트를 더 작은 모델 훈련으로 제한하고, 더 적은 반복 횟수로 실행하는 등의 설정을 확인할 수 있습니다. 이 기능은 새롭고 약간 복잡하며(많은 코드를 수정함), 2025년 10월 21일에 [CPU|MPS PR](https://github.com/karpathy/nanochat/pull/88)로 병합되었습니다.

## 사용자 정의

nanochat을 사용자 정의하려면 Discussions의 [가이드: 나노챗에 개성 주입하기](https://github.com/karpathy/nanochat/discussions/139)를 참조하세요. 이 가이드는 합성 데이터 생성과 해당 데이터를 중간 훈련 및 SFT 단계에 혼합하여 나노챗의 개성을 조정하는 방법을 설명합니다.

## 질문

nanochat은 간결하고 직관적으로 설계되었습니다. 이렇게 함으로써 얻는 큰 장점은 모든 파일을 함께 패키징하여 선호하는 LLM에 복사해 붙여넣고 임의의 질문을 할 수 있다는 것입니다. 예를 들어, [files-to-prompt](https://github.com/simonw/files-to-prompt) 유틸리티를 사용하여 다음과 같이 저장소를 패키징하는 것을 선호합니다:

```bash
files-to-prompt . -e py -e md -e rs -e html -e toml -e sh --ignore "*target*" --cxml > packaged.txt
```

여기에는 모든 py, rs, html, toml, sh 파일이 포함되며, `rustbpe/target` 폴더는 제외되고 cxml 출력 형식을 선택합니다. 모든 내용은 `packaged.txt` 파일에 기록되며, 현재 약 330KB(즉, 최신 LLM의 ~10만 토큰 제한보다 훨씬 적음)이고 45개 파일에 약 8,000줄의 코드가 있습니다.

또는 이 저장소에 대해 질문할 때 Devin/Cognition의 [DeepWiki](https://deepwiki.com/) 사용을 권장합니다. 이 저장소의 URL에서 github.com를 deepwiki.com로 변경하기만 하면 바로 사용할 수 있습니다.

## 테스트

여기에 너무 많은 투자를 하지는 않았지만, 특히 토크나이저를 위한 일부 테스트가 존재합니다. 예를 들어 다음과 같이 실행하세요:

```bash
python -m pytest tests/test_rustbpe.py -v -s
```

## 기여하기

nanochat는 아직 완성되지 않았습니다. 목표는 1,000달러 미만의 예산으로 처음부터 끝까지 작업할 수 있는 마이크로 모델의 최신 기술 수준을 향상시키는 것입니다. 접근성은 전체 비용뿐만 아니라 인지적 복잡성에 관한 것입니다. nanochat는 포괄적으로 구성 가능한 LLM "프레임워크"가 아닙니다. 코드베이스에는 거대한 구성 객체, 모델 팩토리, 또는 if-then-else 괴물이 존재하지 않을 것입니다. 이는 처음부터 끝까지 실행되어 구체적인 ChatGPT 클론과 그 성적표를 생성하도록 설계된 단일적, 통합적, 최소한의, 가독성 높은, 수정 가능한, 최대한 포크하기 쉬운 "강력한 기준선" 코드베이스입니다.

nanochat 저장소와 그 이슈 및 PR을 관리하고 1차 방어선 역할을 수행할 "nanochat repo czar"를 찾고 있습니다. 작업 예시로는 간단한 수정사항(문서, 오타, 명확하고 단순한 버그 등) 병합, vibe coded PR 거부, Issues/PR 관리, 두 공식 지원 플랫폼(Linux/GPU 및 Macbook)에서 PR에 대한 간단한 "정상성 검사 테스트" 수행, 정보를 간략한 업데이트 및 하이라이트로 정리해 저에게 전달하는 것이 포함됩니다. Discord나 X 등 가장 편리한 방법으로 DM을 통해 소통하겠습니다. 저장소에 기여해 주신 대가로 감사의 글에 nanochat repo czar로 이름과 링크가 게시될 예정입니다. 이 직위는 자유의지에 기반하므로, 일정 기간 기여한 후 언제든 "사임"하셔도 전혀 문제없으며 도움에 미리 감사드립니다. 지원은 X를 통해 저에게 DM으로 부탁드립니다. 감사합니다!

## 감사의 말

- nanochat라는 이름은 제 이전 프로젝트 [nanoGPT](https://github.com/karpathy/nanoGPT)(사전 학습만 다룸)에서 유래되었습니다.
- nanochat는 또한 [modded-nanoGPT](https://github.com/KellerJordan/modded-nanogpt)에서 영감을 받았으며, 이는 nanoGPT 저장소를 명확한 지표와 리더보드로 게임화했고, 사전 학습을 위한 많은 아이디어와 일부 구현을 차용했습니다.
- [HuggingFace](https://huggingface.co/)에 fineweb과 smoltalk를 제공해 주셔서 감사합니다.
- 이 프로젝트 개발에 사용된 컴퓨팅 자원을 제공해 주신 [Lambda](https://lambda.ai/service/gpu-cloud)에 감사드립니다.
- 조언과 지도를 해주신 주요 LLM 전문가 🧙‍♂️ Alec Radford에게 감사드립니다.

## 인용

nanochat이 귀하의 연구에 도움이 되었다면 다음과 같이 인용해 주세요:

```bibtex
@misc{nanochat,
  author = {Andrej Karpathy},
  title = {nanochat: The best ChatGPT that $100 can buy},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/karpathy/nanochat}
}
```

## 라이선스

MIT
