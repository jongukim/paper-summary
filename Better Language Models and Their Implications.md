# Better Language Models and Their Implications

- [OpenAI Blog](https://blog.openai.com/better-language-models/)
- [Paper](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf), "Language Models are Unsupervised Multitask Learners"
- [Github](https://github.com/openai/gpt-2)

## Tweets

- 발표된 날 여러 유명 연구자의 트윗이 활활
- 대표적으로 [이걸](https://twitter.com/ryan_t_lowe/status/1096110750211473427?s=20) 보자
- 위 트윗을 한 Ryan Lowe(OpenAI에서 2017년에 인턴을 한 바 있다고 함)가 작성한 [블로그](https://towardsdatascience.com/openais-gpt-2-the-model-the-hype-and-the-controversy-1109f4bfd5e8)도 읽어볼만 함

## 뭘 만든거지?

Seed text를 주면 article을 생성하는데, news article, fiction 등 다양한 스타일로 글을 뽑아준다.

## 왜 난리지?

### 1. 너무 잘 돼서

물론 [unicorns 스토리](https://pbs.twimg.com/media/DzYpsJOU0AA1PO9.png:large)는 cherry-picking을 한 것이지만, 그래도 대단. 블로그에 가면 8개 정도의 샘플이 있다.

유니콘 스토리를 대강 번역하면 다음과 같다.

우선 seed text.

> 안데스 산맥에 위치한 미지의 계곡에서 과학자들이 유니콘 무리를 발견했다. 더 놀라운 것은 발견된 유니콘이 완벽한 영어를 구사한다는 것이다.

생생된 text.

> 독특한 뿔을 가지고 있어, 과학자들은 Ovid's Unicorn이라고 이름 붙였다. 4개의 뿔을 가진(?), 은색-백색 유니콘은 과학계에 알려진 바 없었다.

> 이제, 두 세기가 지나, 이 기이한 현상을 일으킨 미스터리가 풀렸다.(?)

> La Paz 대학교의 진화생물학자인 Jorge Perez 박사와 동료는 안데스 산맥을 탐험하고 있었으며 다른 동물과 인간이 존재하지 않는 작은 계곡을 발견했다. 페레즈 팀은 계곡에서 자연적으로 발생한 샘을 발견했으며, 그것은 은색 눈과 두 개의 암석 봉우리에 둘러싸여 있었다.

> 페레즈 팀은 계속을 더 깊이 탐색했다. "두 개의 봉우리 중 하나의 정상에 도착했을 때, 물은 푸르게 보였고 윗쪽에는 수정이 조금 있었습니다."라고 페레즈는 말했다.

> 페레즈 팀은 유니콘 무리를 보고 크게 놀랐다. These creature could be seen from the air without having to move too much to see them - 너무나 가까이 있어 뿔을 만질 수 있을 정도였다.

> 이 기이한 생명체를 조사하는 동안 과학자들은 이들은 영어를 구사한다는 것을 발견했다. Perez stated, "We can see, for example, that they have a common 'language', something like a dialect or dialectic."

> 페레즈 박사는 유니콘이 아르헨티나로부터 왔을 것이라고 믿고 있다. 남미에 사람들이 도착하기 전에 그곳에서 살고 있던 사라진 인종(?, a lost race of people)의 후손일 것으로 믿는다.

> 그들이 어디서 왔는지 아직 불분명하지만, 일부 사람들은 문명화 이전에 사람과 유니콘이 만났을 때 이들이 탄생했을 것이라고 믿고 있다. 페레즈에 따르면 "남미에서는 그런 일이 흔합니다."

> 그러나, 그들이 정말로 사라진 외계종의 후손인지 확인할 수 있는 방법은 DNA라고 페레즈는 지적했다. "But they seem to be able to communicate in English quite well, which I believe is a sign of evolution, or at least a change in social organization," said the scientist.

잘된다.

중간에 좀 어색한 부분이 있지만, 일관성을 유지하고 있다. '남미에 사람들이 오기 전'이라는 것은 '유럽의 남미 약탈 시기'를 알고 있다는 걸까? Race 간의 만남이나 evolution에 대해서도 이해하고 있는 걸까?

딥러닝 관점에서 보면 memory network 시리즈를 사용한 것도 아닌데, Dr. Perez가 계속 등장한다.

사람이 쓴 text보다 단어의 반복 빈도가 낮다고 한다. Generation 관점에서는 훌륭한 결과.

### 2. 모델을 공개 안해서

악용될 수 있다고 생각해서 모델(정확하게는 weights of its model)을 공개하지 않았다.
Smaller version은 제공한다.

일리가 있다. (아니면 그냥 관종?)

OpenAI는 밝은 미래와 어두운 미래를 모두 나열한다.

[밝은 미래]
- AI writing assistants
- More capable dialogue agents
- Unsupervised translation between languages
- Better speech recognition systems

[어두운 미래]
- Generate misleading news articles
- Impersonate others online
- Automate the production of abusive or faked content to post on social media
- Automate the production of spam/phishing content

생각해볼 필요가 있는 문제: AI and ethics
- Nando 형님의 [트윗](https://twitter.com/NandoDF/status/1096901408236933120?s=20)

코드와 데이터셋, 학습된 결과(모델) 공개는 ML 분야의 트랜드. 제출 안하면 논문이 수락되지 않는다.

Nando의 트윗에 사람들은 "그래도 코드 공개는 해야 되지 않나?" 하는 반응이 많다.
Reproducibility는 중요하다. 특히 학계에서는.

#### 그나저나 fake text에 당하지 않으려면 어떻게 해야 하나?

1. 사람들이 더 똑똑해진다. 속으면 니 탓.
2. AI-generated text를 구별해낼 수 있는 무언가를 또 만든다.

## 어쨌든 지금 분위기

- NLP researchers의 [반응](https://twitter.com/gregd_nlp/status/1096244878600818693?s=20)

## 왜 이렇게 잘 되는거지?

심지어 새로운 알고리즘을 제안하지 않았다.
Just scaling up.
물론 모델을 늘리는 것도 쉬운 것은 아니다.

OpenAI는 Dota2 agent에 대해 비슷하게 모델 크기만 늘리는 실험을 한 바 있다.

### 학습 방법

GPT(generative pretraining transformer)에 비해 10배 많은 데이터와 파라미터.

Input은 그냥 Internet text(8M pages).

전체 trained parameters: 1.5B.

학습 방식은? 간단. 앞의 단어를 가지고 다음 단어 예측.
학습할 때 domain은 고려하지 않는다.
Unsupervised.

### 결과 분석

상세 내용은 블로그에 있다.

물론 잘 안되는 샘플도 많다. 물속에서 불이 난다거나.

학습 때 인터넷 페이지를 사용했기 때문에 브렉시트나 마일리 사이러스, 반지의 제왕 등에 대해 요청하면 텍스트 생성 성공률이 상당히 높았고, 아주 domain-specific한 걸 물으면 잘 못했다.
당연한 결과.
이런 건 fine-tuning으로 해결하면 될 일.

심지어 몇 가지 task에서 fine-tuning을 하지 않고도 (SOTA보다) 좋은 결과를 보였다.
블로그에 몇가지 예제가 나온다.
