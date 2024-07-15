
# Datasets (데이터셋)

개념과 종류에 대한 내용은 [컨셉 가이드](../intros/Evaluation_with_dataset.md) 를 참고하세요.
한줄로 요약하자면, 평가를 하려고 데이터를 준비한다고 보면 됩니다. 튜닝용으로도 사용 가능하고요.


## LangSmith 직접 추가하기

아래와 같이 LangSmith 대시보드에서 바로 추가가 가능합니다.

![](rsc/dataset_1.png)

Dataset 안으로 들어가면, 아래와 같이 Example 들을 추가 할 수 있습니다.
Input 에 대한 예상 Output을 작성하면 됩니다.
Splits 가 base 로 설정이 되어있는데, 이는 아래에서 설명하겠습니다.

![](rsc/dataset_2.png)

## Trace 에서 Dataset 추가하기

Trace 탭에서 선택을 하면, 아래 Dataset 으로 추가하는 버튼이 활성화가 됩니다.

![](rsc/dataset_3.png)

Trace 를 확보하는 데이터 파이프라인이 확보되면, dataset을 구성하는 방법이 갑자기 매우 쉬워집니다.
데이터 수집 관점에서 아주 많이 좋습니다. 운영자가 원버튼으로 원하는 데이터들을 골라 넣어줄 수 있으니까요.

연구/개발 단계에서 여러 시도를 해보면서 픽할 데이터들을 남겨둘 수도 있고, 사용자가 불만족 했던 어려운 사용 사례들을 모아서 미래에 evaluation 데이터셋으로 구성할 수도 있습니다. 


## 데이터 구분하기 - Splits

데이터셋은 역할에 따라 분류가 가능합니다.  
Example 을 선택하면, split 에 넣는 버튼이 활성화 되는데, 멀티 태깅을 하듯이 달아 줄 수 있습니다.  
공식 설명에 따르면, 고전적인 머신러닝의 train, validation, test 스플릿 과 같은 기능을 염두에 두고 디자인을 했습니다.  
사용자의 기호에 맞게 튜닝용, 검증용, 테스트용, 에다가 task 별로 구분도 가능하고, 원하는 대로 태그를 달아 분류하면 됩니다.

![](rsc/dataset_splits.png)



## 기타 기능

이 데이터셋 들은 당연히 import, export 가능하고, csv 를 지원합니다.  
share 해서 공개로 올릴 수도 있습니다.  
코드를 짜서 컨트롤 하는 것도 가능하고, 메타데이터를 달아주고 필터링 하는 것도 가능합니다.  
특히, 버전은 따로 빼서 버저닝이 용이하도록 지원합니다.  
일반적인 데이터 베이스의 스키마를 LLM 어플리케이션에 맞게 미리 지정해 두고, web interface 를 개발자가 아닌 운영자도 쓰기 쉽게 만들어놨다 정도로 보면 충분합니다.  

자세한 내용은 [공식문서](https://docs.smith.langchain.com/how_to_guides/datasets/manage_datasets_in_application) 를 참조하세요.  

데이터가 준비가 되었으니 이제 평가 (Evaluation) 으로 넘어가겠습니다.  