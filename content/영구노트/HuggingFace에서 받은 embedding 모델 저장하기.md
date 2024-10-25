---
tags:
  - 완성
  - 솔루션
  - Python
  - Langchain
aliases: 
date: 2024-10-01
title: HuggingFace에서 받은 embedding 모델 저장하기
---
작성 날짜: 2024-10-01
작성 시간: 19:16


----

## 문제 & 원인

```python
hf_embeddings = HuggingFaceEmbeddings(
    model_name="jhgan/ko-sroberta-multitask",
)
```

이렇게 호출했을 때 어딘가의 로컬 캐싱이 되긴 하지만 영구적으로 캐싱이 되진 않는다.
## 해결 방안

cache_folder라는 속성을 이용해서 해당 directory에 영구적으로 저장하고 HuggingFaceEmbeddings로 호출이 가능하다.

```python
hf_embeddings = HuggingFaceEmbeddings(
    model_name="jhgan/ko-sroberta-multitask",
    cache_folder="./models"
)
```

그러면 이렇게 모델들이 생성된다.

![[Pasted image 20241001192419.png]]
## 질문 & 확장

(없음)

## 출처(링크)

- https://steemit.com/kr-dev/@anpigon/20240609t151052628z
- 
## 연결 노트
