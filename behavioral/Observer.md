# Observer Pattern

- 객체의 상태 변화를 관찰하는 관찰자들에게 자동으로 알림을 보내는 패턴

## 장점

- 상태 변화를 실시간으로 전파할 수 있음
- Subject와 Observer 간의 느슨한 결합을 유지함
- 런타임에 Observer를 추가/제거할 수 있어 유연함
- 새로운 Observer 추가 시 Subject 코드 수정이 불필요함 -> OCP 준수

## 단점

- Observer들에게 알림이 전달되는 순서를 보장할 수 없음
- Observer가 많아지면 성능 저하 발생 가능
- 메모리 누수 위험 (Observer 제거를 잊어버릴 경우)
- 복잡한 의존 관계로 인해 예상치 못한 업데이트가 발생할 수 있음

<br>

## 클래스 다이어그램

![img](/img/observer.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class Observer(ABC):
    @abstractmethod
    def update(self, channel_name, video_title):
        pass

class Subscriber(Observer):
    def __init__(self, name):
        self.name = name

    def update(self, channel_name, video_title):
        print(f"[알림] {self.name}님, '{channel_name}' 채널에 새 영상이 올라왔습니다: {video_title}")

class YoutubeChannel:
    def __init__(self, channel_name):
        self.channel_name = channel_name
        self.subscribers = []

    def subscribe(self, subscriber):
        self.subscribers.append(subscriber)
        print(f"✓ {subscriber.name}님이 '{self.channel_name}' 채널을 구독했습니다.")

    def unsubscribe(self, subscriber):
        self.subscribers.remove(subscriber)
        print(f"✗ {subscriber.name}님이 '{self.channel_name}' 채널을 구독 취소했습니다.")

    def upload_video(self, video_title):
        print(f"\n📹 [{self.channel_name}] 새 영상 업로드: {video_title}\n")
        self.notify_subscribers(video_title)

    def notify_subscribers(self, video_title):
        for subscriber in self.subscribers:
            subscriber.update(self.channel_name, video_title)

channel = YoutubeChannel("코딩 마스터")

user1 = Subscriber("철수")
user2 = Subscriber("영희")
user3 = Subscriber("민수")

channel.subscribe(user1)
channel.subscribe(user2)
channel.subscribe(user3)

channel.upload_video("디자인 패턴 완벽 가이드")

print("\n" + "-" * 50)

channel.unsubscribe(user2)

channel.upload_video("파이썬 고급 테크닉 10가지")
```

## 다트 코드

```dart
abstract class Observer {
  void upadate(String channelName, String videoTitle);
}

class Subscriber extends Observer {
  String name;

  Subscriber(this.name);

  @override
  void upadate(String channelName, String videoTitle) {
    print("[알림] $name님, $channelName 채널에 새 영상이 올라왔습니다. $videoTitle");
  }
}

class YoutubeChannel {
  String channelName;
  List<Subscriber> subscribers = [];

  YoutubeChannel(this.channelName);

  void subscribe(Subscriber subscriber) {
    subscribers.add(subscriber);
    print("${subscriber.name}님이 $channelName 채널을 구독했습니다.");
  }

  void unsubscribe(Subscriber subscriber) {
    subscribers.remove(subscriber);
    print("${subscriber.name}님이 $channelName 채널을 구독 취소했습니다.");
  }

  void uploadVideo(String videoTitle) {
    print("\n[$channelName] 새 영상 업로드: $videoTitle\n");
    notifySubscribers(videoTitle);
  }

  void notifySubscribers(String videoTitle) {
    for (Subscriber subscriber in subscribers) {
      subscriber.upadate(channelName, videoTitle);
    }
  }
}

void main(List<String> args) {
  YoutubeChannel channel = YoutubeChannel("코딩 마스터");

  Subscriber user1 = Subscriber("철수");
  Subscriber user2 = Subscriber("영희");
  Subscriber user3 = Subscriber("민수");

  channel.subscribe(user1);
  channel.subscribe(user2);
  channel.subscribe(user3);

  channel.uploadVideo("디자인 패턴 완벽 가이드");

  print("\n");

  channel.unsubscribe(user2);

  channel.uploadVideo("다트 고급 강의");
}
```
