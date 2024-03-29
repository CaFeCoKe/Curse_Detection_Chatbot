# Curse_Detection_Chatbot
비속어 감지 데이터와 챗봇 데이터를 각각 KcELECTRA, KoGPT2 모델로 fine-tune 하여 챗봇을 실행 시 유저가 비속어를 3회 이상 입력하면 강제종료되게 하여 챗봇과 비속어 감지가 잘 되는지 확인한다.

## 1. 알고리즘 순서도
<table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/209318615-442ba344-d6b8-453c-bf44-af1b3472c8de.png" width="100%" height="100%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/214782336-5c23c06d-a6bf-4536-8269-54050943cb89.png" width="100%" height="100%"></td>
    </tr>
    <tr>
      <td align ="center">label 사용 X</td>
      <td align ="center">label 사용 O</td>
    </tr>
</table>

## 2. 결과
<table border ="0">
    <tr>
      <td><img src="https://user-images.githubusercontent.com/86700191/214517702-12e636ea-3020-439a-97a5-a3b9b2a42cee.PNG" width="100%" height="100%"></td>
      <td><img src="https://user-images.githubusercontent.com/86700191/214499952-5920fef9-223d-4447-b4f1-a2dcac9b5872.PNG" width="100%" height="100%"></td>
    </tr>
    <tr>
      <td align ="center">label 사용 X</td>
      <td align ="center">label 사용 O</td>
    </tr>
</table>

## 3. 주의점 및 개선점
- Tokenizer의 사용법과 special token 추가<br>
처음 계획은 하나의 tokenizer로 두 모델에 입력 텍스트를 토크나이징을 하려하였다. 이를 위해 [다른 tokenizer의 special token을 사용하려는 tokenizer의 special token으로 추가하는 작업](https://www.ohsuz.dev/22f4e8e7-64a3-4789-9dd2-171913883733) 을 거쳐 학습을 진행하였다.
하지만 KcELECTRA의 tokenizer로 사용하여 비속어 감지 모델을 학습이 잘 되었으나, 챗봇 모델을 학습이 잘 되지 않는 현상이 있었고 두 모델의 사전학습에 쓰인 tokenizer가 기본적으로 구성이 다르다는 점을 깨닫고, 다시 각 모델에 맞는 tokenizer를 사용하여 학습을 끝마쳤다.
<br><br>
- 특수 문자의 반복으로 인한 비속어 감지 모델의 성능<br>
&nbsp;&nbsp;soynlp의 repeat_normalze 메소드는 문자와 숫자의 반복을 줄여주지만 특수문자는 그렇지 않다는 것을 확인하였다. 그래서 비속어+특수문자 반복의 텍스트에 대해 비일관적인 성능을 가지게 되었다.<br>
![special_symbol_repeat](https://user-images.githubusercontent.com/86700191/210926740-a938d3eb-5351-4ca7-806e-e3329e862f6e.PNG) <br><br>
따라서, 텍스트 전처리 함수에 특수문자 반복을 축약하기 위한 정규표현식을 추가 하였고, 그 결과로 동일한 특수문자가 2회 이상 반복시 1개로 축약되어 비속어 감지 모델의 출력을 보다 일관적이게 만들 수 있었다.<br>
![special_symbol_repeat2](https://user-images.githubusercontent.com/86700191/210926742-c6fb33ac-c026-4251-b4ab-b5bdf9b95460.PNG)
<br><br>
- 챗봇 모델의 성능<br>
&nbsp;&nbsp; 챗봇 데이터에는 Q, A, label로 이루어져 있다. 전에 했던 [챗봇 프로젝트](https://github.com/CaFeCoKe/KoGPT2_Chatbot) 와 마찬가지로 이번 프로젝트에서도 label부분을 사용하지 않았다.
label은 일상다반사 0, 이별(부정) 1, 사랑(긍정) 2으로 되어 있는데 이를 사용하여 학습을 진행시킨다면 기존 모델의 성능과 얼마나 다른 차이가 있을지 추후 확인하도록 할 것이다. <br>
추가) label을 사용한 챗봇의 결과를 추가하였고, 일상다반사에 대한 모델의 결과 성능은 비슷하거나 살짝 좋아보이나 이별과 사랑에 대한 성능은 훨씬 더 나은 것을 확인할 수 있었다.

## 4. 사용 모델과 데이터
- [KcELECTRA](https://github.com/Beomi/KcELECTRA)
- [KoGPT2](https://github.com/SKT-AI/KoGPT2)
- [챗봇 데이터](https://github.com/songys/Chatbot_data)
- [비속어 감지 데이터](https://github.com/2runo/Curse-detection-data)