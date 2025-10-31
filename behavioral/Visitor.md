# Visitor Pattern

- 객체 구조를 변경하지 않고 새로운 연산을 추가할 수 있게 하는 패턴

## 장점

- 새로운 연산을 쉽게 추가할 수 있음 -> OCP 준수
- 관련된 연산들을 하나의 클래스에 모을 수 있음
- 각 연산 로직을 분리하여 SRP 준수
- 복잡한 객체 구조를 순회하면서 작업을 수행할 수 있음

## 단점

- 새로운 Element 클래스를 추가하기 어려움 (모든 Visitor를 수정해야 함)
- Element의 내부 상태를 외부로 노출해야 할 수 있어 캡슐화가 깨질 수 있음
- 구조가 복잡하여 이해하기 어려울 수 있음

<br>

## 클래스 다이어그램

![img](/img/visitor.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class Product(ABC):
    def __init__(self, name, price):
        self.name = name
        self.price = price

    @abstractmethod
    def accept(self, visitor):
        pass

class Book(Product):
    def accept(self, visitor):
        return visitor.visit_book(self)

class Electronic(Product):
    def accept(self, visitor):
        return visitor.visit_electronic(self)

class Food(Product):
    def accept(self, visitor):
        return visitor.visit_food(self)

class PriceVisitor(ABC):
    @abstractmethod
    def visit_book(self, book):
        pass

    @abstractmethod
    def visit_electronic(self, electronic):
        pass

    @abstractmethod
    def visit_food(self, food):
        pass

class DiscountVisitor(PriceVisitor):
    def visit_book(self, book):
        discount = book.price * 0.1
        print(f"📚 {book.name}: {book.price}원 → 10% 할인 = {book.price - discount}원")
        return book.price - discount

    def visit_electronic(self, electronic):
        discount = electronic.price * 0.15
        print(f"💻 {electronic.name}: {electronic.price}원 → 15% 할인 = {electronic.price - discount}원")
        return electronic.price - discount

    def visit_food(self, food):
        discount = food.price * 0.05
        print(f"🍔 {food.name}: {food.price}원 → 5% 할인 = {food.price - discount}원")
        return food.price - discount

class TaxVisitor(PriceVisitor):
    def visit_book(self, book):
        tax = book.price * 0.0
        print(f"📚 {book.name}: {book.price}원 + 세금 0% = {book.price + tax}원")
        return book.price + tax

    def visit_electronic(self, electronic):
        tax = electronic.price * 0.1
        print(f"💻 {electronic.name}: {electronic.price}원 + 세금 10% = {electronic.price + tax}원")
        return electronic.price + tax

    def visit_food(self, food):
        tax = food.price * 0.08
        print(f"🍔 {food.name}: {food.price}원 + 세금 8% = {food.price + tax}원")
        return food.price + tax

products = [
    Book("디자인 패턴", 30000),
    Electronic("노트북", 1500000),
    Food("피자", 25000)
]

print("=== 할인 가격 계산 ===\n")
discount_visitor = DiscountVisitor()
total_discount = sum(product.accept(discount_visitor) for product in products)
print(f"\n총 할인가: {total_discount}원\n")

print("=" * 50 + "\n")

print("=== 세금 포함 가격 계산 ===\n")
tax_visitor = TaxVisitor()
total_tax = sum(product.accept(tax_visitor) for product in products)
print(f"\n총 세금 포함가: {total_tax}원")
```

## 다트 코드

```dart
abstract class Product {
  String name;
  int price;

  Product(this.name, this.price);

  double accept(PriceVisitor visitor);
}

class Book extends Product {
  Book(super.name, super.price);

  @override
  double accept(PriceVisitor visitor) {
    return visitor.visitBook(this);
  }
}

class Electronic extends Product {
  Electronic(super.name, super.price);

  @override
  double accept(PriceVisitor visitor) {
    return visitor.visitElectronic(this);
  }
}

class Food extends Product {
  Food(super.name, super.price);

  @override
  double accept(PriceVisitor visitor) {
    return visitor.visitFood(this);
  }
}

abstract class PriceVisitor {
  double visitBook(Book book);

  double visitElectronic(Electronic electronic);

  double visitFood(Food food);
}

class DiscountVisitor extends PriceVisitor {
  @override
  double visitBook(Book book) {
    double discount = book.price * 0.1;
    print("${book.name}: ${book.price}원 -> 10% 할인 = ${book.price - discount}원");
    return book.price - discount;
  }

  @override
  double visitElectronic(Electronic electronic) {
    double discount = electronic.price * 0.15;
    print(
      "${electronic.name}: ${electronic.price}원 -> 15% 할인 = ${electronic.price - discount}원",
    );
    return electronic.price - discount;
  }

  @override
  double visitFood(Food food) {
    double discount = food.price * 0.05;
    print("${food.name}: ${food.price}원 -> 5% 할인 = ${food.price - discount}원");
    return food.price - discount;
  }
}

class TaxVisitor extends PriceVisitor {
  @override
  double visitBook(Book book) {
    double tax = book.price * 0.0;
    print("${book.name}: ${book.price}원 + 세금 0% = ${book.price + tax}원");
    return book.price + tax;
  }

  @override
  double visitElectronic(Electronic electronic) {
    double tax = electronic.price * 0.1;
    print(
      "${electronic.name}: ${electronic.price}원 + 세금 10% = ${electronic.price + tax}원",
    );
    return electronic.price + tax;
  }

  @override
  double visitFood(Food food) {
    double tax = food.price * 0.08;
    print("${food.name}: ${food.price}원 + 세금 8% = ${food.price + tax}원");
    return food.price + tax;
  }
}

void main(List<String> args) {
  List<Product> products = [
    Book("디자인패턴", 30000),
    Electronic("노트북", 1500000),
    Food("햄버거", 8000),
  ];

  print("=== 할인 가격 계산 ===\n");
  DiscountVisitor discountVisitor = DiscountVisitor();
  final totalDiscount = products.fold<double>(
    // fold: 리스트의 모든 요소를 순회하며 하나의 최종값을 만들어냄.
    0.0,
    (sum, product) => sum + product.accept(discountVisitor),
  );
  print("\n총 할인가: $totalDiscount원");

  print("\n");

  print("=== 세금 포함 가격 계산 ===");
  TaxVisitor taxVisitor = TaxVisitor();
  final totalTax = products.fold<double>(
    0.0,
    (sum, product) => sum + product.accept(taxVisitor),
  );
  print("\n총 세금 포함가: $totalTax");
}
```
