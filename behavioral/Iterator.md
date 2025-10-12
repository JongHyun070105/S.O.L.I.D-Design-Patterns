# Iterator Pattern

- 컬렉션의 내부 구조를 노출하지 않고 순차적으로 요소에 접근할 수 있게 하는 패턴

## 장점

- 컬렉션의 내부 구조를 숨기고 일관된 인터페이스로 접근 가능
- 동일한 컬렉션에 대해 여러 개의 순회가 동시에 진행 가능
- 새로운 컬렉션이나 반복자를 추가해도 기존 코드 수정이 불필요함 -> OCP 준수
- 컬렉션 순회 로직을 분리하여 SRP 준수

## 단점

- 단순한 컬렉션의 경우 오히려 코드가 복잡해질 수 있음
- 직접 접근보다 성능이 떨어질 수 있음

<br>

## 클래스 다이어그램

![img](/img/iterator.png)

## 코드

```py
from abc import ABC, abstractmethod

class Song:
    def __init__(self, title, artist):
        self.title = title
        self.artist = artist

    def __str__(self):
        return f"{self.title} - {self.artist}"

class Iterator(ABC):
    @abstractmethod
    def has_next(self):
        pass

    @abstractmethod
    def next(self):
        pass

class PlaylistIterator(Iterator):
    def __init__(self, songs):
        self.songs = songs
        self.position = 0

    def has_next(self):
        return self.position < len(self.songs)

    def next(self):
        if self.has_next():
            song = self.songs[self.position]
            self.position += 1
            return song
        return None

class Playlist:
    def __init__(self, name):
        self.name = name
        self.songs = []

    def add_song(self, song):
        self.songs.append(song)

    def create_iterator(self):
        return PlaylistIterator(self.songs)

playlist = Playlist("내가 좋아하는 노래")

playlist.add_song(Song("Dynamite", "BTS"))
playlist.add_song(Song("Butter", "BTS"))
playlist.add_song(Song("봄날", "BTS"))
playlist.add_song(Song("Love Dive", "IVE"))

print(f"🎵 '{playlist.name}' 재생 중...\n")

iterator = playlist.create_iterator()

track_number = 1
while iterator.has_next():
    song = iterator.next()
    print(f"{track_number}. ♪ {song}")
    track_number += 1

print("\n재생 완료! 🎧")
```
