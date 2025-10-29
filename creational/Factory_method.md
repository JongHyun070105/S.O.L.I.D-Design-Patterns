# Factory Method Pattern

- 어떤 객체를 만들지는 서브 클래스에게 맡기고, 객체 생성을 캡슐화하는 패턴

## 장점

- 객체 생성 로직과 사용 로직 분리 -> 코드 유연성 증가
- 새로운 객체 추가 시 기존 코드를 거의 수정하지 않아도 됨
- 상위 코드가 구체 클래스에 의존하지 않음 -> 유지보수 용이

## 단점

- 클래스 수가 늘어나면 코드 구조가 복잡해질 수 있음
- 단순한 경우에는 오히려 불필요하게 복잡해짐

<br>

## 클래스 다이어그램

![img](/img/factory.png)

## 파이썬 코드

```py
class ProductCar:
  def drive(self):
    pass

class Kia(ProductCar):
  def drive(self):
    print('kia차 출고')

class Hyundai(ProductCar):
  def drive(self):
    print('현대차 출고')

class CarFactory:
  def createCar(self):
    pass

class KiaFactory(CarFactory):
  def createCar(self):
    return Kia()

class HyundaiFactory(CarFactory):
  def createCar(self):
    return Hyundai()

def get_factory(brand: str) -> CarFactory:
    if brand == "기아":
        return KiaFactory()
    elif brand == "현대":
        return HyundaiFactory()
    else:
        raise ValueError("Unknown brand")

factory = get_factory("현대")
car = factory.createCar()
car.drive()
```

## 다트 코드

```dart
abstract class ProductCar {
  void drive();
}

class Kia implements ProductCar {
  @override
  void drive() {
    print("기아차 출고 완료");
  }
}

class Hyundai implements ProductCar {
  @override
  void drive() {
    print("현대차 출고 완료");
  }
}

abstract class CarFactory {
  ProductCar createCar();
}

class KiaFactory implements CarFactory {
  @override
  Kia createCar() {
    return Kia();
  }
}

class HyundaiFacotry implements CarFactory {
  @override
  Hyundai createCar() {
    return Hyundai();
  }
}

CarFactory getFactory(String brand) {
  if (brand == "기아") {
    return KiaFactory();
  } else if (brand == "현대") {
    return HyundaiFacotry();
  } else {
    throw Exception("알 수 없는 브랜드입니다.");
  }
}

void main(List<String> args) {
  final factory = getFactory("기아");
  final car = factory.createCar();

  car.drive();
}
```
