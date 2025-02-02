### 🍀 ExposedDropdownMenuBox
<hr/>

- 공식문서 : **https://composables.com/material3/exposeddropdownmenubox**
- dependency
> 2025.02.02 기준 material3의 최신 비전 :  ```1.4.0-alpha07``` 이다
```
implementation("androidx.compose.material3:material3:1.4.0-alpha02")
```
<img width="257" alt="image" src="https://github.com/user-attachments/assets/b5a5a7c6-7e6f-4a10-899b-7aa611e01bee" />

### Default Parameter
```
@ExperimentalMaterial3Api
@Composable
fun ExposedDropdownMenuBox(
    expanded: Boolean,
    onExpandedChange: (Boolean) -> Unit,
    modifier: Modifier = Modifier,
    content: @Composable ExposedDropdownMenuBoxScope.() -> Unit,
)
```

- ```expended``` : 메뉴의 확장 여부
- ```onExpandedChange``` : 드랍다운 메뉴가 선택되고 확장 상태가 변경되었을 때 이벤트
- ```content```: ExposedDropdownMenuBox내에 보여줄 컴포저블

### 공식 문서 Sample Code 분석

```
@Preview
@Composable
fun ExposedDropdownMenuSample() {
    val options: List<String> = SampleData.take(5)
    var expanded by remember { mutableStateOf(false) } // 메뉴 확장 여부 상태 저장
    val textFieldState = rememberTextFieldState(options[0])

    ExposedDropdownMenuBox(
        expanded = expanded,
        onExpandedChange = { expanded = it },
    ) {
        TextField(
            modifier = Modifier.menuAnchor(ExposedDropdownMenuAnchorType.SecondaryEditable),
            state = textFieldState,
            readOnly = true, // 텍스트 필드를 읽기 전용으로 설정
            lineLimits = TextFieldLineLimits.SingleLine, // 한 줄 입력 제한
            label = { Text("Label") }, // 라벨 설정
            trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = expanded) }, // 드롭다운 아이콘 추가
            colors = ExposedDropdownMenuDefaults.textFieldColors(), // 기본 색상 설정
        )

        ExposedDropdownMenu(
            expanded = expanded,
            onDismissRequest = { expanded = false }, // 메뉴 외부 클릭 시 닫힘
        ) {
            options.forEach { option ->
                DropdownMenuItem(
                    text = { Text(option, style = MaterialTheme.typography.bodyLarge) }, // 옵션 텍스트 스타일 지정
                    onClick = {
                        textFieldState.setTextAndPlaceCursorAtEnd(option) // 선택 시 해당 값을 텍스트 필드에 설정
                        expanded = false // 메뉴 닫기
                    },
                    contentPadding = ExposedDropdownMenuDefaults.ItemContentPadding, // 기본 패딩 설정
                )
            }
        }
    }
}

```

### Modifier.menuAnchor 분석
Modifier.menuAnchor : 드랍 다운 메뉴와 연결되어 어떤 방식으로 열릴지 결정한다.
> Anchor : DropDown Menu에 견괼되는 기준이 되는 요소
- PrimaryNotEditable → 읽기 전용(비편집) 텍스트 필드에 사용
- PrimaryEditable → 입력 가능한 텍스트 필드에 사용
- SecondaryEditable → 편집 가능한 필드 내부 아이콘 등에 사용 (접근성 활성화 시 포커스 상태에서 메뉴 열림)

<img width="747" alt="image" src="https://github.com/user-attachments/assets/b397334f-78d5-4818-9c93-d907f80ad2ab" />
