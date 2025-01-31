### 🍀 Compose Navigation 백스택 제거하기
<hr/>

- Navigation에서 화면 이동시 이전 화면 백 스택을 정리하기 위해 ```popUpTo``` 메소드를 사용한다.
```
NavHost(
        navController = navController,
        startDestination = destination ?: IntroRoute.Splash.route
    ){
        composable(route = IntroRoute.Splash.route) {
            SplashScreen(
                videoUri = videoUri,
                navigateToMain = {
                    navigateToMain()
                },
                navigateToOnBoarding = {
                    navController.navigate(IntroRoute.OnBoarding.route){
                        popUpTo(IntroRoute.Splash.route)
                    }
                }
            )
        }
}
```

내부 구현을 살펴보자.

<img width="678" alt="image" src="https://github.com/user-attachments/assets/8ecb5dd1-274f-4959-83e0-8e2f2d6bcff5" />

#### 🔹 매개변수
1. ```route: String``` : ```popUpTo```함수의 인자로 전달받은 경로를 제외하고 그 위해 스택에 쌓인 화면을 모두 제거한다.

2. ```popUpToBuilder: PopUpToBuilder.() -> Unit = {}``` : 추가적인 설정을 할 수 있는 람다 함수 인자로 없어도 된다.

#### 🛠 실행 과정
- ```popUpToRoute = route``` → route 값을 저장한다.
- ```popUpToId = -1``` → ID 기반 popUpTo가 아닌 route 기반 popUpTo 사용한다.
- ```PopUpToBuilder()``` 객체를 생성하고, ```apply(popUpToBuilder)```를 사용하여 람다 내부의 설정을 적용한다.
- ```inclusive```와 ```saveState``` 설정이 존재할 경우 해당 값을 builder에서 가져와 적용한다.

#### 📌 추가 설정 (PopUpToBuilder)
🔹 inclusive : 인자로 전달 받은 (```popUpTo(route: String)```)route 화면을 유지할지 제거할지 설정
- true: ```route```도 함께 백스택에서 제거
- false(기본값): route 화면은 유지하고, 그 위의 화면만 제거
```
navController.navigate(IntroRoute.OnBoarding.route) {
    popUpTo(IntroRoute.Splash.route) { inclusive = true }
}
```
- ```inclusive = true``` 이므로 SpalshScreen도 제거됨

🔹 saveState : 이전 화면의 상태에 대한 저장 여부
- true: 백스택에서 제거된 화면의 상태를 저장 (나중에 복원 가능)
- false(기본값): 상태 저장 없이 제거
