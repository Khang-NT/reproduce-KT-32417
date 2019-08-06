## Reproduce

So here, I have `getHelloString` in `moduleB`

```kotlin
fun getHelloString(firstName: String): String {
    return "Hello $firstName"
}
```

Then I will change it to:

```kotlin
fun getHelloString(firstName: String, lastName: String = ""): String {
    return "Hello $firstName $lastName"
}
```

**And the android app will crash**

```
java.lang.NoSuchMethodError: No static method getHelloString(Ljava/lang/String;)Ljava/lang/String; in class Lcom/example/module/b/HelloStringKt; or its super classes (declaration of 'com.example.module.b.HelloStringKt' appears in /data/app/com.example.myapplication-x5EvJ6MhyrRk76EACAwBsg==/base.apk)
```

Steps:

1. Clone the project
2. Publish `moduleB` v1.0.0 by `./gradlew moduleB:publishToMavenLocal`
3. Publish `moduleA` v1.0.0 by `./gradlew moduleA:publishToMavenLocal`
4. Run module `app`, it should work fine and show "Hello Khang" text
5. **Update method `getHelloString` in `moduleB`**
```kotlin
fun getHelloString(firstName: String, lastName: String = ""): String {
    return "Hello $firstName $lastName"
}
```
6. Update `moduleB/build.gradle`, set new version `"1.0.1"`
7. Re-publish `moduleB` v1.0.1 by `./gradlew moduleB:publishToMavenLocal`
8. Update `app/build.gradle` set `moduleB` version to `1.0.1`
9. Rebuilt and install `app` successfully, but it crashes.
