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

## 파이썬 코드

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

## 다트 코드

```dart
class CharacterStyle {
  final String font;
  final int size;
  final String color;

  const CharacterStyle(this.font, this.size, this.color);
}

class StyleFactory {
  static final Map<(String, int, String), CharacterStyle> _styles = {};

  static CharacterStyle getStyle(String font, int size, String color) {
    final key = (font, size, color);

    // putIfAbsent: Map클래스에 있는 메서드 - 특정 키에 해당하는 값이 맵에 없는 경우메나 값을 추가하는 메서드
    return _styles.putIfAbsent(key, () => CharacterStyle(font, size, color));
  }
}

class Character {
  final String char;
  final CharacterStyle style;

  Character(this.char, this.style);

  void render() {
    print('$char [폰트: ${style.font}, 크기: ${style.size}, 색: ${style.color}]');
  }
}

void main(List<String> args) {
  final chars = [
    Character('H', StyleFactory.getStyle('Arial', 12, 'black')),
    Character('E', StyleFactory.getStyle('Arial', 12, 'black')),
    Character('L', StyleFactory.getStyle('Arial', 12, 'black')),
    Character('L', StyleFactory.getStyle('Arial', 12, 'black')),
    Character('O', StyleFactory.getStyle('Arial', 12, 'black')),
    Character('!', StyleFactory.getStyle('Arial', 12, 'red')),
  ];

  for (final c in chars) {
    c.render();
  }
}
```
