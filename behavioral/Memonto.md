# Memento Pattern

- 객체의 상태를 저장하고 이전 상태로 복원할 수 있게 하는 패턴으로, 캡슐화를 유지하면서 상태를 외부에 저장하는 패턴

## 장점

- 객체의 내부 상태를 외부에 저장하면서도 캡슐화를 유지함
- Undo/Redo 기능 구현이 용이함
- 상태 저장과 복원 로직을 분리하여 SRP 준수
- 객체의 상태를 언제든지 복원할 수 있음

## 단점

- 상태를 자주 저장하면 메모리 사용량이 증가함
- 큰 객체의 상태를 저장할 경우 성능 저하 발생 가능
- 메멘토 객체의 생명주기를 관리해야 함

<br>

## 클래스 다이어그램

![img](/img/memento.png)

## 코드

```py
class SettingsMemento:
    def __init__(self, audio_settings, display_settings, app_settings):
        self.audio_settings = audio_settings
        self.display_settings = display_settings
        self.app_settings = app_settings

    def __str__(self):
        return f"설정(오디오:{self.audio_settings}, 디스플레이:{self.display_settings}, 앱:{self.app_settings})"

class AudioSettings:
    def __init__(self):
        self.volume = 50

    def set_volume(self, volume):
        self.volume = volume
        print(f"🔊 볼륨이 {volume}%로 변경되었습니다.")

    def __str__(self):
        return f"볼륨:{self.volume}%"

class DisplaySettings:
    def __init__(self):
        self.brightness = 80
        self.theme = "라이트"

    def set_brightness(self, brightness):
        self.brightness = brightness
        print(f"💡 밝기가 {brightness}%로 변경되었습니다.")

    def set_theme(self, theme):
        self.theme = theme
        print(f"🎨 테마가 '{theme}'로 변경되었습니다.")

    def __str__(self):
        return f"밝기:{self.brightness}%, 테마:{self.theme}"

class AppSettings:
    def __init__(self):
        self.language = "한국어"
        self.notifications = True

    def set_language(self, language):
        self.language = language
        print(f"🌐 언어가 '{language}'로 변경되었습니다.")

    def toggle_notifications(self):
        self.notifications = not self.notifications
        status = "켜짐" if self.notifications else "꺼짐"
        print(f"🔔 알림이 {status}으로 변경되었습니다.")

    def __str__(self):
        return f"언어:{self.language}, 알림:{'켜짐' if self.notifications else '꺼짐'}"

class SettingsManager:
    def __init__(self):
        self.audio = AudioSettings()
        self.display = DisplaySettings()
        self.app = AppSettings()

    def show_current_settings(self):
        print(f"\n📱 현재 설정:")
        print(f"   🔊 {self.audio}")
        print(f"   💡 {self.display}")
        print(f"   🌐 {self.app}")

    def save_settings(self):
        print("💾 설정이 저장되었습니다!")
        audio_copy = AudioSettings()
        audio_copy.volume = self.audio.volume

        display_copy = DisplaySettings()
        display_copy.brightness = self.display.brightness
        display_copy.theme = self.display.theme

        app_copy = AppSettings()
        app_copy.language = self.app.language
        app_copy.notifications = self.app.notifications

        return SettingsMemento(audio_copy, display_copy, app_copy)

    def restore_settings(self, memento):
        self.audio.volume = memento.audio_settings.volume
        self.display.brightness = memento.display_settings.brightness
        self.display.theme = memento.display_settings.theme
        self.app.language = memento.app_settings.language
        self.app.notifications = memento.app_settings.notifications
        print(f"📂 설정이 복원되었습니다: {memento}")

settings_manager = SettingsManager()

print("=== 앱 설정 변경 및 저장 테스트 ===\n")

settings_manager.show_current_settings()
save1 = settings_manager.save_settings()

print("\n설정 변경 중...")
settings_manager.audio.set_volume(75)
settings_manager.display.set_brightness(90)
settings_manager.display.set_theme("다크")
settings_manager.show_current_settings()
save2 = settings_manager.save_settings()

print("\n더 많은 설정 변경...")
settings_manager.audio.set_volume(100)
settings_manager.app.set_language("English")
settings_manager.app.toggle_notifications()
settings_manager.show_current_settings()

print("\n" + "="*50)
print("첫 번째 저장으로 되돌리기:")
settings_manager.restore_settings(save1)
settings_manager.show_current_settings()

print("\n" + "="*50)
print("두 번째 저장으로 되돌리기:")
settings_manager.restore_settings(save2)
settings_manager.show_current_settings()
```
