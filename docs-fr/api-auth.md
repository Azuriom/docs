# AzAuth

AzAuth est une api qui permet d'authentifier les utilisateurs d'un site sous Azuriom sur n'importe quelles plateformes.

## Utilisation de AzAuth (Java)

### Utilisation avec [OpenLauncherLib](https://github.com/Litarvan/OpenLauncherLib/) _(pour launcher minecraft)_

Pour commencer, ajoutez AzAuth en dépendance à votre projet.
Également si vous utilisez [OpenAuth](https://github.com/Litarvan/OpenAuth/), il est recommandé de le retirer,
bien que ne causant pas de réels problèmes, il n'est sera plus utilisé si vous utilisez AzAuth.

Vous devriez avoir dans le code de votre launcher une méthode `aut` ressemblant au code ci-dessous
```java
	public void auth(String username, String password) throws AuthenticationException {
		Authenticator authenticator = new Authenticator(Authenticator.MOJANG_AUTH_URL, AuthPoints.NORMAL_AUTH_POINTS);
		AuthResponse response = authenticator.authenticate(AuthAgent.MINECRAFT, username, password, "");
		authInfos = new AuthInfos(response.getSelectedProfile().getName(), response.getAccessToken(), response.getSelectedProfile().getId());
	}
```
Il vous suffit de la remplacer par le code ci dessous, de modidifer `<url>` par l'url à la racine de votre site sous Azuriom.
```java
	public void auth(String username, String password) throws AuthException, IOException {
		AzAuthenticator authenticator = new AzAuthenticator("<url>");
		authInfos = authenticator.authenticate(username, password, AuthInfos.class);
	}
```
Une fois ceci fait, AzAuth est intégré à votre launcher.

### Utilisation sans OpenLauncherLib _(utilisation normale)_

AzAuth a été conçu avec comme seule dépendance [Gson](https://github.com/google/gson), vous pouvez donc parfaitement l'utiliser si vous n'utilisez pas
OpenLauncherLib, vous pouvez simplement utiliser `AzAuthenticator#authenticate(String username, String password)` et cela
vous donner directement un `AuthResult` contenant le pseudo, l'uuid ainsi que l'access token.
