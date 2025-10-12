# Strategy Pattern

- 실행(런타임) 중에 알고리즘 전략을 선택하여 객체 동작을 실시간으로 바뀌도록 할 수 있게 하는 행위 디자인 패턴.

## 장점

- 런타임에 알고리즘을 동적으로 변경할 수 있음
- 알고리즘을 독립적으로 관리하고 테스트할 수 있음
- 복잡한 조건문을 제거하여 코드 가독성 향상
- 새로운 전략을 추가해도 기존 코드 수정이 불필요함 -> OCP 준수

## 단점

- 전략 클래스가 많아지면 관리해야 할 객체 수가 증가함
- 클라이언트가 적절한 전략을 선택하기 위해 전략들의 차이를 알아야 함
- 간단한 알고리즘의 경우 오히려 코드가 복잡해질 수 있음

<br>

## 클래스 다이어그램

![img](/img/strategy.png)

## 코드

```py
from abc import ABC, abstractmethod

class Compression(ABC):
  @abstractmethod
  def compress(self, filename):
    pass

class Zip(Compression):
  def compress(self, filename):
    print(f'{filename}.zip 으로 압축했습니다.')

class Rar(Compression):
  def compress(self, filename):
    print(f'{filename}.rar로 압축했습니다.')

class Tar(Compression):
  def compress(self, filename):
    print(f'{filename}.tar로 압축했습니다.')

class FileCompressor:
  def __init__(self, strategy: Compression):
      self.strategy = strategy

  def setStrategy(self, strategy: Compression):
      self.strategy = strategy

  def compress(self, filename):
      self.strategy.compress(filename)

input_ext = input("압축 방식을 입력해주세요 (zip/rar/tar): ")
filename = input("파일명을 입력해주세요: ")

if input_ext == "zip":
    strategy = Zip()
elif input_ext == "rar":
    strategy = Rar()
elif input_ext == "tar":
    strategy = Tar()
else:
    raise ValueError(f"지원하지 않는 압축 형식입니다. {input_ext}")

context = FileCompressor(strategy)
context.compress(filename)
```
