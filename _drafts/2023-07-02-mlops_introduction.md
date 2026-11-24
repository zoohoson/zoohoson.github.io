---
layout: post
title: '[MLops] - production'
subtitle: 'production'
categories: dlog
tags: ml
comments: true
---

# Select and Train a Model
  * 낮은 (평균)에러외에 확인 할 것 -> 테스트셋에서 잘나오는게 아니라, 목적에 맞게 테스트셋을 구성하거나 고려해야할것
    1 공평함 인종 등에서 공평한 알고리즘이 되야함
    2 목적에 맞는
    3 불균형클래스
  * 기준선
    70%가 낮은게 아님. HLP (human level performance)가 70%면 충분한 것.
  * 팁
    가장 위대한 알고리즘찾는데 세월보내지말고 반나절쯤후 가능한빨리 시작하라
    에자일하게 하려면 여러번 반복하는게 더 낫다.

    기준선 설정 (제약사항, 원하는 성능 등)

    처음부터 전체데이터셋을 보지말고, 작은 훈련셋에 오버핏은 잘 되는지 확인하고 큰거로 넘어가자.
# Data Definition and Baseline

![2023-07-02-MLinfrastructure_asset](/assets/dlog/2023/2023-07-02-MLinfrastructure.png)

# ML project 
  1. 

# References
1 : 