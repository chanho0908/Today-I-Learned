## Compose Build System Trouble Shooting
<hr/>

### 📌 문제 상황
<img width="782" alt="image" src="https://github.com/user-attachments/assets/2bad3e51-962a-4e01-88e2-20e4256fb05b" />
<br/>

- 기존 XML View System을 Compose로 부분적으로 마이그레이션 하기 위해 컴포즈에 대한 Convention Plugin 설정을 하던 중 다음 에러가 발생했다.
- Plugins.ANDROID_COMPOSE 상수는 다음 코드다. ```org.jetbrains.kotlin.plugin.compose```

```
An exception occurred applying plugin request [id: 'daongil.presentation', version: '1.0.0']
> Failed to apply plugin 'daongil.android.compose'.
   > Plugin with id 'org.jetbrains.kotlin.plugin.compose' not found.

```
- Compose Plugin을 위한 설정에서 문제가 발생한 것을 확인, ```pluginManager.apply(Plugins.ANDROID_COMPOSE)```을 제거하고 다시 빌드 했으나 다음 에러가 발생했다.
```
e: This version (1.3.2) of the Compose Compiler requires Kotlin version 1.7.20 but you appear to be using Kotlin version 1.9.22 which is not known to be compatible.
Please fix your configuration (or `suppressKotlinVersionCompatibilityCheck` but don't say I didn't warn you!).
```

### 🍀 원인 분석
- 공식 문서에 의하면 첫 번째 방식(Gradle Plugin을 사용해 직접 Compose Complier를 선언)은 **Kotlin 2.0** 이상부터 사용 할 수 있는 방식이다.
<img width="901" alt="image" src="https://github.com/user-attachments/assets/8fb47b9b-ad74-4fdf-891e-d3adea916975" />

- 현재 프로젝트의 Kotlin 버전은 1.9.22으로 2.0 이하이기 때문에 Gradle Plugin을 사용하는 방식은 사용 할 수 없다.
- 대신 Compose Compiler 의존성을 추가하기 위해선 프로젝트에 Google Maven 저장소를 추가해야한다.
- 공식문서를 참고해 현재 코틀린 버전에 맞는 컴파일러 버전을 확인하자.
- https://developer.android.com/jetpack/androidx/releases/compose-kotlin?hl=ko

- composeOptions Block 내에 kotlinExtensionVersion에서 compose를 사용하기 위한 컴파일러의 확장 버전을 지정한다.
- compose-compiler Version은 VersionCatalog에 선언해주었다.
- ```compose-compiler = "1.5.10"```
<img width="857" alt="image" src="https://github.com/user-attachments/assets/8285fb5f-4e6e-4457-b976-9e69503c136c" />
