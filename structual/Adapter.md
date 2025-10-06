# Adpater Pattern

- 서로 다른 인터페이스를 가진 클래스를 연결해, 클라이언트가 호환되지 않는 인터페이스를 신경 쓰지 않고 사용할 수 있게 해주는 패턴

## 장점

- 기존 코드를 변경하지 않고 새로운 클래스나 시스템과 호환 가능
- 클라이언트 코드 수정 최소화
- 라이브러리 통합 시 유용함

## 단점

- Adapter 클래스가 많아지면 코드가 복잡해짐
- 중간 매개체가 하나 더 생기므로 약간의 오버헤드 발생

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
