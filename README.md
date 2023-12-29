# sLLM과 Re-ranking 전략을 결합한 GEC 모델
__KSC 2023 논문__

## data
- AI-Hub: '인터페이스(자판/음성)별 고빈도 오류 교정 데이터
|Train|Val|Test|
|------|---|---|
|260,500|29,000|1,000|

- N-gram: 국립국어원 신문 말뭉치
  
|Train|
|------|
|37,301,333|

# 서론
LLM의 beam-search의 후보 문장안에 정답이 존재했던 확률이 20%이상 존재  
=> 확률이 높지는 않더라도 LLM의 후보 문장안에 정답이 다수 존재, 후보 문장에서 정답을 선택해 성능을 올릴 수 있음
![image](https://github.com/jukyung-j/LLM_GEC/assets/68947314/ba65355f-92b2-4aa4-b1d3-5e9d4c720ae1)


# fine_tuning.py
- model: EleutherAI/polyglot-ko-12.8b을 이용하여 fine-tuning
- 후보 문장 재정렬을 위한 metric: Edit, LCS, GLEU, 1-gram, 2-gram, 3-gram

# Classfication.py
- fine-tuning된 모델의 후보 문장과 후보 문장의 확률, EDIT, LCS, GLEU, N-gram의 데이터를 저장하여 Classification 모델에 학습
- Cls 모델의 확률을 통해 후보 문장을 재정렬하여 정답 도출

  ![image](https://github.com/jukyung-j/LLM_GEC/assets/68947314/4d490ce2-a860-4237-91b6-a3da0a339014)

  # 실험 결과
  ![image](https://github.com/jukyung-j/LLM_GEC/assets/68947314/70123971-f559-4a57-b104-40a695ed4267)


- 기존 Fine-Tuning 모델 보다 재정렬 했을 때 성능 향상
- sLLM을 fine-tuning으로 LLM인 GPT-4의 성능 향상
