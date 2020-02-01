# AzAuth

AzAuth to interfejs API, który umożliwia uwierzytelnianie użytkowników witryny opartej na Azuriomie na dowolnej platformie.

## Ściąganie

Możesz pobrać źródła i wydania AzAuth, klikając poniższy link. 
[GitHub](https://github.com/Azuriom/AzAuth/releases).

Jeśli używasz menedżera zależności, możesz dodać AzAuth jako
zależność w następujący sposób:

> {uwaga} Repozytorium Maven nie jest jeszcze dostępne, ale będzie dostępne wkrótce!

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

## Użycie AzAuth (Java)

Przed użyciem AzAuth upewnij się, że interfejs API jest aktywowany, przechodząc do
ustawienia strony za pośrednictwem panelu administratora.

### Użycie z [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(для лаунчера майнкрафт)_

Aby rozpocząć, dodaj AzAuth jako zależność do swojego projektu.
Również jeśli użyjesz [OpenAuth] (https://github.com/Litarvan/OpenAuth/), zaleca się go usunąć,
chociaż ogólnie jego obecność nie powinna powodować znaczących problemów.

Twój kod uruchamiania powinien mieć metodę `auth` podobną do poniższej:
```java
	public void auth(String username, String password) throws AuthenticationException {
		Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
		AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
		authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
	}
```
Po prostu zmień go na poniższy kod, zastępując również `<url>` adresem URL Twojej witryny w Azuriom.
```java
	public void auth(String username, String password) throws AuthException, IOException {
		AzAuthenticator authenticator = new AzAuthenticator("<url>");
		authInfos = authenticator.authenticate(username, password, AuthInfos.class);
	}
```
Po zakończeniu wszystkich kroków AzAuth zostanie zintegrowany z programem uruchamiającym.

### Użycie bez OpenLauncherLib

AzAuth był opracowany z [Gson] (https://github.com/google/gson) jako jedyne uzależnienie, dzięki czemu możesz bezpiecznie używać go nawet bez
OpenLauncherLib. Po prostu użyj `AzAuthenticator#authenticate(String username, String password)`, i to
da ci `AuthResult` zawierający pseudonim, identyfikator użytkownika, rangę, token dostępu i wiele innych przydatnych informacji.
