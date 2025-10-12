# Mediator Pattern

- 객체들 간의 복잡한 상호작용을 캡슐화하여 중재자 객체를 통해 통신하게 하는 패턴

## 장점

- 객체들 간의 직접적인 의존성을 제거하여 결합도를 낮춤
- 객체 간 통신 로직을 한 곳에 집중시켜 관리가 용이함
- 객체의 재사용성이 높아짐
- 새로운 중재자를 추가하여 객체 간 상호작용 방식을 쉽게 변경 가능

## 단점

- 중재자 객체가 복잡해지고 비대해질 수 있음
- 중재자가 God Object가 될 위험이 있음
- 시스템이 커질수록 중재자의 유지보수가 어려워질 수 있음

<br>

## 클래스 다이어그램

![img](/img/mediator.png)

## 코드

```py
class ChatRoom:
    def __init__(self, name):
        self.name = name
        self.users = []

    def add_user(self, user):
        self.users.append(user)
        print(f"✓ {user.name}님이 '{self.name}' 채팅방에 입장했습니다.")

    def send_message(self, message, sender):
        print(f"\n[{self.name}] {sender.name}: {message}")
        for user in self.users:
            if user != sender:
                user.receive(message, sender)

class User:
    def __init__(self, name, chatroom):
        self.name = name
        self.chatroom = chatroom

    def send(self, message):
        self.chatroom.send_message(message, self)

    def receive(self, message, sender):
        print(f"  → {self.name}님이 받음")

chatroom = ChatRoom("개발자 모임")

user1 = User("철수", chatroom)
user2 = User("영희", chatroom)
user3 = User("민수", chatroom)

chatroom.add_user(user1)
chatroom.add_user(user2)
chatroom.add_user(user3)

print("\n" + "=" * 50)

user1.send("안녕하세요! 처음 뵙겠습니다.")

print("\n" + "-" * 50)

user2.send("반갑습니다~ 잘 부탁드려요!")

print("\n" + "-" * 50)

user3.send("다들 반가워요 ㅎㅎ")
```
