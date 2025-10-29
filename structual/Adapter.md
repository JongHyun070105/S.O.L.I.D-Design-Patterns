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

## 클래스 다이어그램

![img](/img/adapter.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class SendMessage(ABC):
  @abstractmethod
  def send(self, message: str):
    pass


class SMS(SendMessage):
  def __init__(self, phone_number: str):
    self.phone_number = phone_number

  def send(self, message: str):
    print(f"{self.phone_number}로 '{message}'를 보냈습니다.")


class EmailService:
  def __init__(self, sender_email: str, password: str):
    self.sender_email = sender_email
    self.password = password

  def send_email(self, receiver_email: str, message: str):
    print(f"{self.sender_email}가 {receiver_email}에게 이메일로 '{message}'를 보냈습니다.")


class KakaoTalkService:
  def __init__(self, user_id: str, password: str):
    self.user_id = user_id
    self.password = password

  def send_kakao(self, friend_id: str, message: str):
    print(f"{self.user_id}가 {friend_id}에게 카카오톡으로 '{message}'를 보냈습니다.")


class LineService:
  def __init__(self, user_id: str, password: str):
    self.user_id = user_id
    self.password = password

  def send_line(self, friend_id: str, message: str):
    print(f"{self.user_id}가 {friend_id}에게 라인으로 '{message}'를 보냈습니다.")


class MessageAdapter(SendMessage):
  def __init__(self, service, receiver: str):
    self.service = service
    self.receiver = receiver

  def send(self, message: str):
    if isinstance(self.service, EmailService):
      self.service.send_email(self.receiver, message)
    elif isinstance(self.service, KakaoTalkService):
      self.service.send_kakao(self.receiver, message)
    elif isinstance(self.service, LineService):
      self.service.send_line(self.receiver, message)
    else:
      raise TypeError("지원되지 않는 메시지 서비스입니다.")


sms = SMS("010-1234-5678")
sms.send("문자 메시지 테스트")

email = EmailService("user@example.com", "pw1234")
email_adapter = MessageAdapter(email, "friend@example.com")
email_adapter.send("메일 테스트")

kakao = KakaoTalkService("user123", "pw4567")
kakao_adapter = MessageAdapter(kakao, "friendABC")
kakao_adapter.send("카톡 테스트")

line = LineService("user987", "pw7890")
line_adapter = MessageAdapter(line, "friendXYZ")
line_adapter.send("라인 테스트")
```

## 다트 코드

```dart
abstract class SendMessage {
  void send(String message);
}

class SMS implements SendMessage {
  String? phoneNumber;

  SMS(this.phoneNumber);

  @override
  void send(String message) {
    print("$phoneNumber로 '$message'를 보냈습니다.");
  }
}

class EmailService {
  String? senderEmail;
  String? password;

  EmailService({this.password, this.senderEmail});

  void sendEmail(String receiverEmail, String message) {
    print("$senderEmail님이 $receiverEmail님에게 이메일로 '$message'를 보냈습니다.");
  }
}

class KakaoTalkService {
  String? userId;
  String? password;

  KakaoTalkService(this.userId, this.password);

  void sendKakao(String friendId, String message) {
    print("$userId가 $friendId에게 카카오톡으로 '$message'를 보냈습니다");
  }
}

class LineService {
  String? userId;
  String? password;

  LineService(this.userId, this.password);

  void sendLine(String friendId, String message) {
    print("$userId가 $friendId에게 라인으로 '$message'를 보냈습니다.");
  }
}

class MessageAdapter implements SendMessage {
  dynamic service;
  String? receiver;

  MessageAdapter(this.service, this.receiver);

  @override
  void send(String message) {
    if (service is EmailService) {
      service.sendEmail(receiver, message);
    } else if (service is KakaoTalkService) {
      service.sendKakao(receiver, message);
    } else if (service is LineService) {
      service.sendLine(receiver, message);
    } else {
      throw Exception("지원되지 않는 메시지 서비스입니다.");
    }
  }
}

void main(List<String> args) {
  // 일반
  final sms = SMS("010-1234-5678");
  sms.send("문자 메시지 테스트");

  // 어댑터 적용
  // - 이메일
  final email = EmailService(
    password: "pw1234",
    senderEmail: "user@example.com",
  );
  final emailAdapter = MessageAdapter(email, "friend@example.com");
  emailAdapter.send("메일 테스트");

  // - 카톡
  final kakao = KakaoTalkService("user1234", "pw1234");
  final kakaoAdapter = MessageAdapter(kakao, "friendABC");
  kakaoAdapter.send("카톡 테스트");

  // - 라인
  final line = LineService("user1234", "pw1234");
  final lineAdapter = MessageAdapter(line, "friendABC");
  lineAdapter.send("라인 테스트");
}
```
