# Command Pattern

- 요청을 객체로 캡슐화하여 매개변수화하고, 요청을 큐에 저장하거나 로그로 기록하며, 실행 취소 기능을 지원하는 패턴

## 장점

- 요청의 발신자와 수신자를 분리시켜 결합도를 낮춤
- 명령어를 큐에 저장하거나 로그로 기록할 수 있음
- Undo/Redo 기능 구현이 용이함
- 새로운 명령어 추가 시 기존 코드 수정이 불필요함 -> OCP 준수

## 단점

- 각 명령어마다 클래스를 만들어야 하므로 클래스 수가 증가함
- 간단한 작업에 사용하면 오히려 코드가 복잡해질 수 있음

<br>

## 클래스 다이어그램

![img](/img/command.png)

## 파이썬 코드

```py
from abc import ABC, abstractmethod

class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

    @abstractmethod
    def undo(self):
        pass

class TextEditor:
    def __init__(self):
        self.text = ""

    def write(self, content):
        self.text += content

    def delete(self, count):
        removed = self.text[-count:]
        self.text = self.text[:-count]
        return removed

    def __str__(self):
        return self.text

class WriteCommand(Command):
    def __init__(self, editor, content):
        self.editor = editor
        self.content = content

    def execute(self):
        self.editor.write(self.content)

    def undo(self):
        self.editor.delete(len(self.content))

class DeleteCommand(Command):
    def __init__(self, editor, count):
        self.editor = editor
        self.count = count
        self.deleted_text = ""

    def execute(self):
        self.deleted_text = self.editor.delete(self.count)

    def undo(self):
        self.editor.write(self.deleted_text)

class CommandManager:
    def __init__(self):
        self.history = []
        self.redo_stack = []

    def execute_command(self, command):
        command.execute()
        self.history.append(command)
        self.redo_stack.clear()

    def undo(self):
        if not self.history:
            return
        command = self.history.pop()
        command.undo()
        self.redo_stack.append(command)

    def redo(self):
        if not self.redo_stack:
            return
        command = self.redo_stack.pop()
        command.execute()
        self.history.append(command)

editor = TextEditor()
manager = CommandManager()

manager.execute_command(WriteCommand(editor, "Hello "))
manager.execute_command(WriteCommand(editor, "World"))

print(editor)

manager.undo()
manager.undo()
print(editor)

manager.redo()
print(editor)

manager.redo()
print(editor)
```

## 다트 코드

```dart
abstract class Command {
  void execute();

  void undo();
}

class TextEditor {
  String text = "";

  void write(String content) {
    text += content;
  }

  String delete(int count) {
    // substring - 시작인덱스부터 끝인덱스 전까지 문자열을 잘라냄
    final removed = text.substring(text.length - count);
    text = text.substring(0, text.length - count);

    return removed;
  }

  @override
  String toString() {
    return text;
  }
}

class WriteCommand extends Command {
  TextEditor editor;
  String content;

  WriteCommand(this.editor, this.content);

  @override
  void execute() {
    editor.write(content);
  }

  @override
  void undo() {
    editor.delete(content.length);
  }
}

class DeleteCommand extends Command {
  TextEditor editor;
  int count;
  String deletedText = "";

  DeleteCommand(this.editor, this.count);

  @override
  void execute() {
    deletedText = editor.delete(count);
  }

  @override
  void undo() {
    editor.write(deletedText);
  }
}

class CommandManager {
  List<Command> history = [];
  List<Command> redoStack = [];
  late Command command;

  void executeCommand(Command command) {
    command.execute();
    history.add(command); // dart에선 append 말고 add 사용
    redoStack.clear();
  }

  void undo() {
    if (history.isEmpty) {
      // dart에선 isEmpty로 비어있는지 확인
      return;
    }
    command = history.removeLast(); // dart에선 pop 대신 removeLast 사용
    command.undo();
    redoStack.add(command);
  }

  void redo() {
    if (redoStack.isEmpty) {
      return;
    }
    command = redoStack.removeLast();
    command.execute();
    history.add(command);
  }
}

void main(List<String> args) {
  final editor = TextEditor();
  final manager = CommandManager();

  manager.executeCommand(WriteCommand(editor, "Hello "));
  manager.executeCommand(WriteCommand(editor, "World"));

  print(editor.toString());

  manager.undo();
  manager.undo();
  print(editor.toString());

  manager.redo();
  print(editor.toString());

  manager.redo();
  print(editor.toString());
}
```
