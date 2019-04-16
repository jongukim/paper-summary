# OpenAI's DOTA 2 agents

- OpenAI Blog
  - ["OpenAI Five Finals"](https://openai.com/blog/openai-five-finals/) (2019. 3. 26.)
  - ["How to train your OpenAI Five"](https://openai.com/blog/how-to-train-your-openai-five/) (2019. 4. 15.)
  - ["OpenAI Five"](https://openai.com/blog/openai-five/) (2018. 6. 25.)
  - ["OpenAI Five Benchmark: Results"](https://openai.com/blog/openai-five-benchmark-results/) (2018. 8. 6.)
  - ["The International 2018: Results"](https://openai.com/blog/the-international-2018-results/) (2018. 8. 23.)
  - ["More on Dota 2"](https://openai.com/blog/more-on-dota-2/) (2017. 8. 16.)
- 블로그의 모든 글은 "we're hiring" 아니면 "consider joining OpenAI"로 마무리

## OpenAI Five (2018. 6)

OpenAI가 Dota2 5 vs 5 게임에 도전한 것

- Solo play(1 on 1)에서는 2017년에 세계 정상급 선수에게 승리 (2017. 8.)
  - (7/8) 세미프로(7.5k MMR)에게 첫 승리
  - (8/7) Blitz (6.2k former pro) 3-0 승리, Pajkatt (8.5k pro) 2-1 승리, CC&C (8.9k pro) 3-0 승리
    - 모두가 Sumail라면 이길 것이라고 언급
  - (8/9) Arteezy (10k pro, top player) 10-0 승리
    - 역시 Sumail 추천
  - (8/10) **Sumail (8.3k pro, top 1v1 player) 6-0 승리**
    - 8/9일자 봇에게는 2-1 승리
  - (8/11) Dendi (7.3k pro, former world champion, old-school crowd favorite) 2-0 승리
    - 8/10일자 봇에게 60% win rate을 보이던 봇과 시합
- Dota 1 on 1에서 새로운 전략을 제시한 효과
  - 현재 1 on 1에서는 OAI bot이 사용한 전략이 정석이 되었다고 함

### Dota2는 어려운 문제

- The state of Dota game: [about 20,000 numbers](https://openai.com/content/images/2019/04/Dota-Matrix.png)
  - 체스는 대략 70, 바둑은 400
- 1,000개 정도의 가능한 action 존재
  - 체스는 대략 35, 바둑은 250
- Long-term으로 수립되는 전략이 다양
- 전체 map 상황을 모름
  - 역시 체스나 바둑과 다른 점
- 지속적 업데이트 (환경 변화)
- 높은 시뮬레이션 비용
  - 체스와 바둑에 비해 게임 자체가 복잡

### 학습 방법

- Self-play를 통해 학습 (zero-shot learning)
  - 사람 간 게임 replay는 젼혀 사용하지 않았음
- 학습 시간: 180년 (환산)
  - 각 히어로를 모두 고려한다면(x5) 900년
- 개별 hero가 LSTM 네트워크를 가지는 방식
  - [Model arch](https://d4mucfpksywv.cloudfront.net/research-covers/openai-five/network-architecture.pdf)
  - Hero 네트워크 간 연결은 없음
    - *Team spirit*이라는 hyper-parameter를 두어 <팀 평균 reward>와 <내 reward> 간의 가중치를 주는 정도로 설정
- 초기에는 모든 스킬을 다 사용하도록 학습하진 못했음

#### 대규모 학습

- 256 GPUs & 128,000 CPU cores (구글 클라우드)
- 256개의 rollout worker가 학습 후 gradient를 전달, 평균 내어 반영

### 학습 전개

- Random init으로 시작했으므로 당연히 초반에는 맵을 그냥 돌아다니는 등 초보적인 움직임을 보임
- (몇 시간 학습 한 후) 파밍도 하고 lane도 조금씩 이해
- (며칠 학습 후) 사람이 사용하는 전략을 조금씩 사용하기 시작, 이후 5개 히어로가 함께 공격하는 모습까지 보임
- (2017. 3) 학습 시 몇 가지 포인트를 randomize, 사람들을 이기기 시작
  - *World models*에서와 같이 randomness를 주어 문제를 조금 어렵게 만든 듯(?)

### 블로그 작성 시점의 수준

- 무엇을 먼저 해야 할지 판단하거나 파밍할 때를 보면 장기적인 전략에 대해 이해하고 있는 듯 보였음
- 1 on 1을 학습할 때는 필수 전술(ex. creep blocking)에 대해 reward를 줬으나, 이후 실험에는 주지 않았음에도 2 vs 2에서 학습이 되는 모습 확인
- 막타(last-hitting)에 약한 모습

### 사람과의 차이

- 사람은 상황 파악을 위해 화면 이곳저곳을 봐야 하지만 OAI Five는 즉시 파악 가능
  - 화면을 직접 보고 GPU를 통해 visually 분석하는 방식이 아님
  - API를 통해 상태값 직접 전달
- OpenAI의 평균 reaction time은 80ms로, 사람보다 빠름

## OpenAI Five at The International (2018. 8)

### 대회에 나가기 전 benchmark 수행

99.95th percentile Dota player 5명 대상으로 승리

- Game 1: 경기전 승률(히어로 선택 후) 예상 95%, 21분 37초 승
- Game 2: 경기전 승률 예상 76.2%, 24분 58초 승

게임 3은 OpenAI의 히어로를 상대방이 선택하도록 했고 (당연히) 아주 불리한 조합으로 경기

- Game 3: 경기전 승률 예상 2.9%, 경기중 최대 예상 승률 17%, 35분 47초 패

### 본 게임

- 프로와의 대결
- 히어로를 직접 선택하지 않고 third-party가 선택
- Single mortal courier를 사용하도록 룰 변경
  - 점차 [제한을 풀고 발전하는 agents](https://openai.com/content/images/2018/08/loss-1.svg)

#### 본 게임 1

- (대회에서 초반 탈락한 팀,) paiN Gaming과의 시합 ~~심심했던 듯~~
  - top 18 Dota 2 teams in the world
- 51분 패

#### 본 게임 2

- 중국 유명 선수들의 연합팀
- 45분 패

### 학습 시 특이사항

- 게임 버전이 바뀌면 학습을 처음부터 다시 했으나 transfer learning을 도입
- 모델 [변경](https://s3-us-west-2.amazonaws.com/openai-assets/dota_benchmark_results/network_diagram_08_06_2018.pdf)
- Training time: about 10,000 years over 1.5 realtime months

## OpenAI Five Finals

- 부들부들 OpenAI, [스파르타식 교육](https://twitter.com/openai/status/1037765547427954688?lang=en) 적용: 모델을 키우고 더 오래 학습
  - Training time: about 45,000 years of Dota self-play over 10 realtime months
- [효과 만점](https://openai.com/content/images/2019/04/compute_vs_ts_final_log_smoothed.svg)
- 최종 버전은 International 대회에서 사용한 에이전트에게 99.9% winrate를 보였음
- 그 사이 패치로 게임도 좀 변했으나 transfer learning으로 극복 ~~~말이 쉽지...~~~

### Cooperative mode

본 시합 이전에, [사람 2명과 agent 3개의 조합](https://openai.com/content/images/2019/04/coop-versus.png)으로 두 팀을 만들어 시합

- Agent가 협업하는 것을 느꼈다고 플레이어가 코멘트
- OpenAI는 미래의 human-AI interaction, collaboration에 대한 기대감 표시
  - 협업을 위한 학습을 하진 않았음
  - OpenAI 측에서도 놀랐다고 표현

### 시합

- vs OG (e-sports team)
  - Dota2 world champ
- 2-0 승 ~~Deepmind의 AlphaStar는 live pro match에서 털렸다고 깨알 디스~~

## 결론?

> Supervised deep learning systems can only be as good as their training datasets, but in self-play systems, the available data improves automatically as the agent gets better.

지도 학습은 데이터셋에 능력이 한정되지만 self-play system은 그렇지 않다.

> It actually felt nice; my Viper gave his life for me at some point. He tried to help me, thinking “I’m sure she knows what she’s doing” and then obviously I didn’t. But, you know, he believed in me. I don’t get that a lot with [human] teammates. — Sheever
> In fact, we’d considered doing a cooperative match at The International but assumed it’d require dedicated training.

이기기 위해서는 협동이 필요하다.

### 그리고 또 하나 생각해볼 것

사람들은 전문가 반열에 오르면 자신들이 그 분야에 대해 잘 알고 있다고 생각하지만, 과연 그럴까?

- Greg's [tweet](https://mobile.twitter.com/gdb/status/1117812923160621056)

### Plus

- 4월 18-21일에 인터넷에서 직접 플레이 하도록 할 예정
- 팀원이 되거나 적이 될 것
- 이 이벤트를 마지막으로 은퇴할 것이라고
