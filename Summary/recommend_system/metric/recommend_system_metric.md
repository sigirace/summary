---
layout: single
title:  '추천시스템 Metric'
toc: true
categories: recommender_system
tags: [metric, MAP, MRR, NDCG]
---

본 게시물은 추천시스템에 대한 평가 지표에 대해 [해당 포스트](https://medium.com/swlh/rank-aware-recsys-evaluation-metrics-5191bba16832)를 읽고 정리하는 글입니다. {: .notice--primary}

## 1. Introduction ##

일반적으로 추천 시스템은 두가지 task를 수행한다.

1. 아이템의 등급 예측
2. 추천 아이템의 순위 목록 생성

이번 리뷰는 위 작업 수행 후 평가를 위해 아래 세 가지 순위 측정 기준을 소개한다.

- **MRR**: Mean Reciprocal Rank
- **MAP**: Mean Average Precision
- **NDCG**: Normalized Discounted Cumulative Gain

## 2. Confusion Matrix in Recommender System

<img src="https://glassboxmedicine.files.wordpress.com/2019/02/confusion-matrix.png" width="450">

### 2.1 Accuracy

Accuracy (TP + TF / TP + TF + FP + FN) 에 대한 metric은 아래 두가지가 있으며 이들은 실제와 예측값의 등급을 비교하는 것에 초점을 맞춘다.

- mean absolute error (MAE)
- root mean square error (RMSE)

### 2.2 Decision support metrics

Decision support metric은 아래 세가지가 있으며 이들은 추천인이 사용자의 올바른 의사결정에 얼마나 도움이 되었는가를 측정하는 것에 초점을 맞춘다.

- Precision : 추천한 것 중 사용자가 선호하는 것의 비율 (TP / TP + FP)
- Recall : 사용자가 선호하는 것 중 추천한 것의 비율 (TP / TP + FN)
- F1 score : Precision과 Recall의 조화평균

## 3. Rank-less Evaluation Metrics

위 metric들은 사용자가 좋은 아이템을 선택하고 나쁜 아이템을 피하는데 도움을 줄 수 있다. 하지만 이들은 모든 데이터 셋을 대상으로 평가한 것들이기 때문에 ranking task에서는 활용하기 어렵다. 추천 시스템은 단순 아이템을 맞추는 것 보다 상위에 있는 아이템을 맞추도록 하는 것, 즉 'Top-N'을 타겟으로 삼아야 하기 때문이다. 예를들어 유튜브에서 수 많은 영상들을 추천 받았지만 내가 보고싶은 컨텐츠가 스크롤을 한참 내려서 봐야 한다면 추천의 성능이 좋다고 할 수 없다.<br>때문에 위 metric을 확장하여 Precision과 Recall에 대해 추천하는 아이템을 상위 N개로 제한하는 방법이 제안되었으며, 이를 Precision@N, Recall@N, F1@N으로 나타낸다. 하지만 이 또한 기존의 metric과 유사하며 여전히 아이템을 맞추는 것에만 집중한다.

## 4. Rank-Aware Evaluation Metrics

앞선 내용을 통해 추천 시스템의 관심사는 관련 아이템을 추천 목록에서 상위에 올리는 것임을 확인하였다. 즉, 추천 시스템은 아래 두가지 목표(추천 순위를 위한 모델링)를 가져야 한다.

1) 추천 시스템이 아이템을 어느 위치에 제안할 것인가?
2) 추천 시스템이 상대적인 선호도 모델링을 얼마나 잘하는가? 

 이를 위해서는 rank-aware metrics이 필요하며, 대표적으로 아래의 세가지 metric이 있다.

- **MRR**: Mean Reciprocal Rank
- **MAP**: Mean Average Precision
- **NDCG**: Normalized Discounted Cumulative Gain

### 4.1 MRR

가장 단순한 방식으로 "관련(relevant) 항목이 첫번째로 나오는 위치"에 대한 metric이다.

#### 4.1.1 Algorithm

- 추천 리스트를 생성함

- 리스트에서 첫번째로 관련(relevant)있는 아이템의 위치 $k_u$를 구함

- $\frac{1}{k_u}$ 을 계산

- 모든 사용자에 대한 $k_u$ 역수의 평균

  $MRR(O, U) = \frac{1}{\vert{U}\vert}\sum\frac{1}{{k_u}}$

#### 4.1.2 Example

아래 예시의 경우 사용자 1의 경우 relevant한 item이 추천 리스트 중 3번째에 처음 있었기 때문에 1/3, 사용자 2의 경우 추천 리스트 중 2번째에 처음 있었기에 1/2, 사용자 3의 경우 추천 리스트 중 1번째에 있었기에 1로 계산할 수 있다. 이후 이에대한 평균을 구하면 metric 결과는 0.61이 된다.

<img src="https://miro.medium.com/max/643/1*dR24Drmb9J5BLZp8ffjOGA.png" width="500">

#### 4.1.3 MRR Advantages

- 계산하기 쉽고 해석하기 쉽다.

- 첫 번째 관련 요소에 높은 초점을 맞추고 있기에 "나에게 가장 적합한 항목" 인 추천 task 적합하다.

- 탐색 쿼리 또는 팩트 검색과 같은 알려진 항목 검색에 적합하다.

#### 4.1.4 MRR Disadvantages

- 첫번째 관심 아이템을 제외한 나머지를 평가하지 않는다.
- 아이템의 리스트를 검색하려는 사용자에게 적합한 metric이 아닐 수 있다.

### 4.2 MAP

Next is the MAP metric. Let’s say we have a binary relevance data set. 

We want to evaluate the whole list of recommended items up to a specific cut-off N. 

This cut-off was previously incorporated using the Precision@N metric. 

The P@N decision support metric calculates the fraction of n recommendations that are good.

The drawback of this metric is that it does not consider the recommended list as an ordered list.

P@N considers the whole list as a set of items, and treats all the errors in the recommended list equally.

우리는 특정 컷오프 N까지 추천 품목의 전체 목록을 평가하고 싶다. 

이 컷오프는 이전에 Precision@N 메트릭을 사용하여 통합되었습니다. 

P@N 의사 결정 지원 메트릭은 좋은 n개의 권장 사항 중 일부를 계산합니다.

이 메트릭의 단점은 권장 목록을 순서 목록으로 간주하지 않는다는 것입니다.

P@N은 전체 목록을 항목 집합으로 간주하고 권장 목록의 모든 오류를 동일하게 처리합니다.

The goal is to cut the error in the first few elements rather than much later in the list. 

For this, we need a metric that weights the errors accordingly. 

The goal is to weight heavily the errors at the top of the list. 

Then gradually decrease the significance of the errors as we go down the lower items in a list.

목표는 목록에서 나중에 오류를 줄이는 것보다 처음 몇 가지 요소에서 오류를 줄이는 것입니다. 

이를 위해, 우리는 그에 따라 오류의 가중치를 부여하는 메트릭이 필요하다. 

목표는 목록의 맨 위에 있는 오류를 무겁게 평가하는 것이다. 

그런 다음 목록에서 하위 항목으로 내려갈수록 오류의 중요도를 점차 낮춥니다.

The Average Prediction (AP) metric tries to approximate this weighting sliding scale.

It uses a combination of the precision at successive sub-lists, combined with the change in recall in these sub-lists.

The calculation goes as follows:

평균 예측(AP) 메트릭은 이 가중치 슬라이딩 스케일을 근사화하려고 한다.

연속된 하위 목록의 정밀도와 이러한 하위 목록의 리콜 변경을 결합하여 사용합니다.

계산은 다음과 같습니다:
