Chapitre 1 :
===

Dans ce premier chapitre nous allons nous concentrer dans un premier temps sur l'installation et le paramétrage de votre environnement de travail pour vous permettre de commencer à développer au plus vite. Puis vous afficherez votre première page grâce à Symfony.

### Installation de Symfony
#### Prérequis :
Vous devez vous assurer d'avoir d'installé sur votre machine :
+ PHP 7.2 minimum et les extensions suivantes :
  + xml
  + curl
  + mbstring
  + zip
+ MySQL
+ Composer

#### Installation 
Pour installer Symfony commencer par vous rendre sur [le site de Symfony](https://symfony.com/download) et entrez la commande indiquée en fonction de votre OS.

Exemple :  ```wget https://get.symfony.com/cli/installer -O - | bash``` 

### Création de votre environnement de développement
#### Création du projet
Si tout s'est bien passé lors de l'installation de Symfony vous devriez pouvoir ouvrir votre terminal et entrer la commande ```symfony```

Si cela ne retourne pas d'erreurs vous pouvez continuer. Pour créer votre projet et l'initialiser entrez simplement la commande ```composer create-project symfony\website-skeleton Linky ```

Ici Linky est le nom que nous avons choisi de donner à cette application mais libre à vous de choisir un autre nom. Après tout c'est votre application.

Une fois que ceci est fait vous devriez pouvoir vous rendre dans le dossier ainsi créé et vous rendre compte que les fichiers ont été générés automatiquement.
C'est une des fonctions utiles de Symfony que de générer des fichiers pour vous grâce au "maker" que nous vous présenterons plus tard.

Dès à présent rendez-vous via le terminal dans le dossier créé et entrez la commande    
```php bin/console server:run```    
Et voilà, votre serveur est lancé et opérationnel, facile non ? Ouvrez votre navigateur favori et rendez-vous sur localhost:8000 pour voire apparaître votre première page.

### Vos premières pages
Pour diriger l'utilisateur sur votre site vous aurez besoin d'un Controller.
Pour le créer avec Symfony vous pouvez utiliser le maker pour vous faciliter la vie.
#### Maker
Sur votre terminal entrez la commande ```php bin/console make:controller```, elle vous permet de créer votre controller en répondant aux prompts s'affichant sur le terminal.
Consulter [la page officielle du bundle maker](https://symfony.com/doc/current/bundles/SymfonyMakerBundle/index.html#usage) pour connaître la liste des commandes disponibles.
Par convention les noms des controllers se terminent toujours par "Controller" et commencent toujours par une majuscule. Nommons celui-ci "AppController" ce nom aussi est une convention. 
Par défaut lors de la création du controller, le maker crée aussi une vue Twig et une route. Nous reviendrons plus tard sur la vue.
Il est temps d'afficher votre controller et de commencer à développer.
#### AppController
Sur ce document situé dans ```src/Controller```.
Vous pouvez constater lorsque vous l'ouvrez qu'une fonction existe déjà et qu'elle comporte une annotation un peu spéciale.
Nous allons regarder ça de plus près.
##### Annotations
```php
/**
 * @Route("/", name="app_index")
 * */
```
Comme vous pouvez le voir l'annotation commence par le traditionnel /* mais nous avons un * en plus. C'est un format d'annotation spéciale qui sera, à l'inverse d'un commentaire, interprété par le framework.
Cette annotation peut contenir plusieurs informations pour indiquer des comportements.

##### @Route
Cette annotation sert à indiquer la route sur laquelle répondra cette fonction. Auparavant, vous pouviez par exemple faire un switch-case pour déterminer en fonction de la query, la route sur laquelle se trouvait l'utilisateur. Eh bien ```@Route``` effectue ce travail pour vous.
L'annotation prend en premier "paramètre" une string indiquant la route sur laquelle répondre. Et en deuxième partie un tableau de variables. Il sert à faire référence à cette route dans différentes parties de votre application sans avoir à spécifier l'URL complète par exemple.
Il est possible de customiser l'URL de votre route. 
Par exemple :
```php
/**
 * Cette route répond pour /app seulement
 * @Route("/app", name="app_index")
 * */
/**
 * Cette route répond pour /app/test
 * mais pas pour /app/test/page1
 * @Route("/app/{var}", name="app_var")
 * */
```
Il est aussi possible de spécifier une valeur par défaut de {var} ou de lui imposer un type.
Pour consulter toutes les possiblités suivez [ce lien](https://symfony.com/doc/current/routing.html) car nous ne nous attarderons pas sur ces dernières lors de ce cours.

##### La méthode render()
Chaque méthode de votre controller doit **obligatoirement** retourner quelque chose.
Dans la plupart des cas, du moins dans notre application, il s'agira d'une ressource que le navigateur pourra afficher pour votre utilisateur.
```php
return $this->render("app/index.html.twig", [
    'name' => $name
]);
```
Cette méthode est en fait une simplification de la méthode ```response()``` dans laquelle vous devez indiquer la nature de la réponse et insérer à la main le HTML par exemple.
Lorsque que l'on regarde en détail elle prend en arguments une chaine de caractères, qui indique en fait l'emplacement d'un template Twig. Nous parlerons de Twig plus loin. En second paramètre elle accepte un tableau qui contient en fait toutes les options que nous voulons utiliser, par exemple dans ce cas, nous passons ```name```.   
``` name ``` est une variable Twig qui contient la valeur de ``` $name ```. Elle pourra ainsi être affichée par Twig lors du rendu de la page.
Plus d'informations sur la méthode ```render()``` peuvent être trouvé sur [la documentation Symfony]()

#### Twig
Twig est un moteur de rendu utilisé par défaut avec le framework Symfony. Un moteur de framework permet de préparer des templates et de les adapter en fonction des informations données par le controller. Les avantages de ce moteur sont qu'il est rapide, sûr et surtout flexible. Vous verrez qu'il embarque pas mal de fonctions permettant de soulager votre controller lors de l'affichage des données.

##### Index.html.twig
Lors de la création de votre controller je vous avais parlé d'une vue Twig créée par défaut. Eh bien pour la consulter il suffit de vous rendre dans le dossier ```template``` de votre app. Et normalement dans un dossier portant le nom de la route associée vous devriez trouver ce fameux fichier. Ouvrons-le et jetons-y un oeil.

##### {% expression %}
Cette expression est propre à Twig et permet d'effectuer des opéarations mathématiques à l'intérieur de votre vue mais aussi d'effectuer des opérations logiques.
```twig
{% extends "base.html.twig" %}
```
Cette première ligne de votre fichier est comme je vous le disais une expression logique. Elle permet d'étendre le fichier "base.html.twig", logique non ?
Ce qui veux dire que le fichier de base sera celui indiqué et les blocs ```{% block body %}``` de ce dernier seront remplacés par le blocs body de notre fichier index. 

##### {{ var }}
Rappelez-vous lors de l'appel de la fonction render() dans le controller, nous passions par exemple une variable ```'name'=>$name```. Si nous voulions l'afficher dans notre vue Twig il suffirait d'utiliser l'expression ```{{ name }}```.
Il faut utiliser le nom de la clé de votre tableau.

Pour en savoir plus sur les expressions et le traitement de données de Twig vous pouvez consulter la [documentation officielle](https://twig.symfony.com/doc/2.x/templates.html#expressions).