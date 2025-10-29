# Composite pattern

- 객체들을 트리 구조로 구성하여 부분-전체 계층을 표현하고, 클라이언트가 개별 객체와 복합 객체를 동일하게 다룰 수 있게 하는 패턴

## 장점

- 객체들을 계층 구조로 쉽게 구성 가능
- 클라이언트 코드가 단일 객체와 복합 객체를 동일하게 처리 가능
- 트리 구조를 사용하므로 복잡한 구조를 간단히 표현 가능

## 단점

- 트리 구조가 너무 복잡하면 관리가 어려움
- Leaf와 Composite를 동일하게 다루기 때문에 타입 안전성을 잃을 수 있음

<br>

## 클래스 다이어그램

![img](/img/composite.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class EmployeeComponent(ABC):
    @abstractmethod
    def show_info(self, indent=0):
        pass

class Employee(EmployeeComponent):
    def __init__(self, name, position):
        self.name = name
        self.position = position

    def show_info(self, indent=0):
        print(" " * indent + f"{self.position}: {self.name}")

class Team(EmployeeComponent):
    def __init__(self, name):
        self.name = name
        self.members = []

    def add_member(self, member: EmployeeComponent):
        self.members.append(member)

    def remove_member(self, member: EmployeeComponent):
        self.members.remove(member)

    def show_info(self, indent=0):
        print(" " * indent + self.name)
        for member in self.members:
            member.show_info(indent + 4)

emp1 = Employee("박종현", "개발자")
emp2 = Employee("김철수", "디자이너")
emp3 = Employee("이영희", "기획자")
emp4 = Employee("김희영", "재정관리")
emp5 = Employee("김동균", "개발자")

dev_team = Team("개발팀")
design_team = Team("디자인팀")
product_team = Team("기획팀")
money_team = Team("재정관리팀")

dev_team.add_member(emp1)
design_team.add_member(emp2)
product_team.add_member(emp3)
money_team.add_member(emp4)
dev_team.add_member(emp5)

company = Team("회사")
company.add_member(dev_team)
company.add_member(design_team)
company.add_member(product_team)
company.add_member(money_team)

company.show_info()
```

## 다트 코드

```dart
abstract class EmployeeComponent {
  void showInfo({int indent = 0});
}

class Employee extends EmployeeComponent {
  String name;
  String position;

  Employee(this.name, this.position);

  @override
  void showInfo({int indent = 0}) {
    print(" " * indent + "$position: $name");
  }
}

class Team extends EmployeeComponent {
  String name;
  List<EmployeeComponent> members = [];

  Team(this.name);

  void addMember(EmployeeComponent member) {
    members.add(member);
  }

  void removeMember(EmployeeComponent member) {
    members.remove(member);
  }

  @override
  void showInfo({int indent = 0}) {
    print(" " * indent + name);
    for (EmployeeComponent member in members) {
      member.showInfo(indent: indent + 4);
    }
  }
}

void main(List<String> args) {
  final emp1 = Employee("박종현", "개발자");
  final emp2 = Employee("김철수", "디자이너");
  final emp3 = Employee("이용희", "기획자");
  final emp4 = Employee("김희영", "재정관리");
  final emp5 = Employee("김동균", "개발자");

  final devTeam = Team("개발팀");
  final designTeam = Team("디자인팀");
  final productTeam = Team("기획팀");
  final moneyTeam = Team("재정관리팀");

  devTeam.addMember(emp1);
  designTeam.addMember(emp2);
  productTeam.addMember(emp3);
  moneyTeam.addMember(emp4);
  devTeam.addMember(emp5);

  final company = Team("회사");
  company.addMember(devTeam);
  company.addMember(designTeam);
  company.addMember(productTeam);
  company.addMember(moneyTeam);

  company.showInfo();
}
```
