---
tags: 
  - 완성
  - IT
  - Gradle
aliases: 
title: Custom Task와 Plugin 개발해보기 by buildSrc
date: 2023-10-16
---
작성 날짜: 2023-10-16
작성 시간: 23:48


----
## 내용(Content)
### 빌드 스크립트를 활용한 Custom Task

커스텀 Task를 설계해보자. 커스텀 Task는 Java나 Groovy, kotlin으로 작성할 수도 있고, 간편하게 build script를 이용해 작성할 수도 있다. 여러 가지 방법을 통해 custom Task를 만드는 방법을 알아보자

#### simple task 작성하기

```kotlin
tasks.register("myTask") {  
    description = "나의 태스크"  
    doFirst {  
        println("hello first")  
    }  
    println("run")  
    doLast {  
        println("hello last")  
    }  
}
```

task를 등록하는데는 2가지 방법이 있다. 하나는 **register**, 또 다른 하나는 **create**이다.

Task를 등록할 때 register와 create의 차이는 [[Task create vs register]]를 참고하자

kotlin의 by 문법을 이용해 작성하면 task를 변수로서 사용 가능하다.

```kotlin
val myTask by tasks.register("myTask") {  
    description = "tasktask!!!"  
    doFirst {  
        println("hello first")  
    }  
    println("run")  
    doLast {  
        println("hello last")  
    }  
}
```

#### copy task 작성하기

```kotlin
tasks.register<Copy>("copyTask") {  
    from("src/main")  
    into("tasks")  
}
```


### buildSrc를 활용해 독립적인 Task와 Plugin 구축하기
```java
// path: buildSrc/inject/JavaInjectTask.java
public class JavaInjectTask extends DefaultTask {  
  
    private final String message;  
  
    @Inject  
    public JavaInjectTask(String message) {  
       this.message = message;  
    }  
  
    @TaskAction  
    void print() {  
       System.out.println("my Message is " + this.message);  
    }  
}
```

DefaultTask를 상속하면 CustomTask를 만들 수 있다. 

@Inject는 생성자에 적용해서 인수를 받도록 설정한다.
@TaskAction은 Task의 동작을 정의한다.

이제 이 Task로 Plugin을 만들어서 등록해보자

```java
// path: buildSrc/inject/JavaInjectPlugin.java
public class JavaInjectPlugin implements Plugin<Project> {  
    @Override  
    public void apply(Project project) {  
       project.getTasks().register("injectTask", JavaInjectTask.class, "my task");  
    }  
}
```

이제 해당 플러그인에 id를 부여할 것이다.

```kotlin
// path: buildSrc/build.gradle.kts
plugins {  
    `java-gradle-plugin`  
}  
  
gradlePlugin {  
    plugins {  
        create("inject-plugin") {  
            id = "inject"  
            implementationClass = "inject.JavaInjectPlugin"  
        }  
    }
}
```

플러그인에 java-gradle-plugin을 등록한다. 그 이후
gradlePlugin을 정의한다.

create 다음에는 플러그인 네임, 그리고 다른 gradle에서 plugin 로드에 필요한 id를 입력한다.
implementationClass에는 buildSrc 경로에서 내가 만든 자바 플러그인 경로를 넣어준다.

플러그인을 여러 Task와 설정 정보들의 모음이라 생각하면 된다. 

### Extension과 Lazy initialization

Plugin 초기에 생성자에 주입하는 방식이 아닌 Plugin에 Task를 등록하고 나중에 변수를 주입하고 싶을 수도 있다. 이럴 때  extension를 활용하면 된다.

Extension의 task 로직이 간단하면 Extension과 ExtensionPlugin만으로 충분하다. 만약 복잡하다면 ExtensionTask를 따로 분리할 수도 있다. 간단한 버전과 조금 더 복잡한 버전 모두 소개하겠다.


#### 간단한 Task의 경우

우선 lazy하게 변수를 담아서 Inject할 extension을 선언해준다.

```java
// buildSrc/plugin/GreetingPluginExtension.java
public interface GreetingPluginExtension {  
    Property<String> getMessage();  
}
```

인터페이스를 선언할 때는 여러 메서드를 사용해도 되지만 getter 메서드에 Property로 감싸서 반환하도록 하자.

```java
// buildSrc/plugin/GreetingPlugin.java
public class GreetingPlugin implements Plugin<Project> {  
  
    @Override  
    public void apply(Project target) {  
       ExtensionContainer extensions = target.getExtensions();  
       GreetingPluginExtension greeting = extensions.create("greeting", GreetingPluginExtension.class);  
       target.task("hello")  
          .doLast(task -> System.out.println("Hello from " + greeting.getMessage().get() + "!"));  
    }  
}
```

그 이후에는 project에서 extension을 불러온다음 extension에 GreetingPluginExtension 인터페이스를 담아서 extension을 생성해주고 새로 task를 생성한 다음 해당 extension의 property를 가져와 활용하면 된다.

그 다음 똑같이 빌드 스크립트에 다음과 같이 설정해준다.
```kotlin
// buildSrc/build.gradle.kts
plugins {  
    `java-gradle-plugin`  
}  
  
gradlePlugin {  
    plugins {  
        create("greeting-plugin") {  
            id = "greeting"  
            implementationClass = "plugin.GreetingPlugin"  
        }  
    }
}
```

그러나 만약 Task가 굉장히 복잡해지면 Plugin 코드가 복잡해지므로 Task의 분리가 필요할 수 있다.

외부 gradle에서 사용 시 다음과 같다.
```kotlin
plugins {
	"greeting"
}

// configure를 활용해 extension 변수를 정의한다
configure<GreetingPluginExtension> {  
    message = "LHG"  
}
```

그리고 task를 실행하면 다음과 같은 결과를 얻는다.
![[Pasted image 20231019211312.png]]

#### Task 분리 버전
이번엔 Task 분리 버전을 해보자

```java
public interface JavaMessageExtension {  
    Property<Integer> getCode();  
}
```

```java
public class JavaMessageExtensionTask extends DefaultTask {  
  
    private final Property<Integer> code;  
  
    @Inject  
    public JavaMessageExtensionTask(Project project) {  
       JavaMessageExtension messageExtension = project.getExtensions().getByType(JavaMessageExtension.class);  
       this.code = messageExtension.getCode();  
    }  
  
    @TaskAction  
    void print() {  
       System.out.println("Code :" + code.get());
```

생성자 인자로 Project를 넘긴다. 이러면 lazy하게 사용가능하고  configure 를 활용해 configure phase에 인자를 초기화 가능하다.

```java
public class JavaMessageExtensionPlugin implements Plugin<Project> {  
    @Override  
    public void apply(Project project) {  
       ExtensionContainer extensions = project.getExtensions();  
       extensions.create("messageExtension", JavaMessageExtension.class);  
       project.getTasks().register("JavaMessageTask", JavaMessageExtensionTask.class);  
    }  
}
```

마지막으로 extension과 task를 등록해준다.

plugin 정의를 하고 똑같이 다른 모듈에서 호출하면 다음과 같이 결과가 잘 나온다.

simple 버전과 똑같이 사용해보자
![[Pasted image 20231019225804.png]]

다음과 같이 원하는 결과를 얻었다
## 질문 & 확장

(없음)

## 출처(링크)
- https://docs.gradle.org/current/userguide/custom_plugins.html#custom_plugins
- https://velog.io/@jeongyunsung/Gradle%EB%86%80%EC%9D%B43-Custom-Task#task-1
- 
## 연결 노트
- [[Task create vs register]]









