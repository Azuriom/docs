# Résolution des erreurs

Il est possible que des erreurs surviennent, cela ne vient pas forcément du CMS,
mais voilà les erreurs les plus courantes avec leurs solutions !

## Problèmes courants

### La page d'accueil fonctionne mais les autres pages produisent une erreur 404

La réécriture d'URL n'est pas activée, il vous suffit de l'activer (voir question suivante).

### Réécriture d'URL sur Apache2
Il faut modifier le fichier `/etc/apache2/sites-available/000-default.conf` et rajouter ces lignes entre les balises `<VirtualHost>`:
```
<Directory "/var/www/html">
  AllowOverride All
</Directory>
```

Puis redémarrer Apache2 avec
```
service apache2 restart
```

### Erreur 500 lors de l'inscription

Si le compte est bien créé malgré l'erreur, ce problème peut se produire si
jamais l'envoi des mails n'est pas correctement configuré, pour cela vérifiez
la configuration de l'envoi des mails sur le panel admin de votre site.

### Lorsque Azuriom est installé en local, les mises à jour et les thèmes/plugins ne sont disponibles

Si lorsque vous allez dans l'onglet "Mises à jour" et que vous faites
"Vérifier les mises à jour", vous avez cette erreur :
`curl: (60) SSL certificate : unable to get local issuer certificate`, il suffit
de suivre les étapes suivantes :
1) Télécharger le dernier `cacert.pem` sur https://curl.haxx.se/ca/cacert.pem
1) Ajouter cette ligne dans le php.ini (en remplaçant `/path/to/cacert.pem` par
l'emplacement du fichier `cacert.pem`:
   ```
   curl.cainfo="/path/to/cacert.pem"
   ```
1) Redémarrer PHP

### Le fichier n'a pas été téléchargé lors de l'upload d'une image

Ce problème survient lorsque vous uploadez une image dont le poids dépasse le
maximum autorisé par PHP (par défaut 2 mo).

Vous pouvez modifier la taille maximum autorisée lors de l'upload dans la configuration
de PHP (dans le `php.ini`) en modifiant les valeurs suivantes :
```
upload_max_filesize = 10M
post_max_size = 10M
```

### Problème avec AzLink ou les moyens de paiements en utilisant Cloudflare

Cloudflare peut empêcher AzLink ou certains moyens de paiements de fonctionner
correctement.

Pour corriger ce problème vous pouvez désactiver Cloudflare sur l’API, en allant
dans Page Rules -> Ajouter une règle, puis en mettant `/api/*` dans l’URL
et les actions suivantes :
* Niveau de cache : Ignorer
* Always Online : OFF
* Niveau de sécurité : **Pas** "I'm Under Attack"
* Vérification de l'intégrité du navigateur : OFF

Plus de détails sont disponibles sur le [site de Cloudflare](https://support.cloudflare.com/hc/en-us/articles/200504045-Using-Cloudflare-with-your-API).

### Forcer le HTTPS sur Apache2

Ajoutez ces lignes **juste après** le `RewriteEngine On` dans le `.htaccess` à la racine du site :
```
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R,L]
```

### Installer un autre site sur Apache2

Si vous souhaitez installer un autre site (ex : panel Pterodactyl, etc)
sur le même serveur web que celui sur lequel est installé Azuriom, il est recommandé
de l'installer sur un sous-domaine (ex : panel.votre-site.fr).

Dans le cas où ce n'est pas possible, vous pouvez configurer Apache pour les faire
fonctionner sur le même domaine, en ajoutant un fichier `.htaccess` dans le dossier
de l'autre site (ex : /panel) avec le contenu suivant :
```
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^ - [L]
</IfModule>
``` 
