Chapitre 2 :
===

Maintenant que nous avons vu comment créer un controller, diriger l'utilisateur vers une vue, et afficher des variables avec Twig. Nous allons voir dans ce chapitre comment créer un formulaire, le relier à une base de données et traiter l'envoi de ce dernier.

### Formulaire
Pour créer un formulaire avec Symfony il existe plusieurs méthodes. Vous pouvez le créer à la main dans votre vue Twig, le créer directement dans le controller grâce au composant intégré au framework. Mais la méthode la plus versatile reste de lui créer une classe qui lui est propre afin de pouvoir l'instancier à différents endroits de votre application. De plus pour sauvegarder les données de notre form dans notre base de données il lui faut une entité qui servira à reccueillir les informations envoyées et les sauvegarder.

#### Make:entity
Comme pour le controller, pour créer une entité il est plus simple d'utiliser le maker intégré afin de ne pas oublier quoique ce soit. Ouvrez votre terminal et entrez ```php bin/console make:entity```. Il vous sera alors demandé d'entrer un nom, ici "Link". Puis nous pouvons commencer à ajouter des proprietés. Il faut savoir qu'ici les propriétés que vous entrez seront les champs en base de données, notez que l'id est déjà créé il n'est pas nécessaire de le spécifier. On précise pour chacun le type, la taille, la possibilité d'être nul et toutes les options comme vous le feriez normalement en bdd.
Vous trouverez plus de détails [ici](https://symfony.com/doc/current/doctrine.html#creating-an-entity-class) sur la création d'entité.

#### Make:form
Il ne nous reste maintenant plus qu'a créer notre formulaire et le relier à notre nouvelle entité. Puis nous devrons le générer et l'afficher sur notre application.
Vous commençez à connaître la marche maintenant, ouvrez votre terminal et entrez ```php bin/console make:form```.
La classe de notre form doit finir par "Type" c'est une convention, pour le reste vous êtes libres de choisir. Ici je rentre "LinksType". On rentre le nom de l'entité à laquelle on doit relier le form. Et c'est tout !