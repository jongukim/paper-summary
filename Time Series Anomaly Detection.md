# Time Series Anomaly Detection
## Detection of anomalous drops with limited features and sparse examples in noisy highly periodic data
### Dominique T. Shipmon et al.

[arXiv](https://arxiv.org/abs/1708.03665)

#### Features
Timestamp와 bytes per second만 있는 아주 심플한 데이터를 대상으로 이상 탐지를 시도했다.

딥러닝을 이용해 문제를 해결해보려는 입장에서 feature 수가 너무 적은 것은 좋은 조건이 아니라고 본다.

학습할 때 시간 정보를 feature로 사용한 것이 독특했다.
주기적으로 발생하는 패턴이 있다면 시, 분, 초 정보를 함께 주는 것이 도움이 될 수 있다고 본다.
하지만 매 시각 5분 20초마다 어떤 패턴이 발생하는 것이 아니라 5분 단위로, 10초 단위로 발생하는 패턴이라면 과연 이것을 뉴럴넷이 인식할 수 있을까?
덧셈을 잘하는 NAC나 NALU를 이용한다면 꽤 패턴을 잘 인식할 수 있을지도.

#### Normalize
Normalize는 [0, 1]로 했다.
Sigmoid가 LSTM에서 사용되니 생각해보면 [-1, 1]보다 [0, 1]이 나을수도 있겠다는 생각이 든다.

#### 이상 판단
- Accumulation 방식
특정 구간에 연속적인 이상점이 발생하면 알림을 주도록 했다.
이상점을 찾으면 1점 증가, 정상을 만나면 2점 감점과 같은 방식으로 해서 쌓이면 알리는 것이다.

이상점을 찾는 방식은 단순히 threshold를 긋기도 하고 최근 데이터의 mean, var를 가지고 있으면서 너무 벗어나면 이상점으로 판단하는 방식도 실험했다고 한다.
단순한 threshold가 효과가 좋았다고 한다.
아마 여러 실험을 통해 최적의 threshold를 잡았기 때문이라고 생각한다.

- Gaussian tail probability
논문 레퍼런스 [2]에서 사용한 방법이라고 한다.

#### Learning models
DNN, 단순 RNN, LSTM 등 여러 모델을 실험했는데 별 차이는 없었다고 한다.
아마 feature가 너무 간단해서 그랬을 것.

이상 판단하는 방식이 훨씬 중요한 것 같다고 저자가 언급했다.
사실 둘 다 중요하지.
