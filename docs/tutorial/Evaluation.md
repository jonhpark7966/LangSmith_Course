# Evaluation (평가)

데이터를 준비했으니 Evaluation 도 해보겠습니다.
먼저 가장 간단한 예시 부터 시작합니다.

## Quick Start

아래 코드를 수행하면, 데이터셋을 만들고, example을 추가하고, 평가합니다.

``` python 
from langsmith import Client
from langsmith.evaluation import evaluate

client = Client()

# Define dataset: these are your test cases
dataset_name = "Sample Dataset"
dataset = client.create_dataset(dataset_name, description="A sample dataset in LangSmith.")

client.create_examples(
  inputs=[
    {"postfix": "to LangSmith"},
    {"postfix": "to Evaluations in LangSmith"},
  ],
  outputs=[
    {"output": "Welcome to LangSmith"},
    {"output": "Welcome to Evaluations in LangSmith"},
  ],
  dataset_id=dataset.id,
)

# Define your evaluator
def exact_match(run, example):
  return {"score": run.outputs["output"] == example.outputs["output"]}

experiment_results = evaluate(
  lambda input: "Welcome " + input['postfix'], # Your AI system goes here
  data=dataset_name, # The data to predict and grade over
  evaluators=[exact_match], # The evaluators to score the results
  experiment_prefix="sample-experiment", # The name of the experiment
  metadata={
    "version": "1.0.0",
    "revision_id": "beta"
  },
)
```

코드의 주요 요소는 다음과 같습니다.
- Datatsets 에 example 추가
- Evalutation 함수 정의 : exact_match
- 평가 대상인 LLM 어플리케이션은 간단하게 lambda 로 정의해 넣었습니다.
	- 단순 텍스트 치환함수 입니다.

Exact Match 테스트한 결과를 LangSmith에서 확인하면, 다음과 같습니다.  
Experiment 가 생겼고 그 결과가 표시됩니다. 
![](rsc/evaluation_1.png)

결과를 클릭하여 더 자세히 보겠습니다.  

![](rsc/evaluation_2.png)

각 데이터셋 example에 대해 LLM 어플리케이션의 output, 평가 결과까지 잘 매겨져 나왔습니다.
위에서 수행한 evaluation 도 Run 이고 Trace 이기 때문에 프로젝트에 추적 결과가 생성됩니다.  

![[evaluator_3.png]]

evaluators 라는 이름의 프로젝트로 trace 를 추적할 수 있습니다.  
예상과 다르게 평가 결과가 안 좋았다면, 그 이유를 찾는 디버깅이 가능하겠네요.  



## Step-by-Step 가이드



### Step 1.


## Custom Evaluator

위 간단한 예시에서는 exact_match, 정확하게 일치하는 지를 기준으로 평가했습니다. 문제집의 정답 채점하는 것 처럼요.  
LLM 어플리케이션의 가장 어려운 점은 평가가 정성적인 경우가 많다는 점 입니다.  
그래서, Evaluation을 제 맘대로 만들 수 있어야 합니다. 