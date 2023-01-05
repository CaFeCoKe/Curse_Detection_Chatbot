# Curse_Detection_Chatbot
비속어 감지 데이터와 챗봇 데이터를 각각 KcELECTRA, KoGPT2 모델로 fine-tune 하여 챗봇을 실행 시 유저가 비속어를 3회 이상 입력하면 강제종료되게 하여 챗봇과 비속어 감지가 잘 되는지 확인한다.

## 1. 알고리즘 순서도
![Curse_detection_Chatbot](https://user-images.githubusercontent.com/86700191/209318615-442ba344-d6b8-453c-bf44-af1b3472c8de.png)

## 2. 결과
![result](https://user-images.githubusercontent.com/86700191/210519072-148d8a94-7ef0-494e-bc1c-071a133aba2c.PNG)

## 3. 주의점 및 개선점
- Tokenizer의 사용법과 special token 추가<br>
- 특수 문자의 반복으로 인한 비속어 감지 모델의 성능<br>
- 챗봇 모델의 성능<br>