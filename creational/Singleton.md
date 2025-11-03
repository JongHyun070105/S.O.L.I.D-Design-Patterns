# Singleton Pattern

- 클래스의 인스턴스가 오직 하나만 존재하도록 보장하고, 어디서든 그 인스턴스에 접근할 수 있게하는 패턴

## 장점

- 인스턴스가 하나만 존재하기에 메모리 낭비를 줄인다.
- 전역적으로 접근이 가능하기에 상태 공유에 편리하다.
- 객체 생성 비용이 큰 경우에 유용하다 (예시: DB 커넥션)

## 단점

- 전역 상태가 많아지면 코드 의존성이 높아진다.
- 멀티스레드 환경에서 동기화 문제가 발생할 수 있다.
- SRP 원칙을 위반할 수 있다.

<br>

## 클래스 다이어그램

![img](/img/singleton.png)

## 파이썬 코드

```py
class DB:
  _instance = None

  def __new__(cls):
    if not cls._instance:
      print("DB를 생성합니다.")
      cls._instance = super(DB, cls).__new__(cls)
      cls._instance.connection = 'DB 연결중'
    return cls._instance

db1 = DB()
db2 = DB()

print(db1.connection)
print(db2.connection)

if db1 == db2:
  print("기존 DB에 연결되었습니다.")
else:
  print("다른 DB에 연결되었습니다.")
```

## 다트 코드

```dart
class DB {
  // 싱글톤 인스턴스를 저장할 정적 변수
  static DB? _instance;

  late String connection;

  DB._internal() {
    // 최초 인스턴스 생성 시 단 한번만 호출 됨
    print("DB를 생성합니다.");
    connection = "DB 연결중";
  }

  factory DB() {
    // _instance가 null이면 private 생성자를 통해 인스턴스 생성
    _instance ??= DB._internal();
    return _instance!; // 있으면 이미 존재하는 인스턴스 반환
  }
}

void main(List<String> args) {
  var db1 = DB();
  var db2 = DB();

  print(db1.connection);
  print(db2.connection);

  if (identical(db1, db2)) {
    print("기존 DB에 연결되었습니다.");
  } else {
    print("다른 DB에 연결되었습니다.");
  }
}
```
