# AzAuth

AzAuth is an api allowing you to authenticate users of a website under Azuriom on any platform.

## Download

You can download AzAuth's source and releases at
[GitHub](https://github.com/Azuriom/AzAuth/releases).

If you are using a dependency manager, you can add AzAuth as a
dependency in the following way:

> {warn} The Maven repo is not yet available but it will be very soon!

### Gradle

in `build.gradle`:

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

in `pom.xml`:
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

## Use of AzAuth (Java)

Before using AzAuth, please make sure that the api is activated by going to
in the settings of your site, on your admin panel.

### Using with [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(for minecraft launcher)_

To begin, add AzAuth as a dependency to your project.
Also if you are using [OpenAuth](https://github.com/Litarvan/OpenAuth/), it is recommended that you remove it,
although it does not cause any real problems, it is no longer used if you use AzAuth.

You should have in the code of your launcher an `auth` method similar to the code below:
```java
	public void auth(String username, String password) throws AuthenticationException {
		Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
		AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
		authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
	}
```
You just have to replace it by the code below, to modify `<url>` by the url at the root of your site under azuriom.
```java
	public void auth(String username, String password) throws AuthException, IOException {
		AzAuthenticator authenticator = new AzAuthenticator("<url>");
		authInfos = authenticator.authenticate(username, password, AuthInfos.class);
	}
```
Once this is done, AzAuth is integrated into your launcher.

### Using without OpenLauncherLib

AzAuth has been designed with [Gson](https://github.com/google/gson) as its only dependency, so you can use it perfectly well if you don't use
OpenLauncherLib, you can simply use `AzAuthenticator#authenticate(String username, String password)` and that
give you directly an `AuthResult` containing nickname, uuid, rank, access token and lots of other useful data.
