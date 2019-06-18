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
Deuxièmement ```redirectToRoute()``` est capable de prendre le nom de votre route sans avoir à lui préciser son URL, c'est utile si jamais vous souhaitez modifier cette URL sans avoir besoin de la rechanger partout dans votre code. Rediriger l'utilisateur après traitement et validation d'un formulaire permet d'éviter son renvoi accidentel d'un utilisateur distrait et provoquer des éventuelles erreurs.
```php
if ($form->isSubmitted() && $form->isValid()) {
    $links->setDate(new \DateTime('now'));

    $em->persist($links);
    $em->flush();

    $this->addFlash("linkAdded", "Votre lien a bien été ajouté !");
    return $this->redirectToRoute("app_index");
}

return $this->render("app/index.html.twig", [
    "form" => $form->createView(),
    "linkList" => $linkList
]);
```
Juste en dessous de notre ```if``` on trouve un la méthode ```render()``` dont je vous parlais plus tôt qui render la page désirée. Le changement que vous pouvez remarquer ici est la valeur de la variable ```form```.
```createView()``` indique que la variable transmise est en fait un formulaire et demande à être affiché lors du rendu de la page. Des "propriétés" Twig pour l'objet ```form``` seront donc créées pour être utilisé lors de ce rendu. Elle contiendront par exemple les lignes du formulaires, les messages d'erreurs, les noms des champs, etc...

### ORM
Si vous êtes observateur, vous aurez sans doute remarquées les quelques lignes de code en plus dans l'instruction ```if``` un peu plus haut.
Elles concernent en fait la gestion de la base de données dans laquelle vos liens finiront et dans laquelle votre application piochera afin de les afficher pour l'utilisateur.
#### Doctrine ?
Le framework Symfony s'appuie sur un ORM pour gérer ses bases de données. En l'occurrence cet ORM se nomme Doctrine. 
Un ORM sert à gérer de manière complètement invisible pour le développeur les interactions avec la base de données. Mais à quoi cela peut-il bien servir me demanderez-vous ?
Eh bien dans un premier temps, cela vous évite les mauvaises manipulations ou bien les mauvais type de variables entrées dans les mauvais champs. On peut donc rapidement écarter les passages en revue des requêtes SQL quand votre application commence à faire des siennes. Et c'est déjà pas mal comme gain de temps.
De plus Doctrine gère la "protection" basique de votre base de données en évitant les attaques les plus répandues connues à ce jour.
[Ce lien](https://symfony.com/doc/current/doctrine.html) vous mènera à la page de Doctrine si vous êtes désireux d'en apprendre plus immédiatement.
#### Entity manager
Pour commencer à utliser Doctrine et gérer un BDD, il vous faudra ajouter une ligne au début de votre méthode ```index()```
```php
$em = $this->getDoctrine()->getManager();
```
Cette ligne instancie le composant Entity Manager de Doctrine.
#### Envoyer des données en BDD
Avec lui, vous pourrez récupérer vos données simplement.
Il nous faut donc maintenant être capable