---
layout: post
title: '머신러닝(ML) - 편향(Bias)과 분산(Variance)'
subtitle: 'Bias Variance'
categories: dlog
tags: ml
comments: true
---

![2023-03-26-ml_bias_variance_asset](/assets/dlog/2022/2022-06-16-git_intro_asset.png)
편항과 분산은 모델의 성능지표를 판단할 때 사용합니다. 아래의 설명은 훈련데이터를 기준으로 작성된 수식입니다.

# 편향 (Bias)
> bias[f_hat(x)]=E[f_hat(x)]-f(x)  

편향은 모델의 예측값과 실제값이 얼마나 떨어져있는가에 대한 지표입니다. 즉, 높은 편향 (high bias)라 하면 모델이 얼마나 데이터를 맞추지 못하고있는가를 의미합니다. 이를 underfitting 이라고도 표현합니다.

# 분산 (Variance)
> var(f_hat(x))=E[(f_hat( x )-E[ f_hat(x) ])**2]  

분산은 에측값이 예측값의 평균과 얼마나 떨어져있는가에 대한 지표입니다. 즉, 높은 분산 (high variance)라 하면 모델이 얼마나 일괄적이지 않게(?) 예측하는가를 의미합니다. 이를 overfitting 이라고도 표현합니다.


# References
https://towardsdatascience.com/the-bias-variance-tradeoff-8818f41e39e9