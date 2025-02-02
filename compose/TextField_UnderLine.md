### 🍀 TextField 밑줄 제거하기
<hr/>

<img width="231" alt="image" src="https://github.com/user-attachments/assets/bfa29622-a5af-40e7-b67b-10f15dde2e86" />

- TextField는 기본적으로 underline을 가지고 있다.
- TextField에서 underline은 indicator라 표현한다.
- TextField의 내부 구현을 살펴보자

<img width="669" alt="image" src="https://github.com/user-attachments/assets/7b1e66a5-28bf-46d2-a84f-d9d1d0158360" />

- 이 중 indicator는 ```colors``` 에 ```TextFieldDefaults.textFieldColors```를 커스텀해 변경할 수 있다.
- ```focusedIndicatorColor = Color.Trasnparent``` : TextField에 focus가 되었을 때의 indicatorColor를 설정
- ```unfocusedIndicatorColor = Color.Trasnparent``` : TextField에 focus가 되지 않았을 때의 indicatorColor를 설정

<br/>

```
TextField(
    state = textFieldState,
    colors = ExposedDropdownMenuDefaults.textFieldColors(
        focusedIndicatorColor = Color.Transparent,
        unfocusedIndicatorColor = Color.Transparent,
    )
)
```

### 결과
<img width="237" alt="image" src="https://github.com/user-attachments/assets/64c9dfa0-bef7-407f-ab41-09b574c012bc" />
