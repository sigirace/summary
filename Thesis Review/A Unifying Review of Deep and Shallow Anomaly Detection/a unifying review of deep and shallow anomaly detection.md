# A Unifying Review of Deep and Shallow Anomaly Detection

## 0. Abstract

​	최근 이상치 탐색에 대한 딥러닝 방식의 접근은 대규모 이미지 또는 텍스트와 같은 복잡한 데이터 세트에서 성능을 최고로 향상시켰다. 이러한 결과는 이상 탐지 문제에 대한 관심을 불러일으켰고 다양한 새로운 방법들을 이끌어냈다. 예를들어 generative models, one-class classification 및 reconstruction을 포함한 수많은 방법론이 등장함에 따라 이상치 탐색에 대한 방법론들을 체계적인 관점으로 통합할 필요가 증가하고 있다. 이 리뷰는 공통의 기본 원칙뿐만 아니라 다양한 방법론들에의해 암시적으로 만들어지는 가정을 식별하는데 초점을 맞춘다. 특히, 클래식한 'shallow' 접근법과 새로운 심층 접근법 사이의 연결을 그리고 이 관계가 어떻게 상호 발전하여 확장할 수 있는지 보인다. 또한 최근 explainability technique들로 풍부해진 주요 기존 방법에 대한 경험적 평가를 제공하고, 구체적인 해결 사례를 제시한다. 마지막으로, 중요 과제를 개략적으로 설명하고 이상치 탐색에서 향후 연구를 위한 구체적인 방향을 설정한다.

### Keywords

Anomaly detection, deep learning, explainable artificial intelligence, interpretability, kernel methods, neural networks, novelty detection, one-class classification, out-ofdistribution detection, outlier detection, unsupervised learning

### Summary

1. 딥러닝은 이상치 탐색에 대한 성능 향상을 이뤄냈으며, 이로인해 딥러닝을 활용한 다양한 방법론이 등장하게 됨
2. 기존의 방법과 다양한 딥러닝 방법에 대한 관계를 정리하여 통합화 할 필요가 있음

## 1. Introduction

​	이상치는 정규성의 개념에서 상당히 벗어난 관측치이며, 이상치 탐색은 데이터를 기반으로 한 모델 및 알고리즘을 통해 이러한 비정상적인 관측치의 탐색을 연구하는 연구 분야이다. 이상 탐지에 대한 고전적인 접근 방식으로는 주성분 분석(PCA), One-Class Support Vector Machine(OCSVM), Support Vector Data Description(SVDD), nearest neighbor algorithms, Kernel Density Estimation 등이 있다.<br>	위의 방법의 공통점은 모두 unsupervised라는 것인데, 이는 일반적인 이상치 탐색에서 비정상 데이터 레이블이 대부분이기 때문이다. 또한 변칙성의 모든 개념을 완전히 특성화하는 것은 충분하지 않으며, 이는 일반적으로 supervised 접근법을 비효율적으로 만든다. 따라서 이상 탐지의 핵심 아이디어는 정상 데이터를 사용해 모델을 unsupervised 방식으로 학습하여 모델로부터의 편차를 통해 이상을 탐지할 수 있도록 하는 것이다.<br>



The study of anomaly detection has a long history and spans multiple disciplines including engineering, machine learning, data mining, and statistics. 

While the first formal definitions of so-called ‘discordant observations’ date back to the 19th century [13], the problem of anomaly detection has likely been studied informally even earlier, since anomalies are phenomena that naturally occur in diverse academic disciplines such as medicine and the natural sciences. 

Anomalous data may be useless, for example when caused by measurement errors, or may be extremely informative and hold the key to new insights, such as very long surviving cancer patients. 

Kuhn [14] claims that persistent anomalies drive scientific revolutions (see section VI ‘Anomaly and the Emergence of Scientific Discoveries’ in [14]).



이상 탐지 연구는 오랜 역사를 가지고 있으며 공학, 기계 학습, 데이터 마이닝 및 통계를 포함한 여러 분야에 걸쳐 있다. 

소위 '불일치 관찰'에 대한 최초의 공식적인 정의는 19세기로 거슬러 올라가지만[13], 변칙 검출 문제는 의학이나 자연과학과 같은 다양한 학문 분야에서 자연스럽게 발생하는 현상이기 때문에 훨씬 이전에 비공식적으로 연구되었을 가능성이 높다. 

비정상적인 데이터는 예를 들어 측정 오류로 인해 발생할 때 쓸모가 없거나 매우 유용하며 오래 생존한 암 환자와 같은 새로운 통찰력의 열쇠를 쥐고 있을 수 있다. 

쿤[14]은 지속적인 변칙이 과학 혁명을 주도한다고 주장한다([14]의 섹션 VI '변칙과 과학적 발견의 출현' 참조).

