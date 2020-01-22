# An Empirical Model of Large-Batch Training (2018.12, OpenAI)

- [1] [OpenAI Blog](https://blog.openai.com/science-of-ai/): How AI Training Scales
- [2] [Paper](https://arxiv.org/pdf/1812.06162.pdf)

## 적절한 batch size?

딥러닝 초기, 아주 작은 값 사용. 32, 64, 128 정도.

최근 연구는 ImageNet 학습 시 8~32K(2017), 64K(2018) 사용한 바 있음. 학습이 잘 되고, 학습 속도도 빨랐다.

## 배치가 크다는 건?

전체 dataset 중 더 많은 부분을 보고 다음 optimizing step을 결정하는 것.
더 정답에 가까운 방향을 설정할 수 있다.

배치가 작으면 방황을 많이 하게 된다.

재미없게 말하면, *the variance scales inversly with the batch size B*

## 그러면 무조건 키우면 좋은 것 아닌가?

그만큼 하나의 batch로부터 gradient를 계산하는데 자원이 많이 들어간다.
효율적이지 않다.

True gradient에 가까워질수록 계산 결과의 차이는 별로 없다.
명심하자. 어차피 점프는 한 번이다.

적당한 batch size를 잡으면 학습도 빨리하고 자원은 덜 들일 수 있다.
OpenAI에서는 이 '적당한 batch size'를 *critical batch size*라고 표현한다.

![](https://blog.openai.com/content/images/2018/12/basic-scaling-1.svg)
(figure src: [1])

## 방향을 잘 잡았다는 확신이 있다면 크게 점프해도 되지 않나?

맞다. 방향을 잘 잡았다면 그곳을 향해 더 많이 뛰어도 된다.
즉, batch size가 클 때 learning rate를 더 크게 잡아도 괜찮다는 것이다.

그런데, 얼마나 많이 뛸 건가? minina를 건너뛸 수도 있다.
Learning rate는 어려운 파라미터다.

실제 실험에서, 배치가 크면 learning rate가 높아도 된다는 것이 보였다.
배치 크기가 늘어날수록 정확한 방향을 찾아간다.

재밌는 것은 초반에는 배치가 작아도 나름 잘 찾아가긴 한다는 것.
갈수록 못찾는다.

배치가 크면 빠르게 학습.

## 배치를 키우면 training loss는 좋아도 test loss가 별로다(i.e. generalization gap) 연구가 있었단다.

그에 대한 반박 연구도 있었다.

말이 되나 이게? overfitting일 뿐이겠지.

## Noise

OpenAI는 noise라는 개념을 정의한다.

이상적인 grad가 있을 때 내가 계산한 grad가 얼마나 틀렸는지.

즉, 다른 배치로 학습을 할 때 계산되는 grad의 분산.

![](https://blog.openai.com/content/images/2018/12/noise-summary-3.svg)
(figure src: [1])

노이즈가 별로 없으면? 지금 충분히 큰 배치를 사용하고 있다는 것.

노이즈가 많으면? 더 큰 배치를 써도 된다는 것.

더 복잡한 문제 -> 더 많은 노이즈 -> 더 큰 batch size 써도 됨

즉, 복잡한 문제일수록 data-parallel을 써서 더 큰 batch를 사용하면 효과를 본다.

모델 크기와는 강한 연관이 없다고 함 (weak dependence: 모델이 클수록 batch size가 큰 것이 낫긴 함)

## 기타 언급

### 각 샘플의 특성이 강하면 배치를 작게 잡고, 전반적으로 유사하면 크게 잡는 것이 좋다?

합리적인 이야기인가? 결국 전체 데이터셋을 보고 학습하게 되는 것 아닌가?

Generative model일 경우에는 각 샘플로부터 더 많은 정보를 얻어야 하니까 batch size가 작아야 한다?

### 학습이 진행될수록 노이즈 스케일이 커지더라

Minima로 갈수록 정확한 방향을 (오히려) 찾기 힘들어진다는 의미?

또는 (OpenAI에 설명에 따르면) 딥러닝이 학습 초반에는 확실한 feature에 대해 학습을 시작하다가, 점차 복잡한 부분에 대해 공부하기 때문이란다.
뒤로 갈수록 더 어려워져서 틀린다는 것.
정말 그런걸까?

### 구현 시, 학습 도중에 batch size를 바꾸는 것은 손해일까?

DataLoader는 배치에 맞게 memory에 미리 올려두는 것일까, 아니면 그때그때 올리는 것일까?

보통 성능을 위해 **pin_memory=True** 를 준다. 아마도 성능상 손해를 볼 듯.
