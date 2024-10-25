---
tags:
  - 완성
  - 솔루션
  - AI
  - OpenAI
  - 크롤링
  - Selenium
aliases: 
date: 2024-05-22
title: Selenium으로 ChatGPT와 상호작용하기
---
작성 날짜: 2024-05-22
작성 시간: 21:33


----

## 문제 & 원인

1. Selenium을 이용해서 구글에 로그인을 하면 오류가 발생.
2. 로그인 이후 ChatGPT에서 데이터를 크롤링 해야 함.

ChatGPT에서 데이터를 크롤링하는 이유는 API는 비싸고, 프롬프트를 적용하기 어렵기 때문이다.(방법이 있을 것 같은데.. 찾아봐야 겠다.) 크롤링의 장점은 마치 GPT에 직접 접속해서 채팅하고 데이터를 얻어오기 때문에 API 요금이 소비되지 않고, 추가적인 비용 없이 Chat GPT를 이용해 소통할 수 있다. 단점은 API를 호출하는 것이 아니기 때문에 접속에 필요한 아이디로 로그인을 해야하며, API로 호출하는 것이 아니기 때문에, 추가적으로 Chrome을 띄워서 실행해야 한다.

2번이 메인 목표이지만, 로그인이 선행되지 않으면, 내가 만든 커스텀 GPT를 활용할 수 없다. 그리고 커스텀 GPT를 불러오더라도, GPT에서 채팅창에서 입력하고 입력 대기 동안 응답이 출력되고 응답이 완료되면 응답을 가져와야 한다. 
## 해결 방안

ChatGPT에서 나만의 GPT와 소통하고 그 결과를 크롤링할려면 로그인이 필요하다. 그러나 자동 로그인은 OAuth의 경우 막아 놓기 때문에 우회하는 방법이 필요하다.


1. undetected_chromedriver 사용한다.
2. 구글 프로필을 사용한다.

### 로그인 및 로그인 정보를 프로필에 저장하기

자동으로 입력하고 로그인하는 방법이 있지만 python을 활용하는 경우, undetected_chromedriver를 사용하면 디텍되지 않고 손쉽게 로그인이 가능하다.

```shell
pip install undetected_chromedriver
```

>[!caution]
>가끔 undetected_chromedriver를 사용할 때, 크롬 버전이 구식이거나 호환되지 않는 경우 오류가 발생할 수 있으니 해당 라이브러리 

```python
import undetected_chromedriver as uc

options = uc.ChromeOptions()
# 팝업 차단
options.add_argument("--disable-popup-blocking")

# 웹 드라이버 객체 생성
driver = uc.Chrome(options=options, user_data_dir="path/to/profile")
```

### ChatGPT의 채팅 버튼 인식

ChatGPT의 경우 입력과 그에 따른 결과물들을 받아 올 수 있어야 한다. GPT에게 질문을 던지면 응답하는 동안은 입력되면 안되기 때문에 GPT가 입력을 받을 수 있는 상태인지, 받을 수 없는 상태인지 알아야한다.

F12를 통해 개발자 도구를 이용해 분석해보자.

![[Pasted image 20240611103614.png]]

ChatGPT의 오른쪽 버튼이 응답 중에는 중에는
![[Pasted image 20240611103643.png]]

이런 식으로 버튼 모양이 바뀌는 것을 알 수 있다. 이들의 XPath를 추출해서 비교하면, ChatGPT와 채팅 상호작용을 쉽게 할 수 있게 된다.

### Code

```python
import undetected_chromedriver as uc
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time

  

class ChatGPTCommunicator:
	BUTTON_WAIT_XPATH = "//svg[contains(@class, 'icon-2xl')]"
	BUTTON_RUNNING_XPATH = "//svg[contains(@class, 'icon-lg')]"
	CHATGPT_URL = "https://chatgpt.com"
	EMILIA_GPT_URL = "https://chatgpt.com/g/g-m7IvBXDUH-emilria"


	def __init__(self, profile_dir):
		options = uc.ChromeOptions()
		options.add_argument("--disable-popup-blocking")
		self.driver = uc.Chrome(options=options, user_data_dir=profile_dir)

	def open_chat(self, url):
		if not url:
			url = self.CHATGPT_URL
		self.driver.get(url)
		time.sleep(1.5)

	def send_message(self, question):
		input_box = self.driver.find_element(By.XPATH, '//*[@id="prompt-textarea"]')
		input_box.send_keys(question)
		input_box.send_keys(Keys.ENTER)

	def is_running(self, timeout=10):
		"""
		지정된 시간 내에 페이지에 지정된 버튼 요소가 Running 상태에 있는지 확인합니다.

		매개변수:

			timeout (int): 요소가 나타날 때까지 최대 대기 시간 (기본값은 10초).
	
		반환값:
			bool: 요소가 지정된 시간 내에 나타나면 True, 그렇지 않으면 False.
		"""

		try:
			svg_element = WebDriverWait(self.driver, timeout).until(EC.presence_of_element_located((By.XPATH, self.BUTTON_RUNNING_XPATH)))
			return True
		except:
			return False

	def response(self):
		"""
		마지막 메시지를 가져와서 반환하는 함수입니다.
		Returns:
			str: 마지막 메시지의 텍스트
		"""
		last_message_xpath = "//div[contains(@class, 'w-full text-token-text-primary')][last()]"

		response_element = self.driver.find_element(By.XPATH, last_message_xpath)
		response = response_element.text
		return response

	def close(self):
		self.driver.quit()

	def __enter__(self):
		return self

	def __exit__(self, exc_type, exc_value, traceback):
		self.close()

if __name__ == '__main__':
	profilePath = r"C:\programming\python_project\tts_prac\chrome_profile"
	chatGptCommunicator = ChatGPTCommunicator(profilePath)
	chatGptCommunicator.open_chat(ChatGPTCommunicator.EMILIA_GPT_URL)
	while(True):
		if not chatGptCommunicator.is_running():
			question = input("질문을 입력하세요: ")
			chatGptCommunicator.send_message(question)
			while(chatGptCommunicator.is_running()):
				time.sleep(0.2)
			time.sleep(0.5)
			response = chatGptCommunicator.response()
			print(response)
		else:
			time.sleep(0.3)
```

## 질문 & 확장


## 출처(링크)



## 연결 노트
