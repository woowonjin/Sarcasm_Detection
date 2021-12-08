# Short Sentence Sarcasm_Detection

## 프로젝트 개요
### 프로젝트 정의
자연어처리(이하 NLP) 모델을 활용한 온라인 댓글 및 영화 리뷰 등의 문맥이 없는 짧은 문장(Contextless Short Sentence)에서 동작하는 “비꼬기 탐지” 모델을 개발한다..!

### 프로젝트 배경
감성분석(Sentiment Analysis, 긍/부정 분석) 은 Contextless Short Sentence 뿐아니라 여러 자연어처리(NLP) 분야에서 주요한 과제 중 하나로, 여러 사회 현상들에 대한 피드백 및 대중의 반응을 파악하고 수치화 할 수 있기에 중요한 과제로 주목받고 있다. 이러한 감성분석에서 ‘비꼬기 표현’ 은 감성분석을 어렵게하는 표현 중 하나로, ‘비꼬기 표현 탐지’(Sarasm Detection) 모델에 대한 연구는 해외 연구들에 비해 국내에서는 거의 찾아볼 수 없다.

### 관련 연구 및 논문
영어권에서는 트위터 멘션 등의 Short Sentence 을 이용한 Sarcasm Detection 관련 논문들이 여럿 존재하고, 다양한 방법의 시도들이 진행되고 있다.국내에서는‘수사의문문’등의 특정 표현에 집중해 모델링한 논문이 존재한다.

### 프로젝트 목표
감성분석(긍정/중립/부정) 모델에서 “중립/부정” 표현 판단에서 비꼬는 문장형태로 부터 발생하는 오차를 줄여 모델의 성능을 높인다.
-	Task 에 맞도록 기존 NLP 모델을 변형하거나 parameter tuning을 통해 최적화하여, 모델의 분류 정확도를 높인다.
-	“비꼬기 표현”이 애매한 표현이기 때문에 명확한 기준을 가지고 데이터를 분류하여 모델의 성능을 높인다.
-	모델의 성능을 실제 사람들이 비꼬기 표현을 탐지한것과 비교하며 사람들의 탐지 수준까지 향상시킨다.

## 프로젝트 내용
### 관련 기술
- 댓글 및 Short Sentence Data 수집을 위한 Web Parsing Library
  - Beautiful Soup, Selenium
- 한국어 전처리
  - Word Embedding, Konlpy 등의 한국어기반 NLP Library
- NLP task에 대한 Deep Learning 모델 설계
  - ELMo, BERT 등 기존 모델을 변형하여 사용
  - Multi-Channel CNN, Single LSTM 등을 결합하여 모델 생성

### 프로젝트 진행 프로세스
- 데이터 수집
  - 주요 포털사이트의 뉴스 도메인 관련 댓글, 총 24000개
  - 네이버 Corpus 의 영화리뷰 데이터, 4000개
  - 약 30000 건의 데이터를 확보한 이후, 비꼬기 표현 포함 여부를 직접 라벨링하여 모델 생성
- 프로젝트 해결 방법론
  - NLP 과정의 기존 방법론을 활용
  - Task정의 -> Data 확보 -> 모델 생성 -> 모델 최적화
  - 최적화 -> 자소/형태소/음절기반 Word Embedding Layer 적용, Multi-Channel CNN 및 CNN-LSTM 모델 사용
  - HyperParameter Tuning
- 관련 논문
  - 최광원. "‘비꼬기’의 담화 구조와 운율적 특징에 관한 연구." 국내석사학위논문 국민대학교 일반대학원, 2019. 서울
  - 이공주, 김지은. (2019). 스포츠 기사 댓글에서 수사의문문으로 표현된 풍자 표현의 자동인식. 언어, 44(4), 853-870.
  - Multi-channel CNN을 이용한 한국어 감성분석
  - CNN-LSTM 조합모델을 이용한 영화리뷰 감성분석
  - Applying Transformers and Aspect-based Sentiment Analysis approaches on Sarcasm Detection, 2020
  - CASCADE: Contextual Sarcasm Detection in Online Discussion Forums , 2018

## 기술적 내용
### 개발 환경
- Python 3.8.3
- Tensorflow 2.3.1
- Pandas 1.0.5

### Modeling
1. Word Embedding Layer(음절 / 형태소) 
2. Neural Net
  -	Single LSTM 
  -	CNN ( 음절 / 형태소 )
  -	Multi Channel CNN ( 음절 + 형태소 )
  -	CNN - LSTM (음절)
3. Sigmoid (Output Layer)
![image](https://user-images.githubusercontent.com/45147364/145233045-ccbcd5a5-80df-4551-82d4-95a9555f6297.png)

## 결론 및 기대효과
한국어의 특성상 “비꼬기 탐지”라는 것이 사람이 판단하기에도 애매하고 특히 길이가 길어지는 경우 등 일반적이지 않은 케이스에서 올바른 판단이 어려워보였다. 하지만 일반적인 댓글처럼 글의 길이가 짧거나, 확실히 비꼬는 표현이 들어있는 문장들에서는 좋은 성능을 보였다. 그리고 특정 표현(‘?’ 등)에 대해 sigmoid 결과값이 높게 잡히는 것으로 파악되었는데 이는 목적에 따라 Threshold를 적절히 조정을 하면 해당 model 활용의 활용이 가능할것으로 보인다.
이후 모델에서 도메인을 뉴스 기사로 잡고, Input으로 댓글내용 뿐만아니라, 해당 기사의 제목까지 넣어준다면 해당 도메인에서 더욱 높은 성능을 보일것으로 예측하기 때문에 이를 반영할 예정이다.
