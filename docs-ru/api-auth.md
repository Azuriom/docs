# AzAuth

AzAuth - это API, который позволяет аутентифицировать пользователей сайта на базе Azuriom на любой платформе.

## Скачивание

Вы можете скачать исходные тексты и релизы AzAuth перейдя по ссылке ниже. 
[GitHub](https://github.com/Azuriom/AzAuth/releases).

Если Вы используете менеджер зависимостей, то возможно добавить AzAuth как
зависимость следующим образом:

> {warn} Репозиторий Maven пока недоступен, но появится очень скоро!

### Gradle

в `build.gradle`:

```groovy
repositories {
    maven { url 'https://repo.azuriom.com/' }
}
```
```groovy
dependencies {
    implementation 'com.azuriom:azauth:1.0.0-SNAPSHOT'
}
```

### Maven

в `pom.xml`:
```xml
<repositories>
    <repository>
        <id>azuriom-repo</id>
        <url>https://repo.azuriom.com</url>
    </repository>
</repositories>
```
```xml
<dependencies>
    <dependency>
        <groupId>com.azuriom</groupId>
        <artifactId>azauth</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

## Использование AzAuth (Java)

Перед использованием AzAuth, пожалуйста, убедитесь, что API активирован, перейдя в
в настройки сайта через вашу админ-панель.

### Использование с [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(для лаунчера майнкрафт)_

Для начала работы, добавьте AzAuth в качестве зависимости к вашему проекту.
Также, если Вы используете [OpenAuth] (https://github.com/Litarvan/OpenAuth/), рекомендуется удалить его,
хотя в целом его наличие не должно вызвать существенных проблем.

В коде вашего лаунчера должен иметься`auth`-метод, похожий на представленный ниже:
```java
	public void auth(String username, String password) throws AuthenticationException {
		Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
		AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
		authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
	}
```
Просто измените его на код, представленный ниже, также заменив `<url>` url-адресом вашего сайта под Azuriom.
```java
	public void auth(String username, String password) throws AuthException, IOException {
		AzAuthenticator authenticator = new AzAuthenticator("<url>");
		authInfos = authenticator.authenticate(username, password, AuthInfos.class);
	}
```
Как только все этапы будут выполнены, AzAuth будет интегрирован в ваш лаунчер.

### Использование без OpenLauncherLib

AzAuth был разработан с [Gson] (https://github.com/google/gson) в качестве единственной зависимости, так что Вы можете спокойно использовать его даже без
OpenLauncherLib. Просто используйте `AzAuthenticator#authenticate(String username, String password)`, и это
выдаст Вам `AuthResult`, содержащий ник, uuid, ранг, токен доступа и множество другой полезной информации.
