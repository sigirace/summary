---
layout: single
title:  '추천시스템 Metric'
toc: true
categories: recommender_system
tags: [metric, MAP, MRR, NDCG]
---

본 게시물은 추천시스템에 대한 평가 지표에 대해 [해당 포스트](https://medium.com/swlh/rank-aware-recsys-evaluation-metrics-5191bba16832)를 읽고 정리하는 글입니다. {: .notice--primary}

## 1. Intro ##

일반적으로 추천 시스템은 두가지 일을 한다.

1. 아이템의 등급 예측
2. 추천 아이템의 순위 목록 생성

위 작업 수행 후 평가를 위해 아래 세 가지 순위 측정 기준을 소개한다.

- **MRR**: Mean Reciprocal Rank
- **MAP**: Mean Average Precision
- **NDCG**: Normalized Discounted Cumulative Gain

## 2. “Rank-less” Evaluation Metrics



