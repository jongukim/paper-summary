# World Models

- DAVID HA(Google Brain Tokyo, known for [Sketch-RNN](https://magenta.tensorflow.org/assets/sketch_rnn_demo/index.html)), JÜRGEN SCHMIDHUBER(IDSIA, known for LSTM)
- Mar 2018
- Interactive [web page](https://worldmodels.github.io/)
- NIPS 2018 oral presentation: "Recurrent World Models Facilitate Policy Evoltion"

## The first known agent to solve the popular [Car Racing RL env.](https://gym.openai.com/envs/CarRacing-v0/)

> The study demonstrates the possibility of training an agent to perform tasks entirely inside of its simulated latent space dream world.

꿈 속 세상에서 에이전트가 학습할 수 있다는 것을 보여줬다.

## Intro

- 감각을 통해 얻는 정보는 [제한적](https://twitter.com/hardmaru/status/1103320787073859584?s=20)
- 사람의 뇌는 '제한적 정보'를 이용하여 미래를 예측하고 다음 동작을 결정 (예. 야구선수의 스윙)

## How? (네트워크 구성법)

- 위의 과정을 표현한 네트워크 구성
- 세 가지 components: V, A, C
  1. V: 세상으로부터 정보 얻기 (VAE)
  2. M: V에서 추출된 z(latent space)를 이용하여 다음 상황 예측 (MDN-RNN)
  3. C: M에서 예측한 정보와 현재 상황을 반영하여 다음 동작(action) 결정 (CMA-ES)
- Spatio-temporal representations
  - V(공간) + M(시간) = 시공간?

### V

- VAE로 세상을 관찰(observation), latent space(z, # of params: 32) 추출

### M

- MDN-RNN으로 구현
- RNN을 이용하여 시간을 고려
  - V는 특정 시각에 관찰된 정보 다루니까
- 입력
  - V로부터 얻는 z
  - RNN 내부의 h
  - C의 action인 a
- 출력
  - RNN 산출 결과 h+1
- MDN을 사용하는 이유?
  - 복잡한 세상은 stochastic하니까?

### C

- Controller
- C는 간단하게 구성
  - M의 학습에 더 집중하기 위해서
- C를 학습할 때는 backprop로 하지 않고 evolution strategy를 사용했다고
  - 실험에서 ES를 사용한 것이 더 좋은 결과를 얻었다고
- Reward와 관련된 것은 C가 전담
  - V와 M은 무관 

## 실험

### 실험 1: Car Racing

- OpenAI's gym의 [CarRacing-v0](https://gym.openai.com/envs/CarRacing-v0/)
  - 3 continuous actions: 1) steering left/right, 2) acceleration, 3) break
  - solution: 900 points
- M없이 V만 가지고 우선 실험
  - 적당한 결과
  - V + C with hidden layer로 진행하면 조금 나은 결과
- M을 함께 사용하면 좋은 결과
  - 현 순간을 통해 다음을 본능적(확률적)으로 예측하는 것으로 해석
  - Points: $906 \pm 21$

### 실험 2: VizDoom

- OpenAI's gym의 [DoomTakeCover-v0](https://gym.openai.com/envs/DoomTakeCover-v0/)
  - solution: 750 points
- CarRacing 문제와 다른 점
  - Doom에서는 agent가 죽는다
  - 다음 frame $z_{t}$만 예측하는 것이 아니라 죽느냐 사느냐인 $done_{t}$도 함께 예측해야 함

#### Doom 환경에서는 새로운 실험 추가

- M만 이용한 학습(V를 사용하지 않는) 실험: **꿈 속 학습**
  - 꿈 속에서 학습해서 실전 투입이 가능한가?
- V - M - C 로 연결된 네트워크에서 V를 제거
  - V - M을 묶어 end-to-end learning을 하지 않았으므로 가능
- 꿈에 noise를 추가하면 더 어려운 환경 생성이 가능하고 고득점 에이전트를 얻을 수 있었음

#### M만 사용해서 학습하면 좋은 점

- 훨씬 효율적
  - V의 # of params는 상대적으로 큰 숫자였다

#### M이 생성해내는 세상은 그럴싸했나

- M이 생성해내는 세상은 (당연히) 실제 세상과는 달랐다
  - 예) # of monsters가 실제와 달랐다.
- 하지만 학습 후 실제로 옮겼을 때 성적이 좋았다

#### MDN-RNN의 활약

- 게임에서의 cheating
  - C는 당연히 점수를 많이 얻기 위해 수단과 방법을 가리지 않을 것
  - C의 특정 움직임에 따라 M이 이상하게 동작하는 것을 찾아내곤 했다고
    - 실제로 관찰된 것은 Fireball을 전혀 쏘지 않는 환경
  - M은 가상 환경이므로 실제로 없는 상황 발생 가능
  - But, 이런 환경에서 편법만 이용해서 점수를 얻으면? 현실에서는 참교육
- MDN-RNN을 사용한 또 다른 이유
  - Temperature($\tau$, hyper-parameter)를 두어 꿈속 세상을 호락호락하지 않게 만드는 것
  - 물론, $\tau$를 너무 높이면 환경이 너무 어려워져서 오히려 학습에 방해

#### M-V를 연동하면 더 좋은 거 아닐까

- 현재는 V가 독립적으로 학습하는 구조이므로 필요없는 것도 학습
  - 예를 들어 Doom에서 벽면의 타일 모양을 복원한다거나, CarRacing에서 도로 타일이 정확히 복구가 안된다거나
- M과 V를 연동해서 학습하면 어떨까
  - V가 task-relevant 학습을 진행하게 되기 때문에 task별로 observation에 대한 해석이 달라질 것
  - Task마다 V를 별도로 학습해야 한다는 뜻
- 뇌과학에서는 task-relevant feature에 대한 학습을 한다는 연구가 있다고 함

## ES에 대해서는 아직 파악하지 않았음
