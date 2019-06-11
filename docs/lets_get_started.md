Chapitre 1 :
===

Dans ce premier chapitre nous allons nous concentrer dans un premier temps à l'installation et au paramétrage de votre environnement de travail pour vous permettre de commencer à développer au plus vite. Puis vous afficherez votre première page grâce à Symfony.

### Installation de Symfony
#### Prérequis :
Vous devez vous assurer d'avoir d'installé sur votre machine :
+ PHP 7.2 minimum
+ MySQL
+ Composer

#### Installation 
Pour installer Symfony commencer par vous rendre sur [le site de Symfony](https://symfony.com/download) et entrez la commande indiquée en fonction de votre OS.

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

### Vos premières pages
Pour diriger l'utilisateur sur votre site vous avez besoin d'un Controller.
Pour le créer avec Symfony vous pouvez utiliser le maker pour vous faciliter la vie.
#### Maker
Sur votre terminal entrez la commande ```php bin/console make:controller```, elle vous permet de créer votre controller en répondant aux prompts s'affichant sur le terminal.
Par convention les noms des controllers se terminent toujours par "Controller" et commencent toujours par une majuscule. Nommons celui-ci "AppController" ce nom aussi est une convention. 
Par défaut lors de la création du controller, le maker crée aussi une vue Twig et une route. Nous reviendrons plus tard sur la vue.
Il est temps d'afficher votre controller et de commencer à développer.
#### AppController
Sur ce document situé dans ```src/Controller```.
Vous pouvez constater lorsque vous l'ouvrez que une fonction existe déjà et qu'elle comporte une annotation un peu spéciale.
Nous allons regarder ça de plus près.
##### Annotations
```php
    /**
     * @Route("/", name="app_index")
     */
```
Comme vous pouvez le voir l'annotation commence par le traditionnel /* mais nous avons un * en plus. C'est un format d'annotation spéciale qui sera, à l'inverse d'un commentaire, interprété par le framework.
Cette annotation peut contenir plusieurs informations pour indiquer des comportements.

##### @Route
Cette annotation sert à indiquer la route sur laquelle repondra cette fonction. Auparavant, vous pouviez par exemple faire un switch-case pour determiner en fonction de la query, la route sur laquelle se trouvait l'utilisateur. Eh bien @Route effectue ce travail pour vous.
L'annotation prend en premier "paramètre" une string indiquant la route sur laquelle répondre. Et en deuxième partie un name. Il sert à faire référence à cette route dans différentes parties de votre application sans avoir à spécifier l'URL complète par exemple.
Il est possible de customiser l'URL de votre route. 
Par exemple :
```php
    /**
     * Cette route répond pour /app seulement
     * @Route("/app", name="app_index")
     * /
    
    /**
     * Cette route répond pour /app/test
     * mais pas pour /app/test/page1
     * @Route("/app/{var}", name="app_var")
     * /
```
Il est aussi possible de spécifier une valeur par défaut de {var} ou de lui imposer un type.
Pour consulter toutes les possiblités vous pouvez consulter [ce lien](https://symfony.com/doc/current/routing.html) car nous ne nous attarderons pas sur ces dernières dans ce cours.

#### La méthode render()
Chaque méthode de votre controller doit **obligatoirement** retourner quelque chose.
Dans la plupart des cas, du moins dans notre application, cela sera une ressource que le navigateur pourra afficher pour votre utilisateur.
```php
return $this->render("app/index.html.twig", [
    'name' => $name
]);
```
