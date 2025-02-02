### 🍀 setTextAndPlaceCursorAtEnd
<hr/>

> 검색어 자동완성 같은 기능을 만들 때 유용할 것 같으니 기록하고 나중에 사용하자

```
val textFieldState = rememberTextFieldState(options[0])
textFieldState.setTextAndPlaceCursorAtEnd(option)
```

<img width="715" alt="image" src="https://github.com/user-attachments/assets/846781d9-c851-46df-b43a-c6ecc00db8b5" />

- TextFieldState의 확장함수로 선언되어 있는 setTextAndPlaceCursorAtEnd
- 사용자가 입력한 값을 업데이트하고, 자동으로 커서를 마지막 위치로 이동시킨다.
- ```replace(0, length, text)``` : 기존 텍스트를 새 텍스트로 교체
- ```placeCursorAtEnd()``` : 커서를 텍스트 끝으로 이동

[Screen_recording_20250202_124541.webm](https://github.com/user-attachments/assets/a8ff8fb1-6026-4b8b-833b-476806a02e07)
