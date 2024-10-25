---
tags:
  - 완성
  - BenchMark
aliases: 
date: 2024-10-01
title: 한국어 검색 embedding model 벤치마크
---
작성 날짜: 2024-10-01
작성 시간: 10:17


----
## 내용(Content)

한국어 검색 전용 임베딩 벤치마크

https://github.com/teddylee777/Kor-IR?tab=readme-ov-file

| Rank | Model                                                           | NDCG@10 | MRR@10 | MAP@10 | Recall@10 | Average | Link                                                                                     |
| ---- | --------------------------------------------------------------- | ------- | ------ | ------ | --------- | ------- | ---------------------------------------------------------------------------------------- |
| 1    | **Upstage/solar-embedding-1-large**                             | 83.61   | 84.14  | 80.35  | 88.35     | 84.11   | [📎](https://developers.upstage.ai/docs/apis/embeddings)                                 |
| 2    | **Cohere/embed-multilingual-v3.0**                              | 80.79   | 82.33  | 76.80  | 85.96     | 81.47   | [📎](https://docs.cohere.com/reference/embed)                                            |
| 3    | **intfloat/multilingual-e5-large-instruct**                     | 80.68   | 82.52  | 76.61  | 85.74     | 81.39   | [📎](https://huggingface.co/intfloat/multilingual-e5-large-instruct)                     |
| 4    | **intfloat/multilingual-e5-large**                              | 80.35   | 82.01  | 76.37  | 85.40     | 81.03   | [📎](https://huggingface.co/intfloat/multilingual-e5-large)                              |
| 5    | **Voyage/voyage-multilingual-2**                                | 79.69   | 80.53  | 75.24  | 86.43     | 80.47   | [📎](https://docs.voyageai.com/docs/embeddings)                                          |
| 6    | **BAAI/bge-m3**                                                 | 79.40   | 81.12  | 75.07  | 85.20     | 80.20   | [📎](https://huggingface.co/BAAI/bge-m3)                                                 |
| 7    | **intfloat/multilingual-e5-base**                               | 76.35   | 79.04  | 71.60  | 82.06     | 77.26   | [📎](https://huggingface.co/intfloat/multilingual-e5-base)                               |
| 8    | **intfloat/multilingual-e5-small**                              | 75.16   | 77.55  | 69.78  | 82.17     | 76.16   | [📎](https://huggingface.co/intfloat/multilingual-e5-small)                              |
| 9    | **Cohere/embed-multilingual-light-v3.0**                        | 74.22   | 76.31  | 68.91  | 81.26     | 75.17   | [📎](https://docs.cohere.com/reference/embed)                                            |
| 10   | **OpenAI/text-embedding-3-large**                               | 73.74   | 75.70  | 68.47  | 80.99     | 74.73   | [📎](https://platform.openai.com/docs/guides/embeddings/embedding-models)                |
| 11   | **upskyy/kf-deberta-multitask**                                 | 68.42   | 70.14  | 61.64  | 78.71     | 69.73   | [📎](https://huggingface.co/upskyy/kf-deberta-multitask)                                 |
| 12   | **cateto/longformer-ko-sroberta-multitask-23040**               | 66.31   | 68.51  | 59.34  | 76.62     | 67.69   | [📎](https://huggingface.co/cateto/longformer-ko-sroberta-multitask-23040)               |
| 13   | **jhgan/ko-sroberta-multitask**                                 | 65.10   | 67.46  | 58.03  | 75.41     | 66.50   | [📎](https://huggingface.co/jhgan/ko-sroberta-multitask)                                 |
| 14   | **jhgan/ko-sroberta-nli**                                       | 63.90   | 66.56  | 56.84  | 74.09     | 65.35   | [📎](https://huggingface.co/jhgan/ko-sroberta-nli)                                       |
| 15   | **jhgan/ko-sbert-multitask**                                    | 62.86   | 65.60  | 55.61  | 73.34     | 64.35   | [📎](https://huggingface.co/jhgan/ko-sbert-multitask)                                    |
| 16   | **BM-K/KoSimCSE-bert-multitask**                                | 56.93   | 59.17  | 49.21  | 69.03     | 58.59   | [📎](https://huggingface.co/BM-K/KoSimCSE-bert-multitask)                                |
| 17   | **BM-K/KoSimCSE-roberta-multitask**                             | 56.32   | 58.31  | 48.47  | 68.66     | 57.94   | [📎](https://huggingface.co/BM-K/KoSimCSE-roberta-multitask)                             |
| 18   | **OpenAI/text-embedding-3-small**                               | 56.33   | 59.75  | 49.64  | 65.53     | 57.81   | [📎](https://platform.openai.com/docs/guides/embeddings/embedding-models)                |
| 19   | **sentence-transformers/paraphrase-multilingual-mpnet-base-v2** | 53.03   | 54.49  | 46.52  | 63.25     | 54.32   | [📎](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2) |
| 20   | **sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2** | 45.90   | 46.90  | 39.26  | 57.04     | 47.28   | [📎](https://huggingface.co/sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2) |
| 21   | **kakaobank/kf-deberta-base**                                   | 41.25   | 43.82  | 33.45  | 53.42     | 42.99   | [📎](https://huggingface.co/kakaobank/kf-deberta-base)                                   |

## 질문 & 확장

(없음)

## 출처(링크)

- https://github.com/teddylee777/Kor-IR?tab=readme-ov-file
- 
## 연결 노트










