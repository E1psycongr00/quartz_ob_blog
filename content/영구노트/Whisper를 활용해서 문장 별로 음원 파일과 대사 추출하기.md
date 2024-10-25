---
tags:
  - 완성
  - OpenAI
  - AI
  - Python
  - 데이터전처리
aliases: 
date: 2024-06-03
title: Whisper를 활용해서 문장 별로 음원 파일과 대사 추출하기
---
작성 날짜: 2024-06-03
작성 시간: 20:58


----
## 내용(Content)

### 음성 합성을 위한 데이터 전 처리

음성 합성을 위해서는 문장 별 음원과 대사 파일이 필요하다. TTS 와 같은 라이브러리에서는 글에 따라서 음원을 생성하기 때문이다. 이 떄문에 데이터 전 처리를 위해 필요한 것은 다음과 같다.

1. 문장 별로 사운드 파일을 끊어서 보관한다.
2. 사운드 파일 별로 csv에 사운드 파일의 대사를 보관한다.

2번은 크게 어려운 것은 아니지만 1번의 경우 어떻게 해야 문장 별로 예쁘게 끊는 방법은 고민이 많다. 수동으로 하면 정확할 수는 있지만, 예를 들어 한 시간 가량의 음원 파일에 대해서 대사를 매 번 들어가면 분리하는 작업은 솔직히 무리다. 그러나 이 방법을 AI를 활용하면 쉽게 분리 할 수 있다.

OPEN AI의 WHISPER는 STT 라이브러리로 음원을 듣고 언어를 인식해 대사로 바꿔준다. 이를 이용하면, `음원 파일의 대사`와 `sentence별 길이` 모두 알 수 있다. 

### 준비 단계

#### 하드웨어

AI를 Local로도 돌릴 수 있는데 꽤 좋은 하드웨어가 필요하다. 본인의 Local 하드웨어 사양이 좋지 않다면 Colab이나 Open AI의 API를 활용할 수 있다.

#### 라이브러리 설치

우선 필요한 라이브러리를 설치한다.

```shell
pip install faster_whisper
pip install pydub
pip install -U openai-whisper
```


#### 코드

필요한 모든 준비는 끝났다. 다음은 Sentence를 쉽게 분리하기 위해 config.json과 python 코드이다.

```python
# split_setence.py

# LijSpeech 데이터 셋을 만들기 위한 스크립트
# 하나의 오디오 파일을 문장 단위로 분리하고, CSV 파일을 생성하는 스크립트
# Caution: 싱글 Audio 파일만 적용 가능


import whisper
import os
from pydub import AudioSegment
import csv
import json
import shutil


# JSON 파일 경로
CONFIG_FILE_PATH = "config.json"

# JSON 파일 읽기
with open(CONFIG_FILE_PATH, "r") as file:
	config = json.load(file)

MODEL = config["model"]

TRAINING_DATA = config["training_data"]
AUDIO_FILE_PATH = TRAINING_DATA["audio_file_path"]
EXTENSION = TRAINING_DATA["extension"]
OUTPUT_DIR = TRAINING_DATA["output_dir"]


CSV = config["csv"]
CSV_FILE_NAME = CSV["file_name"]
DELIMITER = CSV["delimiter"]


# Step 1: Load Whisper Model
model = whisper.load_model(MODEL)


# Step 2: Load Transcribe the audio file
result = model.transcribe(AUDIO_FILE_PATH)


# Step 3: Extract the segments
segments = result["segments"]
audio = AudioSegment.from_file(AUDIO_FILE_PATH, format=EXTENSION)


# Step 4: Make the output directory and wavs subdirectory
os.makedirs(os.path.join(OUTPUT_DIR, "wavs"), exist_ok=True)


# Step 5: Prepare CSV file
csv_file = os.path.join(OUTPUT_DIR, CSV_FILE_NAME)
csv_data = []


# Step 6: Split the audio based on segments and save to the directory
file_counter = 1
for i, segment in enumerate(segments):
	start_time = segment["start"] * 1000  # type: ignore # Convert to milliseconds
	end_time = segment["end"] * 1000  # type: ignore # Convert to milliseconds
	text = segment["text"] # type: ignore
	duration =  end_time - start_time # type: ignore

	# Check Condition
	if len(text) <= 2 or duration < 1000:
		print(f"Skipped sentence {i+1}: {text} (Duration: {duration} ms)")
		continue


	# Save File if Condition satisfied
	sentence_audio = audio[start_time:end_time]
	file_name = f"sentence_{file_counter+1}.wav"
	sentence_audio.export(os.path.join(OUTPUT_DIR, "wavs", file_name), format="wav")
	print(f"Exported sentence {file_counter+1}: {segment['text']}") # type: ignore
	
	# Add data to CSV list
	csv_data.append([f"sentence_{file_counter+1}", segment['text'], segment['text']]) # type: ignore


	# Increase the File Number
	file_counter += 1


# Step 7: Write the CSV file
with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
	writer = csv.writer(file, delimiter=DELIMITER)
	writer.writerow(["File Name", "Sentence", "Sentence"])
	writer.writerows(csv_data)


print("Audio files split and saved in directory:", OUTPUT_DIR)
print("CSV file saved in directory:", csv_file)


# Step 8: Compress the output directory
shutil.make_archive(OUTPUT_DIR, 'zip', OUTPUT_DIR)
print(f"Directory '{OUTPUT_DIR}' compressed to '{OUTPUT_DIR}.zip'")
```


```json
// json

{
	"model": "large",
	"training_data": {
		"audio_file_path": "audio",
		"extension": "wav",
		"output_dir": "output"
	},
	"csv": {
		"file_name": "metadata.csv",
		"delimiter": "|"
	}
}
```

### 결과

위와 같은 작업을 하면 아래와 같이 setence 별로 분리된 음원과 분리된 음원의 csv를 metadata.csv에 결과를 저장해서 얻을 수 있다.

![[Pasted image 20240605155801.png]]

생성된 setence 정보와 skip된 setence 정보
![[Pasted image 20240605160323.png]]



## 질문 & 확장

(없음)

## 출처(링크)

- https://sesang06.tistory.com/216
- https://colab.research.google.com/drive/1Wa-lgzLp8sNTA7qCt6jmkH5oZArIbj8x?usp=sharing


## 연결 노트