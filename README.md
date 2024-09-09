# Dacon Financial Information AI Search Competition
[![image](https://github.com/user-attachments/assets/a9c0b677-cdac-45de-8c55-24b4702da075)](https://dacon.io/competitions/official/236295/overview/description/)

Public score: 63/359(F1-Score: 0.66333)  
Private score: 96/359(F1-Score: 0.62702)  
***
### 1. Model fine tuning
 huggingface에 있는 야놀자의 solar 모델(yanolja/EEVE-Korean-Instruct-10.8B-v1.0)을 base model로, train.csv(약 500개 정도의 QA 데이터셋) fine tuning.
***
### 2. RAG
1. 기존의 base_line 코드에서 pdf loader를 fitz -> pymupdf4llm.to_markdown 사용  
  -> pdf 문서를 마크다운 형식으로 불러오기 때문에, 소제목에 ##와 같은 특수기호가 붙어서 더욱 제목, 소단락 기준으로 나누기 쉬워졌다.  
  -> meta_data에 자동으로 제목, 소단락 데이터가 들어가진다.  

2. Ensemble Retriever  
  -> faiss와 bm25(w. ko_kiwi)를 앙상블 했을 때, 가장 좋은 성능을 보여주었다. weight = [0.7, 0.3]  
***
### 3. Takeaway
 1. 자원의 한계  
 llm 모델은 확실히 많은 자원을 요구했다. google colab pro+에서 a100 40gb를 사용했음에도 분명한 자원에 한계를 느끼게 되었다. reranker와 같은 다른 여러 가지 시도를 해보고 싶었으나, to_markdown과 10.8B의 모델만으로도 대부분의 메모리를 사용했다.
 
 2. 지식의 한계  
 이번 2024년 여름방학의 PEFT와 RAG에 대한 강의를 마무리하고, 운이 좋게도 바로 써먹을 수 있는 대회가 있어서 참가하게 되었다. 여러 가지 시도를 해보았지만,
 - PEFT의 rank값 조정(r값이 올라가면 성능도 좋아진다고 이론으로 배웠지만, 그렇지 않았음.)
 - Fine tuning 시, retriever가 가져온 context도 같이 훈련시켜보기(Retriever가 알맞은 context를 가져와도 llm이 제대로 대답 못하는 부분을 보안하기 위해 테스트.)
 - prompt의 중요성(prompt engineering이라는 분야가 따로 생긴 이유를 온 몸으로 느꼈음.)
지식적으로도 부족한 부분이 많다고 느껴졌다..

 3. 점수의 과적합  
 아쉽게도 이번 대회는 96등으로 마무리지었다. 더 아쉬운 점은 public과 private 스코어의 차이가 크다는 점이다. 대회가 끝난 이후, 그동안 만들었던 수많은 submission 답지들을 제출해보았는데, 0.66~까지 달성한 답지도 있었다. 최종적으로 우리는 너무 public score에 매몰되어서 과적합을 일으킨 것 같다고 결론지었다.

 4. retriever에서
 pdf문서에서 리트리버를 이용하여 context를 가져올 pdf문서를 pdf loader로 변환한 모습 그대로 가져오기에 context가 정리되지 못하였다. 이로인해 context를 이용한 훈련에서와 모델이 context를 이용하여 답변을 작성할 때 어려움을 겪었다. 이번 대회에서도 model의 성능 자체는 나쁘지 않았으나 리트리버에서 가져오는 context의 품질이 떨어져 답변을 제대로 못하는 경우가 많았다.
***
#### josha106:   
  2024년 학교에서 여름방학 특강을 2주동안 짧게 듣고 PEFT와 RAG에 대해 처음 알게 되었습니다. 2달 전만 하더라도 backpropagation을 배우고 있었는데, 굉장한 발전을 이루었다고 생각합니다. 무엇보다도 특강이 끝난 뒤에 제가 배운 기술을 곧바로 사용할 수 있는 대회가 있었다는 점이 굉장히 운이 좋았습니다. 이 대회를 통해 PEFT와 RAG에 대해서 이론으로 그치지 않고, 실전으로도 경헙해 볼 수 있는 뜻 깊은 시간이었다고 생각합니다.

  #### Kim_Hyeon_Jun:   
  지난 llm 대회때에 비하여 Pdf문서를 이용하는 방법에 관한 기초적인 방법을 알게 되었습니다. 또한 리트리버에 사용될 pdf문서의 전처리과정이 모델 답변의 성능에 크게 좌우 한다는것을 깨달았습니다. 한국형 llm 모델이 24년 3월에 비하여 많이 발전했다고 느꼈으며 retriever를 이용한 전처리 분야에 대하여 좀더 깊게 공부해볼 생각입니다.

