# AzAuth

AzAuth is an api allowing you to authenticate users of a website under Azuriom on any platform.

## Download

AzAuth sources are available on [GitHub](https://github.com/Azuriom/AzAuth)
and the jar file can be downloaded from 
[Sonatype OSS](https://oss.sonatype.org/service/local/repositories/snapshots/content/com/azuriom/azauth/1.0-SNAPSHOT/azauth-1.0-20200221.223801-1.jar).


If you are using a dependency manager, you can add AzAuth as a
dependency by the following way:

### Gradle

in `build.gradle`:

```groovy
repositories {
    maven { url 'https://oss.sonatype.org/content/repositories/snapshots/' }
}
```
```groovy
dependencies {
    implementation 'com.azuriom:azauth:1.0-SNAPSHOT'
}
```

### Maven

in `pom.xml`:
```xml
<repositories>
    <repository>
        <id>sonatype-repo</id>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
    </repository>
</repositories>
```
```xml
<dependencies>
    <dependency>
        <groupId>com.azuriom</groupId>
        <artifactId>azauth</artifactId>
        <version>1.0-SNAPSHOT</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

## Use of AzAuth (Java)

Before using AzAuth, please make sure that the API is activated by going to
in the settings of your site, on your admin panel.

### Using with [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(for minecraft launcher)_

To begin, add AzAuth as a dependency to your project.
Also, if you are using [OpenAuth](https://github.com/Litarvan/OpenAuth/), it is recommended that you remove it,
although it does not cause any real problems, it is no longer used if you use AzAuth.

You should have in the code of your launcher an `auth` method similar to the code below:
```java
public static void auth(String username, String password) throws AuthenticationException {
    Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
    AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
    authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
}
```
You just have to replace it by the code below, to modify `<url>` by the url of your Azuriom's website root.
```java
public static void auth(String username, String password) throws AuthenticationException, IOException {
    AzAuthenticator authenticator = new AzAuthenticator("<url>");
    authInfos = authenticator.authenticate(username, password, AuthInfos.class);
}
```
Once this is done, you just need to import the class `AzAuthenticator` &
`AuthenticationException` from the `com.azuriom.auth` package and AzAuth will be integrated
into your launcher.

### Using without OpenLauncherLib

AzAuth has been designed with [Gson](https://github.com/google/gson) as its only dependency, so you can use it perfectly well if you don't use
OpenLauncherLib, you can simply use `AzAuthenticator#authenticate(String username, String password)` and that will 
give you directly a `User` containing a username, uuid, rank, access token and lots of other useful data.
