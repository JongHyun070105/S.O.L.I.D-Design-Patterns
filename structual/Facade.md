# Facade pattern

- 복잡한 서브시스템을 단순화하여 단일 인터페이스로 제공, 클라이언트가 내부 구조를 몰라도 기능 사용 가능

## 장점

- 클라이언트 코드 단순화
- 서브시스템 변경 시 클라이언트에 영향 최소화
- 라이브러리/모듈 통합 시 유용

## 단점

- 지나치게 많은 기능을 하나로 묶으면 Facade 자체가 복잡해질 수 있음
- 일부 세부 기능이 필요한 경우 Facade가 제한적일 수 있음

<br>

## 클래스 다이어그램

![img](/img/facade.png)

## 파이썬 코드

```py
import time

class Light:
  def on(self):
    print("조명 On")
  def off(self):
    print('조명 Off')

class Tv:
  def on(self):
    print("TV On")
  def off(self):
    print("TV Off")

class Ac:
  def on(self):
    print("에어컨 On")

  def off(self):
    print("에어컨 Off")

class HomeFacade:
  def __init__(self):
    self.light = Light()
    self.tv = Tv()
    self.ac = Ac()

  def come(self):
    print("집에 들어옴")

    time.sleep(1)

    self.light.on()
    self.tv.on()
    self.ac.on()

    print("")

    time.sleep(3)

    print("외출모드")

    time.sleep(1)

    self.light.off()
    self.tv.off()
    self.ac.off()

home = HomeFacade()


home.come()
```

## 다트 코드

```dart
import 'dart:io';

class Light {
  void on() {
    print("조명 ON");
  }

  void off() {
    print("조명 OFF");
  }
}

class TV {
  void on() {
    print("TV ON");
  }

  void off() {
    print("TV OFF");
  }
}

class AC {
  void on() {
    print("에어컨 ON");
  }

  void off() {
    print("에어컨 OFF");
  }
}

class HomeFacade {
  Light light = Light();
  TV tv = TV();
  AC ac = AC();

  void come() {
    print("집에 들어왔을 때");

    sleep(Duration(seconds: 1));

    light.on();
    tv.on();
    ac.on();

    print("");

    sleep(Duration(seconds: 3));

    print("외출 했을 때");

    sleep(Duration(seconds: 1));

    light.off();
    tv.off();
    ac.off();
  }
}

void main(List<String> args) {
  final home = HomeFacade();

  home.come();
}
```
