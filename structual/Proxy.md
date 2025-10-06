# Proxy Pattern

- 실제 객체에 대한 접근을 제어하거나, 객체 생성 비용을 지연시키는 대리자 역할을 하는 객체를 만드는 패턴

## 장점

- 실제 객체 접근을 제어할 수 있음
- 객체 생성 비용 절감
- 클라이언트 코드 변경 없이 부가 기능 추가 가능

## 단점

- Proxy 객체로 인해 코드 구조가 복잡해질 수 있음
- 실제 객체와 Proxy 간의 추가 레이어 -> 오버헤드 발생 가능

<br>

```py
from abc import ABC, abstractmethod

class SendMessage(ABC):
  def send_message(self, sender: str, receiver: str,message: str):
    pass

class SMS(SendMessage):
  def send_message(self, sender:str, receiver:str,message: str):
     print(f"{sender}가 {receiver}에게 SMS로 '{message}'를 발송함")

class Email:
  def send_another(self, sender:str, receiver:str,message: str):
      print(f"{sender}가 {receiver}에게 이메일로 '{message}'를 발송함")

class KakaoTalk:
  def send_another(self, sender:str, receiver:str,message: str):
      print(f"{sender}가 {receiver}에게 카카오톡으로 '{message}'를 발송함")

class MessageAdapter(SendMessage):
  def __init__(self, messageType):
    self.messageType = messageType

  def send_message(self, sender: str, receiver: str, message: str):
    self.messageType.send_another(sender, receiver, message)

sms = SMS()
kakao = KakaoTalk()
mail = Email()

sms.send_message("A", "B", "sms")

kakaotalk = MessageAdapter(kakao)
kakaotalk.send_message("A", "B", "카톡")

email = MessageAdapter(mail)
email.send_message("A", "B", "메일")
```
