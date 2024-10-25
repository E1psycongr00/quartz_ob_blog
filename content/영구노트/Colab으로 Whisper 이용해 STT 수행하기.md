---
tags:
  - 완성
  - AI
  - OpenAI
  - Whisper
  - STT
aliases: 
date: 2024-05-21
title: Colab으로 Whisper 사용하기
---
작성 날짜: 2024-05-21
작성 시간: 16:45


----
## 내용(Content)

### Colab을 이용해 Whisper 사용하기

우선 이 링크에 접속한다.

https://github.com/jhj0517/Whisper-WebUI

Whisper Web-UI로 Whisper를 UI를 통해 쉽게 사용할 수 있도록 해준다. 접속해서 쭉 내려보면

![[Pasted image 20240521165825.png]]

이렇게 Notebook 으로 들어갈 수 있는 링크가 있다. 들어가자.

들어가면 Colab NoteBook이 열리는데 Drive로 복사를 누르면 내 드라이브에 복사된다.

![[Pasted image 20240521165914.png]]


check로 GPU가 연결되어 있는지 확인하고, Installation과 Run을 클릭하면, 

![[Pasted image 20240521170107.png]]

위와 같이 새로운 창이 뜨고, 파일을 넣거나 또는 Youtube 링크를 넣어서 음성을 추출해주면 된다.

내 케이스의 경우 1시간 정도 되는 Youtube 음성을 large-v3 모델로 텍스트로 추출하는데 1시간 20분 정도 걸렸다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://maily.so/dailyprompt/posts/86dd50cc
- https://github.com/jhj0517/Whisper-WebUI
- https://github.com/openai/whisper
## 연결 노트

- [[Whisper]]









