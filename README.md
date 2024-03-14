# Lecteur de formes

## Démonstration du projet (monte le son)

https://user-images.githubusercontent.com/1886792/227051855-121d9694-b161-4590-8250-a5baa2b7a6a4.mp4

## Description

Le logiciel permet de dessiner des formes sur une zone de travail. Chaque forme représente une note de musique sur un instrument. Le « lecteur de formes » va ensuite interpréter les formes dessinées de gauche à droite et jouer les notes correspondantes. Chaque forme est caractérisée par :

- sa position sur l'axe vertical : détermine la note jouée (le haut de la forme correspond à la note ; plus c'est haut, plus c'est aiguë) ;
- sa position sur l'axe horizontal : détermine à quel moment elle sera jouée ;
- sa longueur (horizontale) : détermine la durée de la note ;
- sa hauteur (verticale) : détermine le volume de la note jouée ;
- sa forme et sa couleur : détermine l'instrument associé (au début : un seul instrument représenté par une forme rectangulaire).

## Travail à faire

### 1. Implémenter les fonctionnalités absentes

Le projet actuellement _commité_ ne compile pas. De plus, il manque des fonctionnalités.

Implémentez les ajouts suivants pour que le projet compile et fonctionne comme attendu :

1. Complétez l'implémentation du constructeur de la classe `LecteurDeDessin`.

2. Complétez l'implémentation de la méthode `selectionnerEtJouerLesFormes` de la classe `LecteurDeDessin`.

3. Ajoutez la surcharge constructeur manquante dans la classe `Forme`.

4. Complétez l'implémentation des trois méthodes `contientX`, `contientY` et `contient` de la classe `Forme`.

5. Complétez l'implémentation des trois méthodes `contientLaForme`, `ajouterForme` et `supprimerForme` de la classe `Dessin`.

6. Complétez l'implémentation des deux méthodes `getPremiereFormeEn` et `formesSurLaColonne` de la classe `Dessin`.

7. Compléter l'implémentation de la méthode `pressDansZoneDessin` de la classe `OutilDeplacer`.

8. Compléter l'implémentation des deux méthodes `ajouterAuDessin` et `supprimerDuDessin` de la classe `EditeurDeFormes`.

9. Ajoutez tous les _getters_ et _setters_ nécessaires à la compilation et au bon fonctionnement de l'application.

### 2. Évolution

En plus des formes-rectangles, on va avoir des formes-ovales. Une première approche pourrait implémenter cela de la façon suivante :

- Le bouton « Forme » courant deviendra « Forme rectangle ».

- Ajoutez un bouton « Forme ovale » en examinant le traitement actuel des boutons et en dupliquant/adaptant ces fonctionnalités.

- Modifier la classe `Forme` pour ajouter le traitement des formes ovales, en associant un instrument différent et une couleur différente pour cette forme. Un exemple de code permettant de tester si un point donné est à l'intérieur d'une forme ovale (méthode `contains`) est donné en annexe.

Le problème de cette approche est qu'elle est naïve. Il y a beaucoup de code dupliqué et cela va rendre l'implémentation fastidieuse et plus facilement sujette à des bugs. De plus, l'ajout de nouvelles formes pour de nouveaux instruments va intensifier ce problème. On va donc essayer de factoriser (regrouper) le code commun.

### 3. Refactoring

On rappelle que le _refactoring_ consiste à modifier le code (en général pour l'améliorer !) _sans changer son comportement_.

Vous avez certainement usé de copier/coller lors de l'implémentation de la partie 2. Il y a en effet beaucoup de code réutilisé/dupliqué depuis `OutilForme` et `Forme`. L'objectif ici est de réduire/supprimer la duplication en utilisant l'héritage.

- Concevez la classe abstraite `Forme` dont vont dériver deux classes concrètes `Rectangle` et `Ovale` ; factorisez (regroupez) le code commun dans la classe abstraite.

- Concevez la classe abstraite `OutilForme` dont vont dériver deux classes concrètes `OutilRectangle` et `OutilOvale` ; factorisez le code commun dans la classe abstraite.

L'ajout de nouvelles formes et des outils associés par la suite sera alors grandement facilité.

**Dès lors, à nous la richesse et le pouvoir.**

### 4. Autres évolutions

- Ajouter un bouton « RAZ » qui remet à zéro le dessin.

- Parfois, on ne peut pas sélectionner une forme (pour la jouer, la supprimer, etc.) car elle est « à l'intérieur » d'une autre forme, prioritaire dans la sélection. Ajouter un bouton « Passer la forme en arrière-plan » qui fera en sorte que la forme ensuite sélectionnée soit la moins prioritaire pour la sélection par la suite.

- Ajouter d'autres formes correspondants à d'autres couples couleur/instrument.

## Annexe - Code pour « *hit* » d'une forme ovale

```java
contains(Point pt) {
    final double tolerance = 1.0e-6;
    double moitieLongueur = longueur / 2.0;
    double moitieHauteur = hauteur / 2.0;
    double diff = 0.0;

    if (moitieLongueur > 0.0) {
        diff = diff + sqrDiff(x + moitieLongueur, pt.x) / (moitieLongueur * moitieLongueur);
    } else {
        diff = diff + sqrDiff(x + moitieLongueur, pt.x);
    }
    if (moitieHauteur > 0.0) {
        diff = diff + sqrDiff(y + moitieHauteur, pt.y) / (moitieHauteur * moitieHauteur);
    } else {
        diff = diff + sqrDiff(y + moitieHauteur, pt.y);
    }
    return  diff <= 1.0 + tolerance;
}

private double sqrDiff(double num1, double num2) {
    return (num1 - num2) * (num1 - num2);
}
```
