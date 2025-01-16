## 📌 Fragemnt 내부에 자식 프래그먼트를 사용할 때 Tip

### ❌ 문제 상황
- Fragment가 초기화 될 때 API가 두 번씩 호출 되는 문제 발생
- Fragment는 부모 프래그먼트 내의 자식 프래그먼트로 구현된 상태
- API가 호출되는 시점은 자식 프래그먼트의 onViewCreated에 로그를 출력해본 결과 **onViewCreated가 두 번 호출**되는 현상 발생

```
@AndroidEntryPoint
class SearchPlaceMainFragment : Fragment(R.layout.fragment_search_place_main) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        val binding = FragmentSearchPlaceMainBinding.bind(view)

        childFragmentManager.beginTransaction().apply {
            add(R.id.fragmentContainerView, listFragment, ScreenStatus.List.name)
            add(R.id.fragmentContainerView, mapFragment, ScreenStatus.Map.name)
            hide(mapFragment)
            commit()
        }
}

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/fragmentContainerView"
        android:name="kr.techit.lion.search.SearchListFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/tab_container" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

### 💡원인 분석
```
android:name="kr.techit.lion.search.SearchListFragment"
```
- ```FragmentContainerView```의 ```android:name``` 속성에 선언된 프래그먼트는 자동으로 추가된다.
- 현재 ```add``` 메소드를 사용해 프래그먼트를 추가하므로 프래그먼트를 중복 추가한다.

### Add 제거 결과
![image](https://github.com/user-attachments/assets/d393938c-ae2f-49fe-95d0-5dbc97a87b52)
