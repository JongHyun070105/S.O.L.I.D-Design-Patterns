# Flyweight pattern

- 공유 가능한 객체를 재사용하여 메모리 사용 최소화
- 많은 수의 유사 객체를 생성해야 할 때 성능 최적화

## 장점

- 메모리 사용량 감소
- 객체 생성 비용 절감
- 대규모 시스템에서 성능 향상

## 단점

- 상태를 외부에서 전달해야 하는 경우 코드가 복잡해질 수 있음
- 공유 객체 변경 시 전체에 영향을 줄 수 있음

<br>

## 클래스 다이어그램

![img](/img/flyweight.png)

## 코드

```py
class CharacterStyle:
    def __init__(self, font, size, color):
        self.font = font
        self.size = size
        self.color = color

class StyleFactory:
    _styles = {}

    @classmethod
    def get_style(cls, font, size, color):
        key = (font, size, color)
        if key not in cls._styles:
            cls._styles[key] = CharacterStyle(font, size, color)
        return cls._styles[key]

class Character:
    def __init__(self, char, style):
        self.char = char          
        self.style = style        

    def render(self):
        print(f"{self.char} [폰트:{self.style.font}, 크기:{self.style.size}, 색:{self.style.color}]")


factory = StyleFactory()

chars = [
    Character('H', factory.get_style("Arial", 12, "Black")),
    Character('e', factory.get_style("Arial", 12, "Black")),
    Character('l', factory.get_style("Arial", 12, "Black")),
    Character('l', factory.get_style("Arial", 12, "Black")),
    Character('o', factory.get_style("Arial", 12, "Black")),
    Character('!', factory.get_style("Arial", 12, "Red")),
]

for c in chars:
    c.render()
```
