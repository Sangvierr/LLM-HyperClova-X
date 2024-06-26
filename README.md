# LLM-Study
📗 LLMs(Large Language Models)

### Theme 1. Naver Clova Summary
🎨 ```ClovaSummary.py```
- 일시 : 2024.02.20
- 사용한 데이터셋 : -
- 개요 : Naver Clova Summary API를 사용해서 요약 모델을 생성
- 참고 : [Naver Cloud Platform](https://medium.com/naver-cloud-platform/%EC%9D%B4%EB%A0%87%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%84%B8%EC%9A%94-clova-summary%EB%A1%9C-%EB%89%B4%EC%8A%A4-%EC%9A%94%EC%95%BD-%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EC%9D%B4%EA%B1%B4-%EB%A7%88%EC%B9%98-%EC%84%B8%EC%A4%84-%EC%9A%94%EC%95%BD-%EB%B4%87-dac29e97d1e4)
- 모델 생성 후기

| 구분 | 장점 | 단점 |
| ----------------------- | ---------------------------------------------- | ---------------------------------------------- |
| Clova Summary API | 1. 별도의 Prompt 없이 Summary API를 사용해서 간단하게 요약문을 생성할 수 있다. <br> 2. 결과가 빠르게 생성된다. <br> 3. language, model, tone, summaryCount라는 4가지 파라미터를 조정해서 결과를 다듬을 수 있다. <br> 4. 전체 기사에서 중요한 문장을 선정해서 요약문을 만드는 방식으로 문장 재구성, 새로운 어휘 등을 사용하지 않는다.|1. Prompt가 없기 때문에 여러 명령을 입력할 수 없다. 결과가 정적이다. <br> 2. 한국어, 일본어만 요약문을 생성할 수 있다. <br> 3. 문장 단위로만 요약해주기 때문에 빠르게 읽기는 좋지만, 생략되는 문장이 많다. <br> 4. 중요 문장 추출 같은 느낌이라, summaryCount를 늘리면 요약 이전의 기사와 비슷해진다.|
| OpenAI GPT | 1. Prompt를 사용해서 모델의 성격이나, 답변의 어조 등 다양한 부분을 조절할 수 있다. <br> 2. 문장을 연결하고, 재구성하는 등 요약문에 더 많은 내용을 담는다. |1. 결과 생성이 오래 걸린다. API 요청 전송과 결과를 받아오는데 약 20초 정도 걸린다. <br> 2. GPT가 오직 요약만을 위한 모델이 아니기 때문에 재구성하는 과정에서 Hallucination(환각효과)이 우려되기도 한다.|

### Theme 2. Naver Clova Sentimental Analyzer
🎨 ```ClovaSentimentAnalyzer.py```
- 일시 : 2024.04.05
- 사용한 데이터셋 : -
- 개요 : Naver Clova Sentiment API를 사용해서 감정분석 모델을 생성
- 참고 : [Naver Cloud Platform](https://medium.com/naver-cloud-platform/%EC%9D%B4%EB%A0%87%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%84%B8%EC%9A%94-%ED%85%8D%EC%8A%A4%ED%8A%B8-%EA%B0%90%EC%A0%95-%EB%B6%84%EC%84%9D-%EC%84%9C%EB%B9%84%EC%8A%A4-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-clova-sentiment-%ED%99%9C%EC%9A%A9%EA%B8%B0-5d9db7b0209b)
- 모델 생성 후기
  - 기존에는 IMDB 리뷰 데이터를 활용해서 LSTM, GRU 등의 순환 신경망을 학습시키고, 새로운 문장에 대해서 Inference하도록 함
  - CLOVA Sentiment는 BERT를 활용한 모델임(+ BERT-base보다 4배 정도 빠르다고 함)
  - BERT를 사용했기 때문에 Attention 구조를 활용할 수 있음
    - Attention 구조에 나타나는 가중치는 모델이 어떤 단어에 집중해서 Inference했는지를 알 수 있음
    - 따라서 긍정/부정 판단에 영향을 준 문장을 추출할 수 있음
    - ```ClovaSentimentAnalyzer```에서도 해당 결과를 인덱싱하여 긍정/부정 판단에 영향을 준 문장을 보여주고 있음
    - 기존 감정 분석 방식보다 속도와 해석 측면에서 매우 우수한 모델임
- 결과 예시 : 중립인 리뷰에서 각 문장의 판단 결과가 ```neutral```로 나오는 경우가 많음을 볼 수 있음
  
  1️⃣ 쿠팡 1점 리뷰(부정)
  
  ![image](https://github.com/Sangvierr/LLM-HyperClova-X/assets/165464507/e4a1a5b0-7535-4603-9fee-4094f09bd10a)

  2️⃣ 쿠팡 3점 리뷰(중립)
  
  ![image](https://github.com/Sangvierr/LLM-HyperClova-X/assets/165464507/55cbb8c9-e67e-44f3-be6a-c639d0034617)

### Theme 3. Fine-tuning LLM with LoRA
🎨 ```Fine-tuning BERT for text classification with LoRA.ipynb```
- 일시 : 2024.05.01
- 사용한 데이터셋 : imdb datasets
- 개요 : 효율적인 파라미터 갱신 방법(PEFT)을 통한 BERT Fine-tuning
- 참고 : [Medium](https://medium.com/@karkar.nizar/fine-tuning-bert-for-text-classification-with-lora-f12af7fa95e4)
- 후기
  - 일반적인 Fine-tuning은 고성능의 GPU가 필요하고, 오랜 시간이 걸린다는 점에서 부담이 됨
  - LLM을 목적에 맞게 Fine-tuning하는 것은 특히나 쉽지 않음
  - PEFT(parameter-efficient fine tuning) 중에서도 LoRA를 활용하여 시간적/경제적 비용을 낮추며 LLM을 Fine-tuning할 수 있다.
  - 본 예시에서는 100%의 파라미터를 Fine-tuning하는 것과 LoRA를 통해서 0.03%의 파라미터를 Fine-tuning하는 것에 큰 차이가 없음을 확인함

  1️⃣ LoRA를 통한 0.03%만의 파라미터만 업데이트한 결과
  
  ![image](https://github.com/Sangvierr/LLM-Study/assets/165464507/28bc2202-fb4b-425b-902a-88a3e30d041c)

  2️⃣ 100%의 파라미터를 업데이터한 결과

  ![image](https://github.com/Sangvierr/LLM-Study/assets/165464507/4d98c606-491f-447e-9703-5f5b328a78d8)

### Theme 4. BERT
📁 ```BERT```
- 일시 : Since 2024.03
- 사용한 데이터셋 : -
- 개요 : BERT 모델과 토크나이저에 대해서 공부하고, 코드로 활용 방법 정리
- 참고 : 구글 BERT의 정석
- 모델 생성 후기
  - BERT의 기본 구조와 사전 학습 방법을 배울 수 있었음
  - 토크나이저의 작동 방식과 원리를 이해할 수 있었음
  - 다운스트림 태스크를 위해 Fine-tuning하는 기초적인 방법을 이해할 수 있었음
  - BERT의 파생 모델인 RoBERTa, ALBERT 등에 대해서도 공부할 수 있었음
