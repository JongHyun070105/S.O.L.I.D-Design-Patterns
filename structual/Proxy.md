# Proxy Pattern

- 실제 객체에 대한 접근을 제어하거나, 객체 생성 비용을 지연시키는 대리자 역할을 하는 객체를 만드는 패턴

## 장점

- 실제 객체 접근을 제어할 수 있음
- 객체 생성 비용 절감
- 클라이언트 코드 변경 없이 부가 기능 추가 가능

## 단점

- Proxy 객체로 인해 코드 구조가 복잡해질 수 있음
- 실제 객체와 Proxy 간의 추가 레이어 -> 오버헤드 발생 가능

<br>

## 클래스 다이어그램

![img](/img/proxy.png)

## 코드

```py
import time
import random
from abc import ABC, abstractmethod

class ServiceInterface(ABC):
    @abstractmethod
    def fetch_data(self, endpoint):
        pass

class APIService(ServiceInterface):
    def fetch_data(self, endpoint):
        print(f"API 호출: {endpoint}")
        if random.random() < 0.3:
            raise Exception("API 요청 실패!")
        return f"데이터 from {endpoint}"


class APIServiceProxy(ServiceInterface):
    def __init__(self, real_service, retry=3, rate_limit=2):
        self._real_service = real_service
        self._retry = retry
        self._rate_limit = rate_limit
        self._last_call = 0

    def fetch_data(self, endpoint):
        current_time = time.time()
        if current_time - self._last_call < self._rate_limit:
            wait_time = self._rate_limit - (current_time - self._last_call)
            print(f"호출 제한: {wait_time:.1f}초 대기")
            time.sleep(wait_time)
        self._last_call = time.time()

        for attempt in range(1, self._retry + 1):
            try:
                print(f"[시도 {attempt}] Proxy가 APIService에 요청 위임...")
                result = self._real_service.fetch_data(endpoint)
                print("API 호출 성공!")
                return result
            except Exception as e:
                print(f"실패: {e}")
                if attempt == self._retry:
                    print("모든 재시도 실패!")
                    return None
                print("재시도 중...")

real_service = APIService()
proxy = APIServiceProxy(real_service)

print(proxy.fetch_data("/users"))
print(proxy.fetch_data("/posts"))
```
