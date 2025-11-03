# Provider Pattern

- 상위 위젯트리에서 상태를 하위 위젯 트리에 전달할 수 있음
- 애플리케이션에서 상태 관리를 간단하고 효율적으로 수행하기 위해 사용

## Provider 패턴 핵심 3가지

- ChangeNotifier: 상태를 가지고 있으며, 상태가 변경되면 notifyListeners()를 호출해서 구독자들에게 알림
- ChangeNotifierProvider: 위젯 트리 상단에서 ChangeNotifier 객체를 제공
- Consumer & Provider.of(): 하위 위젯에서 상태를 구독하여 변경되면 자동으로 rebuild를 함

## 동작 흐름

1. ChangeNotifierProvider 생성
2. ChangeNotifier 객체 생성 및 제공
3. 하위 위젯에서 Consumer로 구독
4. 상태 변경
5. notifyListeners() 호출
6. 구독 중인 Consumer 위젯만 rebuild

## Consumer와 Provider.of 차이점

### Consumer

- 상태가 변경되면 해당 위젯만 rebuild
- 화면을 그릴 때 사용

### Provider.of(context, listen: false)

- 상태를 구독하지 않음
- 단순히 메서드만 호출할 때 사용

### Provider.of(context, listen: true)

- Consumer와 동일하게 작동


## 장점

- 코드를 간결하게 유지하며 데이터를 효율적으로 공유함
- 다른 패턴에 비해 문법이 간단함
- UI별로 상태 관리를 분리하여 코드를 깔끔하게 유지함

## 단점

- 프로젝트 규모가 커질수록 상태를 추적하기 어려워짐
- 전역 상태가 많아질 경우 단위 테스트가 어려워짐

<br>

## 구현 이미지

### 실행 전

![img](/flutter_img/provider1.png)

### 실행 후

![img](/flutter_img/provider2.png)

## 코드

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// 상태 클래스: 카운터 값을 관리하고 변경 알림
class Counter with ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners(); // 구독자들에게 알림
  }
}

void main() {
  runApp(
    ChangeNotifierProvider(
      // Counter 객체 생성. runApp 호출될 때 바로 만드는게 아니라 실제로 필요할때 딱 한번 생성함 (싱글톤 패턴과 유사함)
      create: (context) => Counter(),
      child: MyApp()), // ChangeNotifierProvider 아래에 있는 모든 위젯이 Counter에 접근 가능
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("Provider Example")),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text("You have pushed the button this many times"),
              Consumer<Counter>(
                // Counter 타입의 Provider를 찾고 구독
                builder: (context, counter, _) { // 파라미터 3개 입력 (child는 미사용해서 _처리함 )
                // 여기서 child는 rebuild 안되는 부분을 child로 분리해서 성능 최적화를 함
                  return Text(
                    '${counter.count}',
                    style: Theme.of(context).textTheme.headlineLarge,
                  );
                },
              ),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            // <Counter> 타입의 Provider를 찾고, 상태 변경을 구독하지 않기
            // listen: false 이유: true라면 버튼을 클릭했을 때 notifyListeners()를 호출되면 fab버튼이 리빌드 됨
            // => 메서드만 사용하면 되기에 listen:false를 사용함
            Provider.of<Counter>(context, listen: false).increment();
          },
          tooltip: 'Increment',
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}

```
