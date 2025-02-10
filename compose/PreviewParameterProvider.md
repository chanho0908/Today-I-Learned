## 🍀 PreviewParameterProvider
<hr/>

- Preview에 State를 넘기다보면 Preview 관련 코드가 굉장히 길어질 수 있다.
- 예를 들면 이렇게 ...
```
@Composable
@Preview(showBackground = true)
fun ListSearchScreenPreview(){
  ListSearchScreen( 
       uistate =  ListSearchUiState(
            place = listOf(
                PlaceModel(
                    placeName = "달빛곳간",
                    placeAddr = "전북특별자치도 임실군 삼계면 세심길 26",
                    placeId = 3354268,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/31/3354231_image2_1.png",
                    disability = listOf("1", "2"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "오로시예술공방",
                    placeAddr = "전라남도 화순군 능주면 잠정햇살길 22",
                    placeId = 3354209,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/81/3354181_image2_1.jpg",
                    disability = listOf("1"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "삼포 만화북투어",
                    placeAddr = "인천광역시 동구 어촌로5번길 3-3 (만석동, 만석3 우리집)",
                    placeId = 3354154,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/88/3354088_image2_1.jpg",
                    disability = listOf("1", "2", "4"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "소잉",
                    placeAddr = "광주광역시 북구 첨단연신로108번길 31-15 (신용동)",
                    placeId = 3353909,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/08/3353808_image2_1.jpg",
                    disability = listOf("1", "2"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "꾸꾸네",
                    placeAddr = "경상북도 청도군 운문면 운문로 1551",
                    placeId = 3353714,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/99/3353599_image2_1.jpg",
                    disability = listOf("1", "2"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "레인보우힐링센터",
                    placeAddr = "충청북도 영동군 영동읍 영동힐링로 95",
                    placeId = 3346305,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/85/3346285_image2_1.jpg",
                    disability = listOf("1", "2", "4"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "완주 고산공원 무궁화테마식물원",
                    placeAddr = "전북특별자치도 완주군 고산면 고산휴양림로 89",
                    placeId = 3345112,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/92/3345092_image2_1.jpg",
                    disability = listOf("1", "2", "4"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "가파도 소망전망대",
                    placeAddr = "제주특별자치도 서귀포시 대정읍 가파리",
                    placeId = 3344437,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/99/3344399_image2_1.jpg",
                    disability = listOf("1"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "대봉캠핑랜드",
                    placeAddr = "경상남도 함양군 병곡면 원산지소길 192",
                    placeId = 3344166,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/28/3344128_image2_1.jpg",
                    disability = listOf("1", "2"),
                    itemCount = 5640
                ),
                PlaceModel(
                    placeName = "소도야영장",
                    placeAddr = "강원특별자치도 태백시 천제단길 181 (소도동)",
                    placeId = 3344099,
                    placeImg = "http://tong.visitkorea.or.kr/cms/resource/87/3344087_image2_1.jpg",
                    disability = listOf("1", "2"),
                    itemCount = 5640
                )
            ),
            area = AreaModel(
                persistentListOf(
                    "서울특별시", "인천광역시", "대전광역시", "대구광역시", "광주광역시", "부산광역시",
                    "울산광역시", "세종특별자치시", "경기도", "강원특별자치도", "충청북도", "충청남도",
                    "경상북도", "경상남도", "전북특별자치도", "전라남도"
                )
            ),
            sigungu = SigunguModel(
                persistentListOf(
                    "강남구", "강동구", "강북구", "강서구", "관악구", "광진구", "구로구", "금천구", "노원구", "도봉구",
                    "동대문구", "동작구", "마포구", "서대문구", "서초구", "성동구", "성북구", "송파구", "양천구", "영등포구",
                    "용산구", "은평구", "종로구", "중구", "중랑구", "강화군", "계양구", "미추홀구", "남동구", "동구",
                    "부평구", "서구", "연수구", "옹진군", "대덕구", "동구", "서구", "유성구", "중구", "남구", "달서구",
                    "달성군", "동구", "북구", "서구", "수성구", "중구", "군위군", "광산구"
                )
            )
        )
    )
}
```

### PreviewParameterProvider를 사용하면 외부에서 State를 주입할 수 있따~
<img width="506" alt="image" src="https://github.com/user-attachments/assets/139e6c2b-e59d-4315-ac9c-38dc9339ca32" /> <br/>
- PreviewParameterProvider는 Interface로 내부에 데이터를 ```sequence```형태로 구현한다.
- 위 인자 중 PlaceModel 하나만 분리해보자
- ```PreviewParameterProvider```인터페이스를 상속하는 Class를 만들고 values 를 override하여 필요한 데이터를 만들어준다.
```
class PlaceModelParameterProvider : PreviewParameterProvider<List<PlaceModel>> {
    override val values = sequenceOf(
        listOf(
            PlaceModel(
                placeName = "달빛곳간",
                placeAddr = "전북특별자치도 임실군 삼계면 세심길 26",
                placeId = 3354268,
                placeImg = "http://tong.visitkorea.or.kr/cms/resource/31/3354231_image2_1.png",
                disability = listOf("1", "2"),
                itemCount = 5640
            ),
            PlaceModel(
                placeName = "오로시예술공방",
                placeAddr = "전라남도 화순군 능주면 잠정햇살길 22",
                placeId = 3354209,
                placeImg = "http://tong.visitkorea.or.kr/cms/resource/81/3354181_image2_1.jpg",
                disability = listOf("1"),
                itemCount = 5640
            ),
            PlaceModel(
                placeName = "삼포 만화북투어",
                placeAddr = "인천광역시 동구 어촌로5번길 3-3 (만석동, 만석3 우리집)",
                placeId = 3354154,
                placeImg = "http://tong.visitkorea.or.kr/cms/resource/88/3354088_image2_1.jpg",
                disability = listOf("1", "2", "4"),
                itemCount = 5640
            )
      )
    )
}
```

- Preview에서 사용할 때는 ```@PreviewParameter(주입하려는 Class Name::class)``` 어노테이션을 사용
- 인자로 주입하려는 Class Name을 전달한다.
```
@Composable
@Preview
fun AreaExposedDropdownMenuPreview(
    @PreviewParameter(CityParameterProvider::class)
    items: AreaModel
) {}
```

## 활용해보기

- Composable을 만들다보면 컴포넌트들을 여러개로 분리하여 최상위 컴포즈에서 하위로 상태를 전달하는 형태로 구현한다.
- 하나의 PreviewParameterProvider Class도 상태가 많아진다면 이 상태를 각각의 PreviewParameterProvider class로 분리할 수 있다.
- PlaceModelParameterProvider을 분리해 다른 PreviewParameterProvider class에서도 활용할 수 있다.

```
class ListSearchParameterProvider : PreviewParameterProvider<ListSearchUiState> {
    override val values = sequenceOf(
        ListSearchUiState(
            place = PlaceModelParameterProvider().values.first(),
            area = AreaModel(),
            sigungu = SigunguModel()
        )
    )
}
```
#### ```PlaceModelParameterProvider().values```를 사용해 sequence 인자를 추출할 수 있다.

## Sealed interface와 조합해 사용해보기
- 다음과 같이 하나의 화면에서 지역 리스트, 선택된 지역에 따른 시/군/구 리스트를 보여줘야 한다.
```
sealed interface City{
    val names: List<String>

    data class AreaModel(
        override val names: List<String>,
    ) : City

    data class SigunguModel(
        override val names: List<String>
    ) : City
}
```

- ```PreviewParameterProvider```의 Interface Type을 최상위 인터페이스 Type으로 선언 후 각각의 하위 타입 Model Class를 만든다. 
```
class CityParameterProvider : PreviewParameterProvider<City> {
    override val values = sequenceOf(
        AreaModel(
            persistentListOf(
                "서울특별시", "인천광역시", "대전광역시", "대구광역시", "광주광역시", "부산광역시",
                "울산광역시", "세종특별자치시", "경기도", "강원특별자치도", "충청북도", "충청남도",
                "경상북도", "경상남도", "전북특별자치도", "전라남도"
            )
        ),
        SigunguModel(
            persistentListOf(
                "강남구", "강동구", "강북구", "강서구", "관악구", "광진구", "구로구", "금천구", "노원구", "도봉구",
                "동대문구", "동작구", "마포구", "서대문구", "서초구", "성동구", "성북구", "송파구", "양천구", "영등포구",
                "용산구", "은평구", "종로구", "중구", "중랑구", "강화군", "계양구", "미추홀구", "남동구", "동구",
                "부평구", "서구", "연수구", "옹진군", "대덕구", "동구", "서구", "유성구", "중구", "남구", "달서구",
                "달성군", "동구", "북구", "서구", "수성구", "중구", "군위군", "광산구"
            )
        )
    )
}
```
- sequence의 ```elementAt``` 함수를 사용해 n번째 인자를 추출 후 하위 타입으로 캐스팅한다.
```
class ListSearchParameterProvider : PreviewParameterProvider<ListSearchUiState> {
    override val values = sequenceOf(
        ListSearchUiState(
            place = PlaceModelParameterProvider().values.first(),

            // City의 AreaModel Type 으로 Casting
            area = CityParameterProvider().values.elementAt(0) as City.AreaModel,

            // City의 SigunguModel Type 으로 Casting
            sigungu = CityParameterProvider().values.elementAt(1) as City.SigunguModel,
        )
    )
}
```
- Preview에선 ```when - is``` 로 Type Casting 하여 사용할 수 있다.
```
@Composable
@Preview
fun AreaExposedDropdownMenuPreview(
    @PreviewParameter(CityParameterProvider::class)
    items: City
) {
    when (items) {
        is AreaModel -> {
            CityExposedDropdownMenu(
                area = items,
                sigungu = SigunguModel(names = emptyList()),
            )
        }

        is SigunguModel -> {
            CityExposedDropdownMenu(
                area = AreaModel(names = emptyList()),
                sigungu = items,
            )
        }
    }
}
```
- PreviewParameter Class에 만든 sequence의 size 만큼 Preview가 생성된다.
<img width="340" alt="image" src="https://github.com/user-attachments/assets/85c850db-8106-42dc-94b9-e8406021062c" />

