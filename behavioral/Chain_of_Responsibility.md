# Chain of Responsibility Pattern

- 요청을 처리할 수 있는 객체들을 체인으로 연결하고, 요청이 처리될 때까지 체인을 따라 전달하는 패턴

## 장점

- 요청의 발신자와 수신자를 분리시켜 결합도를 낮춤
- 체인에 새로운 핸들러를 추가하거나 순서를 변경하기 쉬움 -> OCP 준수
- 각 핸들러가 단일 책임만 가지므로 코드 관리가 용이함

## 단점

- 요청이 처리되지 않을 수 있음 (체인의 끝까지 전달되어도 처리되지 않는 경우)
- 체인이 길어지면 성능 저하 발생 가능
- 디버깅이 어려워질 수 있음 (어느 핸들러에서 처리되는지 추적 필요)

<br>

## 클래스 다이어그램

![img](/img/chain_of_responsibility.png)

## 코드

```py
from abc import ABC, abstractmethod

class SupportTicket:
    def __init__(self, level, description, priority="normal"):
        self.level = level
        self.description = description
        self.priority = priority

class SupportHandler(ABC):
    LEVEL_1 = 1
    LEVEL_2 = 2
    LEVEL_3 = 3

    def __init__(self):
        self.level = 0
        self.next_handler = None

    def set_next(self, handler):
        self.next_handler = handler
        return handler

    def handle(self, ticket: SupportTicket):
        if self.can_handle(ticket):
            self.process(ticket)
        elif self.next_handler:
            print(f"  → {self.__class__.__name__}에서 처리할 수 없어 다음 레벨로 전달합니다.")
            self.next_handler.handle(ticket)
        else:
            print(f"어떤 팀도 이 문의를 처리할 수 없습니다.")

    @abstractmethod
    def can_handle(self, ticket: SupportTicket):
        pass

    @abstractmethod
    def process(self, ticket: SupportTicket):
        pass

class Level1Support(SupportHandler):
    def __init__(self):
        super().__init__()
        self.level = SupportHandler.LEVEL_1

    def can_handle(self, ticket: SupportTicket):
        return ticket.level == self.level

    def process(self, ticket: SupportTicket):
        print(f"Level 1 Support에서 처리: {ticket.description}")
        print("    → 일반 상담원이 FAQ를 안내했습니다.")

class Level2Support(SupportHandler):
    def __init__(self):
        super().__init__()
        self.level = SupportHandler.LEVEL_2

    def can_handle(self, ticket: SupportTicket):
        return ticket.level == self.level

    def process(self, ticket: SupportTicket):
        print(f"Level 2 Support에서 처리: {ticket.description}")
        print("    → 기술 지원팀이 문제를 해결했습니다.")

class Level3Support(SupportHandler):
    def __init__(self):
        super().__init__()
        self.level = SupportHandler.LEVEL_3

    def can_handle(self, ticket: SupportTicket):
        return ticket.level == self.level

    def process(self, ticket: SupportTicket):
        print(f"Level 3 Support에서 처리: {ticket.description}")
        print("    → 전문가가 심층 분석을 진행했습니다.")

class ChainPatternDemo:
    def main(self):
        level1 = Level1Support()
        level2 = Level2Support()
        level3 = Level3Support()

        level1.set_next(level2).set_next(level3)

        tickets = [
            SupportTicket(SupportHandler.LEVEL_1, "비밀번호를 어떻게 재설정하나요?"),
            SupportTicket(SupportHandler.LEVEL_2, "앱이 계속 크래시가 발생합니다."),
            SupportTicket(SupportHandler.LEVEL_3, "서버 아키텍처 마이그레이션 문의"),
            SupportTicket(4, "처리할 수 없는 레벨의 요청")
        ]

        print("=== 고객 지원 티켓 시스템 ===\n")
        for i, ticket in enumerate(tickets, 1):
            print(f"[티켓 {i}] {ticket.description}")
            level1.handle(ticket)
            print()

demo = ChainPatternDemo()
demo.main()
```
