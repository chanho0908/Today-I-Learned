## 🚨 TabRow Trouble Shooting

### 📌 문제 상황
- Tab Indicator가 Tab 너비 전체를 차지하는 문제가 발생했다.
<img width="420" alt="image" src="https://github.com/user-attachments/assets/65d1e23a-9dbf-44eb-ad8c-ab715a2e15b4" />

```
TabRow(
        selectedTabIndex = selectedTabIndex,
        contentColor = Color.Transparent,
        modifier = modifier,
        indicator = { tabPositions ->
            TabRowDefaults.SecondaryIndicator(
                Modifier
                    .height(2.dp)
                    .clip(RoundedCornerShape(2.dp)),
                color = Color(0xFFE53935)
            )
        },
    ) {
        enumValues<CategoryStatus>().forEachIndexed { index, value ->
            Tab(
                selected = selectedTabIndex == index,
                onClick = { selectedTabIndex = index },
                text = {
                    Text(
                        text = stringResource(value.titleResourceId),
                        color = if (selectedTabIndex == index) Color(0xFFE53935) else Color.Gray,
                    )
                }
            )
        }
    }
```

### 🧐 원인 분석 
- ```tabIndicatorOffset``` 함수를 사용해 현재 탭 위치를 계산하지 않아 고정된 높이로 설정되어있다.
<img width="621" alt="image" src="https://github.com/user-attachments/assets/d1f722c0-9c61-417f-800e-dd7372bb233b" />
<br/>

- ```tabIndicatorOffset```
  - SecondaryIndicator 내부에 Modifier의 확장 함수로 선언 되어있다.
  - 현재 Tab의 Position을 인자로 받아 인디케이터의 OffSet과 너비를 계산한다.

### 결과
```
TabRow(
        selectedTabIndex = selectedTabIndex,
        contentColor = Color.Transparent,
        modifier = modifier,
        indicator = { tabPositions ->
            TabRowDefaults.SecondaryIndicator(
                Modifier
                    .tabIndicatorOffset(tabPositions[selectedTabIndex])
                    .height(2.dp)
                    .clip(RoundedCornerShape(2.dp)),
                color = Color(0xFFE53935)
            )
        },
    ) 
```
<img width="406" alt="image" src="https://github.com/user-attachments/assets/6fdbe149-2570-4a3f-8578-7e6da2ddd27d" />

