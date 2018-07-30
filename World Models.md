# World Models
## David Ha et al. arXiv:1803.10122

> Nobody in his head imagines all the world.

사람도 뇌의 예측에 따를 뿐, 얻는 정보는 제한적.

야구 선수가 스윙을 할 때를 생각해보자.
Prediction에 의한 행동일 뿐.
정확한 정보로부터 어떤 판단을 하고 행동하는 것이 아니다.

과거와 현재를 최대한 이해하려 노력하고 미래를 예측하는 것.
물론 이해도가 높을 수록 예측 성공률은 올라갈 것.

렌즈를 통해 세상을 배움으로써 오히려 빨리 배우게 될 수도?

VAE를 통해 시각적 정보를 인코딩.

VAE의 latent vector(z)에 대해 RNN.
MDN-RNN by Graves.
벡터를 직접 예측하지 않고 다시 distribution을 예측.
Generalization에 도움이 될 것.

Temperature는 genetic algorithm에서 돌연변이와 비슷한 느낌.
꿈이 현실보다 더 어려운 경우를 만들 수 있다.
어려운 꿈 속에서 훈련을 하면 현실에서 더 좋은 결과를 얻기도 했다.

모델이 현실을 그대로 투영하지 못하면 controller는 그 빈틈을 비집고 편법을 찾아내곤 한다.
그렇게 편법을 배우고 나면 현실에서 참교육으 당한다.
MDN-RNN은 분산을 배우기 때문에 이런 빈틈을 어느정도 매꾸는 효과가 있다.

핸들을 중립으로 두는 건 옵션에 없다. 흔들흔들!

V를 먼저 학습하고 M을 학습하는 것이 효율적이었다.
한번에 end-to-end learning하는 것보다.

VAE의 z는 32개 파라미터. 생각보다 적다.

마지막 policy layer에는 non-linear activation을 추가하지 않았는데 이는 성능을 위해서 인 듯.
추가할 경우 학습에는 도움이 된다고 한다.

