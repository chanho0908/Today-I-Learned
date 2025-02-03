## 🍀 Material3 Chip Custom하기
<br/>

### 다음과 같이 선택 되었을 때 Border와 아이콘, 텍스트의 색상이 변경되는 Chip을 만들어보자

<img width="68" alt="image" src="https://github.com/user-attachments/assets/2ec7ba8a-33e2-49d1-a696-70820247ad41" /> <img width="73" alt="image" src="https://github.com/user-attachments/assets/7dd7b145-e6b7-4ea9-b542-4ca490c8b639" />

### Basic
- 공식문서를 참고하면 많은 유형의 칩이 있다<br/>
https://developer.android.com/develop/ui/compose/components/chip?hl=ko
- 이 중 사용자의 선택 여부에 따라 커스텀할 수 있는 ```FilterChip```을 사용한다.

```
@Composable
fun FilterChipExample() {
    var selected by remember { mutableStateOf(false) }

    FilterChip(
        onClick = { selected = !selected },
        label = {
            Text("Filter chip")
        },
        selected = selected,
        leadingIcon = if (selected) {
            {
                Icon(
                    imageVector = Icons.Filled.Done,
                    contentDescription = "Done icon",
                    modifier = Modifier.size(FilterChipDefaults.IconSize)
                )
            }
        } else {
            null
        },
    )
}
```
- Sample Code 기준으로 각 파라미터는 다음과 같다
  - ```label``` : Filter 내의 텍스트
  - ```leadingIcon``` : Filter 내의 아이콘

## Chip Custom
### 1. Border
❌ 일반적인 방식대로 ```Modifer.border()```로 border를 설정하게 되면 다음과 같이 Chip이 아닌 Chip Composable을 가지고 있는 
Composable(?)의 테두리에 적용된다.<br/>
![image](https://github.com/user-attachments/assets/e95a1d1f-f3e9-498b-a820-3e78b762c818)

- chip의 border를 커스텀하기 위해선 chip의 내부 구현 코드를 살펴봐야한다.
- ```FilterChipDefaults.filterChipBorder(enabled, selected)``` 사용하여 커스텀 할 수 있다.
<img width="658" alt="image" src="https://github.com/user-attachments/assets/ffb7b0ad-a55a-43b7-9090-9c8e0c203d16" />

- ```filterChipBorder```의 내부 구현을 살펴보자
![image](https://github.com/user-attachments/assets/c87563ae-f978-41b8-9a05-5da0c2a0a8ba)

- ```enable```과 ```selected```를 필수 인자로 받고 있는데, 함수내에서 위 인자에 따라 색상을 판별하기 위함이다.

```
@Composable
fun DisabilityOptionChip(
//...
) {
    val title = stringResource(id = item.contentDescriptionResourceId)

    val selectedColor = colorResource(id = R.color.detail_category_selected_color)
    val unselectedColor = colorResource(id = R.color.detail_category_unselected_color)
    val mainColor = if (selected) selectedColor else unselectedColor

    FilterChip(
        onClick = {
            onClick(item.code)
        },
        modifier = modifier,
        border = FilterChipDefaults.filterChipBorder(
            borderWidth = 1.dp,
            enabled = true,
            selected = selected,
            borderColor = mainColor,
            selectedBorderWidth = 2.dp,
            selectedBorderColor = selectedColor
        ),
        selected = selected,
        label = @Composable {
            Text(
                text = title,
                color = mainColor
            )
        },
        leadingIcon = @Composable {
            Icon(
                imageVector = ImageVector.vectorResource(id = item.iconResourceId),
                contentDescription = title,
                modifier = Modifier.size(20.dp),
                tint = mainColor
            )
        }
    )
}
```
- 위와 같이 ```FilterChipDefaults.filterChipBorder```을 커스텀 하면 Chip의 border을 커스텀 할 수 있다.
