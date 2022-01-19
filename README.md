# Jeu d'aventure texte

L'objectif de ce projet est de créer un [jeu d'aventure texte](https://fr.wikipedia.org/wiki/Jeu_vid%C3%A9o_textuel) en ligne de commandes. Le jeu doit décrire un univers dans lequel le joueur pourra évoluer. Le fonctionnement de base de chaque jeu d'aventure texte est le suivant:
- Le jeu décrit au joueur ce que son personnage voit et ce qui se passe autour de lui.
- Le joueur doit entrer des commandes afin de se déplacer et d'agir sur son environnement.
- Le jeu décrit le résultat des actions entreprises par le joueur.
- On recommence à la première étape jusqu'à ce que l'aventure soit résolue.

## Mission 1: où suis-je? que vois-je?

Avant de commencer à évoluer dans un univers, il faut déjà que celui-ci existe. La première étape consiste donc à modéliser un univers de jeu sous la forme de lieux (pièces, couloirs, boutiques, terrains, etc.) et d'éléments interactifs présents dans ceux-ci (éléments de décor, meubles, outils, créatures, personnes, etc.). 

L'objectif de cette première mission est de faire en sorte que le joueur puisse demander au jeu de lui décrire n'importe quel lieu existant dans son univers. À ce stade, le joueur n'incarne pas encore un personnage de l'univers, il se contente de l'observer de l'extérieur comme à travers un guide touristique.

<details>
<summary>Exemple</summary>

`bedroom`

> This is where you usually sleep. It's quite small, but at least the bed is comfy. **Available items**: bed, curtain.

`bathroom`

> This is the bathroom. There's no windows in there, so it tends to get easily dank. **Available items**: shower, toothbrush.

`kitchen`

> This is the kitchen. It still smells of yesterday's dinner. **Available items**: cookie, faucet.

</details>

### 1. Créer des lieux

Implémenter les classes répondant aux spécifications suivantes:

#### `Room`

> Représente un lieu de l'univers de jeu dans lequel le joueur peut se trouver.

| Méthode | Description |
|---|---|
| _**String** getName()_ | Renvoie le nom du lieu (exemple: `"bedroom"`) |
| _**String** getDescription()_ | Renvoie la description du lieu (exemple: `"This is where you usually sleep. It's quite small, but at least the bed is comfy."`) |

#### `RoomTest` _(facultative mais recommandée)_

> Permet de s'assurer que la classe `Room` fonctionne comme attendu.

| Méthode | Description |
|---|---|
| _**void** testConstructors()_ | Crée une nouvelle instance de `Room` et vérifie que les valeurs passées au constructeur on été correctement assignées aux propriétés correspondantes. |

### 2. Demander au jeu de décrire des lieux

Implémenter les classes répondant aux spécifications suivantes:

#### `Game`

> Représente une partie jouée par le joueur.

| Méthode | Description |
|---|---|
| _**void** setup()_ | Initialise la partie en créant les objets de l'univers (pour l'instant, les lieux) |
| _**void** update()_ | Décrit un cycle d'exécution de la partie (pour l'instant: attend une saisie utilisateur, chercher si celle-ci correspond à un lieu existant, puis affiche la description du lieu concerné) |
| _**boolean** isRunning()_ | Permet de savoir si la partie est en cours (`true`) ou si elle est terminée (`false`) |

### 3. Ajouter des éléments interactifs dans les lieux

Implémenter les classes répondant aux spécifications suivantes:

#### `Item`

> Représente un élément de l'univers de jeu, avec lequel le joueur pourra interagir.

| Méthode | Description |
|---|---|
| _**Room** getRoom()_ | Renvoie le lieu dans lequel se trouve l'élément interactif (exemple: `Room@2 { name: "bathroom" }`) |
| _**String** getName()_ | Renvoie le nom de l'élément interactif (exemple: `"toothbrush"`) |
| _**boolean** isVisible()_ | Permet de savoir si l'élément interactif est visible (il apparaît dans les descriptions fournies par le jeu): `true` = oui, `false` = non |

#### `ItemTest` _(facultative mais recommandée)_

> Permet de s'assurer que la classe `Item` fonctionne comme attendu.

| Méthode | Description |
|---|---|
| _**void** testConstructors()_ | Crée une nouvelle instance de `Item` et vérifie que les valeurs passées au constructeur on été correctement assignées aux propriétés correspondantes. |

#### `Room`

| Méthode | Description |
|---|---|
| _**List\<Item\>** getItems()_ | Renvoie la liste de tous les éléments interactifs présents dans ce lieu. |
| _**void** addItem(Item item)_ | Ajoute un élément interactif à la liste des éléments interactifs présents dans ce lieu. |

**Attention**: il convient de s'assurer que la valeur de retour de la méthode `Room.getItems()` soit synchronisée avec la valeur de retour de la méthode `Item.getRoom()`. Par exemple, si la méthode `getItems()` de la chambre renvoie le lit et les rideaux, il faut s'assurer que la méthode `getRoom()` du lit et des rideaux renvoie bien la chambre.

## Mission 2: naviguer entre les lieux

Maintenant que nous sommes capables d'obtenir la description de chaque lieu en écrivant son nom, nous allons pouvoir proposer au joueur d'incarner un personnage qui se trouve dans un lieu donné, et qui est capable de se déplacer de l'un à l'autre. Le déroulement du jeu pourrait ressembler à l'exemple suivant:

<details>
<summary>Exemple</summary>

> This is where you usually sleep. It's quite small, but at least the bed is comfy. **Available items**: bed, curtain. **Available directions**: west, north.

`west`

> This is the bathroom. There's no windows in there, so it tends to get easily dank. **Available items**: shower, toothbrush. **Available directions**: east.

`west`

> You cannot go into that direction!

`east`

> This is where you usually sleep. It's quite small, but at least the bed is comfy. **Available items**: bed, curtain. **Available directions**: west, north.

</details>

Afin d'obtenir ce résultat, implémenter les classes ci-après en suivant les spécifications fournies.

### `Room`

- Représente un lieu dans lequel le joueur peut se trouver.

| Méthode | Description |
|---|---|
| _**Room** getRoomInDirection(**Direction** direction)_ | Renvoie le lieu où l'on arrive lorsque l'on part de ce lieu et qu'on emprunte la direction passée en paramètre (exemple: depuis la chambre à coucher, en passant la direction ouest, on devrait obtenir la salle de bain) |

### `Direction`

- Représente une direction que le joueur peut emprunter pour se déplacer d'un lieu à l'autre.

| Méthode | Description |
|---|---|
| _**String** getName()_ | Renvoie le nom de la direction (exemple: `"north"`) |

### `RoomConnection`

- Représente un passage entre deux lieux.

| Méthode | Description |
|---|---|
| _**Room** getFromRoom()_ | Renvoie le lieu dont part le passage |
| _**Room** getToRoom()_ | Renvoie le lieu auquel le passage aboutit |
| _**Direction** getDirection()_ | Renvoie la direction qu'il faut suivre pour emprunter ce passage |

### `Game`

- Représente une partie jouée par le joueur.

| Méthode | Description |
|---|---|
| _**void** setup()_ | Initialise la partie en créant les objets de l'univers (les lieux et les directions) et en les associant les uns aux autres de la manière adéquate, et détermine le lieu de départ |
| _**void** update()_ | Décrit un cycle d'exécution de la partie: décrire le lieu courant, attendre une saisie de l'utilisateur, vérifier qu'elle correspond à une direction, changer de lieu si cette direction est empruntable depuis le lieu dans lequel on se trouve actuellement |
| _**boolean** isRunning()_ | Permet de savoir si la partie est en cours (`true`) ou si elle est terminée (`false`) |
| _**Room** getCurrentRoom()_ | Renvoie le lieu dans lequel le joueur se trouve actuellement |

## Mission 3: interagir avec les éléments

Maintenant que nos joueurs sont capables de se déplacer d'un lieu à une autre et de voir les éléments qui s'y trouvent, il faudrait qu'ils soient puissent interagir avec en utilisant des commandes.

<details>
<summary>Exemple</summary>

> This is where you usually sleep. It's quite small, but at least the bed is comfy. **Available items**: bed, curtain. **Available directions**: west, north.

`use bed`

> You take a quick nap. You feel refreshed!

`use mirror`

> You see your reflection. Looking good!

`open mirror`

> This does not open!

`talk to mirror`

> Silence...

`use toothbrush`

> There is no such item here!

</details>

### 1. Interagir avec des éléments

- Écrire une classe `Command` qui représente une commande que l'utilisateur peut entrer dans la console.
- Chaque commande doit avoir un texte par défaut qui s'affichera si jamais l'utilisateur tente de l'utiliser avec un élément qui n'a pas été prévu pour (exemple: `talk to mirror`).
- Chaque élément peut réagir à un nombre indéterminé de commandes. Dans un premier temps, utiliser une commande particulière avec un élément particulier doit produire l'affichage d'un texte particulier (exemple: `use bed` doit produire l'affichage du texte `You take a quick nap. You feel refreshed!`).

<details>
<summary>[INDICE] Ça peut être utile</summary>

[Documentation Java: HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

</details>

### 2. Programmer des interactions complexes

Utiliser une commande sur un élément doit pouvoir produire une variété d'effets, dont afficher un texte n'est qu'un exemple.

Implémenter une ou plusieurs des classes suivantes:

| Classe | Description |
|---|---|
| **MessageEffect** | Produit l'affichage d'un message dans la console. |
| **EndGameEffect** | Termine la partie en cours. |
| **ChangeCurrentRoomEffect** | Change le lieu dans lequel le joueur se trouve actuellement. |
| **ChangeItemVisibilityEffect** | Change la visibilité d'un élément interactif. |
| **ModifyInventoryEffect** | Ajoute ou retire un élément interactif de l'inventaire du joueur. |

- Chaque élément peut réagir à chaque commande en utilisant l'un des effets proposés ci-dessus (au lieu de simplement afficher un message comme précédemment demandé).
- BONUS: Chaque élément peut réagir à chaque commande en utilisant une série d'effets, au lieu d'un seul effet.

#### Exemples d'interactions à implémenter

- Manger le biscuit sur la table de la cuisine (`eat cookie`) doit produire sa disparition de la pièce.
- Ouvrir le tiroir du bureau dans la chambre (`open drawer`) doit rendre visible un élément présent dans celui-ci (par exemple, un carnet de notes), le refermer (`close drawer`) doit le rendre invisible.
- Utiliser la voiture dans le garage (`use car`) doit produire le déplacement du joueur vers un autre lieu (par exemple, la ville). Utiliser la voiture dans ce dernier lieu doit produire le retour du joueur au garage.
- Ramasser une brosse à dents dans la salle de bain (`pick up toothbrush`) doit provoquer son ajout à l'inventaire et sa disparition de la pièce.
- Toucher une prise électrique (`touch plug`) doit produire la mort du héros, et donc la fin de la partie.

Si le bonus de l'étape 2 a été réalisé, chaque interaction doit être accompagnée d'au moins un message décrivant l'effet obtenu.

## Mission 4: harmoniser les commandes

Le processus principal qui permet de faire fonctionner le jeu est désormais capable de reconnaître les saisies utilisateur qui correspondent à une direction (`east`, `south`, `west`…) ainsi que celles qui correspondent à une interaction avec un objet (`use bed`, `open drawer`, `pick up notepad`…). À ce stade, nous aimerions ajouter des commandes générales comme `help` qui pourrait afficher la liste des commandes possibles, ou encore `exit` qui permettrait d'interrompre le jeu. Cependant, nous commençons à entrevoir que le fait de rajouter des nouvelles commandes de la sorte risque de complexifier le processus principal du jeu, qui est déjà bien chargé: car si nous continuons sur notre lancée, chaque nouveau type de commandes va devoir être traité séparément des autres.

Dans un premier temps, considérant qu'il est de la responsabilité de chaque commande de savoir quel effet elle est censée produire, il pourrait être judicieux d'alléger le processus principal en déplaçant les différents effets possibles (quitter le jeu, afficher les commandes disponibles, changer de lieu, etc…) dans la classe correspondante.

De plus, considérant que les nouvelles commandes que nous souhaiterions implémenter, mais aussi les directions, et les actions que nous pouvons utiliser sur les éléments interactifs, sont finalement toutes des types de commandes qui ont simplement leur particularités, l'objectif ultime de cette mission est de parvenir à refactoriser le code de manière que tous les types de commandes soient traités de la mème manière, au lieu d'ètre traités séparément.

<details>
<summary>Illustration</summary>

La logique actuelle:
```java
class Game
{
    public void update()
    {
        // Attend une saisie utilisateur
        // Si la saisie utilisateur correspond à la commande "quitter le jeu"
            // Termine la partie
        // Si la saisie utilisateur correspond à la commande "afficher l'aide"
            // Affiche la liste des commandes
        // Si la saisie utilisateur correspond à une direction
            // Modifie le lieu actuel
        // Si la saisie utilisateur correspond à une interaction avec un élément présent dans le lieu actuel
            // Déclenche l'effet correspondant à la commande spécifiée sur cet élément
        // etc…
    }
}
```

La logique désirée:
```java
class Game
{
    public void update()
    {
        // Attend une saisie utilisateur
        // Pour chaque commande possible, peu importe son type réel (commande globale, direction, interaction…)
            // Demande à la commande de traiter la saisie utilisateur. Si la commande correspond à la saisie utilisateur, elle réalise l'effet de la commande par elle-même, et la boucle est interrompue. Sinon, rien ne se passe.
    }
}
```

</details>

### 1. Ajouter des commandes globales

- Implémenter une ou plusieurs des classes suivantes:

| Classe | Description |
|---|---|
| **ExitCommand** | Termine la partie en cours. |
| **HelpCommand** | Affiche la liste de toutes les commandes possibles. |
| **ResetCommand** | Recommence une nouvelle partie. |

> - Chacune de ces classes doit posséder une propriété **Game** qui fait référence à la partie en cours.
> - Chacune de ces classes doit posséder une méthode _**boolean** process(**String** userInput)_. Le rôle de cette méthode est de traiter une saisie utilisateur. Si la saisie utilisateur correspond à la commande concernée, alors elle doit produire l'effet de la commande et renvoyer **true**. Sinon, elle doit ne rien faire et renvoyer **false**.
- Ajouter au processus principal dans la classe **Game** une condition demandant à une instance de chacune de ces classes de traiter par elle-même la saisie utilisateur.

### 2. Refactoriser les directions

- Renommer la classe **Direction** en **DirectionCommand**.
- Adapter la classe **DirectionCommand** pour qu'elle corresponde aux mêmes spécifications que les commandes globales, énumérées au point 1.
- Adapter le processus principal dans la classe **Game** de façon que celui-ci se contente de demander à chaque direction de traiter par elle-même la saisie utilisateur.

### 3. Refactoriser les interactions avec les éléments

- Renommer la classe **Command** en **ItemCommand**.
- Adapter la classe **ItemCommand** pour qu'elle corresponde aux mêmes spécifications que les commandes globales, énumérées au point 1.
- Adapter le processus principal dans la classe **Game** de façon que celui-ci se contente de demander à chaque commande représentant une interaction avec un élément de traiter par elle-même la saisie utilisateur.

### 4. Unifier tous les types de commandes

- Écrire une interface **Command** qui synthétise la structure commune à toutes les classes de commandes crées précédemment. Toutes les classes de commandes doivent implémenter cette interface.
- Refactoriser le processus principal dans la classe **Game** de façon à rassembler tous les appels aux méthodes _process_ en un seul et même appel.
