# Implementing Neural Turing Machines
## Mark Collier et al. arXiv:1807.08518

예전에 [NTM(neural Turing machine) 논문](https://arxiv.org/abs/1410.5401)을 재밌게 읽었었다.
저자가 소스를 공개하지 않았던 사실은 모르고 있었다.
아이디어가 재밌어서 그랬는지 그동안 많은 오픈소스 구현이 있었다고 한다.
그런데 재밌게도 안정적인 버전은 없었다고.

이번 버전도 '정말' 그런지는 알 수 없지만 트위터에 몇몇 사람들이 "드디어 안정적인 버전이 나왔다"고 하는 걸 봤다.

논문의 핵심은 memory contents를 초기화할 때 작은 수($10^{-6}$)로 상수 초기화를 하면 가장 안정적이라는 것이다.

그리고 자신들이 구현한 NTM을 다음 두 개 모델과 비교했다.
1. LSTM
2. [Deepmind의 DNC](https://github.com/deepmind/dnc)

결과 그래프를 보면 NTM 초기 논문에서 제안했던 copy, repeat copy, associative recall 작업에 대해 모두 LSTM보다 좋은 결과를 보인다.
그리고 모든 작업에서 DNC가 NTM보다 낫다.
