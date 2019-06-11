Chapitre 1 :
===
Let's get started !
---

Dans ce premier chapitre nous allons nous concentrer dans un premier temps à l'installation et au paramétrage de votre environnement de travail pour vous permettre de commencer à développer au plus vite. Puis vous afficherez votre première page grâce à Symfony.

### Installation de Symfony
#### Prérequis :
Vous devez vous assurer d'avoir d'installé sur votre machine :
+ PHP 7.2 minimum
+ MySQL
+ Composer

#### Installation 
Pour installer Symfony commencer par vous rendre sur [le site de Symfony](https://symfony.com/download) et entre la commande indiquée en fonction de votre OS.

Exemple :  ```wget https://get.symfony.com/cli/installer -O - | bash``` 

### Création de votre environnement de développement
#### Création du projet
Si tout s'est bien passé lors de l'installation de Symfony vous devriez pouvoir ouvrir votre terminal et entrer la commande ```symfony```

Si cela ne retourne pas d'erreurs vous pouvez continuer. Pour créer votre projet et l'initialiser entre simplement la commande ```composer create-project symfony\website-skeleton Linky ```

Ici Linky est le nom que nous avons choisi de donner à cette application mais libre à vous de choisir un autre nom. Après tout c'est votre application.

Une fois que ceci est fait vous devriez pouvoir vous rendre dans le dossier ainsi crée et vous rendre compte que les fichiers ont été générés automatiquement.
C'est une des fonctions utiles de Symfony que de générer de fichiers pour vous grâce au "maker" que nous vous présenterons plus tard.

Dès à présent rendez-vous via le terminal dans le dossier crée et entrez la commande    
```php bin/console server:run```    
Et voilà, votre serveur est lancé et opérationnel, facile non ? Ouvrez votre navigateur favori et rendez-vous sur localhost:8000 pour voire apparaître votre première page.
