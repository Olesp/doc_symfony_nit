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

#### Instanciation du formulaire
Maintenant que notre formulaire est créé et lié à une entité il nous faut l'instancier dans notre controller.
```php
$form = $this->createForm(LinksType::class, $links);
```
Cette ligne de code créé le formulaire en utilisant la classe LinkType créée plus haut.
Nous verrons un peu plus bas ```$links```.

#### Traitement du formulaire
Avec Symfony le traitement du formulaire se fait dans la même méthode dans laquelle nous l'avons instancié. On rajoute donc une ligne pour récupérer les données postées par l'utilisateur.
```php
$form->handleRequest($request);
```
Notez que cette méthode fait appelle à une variable ```$request``` il faudra donc récupérer la requête lors de l'appelle de notre fonction ```index()```.
```php
public function index(Request $request)
```
Comme ceci. Si vous êtes observateur vous remarquerez que la class Request n'existe pas ici, il faut donc l'importer en haut de notre fichier pour que tout fonctionne parfaitement. 
```php
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\Request;
```
On en profite pour inclure Response qui nous servira pour afficher une erreur si notre controller ne sais pas quoi renvoyer à l'utilisateur.
Tout est presque prêt pour afficher et traiter notre formulaire. Passons maintenant au cas où le formulaire est envoyé, ou du moins, c'est ce que l'on veut vérifier.
Pour ce faire il faut tester le formulaire en utilisant deux méthodes bien pratiques :
```php
$form->isSubmitted();
$form->isValid();
```
Je pense que leurs noms sont plutôt evocateurs de leur fonctions.
On teste donc le formulaire reçu et on agit en conséquences.
```php
if ($form->isSubmitted() && $form->isValid()) {

    $this->addFlash("linkAdded", "Votre lien a bien été ajouté !");
    return $this->redirectToRoute("app_index");
}
```
Si le formulaire est valide on ajoute un message flash pour le rendu et on redirige l'utilisateur vers notre route "app_index".
Prenons le temps de noter quelques détails ici qui ne vous choquent peut être pas au premier regard mais qui valent la peine d'être soulignés.
Premièrement ```addFlash()``` ajoute directement un message flash dans les variables de session sans plus de manipulation. Vous pouvez consulter [sa page de documentation](https://symfony.com/doc/current/controller.html#flash-messages) pour vous rendre compte des possibilités que cela implique.
Deuxièmement ```redirectToRoute()``` est capable de prendre le nom de votre route sans avoir à lui préciser son URL, c'est utile si jamais vous souhaitez modifier cette URL sans avoir besoin de la rechanger partout dans votre code.