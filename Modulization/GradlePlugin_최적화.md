## Build-Logic Gradle Plugin 최적화

### **Before**
- 다음과 같이 Custom Plugin을 등록할 때 플러그인을 등록하기 위한 코드가 반복된다 
````
plugins {
    `kotlin-dsl`
}

group = "kr.techit.lion.buildlogic"

java {
// ....
}

dependencies {
// ....
}

gradlePlugin {
    plugins {
        register("androidApplication") {
            id = "daongil.android.application"
            implementationClass = "kr.techit.lion.convention.AndroidApplicationConventionPlugin"
        }
        register("androidLibrary"){
            id = "daongil.android.library"
            implementationClass = "kr.techit.lion.convention.AndroidLibraryPlugin"
        }
        register("javaLibrary"){
            id = "daongil.java.library"
            implementationClass = "kr.techit.lion.convention.JavaLibraryPlugin"
        }
        register("androidRoom"){
            id = "daongil.android.room"
            implementationClass = "kr.techit.lion.convention.AndroidRoomConventionPlugin"
        }
        register("androidDataStore"){
            id = "daongil.androidx.datastore"
            implementationClass = "kr.techit.lion.convention.AndroidDataStoreConventionPlugin"
        }
        register("hilt"){
            id = "daongil.hilt.library"
            implementationClass = "kr.techit.lion.convention.HiltConventionPlugin"
        }
        register("androidFirebase"){
            id = "daongil.android.firebase"
            implementationClass = "kr.techit.lion.convention.AndroidApplicationFirebaseConventionPlugin"
        }
        register("androidMoshi"){
            id = "daongil.android.moshi"
            implementationClass = "kr.techit.lion.convention.MoshiConventionPlugin"
        }
        register("androidRetrofit"){
            id = "daongil.android.retrofit"
            implementationClass = "kr.techit.lion.convention.AndroidRetrofitConventionPlugin"
        }
        register("daongilApp"){
            id = "daongil.app"
            implementationClass = "kr.techit.lion.convention.DaongilAppConventionPlugin"
        }
        register("daongilDomain"){
            id = "daongil.domain"
            implementationClass = "kr.techit.lion.convention.DaongilDomainConventionPlugin"
        }
        register("daongilFeature"){
            id = "daongil.feature"
            implementationClass = "kr.techit.lion.convention.DaongilFeatureConventionPlugin"
        }
    }
}

````

### **Solution**
```
fun NamedDomainObjectContainer<PluginDeclaration>.pluginRegister(data: Pair<String, String>) {
    val (pluginName, className) = data
    register(pluginName) {
        id = "daongil.$pluginName"
        implementationClass = "kr.techit.lion.convention.$className"
    }
}
```
- NamedDomainObjectContainer<PluginDeclaration>:
  - Gradle의 DSL에서 사용되는 컨테이너 객체로, 특정 타입(PluginDeclaration)의 객체를 등록하거나 관리한다.


- 등록할 플러그인을 List<Pair<String, String>> 형태로 정의하고 위 선언된 확장함수를 적용한다.

```
plugins {
    `kotlin-dsl`
}

group = "kr.techit.lion.buildlogic"

java { // ...}

dependencies { // ... }

gradlePlugin {
    val conventionPluginClasses = listOf(
        "android.application" to "AndroidApplicationConventionPlugin",
        "android.library" to "AndroidLibraryPlugin",
        "java.library" to "JavaLibraryPlugin",
        "android.room" to "AndroidRoomConventionPlugin",
        "androidx.datastore" to "AndroidDataStoreConventionPlugin",
        "hilt.library" to "HiltConventionPlugin",
        "android.firebase" to "AndroidApplicationFirebaseConventionPlugin",
        "android.moshi" to "MoshiConventionPlugin",
        "android.retrofit" to "AndroidRetrofitConventionPlugin",
        "app" to "DaongilAppConventionPlugin",
        "domain" to "DaongilDomainConventionPlugin",
        "feature" to "DaongilFeatureConventionPlugin"
    )
    plugins {
        conventionPluginClasses.forEach { pluginClass ->
            pluginRegister(pluginClass)
        }
    }
}

fun NamedDomainObjectContainer<PluginDeclaration>.pluginRegister(data: Pair<String, String>) {
    val (pluginName, className) = data
    register(pluginName) {
        id = "daongil.$pluginName"
        implementationClass = "kr.techit.lion.convention.$className"
    }
}
```
