# Strategy Pattern

- 실행(런타임) 중에 알고리즘 전략을 선택하여 객체 동작을 실시간으로 바뀌도록 할 수 있게 하는 행위 디자인 패턴.

## 장점

- 

## 단점

- 

<br>

```py
from abc import ABC, abstractmethod

class Compression(ABC): # 추상 전략
  @abstractmethod
  def compress(self, filename):
    pass

# 압축확장자 3개 클래스 - 구체 전략
class Zip(Compression):
  def compress(self, filename):
    print(f'{filename}.zip 으로 압축했습니다.')

class Rar(Compression):
  def compress(self, filename):
    print(f'{filename}.rar로 압축했습니다.')

class Tar(Compression):
  def compress(self, filename):
    print(f'{filename}.tar로 압축했습니다.')

# Context - 전략 보관 및 실행 & 교체 가능
class FileCompressor:
  def __init__(self, strategy: Compression): # 확장자 클래스를 받음
      self.strategy = strategy

  def setStrategy(self, strategy: Compression): # set을 통해 확장자 변경도 가능
      self.strategy = strategy

  def compress(self, filename): # 출력
      self.strategy.compress(filename)

# 사용예시
input_ext = input("압축 방식을 입력해주세요 (zip/rar/tar): ")
filename = input("파일명을 입력해주세요: ")

# 확장자에 따라 실행
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
