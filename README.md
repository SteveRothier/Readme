# BROKEN CANVAS — Cahier des charges

- **Version :** 2.0 — document unique qui remplace les versions 1.0 à 1.6 (les anciens addenda sont fusionnés dans les chapitres thématiques)
- **Statut :** référence de conception proposée. Les valeurs chiffrées sont des valeurs de départ à confirmer par prototype et tests. Aucune fonctionnalité n'est réputée implémentée.
- **Moteur :** Unity 6, URP, C#
- **Plateforme :** PC Windows
- **Mode :** solo
- **Genre :** survivor roguelite 3D à la troisième personne, avec combats de boss exigeants

## Sommaire

- [1. Présentation du projet](#1-présentation-du-projet)
- [2. Boucle de jeu](#2-boucle-de-jeu)
- [3. Contrôles, déplacement et caméra](#3-contrôles-déplacement-et-caméra)
- [4. Personnages](#4-personnages)
- [5. Instruments](#5-instruments)
- [6. Reliques](#6-reliques)
- [7. Progression en partie](#7-progression-en-partie)
- [8. Combat, dégâts, statuts et synergies](#8-combat-dégâts-statuts-et-synergies)
- [9. Peinture au sol](#9-peinture-au-sol)
- [10. Ennemis, élites, mini-boss et boss](#10-ennemis-élites-mini-boss-et-boss)
- [11. Vagues et difficulté](#11-vagues-et-difficulté)
- [12. Hordes, simulation et performance](#12-hordes-simulation-et-performance)
- [13. Cartes et environnement](#13-cartes-et-environnement)
- [14. Interface et expérience du joueur](#14-interface-et-expérience-du-joueur)
- [15. Effets et audio](#15-effets-et-audio)
- [16. Progression permanente et sauvegarde](#16-progression-permanente-et-sauvegarde)
- [17. Architecture technique Unity 6 URP](#17-architecture-technique-unity-6-urp)
- [18. Fiches de contenu normalisées](#18-fiches-de-contenu-normalisées)
- [19. Tests et critères d'acceptation](#19-tests-et-critères-dacceptation)
- [20. Équilibrage](#20-équilibrage)
- [21. Plan de production](#21-plan-de-production)
- [22. Règles de travail pour l'assistant IA](#22-règles-de-travail-pour-lassistant-ia)
- [23. Décisions et journal des changements](#23-décisions-et-journal-des-changements)


## Comment lire et mettre à jour ce document

Ce document est la **référence unique** du projet. Il n'y a ni addendum ni « version 1.7 » à ajouter en fin de fichier.

- **Une règle, un seul endroit.** Chaque règle est écrite dans le chapitre de son sujet. Les autres chapitres y renvoient (→ §x.y) sans la recopier.
- **Pour changer une règle**, modifier la section concernée à sa place, puis noter le changement dans le journal (§23.3) : date, ancienne règle, nouvelle règle, raison.
- **Pour trancher une décision ouverte** (§23.2), reporter la règle retenue dans le chapitre concerné, puis déplacer la ligne vers le registre des décisions (§23.1).
- **Pour ajouter du contenu** (personnage, Instrument, Relique, ennemi, boss), remplir la fiche normalisée correspondante (chapitre 18) et l'ajouter à la liste du chapitre concerné.
- **Valeurs chiffrées :** toute valeur est une valeur de départ à tester, sauf mention contraire. Aucune n'est validée avant un test jouable.

## Résumé exécutif

BROKEN CANVAS est un survivor roguelite 3D solo sur PC Windows, développé sous Unity 6 URP. Le joueur contrôle un personnage à la troisième personne ; il se déplace, saute et esquive manuellement tandis que ses Instruments attaquent automatiquement. Pendant une run de 20 à 30 minutes, il collecte du Pigment, monte de niveau et compose un équipement de six Instruments et six Reliques au maximum. La pression augmente jusqu'à un combat de boss dédié, pendant lequel les apparitions ordinaires cessent.

Les Instruments ont cinq rangs de base. Une évolution, transformation spéciale du comportement d'un Instrument, se débloque grâce à un Sceau d'Évolution gagné contre un mini-boss, à un Instrument au rang 5 et à un niveau de joueur minimal.

La première version jouable reste volontairement limitée : un personnage, une carte (le Musée des Ombres), trois Instruments, quatre Reliques, quatre ennemis communs, un élite et un boss. Après validation de la boucle principale, le projet pourra s'étendre aux six personnages, à l'arsenal complet, aux évolutions, aux Fractures, aux autres cartes et à la progression permanente.

La priorité est de construire une base stable, lisible et testable, puis d'ajouter le contenu par étapes sans perdre l'identité artistique du jeu.

## Règles d'or

- **La rareté mesure une quantité de statistiques, jamais une transformation de gameplay.**
- **L'évolution change le comportement d'un Instrument et a des prérequis explicites.**
- **Les niveaux de joueur (10, 15, 20) et les rangs d'Instrument (1 à 5) sont deux progressions distinctes.**
- **Une seule vitesse de déplacement par personnage** : ni sprint, ni marche/course.
- **Aucune téléportation, aucun portail, aucun déplacement instantané.**
- **Les dégâts et les statuts suivent des règles communes, déterministes et testables.**
- **La difficulté monte par la composition, le budget de menace et le rythme, pas seulement par les points de vie.**
- **Les surfaces de peinture ont une limite, une durée et une règle de chevauchement.**
- **La sauvegarde permanente ne restaure jamais l'équipement ni l'état temporaire d'une ancienne run.**
- **Le MVP prouve une run complète avant tout ajout massif de contenu.**
- **Tout chiffre non validé par un test reste une proposition.**

---

## 1. Présentation du projet

### 1.1 Objectif et principes de production

Ce document sert de référence pour le développeur et les assistants IA. Il décrit ce que le jeu doit faire ; il ne demande aucune programmation en tant que tel.

Lecture des statuts :

- **V1 / MVP :** nécessaire à la première version jouable.
- **Futur :** à réaliser après validation de la boucle principale.
- **Valeur de départ :** chiffre proposé, à tester avant d'être figé.

Principes de production :

- Construire un prototype jouable avant de produire beaucoup de contenu.
- Inspecter les scripts réels avant toute modification ; ne pas supposer que le projet correspond parfaitement à ce document.
- Préserver les références Inspector et les systèmes existants, sauf nécessité expliquée.
- Préférer des systèmes simples, réutilisables et testables.
- Ne jamais prétendre qu'un script a été compilé ou testé si ce n'est pas le cas.
- Ne pas créer un système générique complexe tant que le prototype n'en a pas besoin.

### 1.2 Vision et piliers

Le joueur incarne un personnage lié à l'art et à la peinture maudite. Il explore des lieux contaminés par des œuvres vivantes, combat des hordes de créatures peintes, collecte du Pigment pour monter de niveau, compose une combinaison d'Instruments et de Reliques, puis affronte un boss final.

Piliers :

1. **Construire sa puissance :** choix de niveau et synergies qui changent réellement les attaques.
2. **Survivre par le mouvement :** placement, saut et esquive ont une utilité claire.
3. **Lire le danger :** les attaques importantes possèdent des indices visuels ou sonores.
4. **Identité artistique :** tableaux vivants, encre surnaturelle et pigments corrompus.
5. **Contenu évolutif :** ajouter des personnages, objets et cartes sans réécrire les systèmes centraux.

### 1.3 Direction artistique

Palette principale : noir d'encre, charbon, ivoire, toile vieillie et rouge vermillon. Les autres couleurs de pigments sont réservées aux effets de gameplay et aux objets particuliers. La palette et les matériaux de l'interface sont détaillés au §14.2.

Privilégier les silhouettes lisibles, les modèles stylisés réutilisables, les effets reconnaissables et les télégraphes de boss qui ne se confondent pas avec les attaques du joueur. Réutiliser matériaux, accessoires, éclairages et éléments de décor d'une carte à l'autre.

### 1.4 Fiche de référence

| Élément | Décision |
|---|---|
| Perspective | Troisième personne, caméra derrière le personnage, rotation à la souris |
| Mode | Solo |
| Plateforme | PC Windows |
| Moteur | Unity 6 avec URP, C# |
| Entrées | New Input System ; clavier et souris, manette prise en charge (§3.1) |
| Combat courant | Attaques automatiques des Instruments |
| Actions manuelles | Déplacement, saut, esquive et caméra |
| Esquive | Manuelle, avec courte invulnérabilité (§3.4) |
| Durée cible | 20 à 30 minutes, boss compris ; mode prototype raccourci pour itérer |
| Première carte | Musée des Ombres |
| Emplacements maximum | 6 Instruments et 6 Reliques ; le MVP peut utiliser une limite plus basse |
| Rang maximum d'un Instrument | 5 |
| Évolutions | Deux branches par Instrument, mutuellement exclusives par run (§5.6) |
| Raretés | Commun, Peu commun, Épique, Légendaire ; quantité statistique uniquement (§7.3) |
| Pigments de terrain du MVP | Encre noire et Vermillon uniquement (chapitre 9) |
| Boss | Les apparitions ordinaires cessent à son entrée (§11.6) |
| Progression permanente | Déblocages et bonus de puissance modestes (chapitre 16) |
| Téléportation | Interdite : aucun portail ni déplacement instantané |
| Données | Contenus configurables via assets de données ; pas de valeurs de gameplay dispersées dans les scripts |
| Contenu initial ciblé | Voir §1.6 |

### 1.5 Vocabulaire

- **Instrument :** arme principale qui déclenche une attaque automatique.
- **Relique :** équipement secondaire qui modifie les statistiques, les attaques ou les synergies.
- **Pigment :** expérience collectée pendant une run.
- **Rang :** niveau d'amélioration d'un Instrument (1 à 5) ou, si cette règle est retenue, d'une Relique.
- **Palier :** niveau de joueur (10, 15 ou 20) auquel une évolution peut être proposée. Ne pas confondre avec un rang.
- **Évolution :** transformation spéciale d'un Instrument, débloquée par un Sceau d'Évolution, un rang requis et un palier de joueur.
- **Sceau d'Évolution :** récompense garantie par la victoire contre un mini-boss ou un événement prévu ; condition d'une évolution.
- **Surface :** zone de peinture temporaire sur le sol, avec des effets de gameplay (chapitre 9).
- **Fracture :** anomalie dans un tableau qui peut déclencher un événement facultatif ; fonctionnalité future.
- **Run :** partie complète jusqu'à la victoire ou la défaite.
- **Élite :** ennemi plus dangereux qui donne une récompense supérieure.
- **Directeur de vague :** système qui choisit les apparitions d'ennemis selon le temps et un budget de menace (chapitre 11).
- **Fragments de Toile :** nom provisoire de la ressource permanente (chapitre 16).
- **Archives / catalogue :** collection permanente des découvertes et déblocages, consultable depuis le menu (§14.3).

Les noms d'interface peuvent être artistiques ; les noms de classes, méthodes et variables C# doivent rester explicites et maintenables.

### 1.6 Périmètre : MVP et contenu futur

**Objectif du MVP.** Prouver qu'une run complète est amusante, compréhensible et techniquement stable avant d'élargir le contenu. Le MVP n'est pas le jeu complet : c'est une tranche jouable qui valide le cœur du projet.

**Contenu inclus dans le MVP :**

- 1 personnage jouable : l'Exilé ;
- 3 Instruments, de départ ou obtenus pendant la run ;
- 4 Reliques au maximum dans le contenu du MVP, même si la limite finale reste 6 ;
- 4 ennemis communs aux rôles distincts (§10.2) ;
- 1 élite ;
- 1 mini-boss et au moins une évolution d'Instrument testable ;
- 1 boss final avec au moins 3 attaques lisibles ;
- 1 arène : le Musée des Ombres ;
- déplacement, saut, esquive et caméra ;
- attaques automatiques ;
- Pigment, expérience et choix à 3 options ;
- raretés d'amélioration statistiques ;
- une version limitée des surfaces encre noire / vermillon, si elle est nécessaire à l'identité du prototype ;
- écrans de pause, victoire, défaite et bilan ;
- sauvegarde minimale des paramètres et du résultat permanent, si nécessaire au test.

**Reporté après le MVP :**

- personnages supplémentaires non nécessaires à la validation ;
- toutes les évolutions des dix Instruments ;
- pigments supplémentaires ;
- plusieurs cartes ;
- événements aléatoires complexes ;
- méta-progression étendue ;
- défis quotidiens ou contenu en ligne ;
- tout système qui ne contribue pas à prouver le combat, la progression ou la lisibilité.

**Fonctionnalités futures, à réaliser après validation de la boucle principale :**

1. Les cinq personnages supplémentaires, puis les personnages d'extension (§4.4).
2. Les Instruments et Reliques restants.
3. Les évolutions de tous les Instruments.
4. Les Fractures et événements facultatifs.
5. Les contrats de restauration (concept à définir).
6. Les autres cartes (§13).
7. Les deux boss supplémentaires (§10.6).
8. Les Archives et la sauvegarde complète.
9. Les paramètres avancés et la remappabilité des commandes.
10. Les modèles, animations, effets et sons finaux.
11. Les modes ou options de difficulté supplémentaires, si les tests le justifient.

Les critères de sortie du MVP sont au §19.2.

---

## 2. Boucle de jeu

### 2.1 Boucle générale d'une run

1. Choisir un personnage disponible.
2. Lancer une run : le joueur entre dans la carte avec son équipement de départ.
3. Explorer et combattre. Les Instruments attaquent automatiquement ; les ennemis apparaissent selon le directeur de vague (chapitre 11).
4. Se déplacer, sauter et esquiver manuellement.
5. Ramasser le Pigment laissé par les ennemis.
6. Monter de niveau et choisir une amélioration parmi trois options. Le jeu est en **pause complète** pendant le choix.
7. Équiper et améliorer des Instruments et des Reliques dans la limite des emplacements.
8. Survivre à une difficulté croissante. Un élite, un événement ou un mini-boss peut fournir un Sceau d'Évolution (§5.6).
9. Arrêter les apparitions ordinaires lorsque le boss commence (§11.6).
10. Vaincre le boss ou perdre tous ses points de vie.
11. Afficher le bilan, attribuer les récompenses permanentes, puis permettre de recommencer ou de revenir au menu.

En combat ordinaire, la boucle doit rester dynamique : la pause n'intervient que pour les choix, le menu pause et les écrans de fin.

### 2.2 Boucle seconde par seconde

1. Le joueur lit la position des ennemis et les avertissements d'attaque.
2. Il se déplace pour maintenir une distance utile, éviter les zones dangereuses ou regrouper les ennemis.
3. Les Instruments évaluent leurs conditions de déclenchement et attaquent automatiquement.
4. Les ennemis se déplacent selon leur rôle et déclenchent leurs attaques télégraphiées.
5. Le joueur saute ou esquive pour éviter une menace ; le coût de récupération de l'esquive empêche l'esquive permanente.
6. Les impacts appliquent dégâts, critiques, statuts et interactions de terrain selon un ordre de résolution unique (§8.2).
7. Les ennemis vaincus peuvent laisser du Pigment, des objets ou une récompense spécifique.
8. Le joueur arbitre entre continuer à attaquer, récupérer du Pigment et se repositionner.

Le rythme par minute (phases A à F) est défini au §11.1.

### 2.3 Machine à états

Le jeu utilise une machine à états explicite, avec au minimum :

- `MainMenu`
- `CharacterSelect`
- `RunStarting`
- `Combat`
- `LevelUpChoice`
- `EventReward`
- `BossIntro`
- `BossCombat`
- `Victory`
- `Defeat`
- `Pause`

Chaque transition est définie et testable. Exemples :

- `Combat -> LevelUpChoice -> Combat` met les systèmes de jeu en pause une seule fois, conserve les données de run et reprend sans dupliquer une amélioration.
- `BossCombat -> Victory` ne peut se produire qu'une fois.
- La fermeture d'un menu restaure l'état précédent ; si l'écran précédent était un écran de choix, c'est lui qui est restauré, pas le combat.

### 2.4 Mort, victoire et résultats

À zéro point de vie du joueur :

- interrompre immédiatement les nouvelles attaques du joueur ;
- arrêter les apparitions et désactiver les entrées de combat ;
- laisser les effets visuels se terminer brièvement, sans infliger de nouveaux dégâts au joueur ;
- afficher le bilan de run (§14.7) ;
- calculer les Fragments de Toile **une seule fois** ;
- enregistrer les déblocages et les statistiques de run ;
- permettre `Recommencer` ou `Menu principal`.

Règles communes à la victoire et à la défaite :

- une mort ne déclenche pas plusieurs écrans de défaite et ne distribue pas plusieurs fois les récompenses ;
- la victoire et la défaite ne sont déclenchées qu'une seule fois ;
- le redémarrage crée un **nouvel état de run** : il ne réutilise ni anciens ennemis, ni projectiles, ni effets, ni minuteries, ni buffs temporaires, ni objets de Pigment, et il réinitialise santé, statistiques temporaires et temps de jeu ;
- le retour au menu permet de choisir un autre personnage.

---

## 3. Contrôles, déplacement et caméra

### 3.1 Commandes

| Action | Clavier / souris | Manette |
|---|---|---|
| Déplacement | ZQSD (AZERTY) ou WASD selon la disposition | Stick gauche |
| Caméra | Souris | Stick droit |
| Saut | Espace | Bouton de saut |
| Esquive | Maj gauche (valeur provisoire) | Bouton d'esquive |
| Pause | Échap | Bouton pause / menu |
| Attaque | Automatique | Automatique |
| Navigation dans l'interface | Souris et clavier | Manette |

Règles :

- Toutes les actions sont centralisées dans le **New Input System**. Aucune touche n'est codée en dur dans les scripts de gameplay.
- La remappabilité des commandes est prévue dans une version ultérieure ; la centralisation des actions la rend possible.
- Le bouton secondaire de la souris peut être envisagé pour l'esquive ; la touche finale reste à confirmer (§23.2).

### 3.2 Déplacement

Le déplacement se fait sur le plan de jeu, en fonction de l'orientation de la caméra. La vitesse diagonale est normalisée. Les collisions ne permettent ni de traverser les murs ni de rester coincé dans la géométrie.

**Une seule vitesse de déplacement.** Chaque personnage possède une vitesse de base unique.

- Il n'existe ni mode marche/course, ni sprint maintenu, ni seconde vitesse activable, ni accélération par double-tap ou combinaison secrète.
- Une direction valide déclenche un déplacement à la vitesse du personnage ; relâcher les commandes arrête le personnage selon la réponse d'accélération et de freinage définie.
- Les ralentissements imposés par un effet, une attaque, un terrain ou un statut sont des modificateurs temporaires, pas une seconde vitesse sélectionnable.
- Les Reliques et améliorations ne modifient la vitesse de base que si leur effet le précise explicitement.

**Valeur de départ :** 5,5 m/s pour le personnage de référence ; plage de test de 4,8 à 6,2 m/s pour les profils différents.

**Réponse du déplacement.** Le déplacement est immédiat et lisible, sans glisse incontrôlée. Réglages exposés dans les données du personnage ou dans une configuration commune :

- vitesse de déplacement ;
- accélération au démarrage ;
- décélération à l'arrêt ;
- vitesse de rotation du personnage ;
- contrôle aérien ;
- influence des effets de ralentissement.

Valeurs de départ : accélération d'environ 0,08 à 0,18 s pour approcher la vitesse cible ; arrêt d'environ 0,08 à 0,20 s selon l'inertie retenue ; rotation vers la direction de déplacement sans rotation brusque de la caméra. Le contrôleur évite les oscillations et les micro-déplacements involontaires.

### 3.3 Saut

Le saut est un outil central de déplacement, d'évitement et de lecture des attaques ennemies. Il fait partie de la première version jouable.

Règles :

- Déclenchement manuel, autorisé lorsque le personnage est au sol.
- Un seul saut avant de toucher à nouveau le sol, sauf effet explicitement accordé par une amélioration. Pas de double saut par défaut.
- Maintenir le bouton ne provoque pas de sauts répétés.
- Gravité cohérente et retour au sol fiable. Une pente, un rebord ou un petit obstacle ne provoque ni saut fantôme ni perte injustifiée du contrôle.
- Le joueur influence sa direction en l'air, dans la limite de la statistique de contrôle aérien du personnage.
- **Le saut ne donne pas d'invulnérabilité** et ne remplace pas l'esquive. Il sert surtout à la mobilité et au franchissement léger.
- Le saut ne permet pas de franchir les limites de la carte ni d'éviter toutes les mécaniques d'un boss. La possibilité de sauter par-dessus une attaque de boss est décidée attaque par attaque (§10.6).
- Les attaques pouvant toucher le joueur en l'air sont clairement lisibles ; les attaques au sol peuvent avoir des règles distinctes si leur télégraphe l'indique.
- Les attaques automatiques pendant le saut suivent une règle explicite et testée (décision ouverte, §23.2).
- Le joueur n'est jamais forcé à réussir des sauts de précision de type plateforme pour terminer une partie standard.
- Animations provisoires acceptées pendant le prototype.

Valeurs de départ pour le personnage de référence :

- hauteur de saut cible : environ 1,5 à 2,0 m ;
- durée totale d'un saut simple : environ 0,65 à 0,9 s ;
- contrôle aérien : valeur de référence 1,0, les personnages pouvant être en dessous ou au-dessus.

Hauteur, durée et gravité sont calibrées ensemble : privilégier un saut lisible et réactif plutôt qu'une simulation réaliste. Chaque personnage peut avoir ses propres valeurs d'impulsion, de contrôle aérien et, si les tests le justifient, de délai après l'atterrissage. Si la gravité est dérivée de la hauteur et de la durée, ne pas exposer plusieurs réglages contradictoires.

### 3.4 Esquive

L'esquive est une action manuelle, distincte du déplacement ordinaire et du saut.

Règles :

- Déclenchement par une pression dédiée, avec impulsion de déplacement dans une direction déterminée par le joueur.
- **Direction :** celle du déplacement s'il y en a une ; sinon, esquive vers l'arrière par rapport à l'orientation du personnage.
- Trois phases : déplacement rapide, **courte fenêtre d'invulnérabilité**, récupération.
- Les limites de l'invulnérabilité sont synchronisées avec les données de l'action et les effets visuels.
- Aucun dégât antérieur au déclenchement n'est annulé.
- Les murs et les limites de la carte restent bloquants. Pendant la phase d'esquive, les ennemis ordinaires ne bloquent pas le joueur (règle de départ, à tester).
- Délai de récupération : pas d'esquive continue.
- Feedback visuel ou sonore à chaque esquive ; l'état de recharge est immédiatement compréhensible (§14.4).
- Une esquive ne réinitialise pas les temps de recharge des Instruments.
- **Esquive en l'air :** autorisée, avec le même délai de recharge, pour qu'elle reste utile sans permettre un vol permanent. Saut et esquive ne peuvent pas être enchaînés indéfiniment pour supprimer tout risque.

Valeurs de départ pour le personnage de référence :

| Paramètre | Valeur de départ |
|---|---|
| Durée du déplacement d'esquive | 0,22 à 0,32 s |
| Fenêtre d'invulnérabilité | 0,12 à 0,20 s |
| Récupération totale | 0,65 à 0,90 s |
| Distance parcourue | environ 2,5 à 3,5 m |

Ces plages sont testées contre des attaques lentes, rapides, de zone et de boss. L'esquive doit récompenser le timing sans éclipser le saut, le placement et le choix d'itinéraire.

Chaque personnage peut modifier la distance, la durée de déplacement, la fenêtre d'invulnérabilité (dans des limites équilibrées), le temps de récupération et, si la différence reste compréhensible, la réponse directionnelle. Un personnage ne cumule pas grande distance, longue invulnérabilité et récupération très courte : les avantages sont compensés par des faiblesses ou un autre rôle.

### 3.5 Caméra

Objectifs :

- maintenir le joueur lisible au milieu des hordes ;
- permettre de regarder autour de soi sans perdre la compréhension de la direction de déplacement ;
- éviter que murs et décor masquent durablement le joueur ;
- garder les attaques dangereuses, les élites et le boss visibles autant que possible ;
- limiter le mal des transports : accélération de caméra modérée, pas de secousses constantes, sensibilité réglable.

Comportement :

- vue à la troisième personne, suivi stable, rotation à la souris (stick droit à la manette) ;
- limites verticales raisonnables ;
- curseur **capturé pendant le jeu** et **libéré** dans les menus, la pause, les écrans de choix et de fin ; la caméra ne recapture pas la souris tant que le jeu est en pause ou terminé.

Réglages exposés dans une configuration caméra : sensibilité horizontale et verticale, distance de suivi, hauteur et angle, limites verticales, amortissement du suivi, comportement en collision avec le décor, inversion verticale facultative, intensité des secousses (réglable et désactivable).

Point de départ à tester : caméra légèrement surélevée, angle plongeant modéré, distance suffisante pour lire les menaces proches. Les chiffres exacts dépendent de la taille des personnages, de la carte et du champ de vision ; ils ne sont pas figés avant un test dans le Musée des Ombres.

**Collision caméra.** La caméra ne traverse ni murs ni piliers. En cas d'obstruction : rapprochement temporaire, pas de changement brusque de distance, retour progressif à la distance normale, et le joueur ne disparaît jamais durablement. Les tests incluent coins de murs, couloirs étroits, piliers, plafonds bas et combat contre un boss volumineux.

Ne pas faire trembler la caméra à chaque impact mineur (§8.4).

---

## 4. Personnages

### 4.1 Règles communes

La vision finale prévoit six personnages. Pour la V1, seul l'Exilé est nécessaire afin de valider les systèmes communs.

- Les personnages partagent le contrôleur, la caméra, la santé et les règles de combat. Les règles de déplacement, de saut et d'esquive du chapitre 3 s'appliquent à tous.
- Les différences viennent des **données** : statistiques de base, passif, Instrument de départ.
- Le passif fonctionne automatiquement dès le début de la run.
- Chaque personnage a une force, une faiblesse et des synergies distinctes.
- Chaque personnage reste jouable avec les commandes de base. Aucun profil ne rend le saut ou l'esquive inutilisables sur une carte standard.
- Les personnages verrouillés indiquent leur condition de déblocage.
- Les cinq personnages supplémentaires sont du contenu futur, pas un prérequis du prototype.

### 4.2 Les six personnages

1. **L'Exilé (départ).**
   - **Instrument :** Pinceau du Dernier Trait.
   - **Passif — Trait fondateur :** les éliminations augmentent temporairement les dégâts de l'Instrument ; le bonus expire après une période sans élimination.
   - **Style :** équilibré, adapté à la découverte.
2. **La Virtuose.**
   - **Instrument :** Rapière Vermillon.
   - **Passif — Coup de maître :** les coups critiques marquent les ennemis ; une prochaine attaque critique contre une cible marquée reçoit un bonus.
   - **Style :** attaques rapides et dégâts concentrés.
3. **Le Graveur.**
   - **Instrument :** Arbalète à Encre.
   - **Passif — Perspective :** les attaques à distance gagnent en puissance jusqu'à une limite.
   - **Style :** portée et précision.
4. **L'Alchimiste.**
   - **Instrument :** Palette Catalytique.
   - **Passif — Mélange instable :** les effets de pigments peuvent déclencher une réaction de zone, avec une probabilité limitée.
   - **Style :** effets de statut et synergies.
5. **Le Sculpteur.**
   - **Instrument :** Maillet de Marbre Noir.
   - **Passif — Œuvre monumentale :** les niveaux renforcent légèrement sa résistance, avec un compromis de mobilité.
   - **Style :** robuste, lent et puissant.
6. **La Restauratrice.**
   - **Instrument :** Fil de Restauration.
   - **Passif — Retouche vitale :** éliminer des ennemis marqués peut rendre un peu de santé, avec une limite de fréquence.
   - **Style :** survie et récupération.

Chaque personnage démarre avec l'Instrument qui porte son identité (liste complète au §5.1). Les fiches détaillées suivent le modèle du §18.1.

### 4.3 Statistiques de base et profils

Tous les personnages utilisent les mêmes règles de contrôle mais peuvent avoir des statistiques de base différentes. La vitesse de déplacement reste unique par personnage ; elle peut différer d'un personnage à l'autre.

Statistiques de base potentielles :

- points de vie maximum ;
- vitesse de déplacement (unique) ;
- puissance ou multiplicateur de dégâts ;
- hauteur ou impulsion de saut ;
- contrôle aérien ;
- distance, durée d'invulnérabilité et récupération de l'esquive ;
- portée ou rayon de ramassage ;
- vitesse d'attaque de base, uniquement si elle s'applique clairement à l'Instrument ;
- chance de critique, uniquement si le système de critique est retenu ;
- armure ou réduction de dégâts ;
- bonus spécifique de personnage.

N'ajouter une statistique que si elle modifie une décision ou une sensation perceptible, pas parce qu'elle est courante dans le genre.

Profils de conception (qualitatifs, à convertir en nombres après un premier prototype) :

| Personnage | Profil envisagé | Saut / esquive | Compromis suggéré |
|---|---|---|---|
| L'Exilé | Polyvalent, accessible | Valeurs proches de la référence | Pas de faiblesse extrême |
| La Virtuose | Agile, exigeante | Esquive plus réactive, récupération favorable | Moins résistante |
| Le Graveur | Positionnement et précision | Saut correct, esquive plus longue mais plus engageante | Moins performant au contact |
| L'Alchimiste | Zone et effets | Mobilité moyenne, contrôle aérien stable | Dépend davantage du placement |
| Le Sculpteur | Résistance et impact | Saut moins haut, esquive plus courte | Plus de vie ou de résistance |
| La Restauratrice | Survie et soutien | Esquive équilibrée, contrôle aérien correct | Dégâts directs plus modestes |

**Source unique des données.** Les statistiques de personnage sont stockées dans des données Unity dédiées (ScriptableObjects) :

- un asset par personnage ;
- des valeurs clairement nommées, avec unités ;
- une validation des valeurs incohérentes ;
- une séparation entre statistiques de base, aptitude spéciale et Instrument de départ ;
- aucun chiffre d'équilibrage important dupliqué dans plusieurs scripts.

Les effets temporaires, les améliorations de run et les bonus permanents sont appliqués par un **système de modificateurs** défini, et non en remplaçant les valeurs de base. Le calcul final est inspectable et testable.

### 4.4 Personnages futurs à prototyper après la V1

Ces trois personnages sont des pistes d'extension, pas des prérequis. Le Patineur est le premier candidat au prototypage après validation du combat de base. Aucun ne doit utiliser de portail ou de téléportation : la mobilité s'appuie sur le déplacement physique, l'inertie, les esquives et les interactions avec le terrain.

- **Le Patineur :** personnage axé sur l'inertie. Il accélère en glissant sur des traces compatibles, perd de la vitesse dans les virages serrés et peut effectuer un dérapage qui dépose une ligne de peinture. À haute vitesse, une collision offensive peut produire une onde de choc. *Force :* maîtrise de la trajectoire. *Faiblesses :* espaces encombrés et arrêts fréquents.
- **L'Encré :** peut se transformer brièvement en flux d'encre au ras du sol. La transformation consomme une jauge ; traverser ses propres flaques peut contribuer à la restaurer ; reprendre forme près d'une flaque peut provoquer une explosion. La transformation reste un déplacement continu : ni téléportation ni déplacement instantané.
- **Le Marionnettiste :** relie plusieurs ennemis avec des fils d'encre ; une fraction des dégâts infligés à un ennemi lié peut être propagée aux autres. Une compétence peut rapprocher les ennemis liés. Plus faible contre une cible isolée.

L'effectif final de personnages (six, ou davantage avec ces pistes) reste à décider (§23.2). Les interactions de ces personnages avec les surfaces sont au §9.5.

---

## 5. Instruments

### 5.1 Liste de référence et évolutions

Dix Instruments sont prévus. Chacun a deux évolutions (A et B) mutuellement exclusives par run (§5.6). Les synergies citées sont des propositions à tester.

1. **Pinceau du Dernier Trait — zone rapprochée :** dessine un arc d'encre devant le personnage.
   - **Évolution A — Grande Marée :** l'arc devient une vague qui avance et traverse les ennemis. Synergie : les ennemis ralentis par la Palette Catalytique restent plus longtemps dans la vague.
   - **Évolution B — Fresque Tourbillonnante :** trois arcs tournent brièvement autour du joueur après l'attaque. Synergie : les Éclats de Fusain ajoutent une explosion d'encre lorsqu'un fragment touche un ennemi déjà marqué.
2. **Rapière Vermillon — critiques :** frappe une cible proche.
   - **Évolution A — Estoc de Rubis :** les coups critiques appliquent une marque ; après un court délai, la marque explose. Synergie : la Broche du Critique augmente les déclenchements, et le Flacon de Vermillon amplifie l'explosion dans ses flaques.
   - **Évolution B — Danse des Entailles :** chaque élimination déclenche une entaille secondaire vers une cible proche, avec une limite de rebonds. Synergie : le Fil de Restauration propage la marque aux ennemis reliés.
3. **Arbalète à Encre — longue portée :** tire un projectile précis.
   - **Évolution A — Trait Perforant :** le projectile traverse plusieurs ennemis et inflige davantage de dégâts aux cibles alignées. Synergie : la Cloche du Vernissage regroupe ou interrompt les ennemis pour faciliter l'alignement.
   - **Évolution B — Pointe de Contamination :** le projectile se plante dans la première cible, puis éclate en éclaboussures après un délai. Synergie : la Palette Catalytique ajoute un effet de statut aux éclaboussures.
4. **Flacon de Vermillon — zone persistante :** éclate et laisse une flaque dangereuse.
   - **Évolution A — Marée Vermillon :** la flaque se divise en petites flaques qui se propagent vers les ennemis proches. Synergie : le Sablier du Dernier Regard prolonge leur durée.
   - **Évolution B — Fournaise Écarlate :** les flaques s'enflamment après quelques secondes et explosent, infligeant des dégâts de zone. Synergie : la Palette Catalytique transforme les ennemis contaminés dans la zone en source d'une réaction secondaire.
5. **Éclats de Fusain — défense orbitale :** fragments tournant autour du personnage.
   - **Évolution A — Couronne de Charbon :** les fragments sont plus nombreux et repoussent légèrement les ennemis au contact. Synergie : le Maillet de Marbre Noir inflige des dégâts bonus aux ennemis repoussés ou déséquilibrés.
   - **Évolution B — Éclipse de Fusain :** à intervalles réguliers, les fragments quittent l'orbite et plongent sur les ennemis proches avant de revenir. Synergie : la Fiole d'Encre Vive réduit le délai entre ces salves.
6. **Palette Catalytique — effets de statut :** applique brûlure, ralentissement ou contamination.
   - **Évolution A — Réaction en Chaîne :** l'application d'un statut sur une cible déjà affectée déclenche une réaction de zone. Synergie : le Flacon de Vermillon fournit des zones persistantes où la réaction peut se propager.
   - **Évolution B — Prisme Instable :** les statuts alternent entre ralentissement, brûlure et contamination ; deux statuts différents sur une même cible déclenchent une décharge. Synergie : le Fil de Restauration transmet une partie de la décharge aux ennemis reliés.
7. **Maillet de Marbre Noir — impact lourd :** frappe une zone avec cadence lente et dégâts élevés.
   - **Évolution A — Faille du Musée :** l'impact crée des fissures qui explosent en ligne après un bref délai. Synergie : la Cloche du Vernissage immobilise brièvement les ennemis légers dans la trajectoire.
   - **Évolution B — Sceau du Sculpteur :** chaque impact laisse une zone de gravité qui ralentit les ennemis avant une frappe secondaire. Synergie : le Pinceau Miniature augmente la taille de la zone et la portée de l'onde.
8. **Fil de Restauration — ricochets :** relie plusieurs ennemis et transmet une partie des dégâts.
   - **Évolution A — Réseau de Restauration :** davantage d'ennemis sont reliés et une partie des dégâts reçus par l'un est répercutée sur les autres. Synergie : la Palette Catalytique fait se propager les statuts à travers les liens.
   - **Évolution B — Suture Vitale :** les éliminations d'ennemis reliés rendent une petite quantité de santé, avec un délai interne et un plafond par vague. Synergie : le Cœur de Porcelaine augmente la marge de survie nécessaire pour rester au contact.
9. **Cloche du Vernissage — contrôle :** onde périodique repoussant ou interrompant les ennemis légers.
   - **Évolution A — Résonance Fragile :** l'onde marque les ennemis touchés ; ils subissent davantage de dégâts lors de leur prochain impact puissant. Synergie : le Maillet de Marbre Noir consomme la marque pour infliger une onde secondaire.
   - **Évolution B — Silence de Galerie :** l'onde crée une zone de ralentissement et interrompt plus efficacement les attaques légères, sans neutraliser les boss. Synergie : le Flacon de Vermillon inflige davantage de dégâts aux ennemis ralentis dans ses flaques.
10. **Cadre Fracturé — invocation :** ouvre un cadre qui tire des éclats d'encre.
    - **Évolution A — Galerie Jumelle :** un second cadre apparaît à une distance limitée et les deux cadres peuvent viser une même cible. Synergie : la Miniature Fracturée augmente la fréquence de tir des cadres.
    - **Évolution B — Portail de la Toile :** le cadre ouvre périodiquement une brèche qui téléporte ses projectiles vers une cible marquée et provoque une petite explosion à l'arrivée. Synergie : l'Arbalète à Encre marque les cibles prioritaires et la Palette Catalytique ajoute un statut à l'explosion. **⚠ Cette évolution contredit l'interdiction de téléportation et de portail : à reformuler avant prototypage (§23.2).**

### 5.2 Règles des Instruments

- Maximum de six Instruments équipés, Instrument de départ inclus.
- Améliorer un Instrument équipé ne consomme pas un nouvel emplacement.
- Si les emplacements sont pleins, le joueur peut remplacer un Instrument ou refuser le choix. La conversion d'un objet refusé en ressource est une option future.
- Les attaques automatiques sélectionnent des cibles valides et respectent la cadence, la portée et, si applicable, la ligne de vue.
- Projectiles, zones et effets persistants ont des règles de collision et de durée explicites.
- Un Instrument déjà possédé n'est pas proposé en doublon, sauf règle explicite.

### 5.3 Rangs

- **Rang 1 :** version de base.
- **Rangs 2 à 4 :** amélioration progressive de paramètres ou d'effets.
- **Rang 5 :** version de base pleinement améliorée. Le rang 5 ne suffit pas, à lui seul, à rendre un Instrument éligible à une évolution.
- Les rangs peuvent améliorer dégâts, cadence, portée, nombre de projectiles, taille de zone ou durée d'effet.
- Les valeurs sont assez perceptibles pour que chaque rang soit utile.
- Un Instrument au rang maximum ne reçoit plus de rang statistique ordinaire mais reste éligible aux évolutions si les prérequis sont remplis.

### 5.4 Statistiques compatibles par type d'Instrument

Chaque Instrument déclare explicitement les statistiques compatibles. Baseline recommandée :

| Type d'Instrument | Dégâts | Cadence | Portée | Taille | Vitesse projectile | Durée | Critique |
|---|---|---|---|---|---|---|---|
| Projectile direct | Oui | Oui | Oui | Oui | Oui | Non, sauf effet persistant | Oui si applicable |
| Attaque de mêlée | Oui | Oui | Oui | Oui | Non | Non | Oui |
| Zone persistante | Oui | Oui ou recharge | Oui | Oui | Selon projectile | Oui | Selon design |
| Aura / effet autour du joueur | Oui | Selon cadence | Oui, rayon d'aura | Oui | Non | Selon effet | Selon design |
| Fil / lien | Oui | Selon activation | Oui | Selon nombre de cibles ou rayon | Non | Oui si lien temporaire | Selon design |
| Objet invoqué | Oui | Cadence d'attaque | Rayon de détection | Oui | Selon projectile | Durée d'invocation | Selon design |

La matrice est un guide, pas une permission automatique : la fiche de l'Instrument (§18.2) est la source de vérité.

### 5.5 Ciblage et cadence

**Ciblage.** Chaque Instrument définit sa règle de ciblage dans ses données :

- ennemi valide le plus proche ;
- ennemi le plus proche dans une portée ;
- ennemi dans une direction ou un cône ;
- point de sol ou zone autour du joueur ;
- tir dans la direction du déplacement ou de la caméra, si le design de l'Instrument l'exige.

Le ciblage évite les ennemis morts, invulnérables ou hors de portée. En cas d'égalité, la sélection est stable, pour que le tir ne saute pas d'une cible à l'autre à chaque image.

**Paramètres séparés par Instrument :** dégâts de base, temps entre attaques, portée, nombre de projectiles ou de cibles, vitesse et durée de vie des projectiles, taille de zone, règles de perforation ou de rebond, type de ciblage, effets de contrôle.

Tous les Instruments ne sont pas des projectiles : mêlée, zones, lignes et effets orbitaux partagent les règles générales de dégâts (§8.2) sans être forcés dans une seule implémentation.

**Limites techniques.**

- Pooler les projectiles et effets fréquemment créés.
- Ne pas rechercher tous les ennemis de la scène à chaque image pour chaque Instrument ; réutiliser les résultats de ciblage lorsque c'est sûr, à une fréquence adaptée.
- Les attaques de boss et les événements critiques conservent leur priorité de simulation.
- Les optimisations ne font pas disparaître une cible à l'écran et ne rendent pas une attaque injuste.

### 5.6 Évolutions : conditions et déroulement

Une évolution transforme le comportement d'un Instrument ; elle n'est ni un rang ni une rareté.

**Conditions requises (toutes) :**

1. L'Instrument est équipé et a atteint le **rang minimum** défini dans sa fiche (proposé : rang 5).
2. Le joueur possède un **Sceau d'Évolution**, obtenu en vainquant un mini-boss ou lors d'un événement prévu (§10.5).
3. Le niveau de joueur atteint un **palier** (niveau 10 recommandé ; paliers 10, 15 et 20 comme occasions, avec variantes d'équilibrage selon les branches).
4. Aucune évolution n'a déjà été choisie pour cet Instrument.

**Règles :**

- Les paliers 10, 15 et 20 sont des occasions de proposer une évolution, pas une évolution automatique. Les rangs 1 à 5 et les paliers de joueur sont deux progressions distinctes.
- Une évolution ne consomme pas d'emplacement supplémentaire et s'applique une seule fois par Instrument, sauf mécanique future explicite.
- Chaque Instrument dispose d'au moins **deux évolutions mutuellement exclusives** par run, de styles distincts. Chacune crée au moins deux interactions lisibles avec d'autres Instruments ou Reliques ; éviter les synergies qui n'ajoutent que des dégâts sans changer les décisions du joueur.
- L'évolution a un effet de gameplay visible et une identité visuelle et audio distincte, pas seulement un bonus de dégâts.
- Si plusieurs évolutions sont éligibles, seules celles compatibles avec les conditions sont proposées. Le joueur choisit l'Instrument puis la branche ; le système ne choisit jamais à sa place.
- Les raretés des améliorations ordinaires n'interviennent pas dans l'accès aux évolutions.
- L'évolution est une récompense puissante, pas une condition de victoire.

**Sceau d'Évolution et absence d'éligibilité :**

- Le Sceau n'est consommé que lorsqu'une évolution est effectivement appliquée.
- Si aucun Instrument n'est éligible, le Sceau est conservé pour plus tard, et le mini-boss accorde tout de même sa récompense secondaire.
- Si le joueur atteint un palier sans évolution éligible, il reçoit une récompense alternative ; le palier ne bloque jamais la progression.

L'interface d'évolution est décrite au §14.6.

---

## 6. Reliques

### 6.1 Liste proposée

1. **Masque de l'Inspiration :** augmente les dégâts au prix d'une légère réduction de résistance.
2. **Fiole d'Encre Vive :** réduit le délai entre les attaques.
3. **Bottes du Restaurateur :** augmentent la vitesse et améliorent légèrement l'esquive.
4. **Miroir Brisé :** renvoie une partie des dégâts reçus aux ennemis proches, selon un délai interne.
5. **Pinceau Miniature :** augmente la portée et la taille des zones.
6. **Sablier du Dernier Regard :** augmente la durée des effets de statut.
7. **Cœur de Porcelaine :** augmente les points de vie maximum.
8. **Boussole du Cadre :** augmente la quantité de Pigment, avec un plafond.
9. **Broche du Critique :** augmente les chances de coup critique.
10. **Miniature Fracturée :** améliore les Instruments liés aux cadres.

### 6.2 Règles

- Maximum de six Reliques équipées. Le contenu du MVP n'en a besoin que de quatre pour tester les interactions.
- Les Reliques modifient les statistiques ou ajoutent des effets passifs.
- Si les emplacements sont pleins, le joueur peut remplacer une Relique ou refuser la nouvelle.
- Les règles de cumul des effets similaires sont explicites (§7.4).
- Les effets affichent leur valeur et, si pertinent, leur durée.
- Les modificateurs de vitesse d'une Relique respectent la règle de vitesse unique (§3.2) : ils modifient la vitesse de base uniquement si leur effet le précise.
- Le système de rangs des Reliques reste à décider (§23.2). S'il est activé, les Reliques suivent les mêmes règles d'éligibilité que les Instruments (§7.5).

---

## 7. Progression en partie

### 7.1 Pigment et niveaux

- Les ennemis vaincus peuvent laisser du Pigment ; les quantités sont configurables par type d'ennemi, et la récompense n'est attribuée qu'une seule fois.
- Le joueur le récupère au contact ou dans un rayon de ramassage configurable.
- Le seuil d'expérience suit une **formule centrale et configurable**, calibrée sur une partie complète de 20 à 30 minutes, le nombre moyen d'éliminations et la distribution des récompenses, et non choisie parce qu'elle est populaire ailleurs.
- Une montée de niveau ne perd pas l'expérience excédentaire.
- Si plusieurs niveaux sont gagnés d'un coup, chaque niveau est résolu correctement, sans dupliquer les récompenses ni écraser un choix.
- L'interface affiche le Pigment courant, le seuil suivant et le niveau (§14.4).
- Les objets de Pigment sont nettoyés correctement après collecte.

### 7.2 Offres d'amélioration

À chaque niveau, le jeu se met en pause et présente **trois choix** valides. Une offre respecte :

- un nombre d'options clairement visible ;
- une description concise de l'effet ;
- la distinction nette entre Instrument, Relique, amélioration de rang, bonus générique et évolution ;
- l'indication des valeurs modifiées ;
- la prévention des doublons invalides ;
- la prise en compte des emplacements occupés et des limites d'équipement.

**Catégories de choix :**

1. amélioration d'un Instrument possédé ;
2. nouvel Instrument, si un emplacement est libre ;
3. amélioration d'une Relique possédée, si les rangs de Relique sont activés ;
4. nouvelle Relique, si un emplacement est libre ;
5. bonus statistique générique prévu par la table de récompenses ;
6. évolution d'Instrument, uniquement dans l'interface spéciale et si les conditions sont remplies (§5.6).

Les évolutions n'occupent jamais silencieusement une carte ordinaire : elles ont une présentation spéciale et des prérequis explicites.

**Ordre du générateur :**

1. Construire la liste des récompenses éligibles selon l'inventaire, les emplacements et les prérequis.
2. Éliminer les options déjà maximisées, incompatibles ou bloquées.
3. Tirer les catégories selon des poids configurables.
4. Pour chaque amélioration statistique d'Instrument, tirer la rareté (§7.3).
5. Choisir une statistique autorisée pour cet Instrument.
6. Calculer le montant du bonus selon la rareté et la statistique (§7.4).
7. Éliminer les doublons visuels ou mécaniques parmi les trois cartes.
8. S'il reste moins de trois options valides, compléter avec des bonus génériques autorisés.
9. Si aucune option n'est disponible, utiliser une option de secours définie (par exemple un petit bonus de santé ou une ressource de run), sans bloquer le joueur.

Règles complémentaires :

- Le joueur dispose toujours d'au moins une option valide. Les options impossibles sont exclues ou clairement expliquées ; une carte qui ne peut pas être prise n'est jamais affichée.
- Le générateur utilise une graine aléatoire de run pour reproduire les bugs.
- Le choix appliqué est validé une seule fois, puis l'écran se ferme.
- Les poids des catégories (fréquence des Instruments, Reliques et bonus) sont des données configurables, à équilibrer par les tests.

### 7.3 Raretés des améliorations ordinaires

La rareté détermine **uniquement l'ampleur du bonus statistique** d'une amélioration ordinaire. Elle ne change pas le comportement de l'Instrument et ne crée aucune mécanique. Les transformations de gameplay sont réservées aux évolutions, aux passifs de personnage et aux systèmes explicitement conçus pour cela.

| Rareté | Couleur | Probabilité de base | Contenu |
|---|---|---:|---|
| Commun | Vert | 60 % | Petit bonus sur une statistique |
| Peu commun | Bleu | 25 % | Bonus plus élevé, généralement sur une seule statistique |
| Épique | Violet | 12 % | Bonus élevé, éventuellement réparti sur deux statistiques compatibles |
| Légendaire | Jaune | 3 % | Bonus exceptionnel, pouvant combiner plusieurs statistiques compatibles, sans mécanique nouvelle |

Règles :

- La somme des probabilités vaut 100 % avant tout modificateur de chance.
- Ces probabilités s'appliquent aux **améliorations statistiques d'Instrument**, pas à toutes les cartes. Un nouvel Instrument, une Relique, un soin ou une évolution ne reçoit pas de rareté d'arme si ce n'est pas prévu par sa catégorie.
- Chaque Instrument ne propose que les statistiques pertinentes pour son fonctionnement.
- Une amélioration légendaire n'est pas une évolution et ne doit jamais suggérer une nouvelle attaque.
- Les légendaires restent rares et plaisantes, sans être nécessaires pour gagner. Une construction reste viable sans obtenir une rareté spécifique.
- **Prototype :** probabilités fixes. Une statistique de Chance (personnage ou Relique) pourra les modifier plus tard, avec une limite explicite et des tests. Ne pas augmenter automatiquement la rareté au fil de la run tant que ce comportement n'est pas testé. Une protection contre la malchance (chances progressivement accrues après une longue série sans rareté supérieure, puis réinitialisées) est une option à tester ultérieurement.
- La couleur n'est jamais le seul indicateur : afficher aussi le nom de la rareté et une icône (§14.10).

### 7.4 Montants et cumul des bonus

Valeurs de départ pour les statistiques **de base de l'Instrument**, avant multiplicateurs de personnage ou de Relique :

| Rareté | Dégâts | Cadence | Portée | Taille / rayon | Vitesse projectile |
|---|---:|---:|---:|---:|---:|
| Commun | +10 % | +8 % | +10 % | +10 % | +10 % |
| Peu commun | +18 % | +14 % | +18 % | +18 % | +18 % |
| Épique | +25 % | +20 % | +25 % | +25 % | +25 % |
| Légendaire | +40 % | +25 % | +35 % | +35 % | +35 % |

Exemple de carte Épique : +25 % de dégâts avec, en statistique secondaire plus petite, +15 % de taille.

Règles sur les montants :

- Ajuster les valeurs par Instrument ; ne proposer que les statistiques réellement utiles.
- Une carte peut donner une statistique principale et, pour Épique et Légendaire, une statistique secondaire plus petite.
- La cadence ne dépasse pas la limite technique définie par arme.
- Les bonus de durée ne s'appliquent qu'aux effets persistants.
- Les bonus de critique sont exprimés en points de pourcentage, pas en pourcentage relatif, et sont plafonnés.
- Une amélioration ne produit jamais une valeur négative ou NaN.
- La carte affiche la valeur du bonus et la valeur finale estimée.

**Cumul des statistiques :**

- **Bonus additifs d'une même famille :** les bonus de dégâts d'améliorations s'additionnent dans un même multiplicateur d'amélioration.
- **Familles distinctes :** les multiplicateurs de personnage, d'amélioration et de synergie se multiplient entre eux, si cette séparation est nécessaire.
- **Cadence :** utiliser soit un intervalle entre attaques, soit une fréquence par seconde, sans mélanger les deux représentations.
- **Critique :** `dégâts moyens = dégâts normaux × (1 − chanceCritique) + dégâtsCritiques × chanceCritique`.
- **Limites :** toute statistique pouvant casser l'équilibrage a une limite supérieure documentée.
- **Arrondi :** arrondir uniquement pour l'affichage ; conserver la précision interne.

L'interface explique les valeurs finales sans exposer de formules internes complexes.

### 7.5 Éligibilité

- Un nouvel Instrument n'est proposé que si un emplacement est libre et qu'il n'est pas déjà possédé (sauf règle explicite de doublon).
- Un Instrument possédé reçoit des améliorations jusqu'au rang maximum.
- Si tous les Instruments sont au rang maximum, les options correspondantes sont retirées du pool.
- Les options uniques déjà obtenues ne sont pas proposées une seconde fois.
- Le système affiche un motif clair quand une récompense n'est pas disponible, sans remplir l'écran de messages techniques.

---

## 8. Combat, dégâts, statuts et synergies

### 8.1 Architecture du combat

Le système de combat sépare au minimum :

- la vie et la réception des dégâts ;
- l'émission des dégâts ;
- les attaques des Instruments ;
- les projectiles et zones persistantes ;
- les effets de statut ;
- l'invulnérabilité et les fenêtres d'action ;
- les retours visuels et sonores ;
- la mort, les récompenses et le nettoyage des entités.

Éviter un script unique qui gère à la fois entrées, vie, projectiles, IA, effets visuels et interface.

### 8.2 Application des dégâts

Chaque attaque fournit des informations structurées :

- source de l'attaque et cible ;
- valeur de dégâts et type d'attaque ;
- tags éventuels (contact, projectile, zone, boss, environnement) ;
- effets additionnels éventuels ;
- identifiant d'origine, qui évite qu'une même zone inflige des dégâts à chaque image par erreur.

Le récepteur vérifie les règles applicables : cible valide, invulnérabilité, réduction, immunité spécifique et éventuel temps d'invulnérabilité après impact.

**Ordre de résolution (stable et documenté) :**

1. valider que la source et la cible sont actives ;
2. vérifier immunité et invulnérabilité ;
3. calculer les dégâts de base ;
4. appliquer les bonus additifs de la famille ;
5. appliquer les multiplicateurs de famille ;
6. résoudre le critique ;
7. appliquer résistance et réduction ;
8. appliquer les règles de bouclier ;
9. réduire la santé ;
10. déclencher une seule fois les effets d'impact, les statuts et l'élimination.

Les exceptions (dégâts fixes, dégâts qui ignorent l'armure) sont déclarées explicitement. Le calcul final est centralisé et testé de manière isolée.

### 8.3 Règles de combat

- Une attaque n'inflige pas de dégâts plusieurs fois par image à une même cible, sauf effet explicitement conçu pour cela.
- Une zone persistante a une fréquence de dégâts explicite.
- Les dégâts du joueur ne sont pas appliqués à un ennemi déjà mort. Les ennemis morts ne bloquent plus le ciblage et ne sont plus des menaces actives.
- La logique de dégâts suit la collision, pas seulement une animation : collision et visuel restent cohérents.
- Les nombres de dégâts, de critiques et d'effets sont réglables dans les données.
- Les effets de statut ont une durée, une intensité et une règle de cumul définies (§8.6).

### 8.4 Retours de combat

Chaque impact important fournit une combinaison proportionnée de :

- réaction d'impact ou animation ;
- effet visuel lisible ;
- son adapté ;
- nombre de dégâts facultatif (éviter les chiffres permanents s'ils masquent le combat) ;
- léger recul si le type d'ennemi l'autorise.

Pas de tremblement de caméra sur chaque impact mineur. Les effets importants ne masquent ni le sol, ni les projectiles, ni les avertissements de boss.

### 8.5 Points de vie, soins et défaite

- Les points de vie maximum viennent des données du personnage, puis des modificateurs applicables.
- Les soins ne dépassent pas le maximum, sauf mécanisme explicitement défini.
- Le joueur reçoit un retour clair lorsqu'il subit un dégât important.
- L'invulnérabilité de l'esquive est distincte d'une éventuelle invulnérabilité temporaire après impact ; si les deux existent, leur interaction est documentée.
- Le prototype commence avec une règle simple de soins et de dégâts. Résurrection, bouclier, vol de vie et armure temporaire ne sont ajoutés que si un Instrument, une Relique ou un personnage les justifie.
- La défaite suit la procédure du §2.4.

### 8.6 Statuts

Chaque statut définit :

- identifiant stable et nom affiché ;
- durée de base ;
- source autorisée et cible autorisée ;
- règle de cumul ;
- fréquence de tick ;
- résistance ou immunité éventuelle ;
- feedback visuel et audio ;
- effets de fin ;
- interactions avec les autres statuts.

**Règles de cumul disponibles** (chaque statut en choisit une et l'indique dans sa fiche ; ne pas les mélanger implicitement) :

- **Renouveler la durée :** réapplique la durée sans augmenter l'intensité.
- **Intensité plafonnée :** augmente une valeur jusqu'à un plafond explicite.
- **Instances indépendantes :** seulement si l'effet exige réellement plusieurs sources ; nombre d'instances limité par cible.
- **Remplacement :** un statut plus fort remplace un statut plus faible du même type.
- **Immunité temporaire :** après certains contrôles de foule, une cible reçoit une courte résistance pour éviter les verrouillages permanents.

**Catalogue initial :**

| Statut | Effet proposé | Règle de cumul |
|---|---|---|
| Marqué | Rend une cible éligible aux effets qui exigent un marquage | Renouvelle la durée |
| Contamination vermillon | Dégâts périodiques modérés | Intensité plafonnée, cadence fixe |
| Ralentissement | Réduit la vitesse de déplacement | La valeur la plus forte prévaut, durée renouvelable |
| Vulnérabilité | Augmente les dégâts reçus | Valeur plafonnée, pas de cumul sans limite |
| Étourdissement | Interrompt une action non protégée | Durée réduite ou immunité temporaire sur élites et boss |

Le MVP n'implémente que les statuts requis par ses personnages, Instruments et ennemis. Les règles de cumul précises de chaque statut sont à confirmer par les tests (§23.2).

### 8.7 Synergies

**Catégories :**

1. **Instrument + Instrument :** les attaques se complètent ou déclenchent un effet secondaire limité.
2. **Instrument + Relique :** la Relique amplifie une statistique ou une condition de l'Instrument.
3. **Instrument + personnage :** le passif favorise un style de jeu sans rendre les autres Instruments inutiles.
4. **Instrument + terrain :** une attaque exploite une surface ou un pigment.
5. **Statut + statut :** réaction explicite entre deux statuts différents, si elle est autorisée.
6. **Personnage + terrain :** le terrain modifie déplacement ou ressource selon une règle propre au personnage.

**Contrat d'une synergie.** Chaque synergie définit :

- les identifiants des contenus requis ;
- les conditions d'activation ;
- l'effet exact et sa durée ;
- la priorité si plusieurs synergies se déclenchent ensemble ;
- la fréquence maximale et les limites de propagation ;
- le feedback visuel et sonore ;
- les règles de cumul avec les autres bonus ;
- le comportement quand un prérequis est retiré ou remplacé.

**Règles anti-boucle :**

- Une réaction déclenchée par une autre réaction ne se réactive pas indéfiniment.
- Les dégâts propagés ne déclenchent pas une propagation identique en cascade, sauf règle explicite avec profondeur maximale.
- Chaque élimination ne déclenche qu'une seule récompense d'élimination.
- Une explosion qui tue plusieurs ennemis ne génère pas une quantité illimitée de surfaces ou de projectiles.
- La propagation d'un statut est plafonnée par cible et par seconde.
- Les effets d'une même source conservent un identifiant d'origine pour éviter les doubles déclenchements.

**Exemples à tester** (propositions, non équilibrées) :

| Combinaison | Effet proposé | Garde-fou |
|---|---|---|
| Flacon de Vermillon + surface de vermillon | Augmente légèrement la durée ou le rayon des surfaces créées | Durée et rayon plafonnés |
| Éclats de Fusain + ennemis marqués | Les projectiles peuvent privilégier les cibles marquées | Priorité de ciblage, pas de dégâts gratuits infinis |
| Palette Catalytique + statut existant | Peut déclencher une réaction de pigment | Temps de recharge par cible |
| Fil de Restauration + ennemis liés | Une partie limitée des dégâts se transmet aux cibles reliées | Propagation sans chaîne récursive |
| Maillet de Marbre Noir + groupe d'ennemis | Onde de choc plus efficace lorsque plusieurs ennemis sont proches | Rayon et multiplicateur plafonnés |
| Bottes du Restaurateur + surfaces noires | Bonus de mobilité réduit mais régulier sur surface compatible | Ne dépasse pas le plafond de vitesse |

---

## 9. Peinture au sol

La peinture au sol est une mécanique de déplacement et de combat, pas seulement un effet visuel. Pour préserver la lisibilité et limiter le coût de production, le MVP commence avec deux types de surface ; le système est étendu après validation.

### 9.1 Règles générales

Une **surface** est une zone de gameplay temporaire créée par une arme, un personnage, un ennemi ou un événement. Elle possède :

- un identifiant de type de pigment ;
- une position et un rayon ;
- une durée restante ;
- une source de création ;
- les factions ou propriétaires affectés ;
- les effets activés ;
- une priorité de superposition ;
- un budget d'effets et une limite de cumul.

Valeurs de départ :

- durée standard : 5 secondes ; durée maximale après extension : 8 secondes ;
- rayon initial typique : 1,5 m, ajustable selon la source ;
- nombre maximal de surfaces actives du joueur : 20 ;
- au-delà de la limite, remplacer ou rafraîchir la surface la plus ancienne du même type selon la règle de la source ;
- pas de collider physique par éclaboussure visuelle : utiliser une représentation de zone légère et un contrôle de chevauchement.

Les surfaces ne doivent ni enfermer le joueur ni rendre les déplacements imprévisibles. L'effet visuel peut s'estomper avant la disparition logique, tant que l'état de gameplay reste lisible. Ces valeurs sont testées dans l'arène réelle.

### 9.2 Encre noire (terrain initial)

- Le Patineur (futur) gagne de l'accélération sur une surface noire compatible, jusqu'à un plafond de vitesse.
- L'Encré (futur) peut récupérer une petite quantité de ressource en traversant ses propres surfaces, si cette mécanique est activée.
- Les ennemis ne sont pas ralentis par défaut : l'encre noire ne devient pas un contrôle universel.
- Les surfaces noires ne modifient pas la friction du moteur physique de façon imprévisible ; le bonus est appliqué via le contrôleur de déplacement.

### 9.3 Vermillon (terrain initial)

- Peut appliquer un effet de marquage ou de dégâts aux ennemis qui entrent dans la zone, selon la source qui a créé la surface.
- Un même ennemi ne reçoit le même déclenchement qu'une fois par intervalle défini (valeur de départ : 0,75 s).
- Les déclenchements sont indépendants du nombre de colliders qui se chevauchent.
- Les dégâts périodiques ne se cumulent pas sans limite : renouveler la durée ou appliquer une intensité plafonnée.

### 9.4 Superposition et pigments futurs

Pour le MVP, seuls l'encre noire et le vermillon existent en gameplay. Si les deux se chevauchent :

- les deux effets coexistent s'ils sont compatibles ;
- un même effet n'est jamais appliqué plusieurs fois par image ;
- les dégâts périodiques d'un même type ne se cumulent pas sans plafond ;
- l'effet de déplacement est résolu séparément de l'effet offensif ;
- le rendu affiche les deux couleurs de façon lisible, sans couvrir le personnage ni les télégraphes ennemis.

Pigments futurs (hors périmètre tant que les deux premiers ne sont pas validés, et jamais codés comme systèmes cachés dans le MVP) : un pigment bleu qui ralentit les ennemis, un pigment or, une peinture contaminée qui transmet un statut aux ennemis qui la traversent.

### 9.5 Interactions entre surface et personnages

| Personnage / rôle | Interaction proposée |
|---|---|
| Exilé | Aucun bonus spécial de terrain dans le MVP ; sert de référence de contrôle |
| Virtuose | Traverse les surfaces sans ralentissement spécial ; ses attaques de précision restent lisibles |
| Graveur | Les lignes de tir et les projectiles restent lisibles au-dessus des surfaces |
| Alchimiste | Peut déclencher les réactions de pigment définies par sa compétence, avec un plafond de fréquence |
| Sculpteur | Ne perd pas le contrôle de ses attaques lourdes sur les surfaces |
| Restauratrice | Les fils et marquages restent lisibles sur les surfaces |
| Patineur (futur) | Utilise l'encre noire pour conserver ou gagner de l'inertie |
| Encré (futur) | Récupère une ressource sur ses surfaces compatibles |
| Marionnettiste (futur) | Peut exploiter les surfaces comme support visuel ; aucun bonus implicite |

### 9.6 Performance et lisibilité

- Séparer les surfaces de gameplay des particules décoratives.
- Réutiliser les objets visuels par pooling si le profilage confirme un gain.
- Éviter un système de physique par goutte de peinture.
- Limiter les déclenchements de statut par cible et par intervalle.
- Réduire les particules décoratives avant de réduire les télégraphes d'attaque.
- Les avertissements de boss restent toujours visibles au-dessus des effets de terrain.
- Tester l'arène avec la densité maximale d'ennemis, de projectiles et de surfaces prévue.

---

## 10. Ennemis, élites, mini-boss et boss

### 10.1 Principes

Les modèles sont stylisés, simples et lisibles. Plusieurs ennemis peuvent partager des animations ou des composants. Une variante apporte un comportement ou un rôle distinct, pas seulement une couleur. Chaque ennemi configure ses points de vie, sa vitesse, ses dégâts, sa portée et ses délais d'attaque. Les récompenses ne sont attribuées qu'une seule fois. Les noms, formes, animations et attaques s'intègrent à l'esthétique du jeu.

### 10.2 Ennemis communs

| Ennemi | Silhouette et comportement |
|---|---|
| **Coulure** | Masse noire arrondie ; avance lentement |
| **Masque Fêlé** | Masque flottant ; poursuit et charge brièvement |
| **Chevalet Brisé** | Assemblage de bois ; lent, résistant, attaque au contact |
| **Éclat de Toile** | Fragment flottant ; tire à distance |
| **Tache Vorace** | Petite masse à bouche ; bondit puis se replie |
| **Marionnette du Peintre** | Humanoïde simple ; approche et attaque lisiblement |
| **Cadre Ambulant** | Cadre animé ; bloque le passage et protège ses alliés |
| **Corbeau de Fusain** | Oiseau noir ; tourne autour du joueur et plonge |
| **Portrait Effacé** | Humanoïde pâle ; disparaît puis réapparaît |
| **Goutte Vermillon** | Sphère rouge ; explose au contact ou après un délai |

**Les quatre rôles du MVP.** Les ennemis communs du MVP couvrent quatre rôles suffisamment différents :

1. **Poursuivant (mêlée lente) :** se rapproche du joueur et maintient la pression.
2. **Rapide et fragile :** menace le positionnement et punit les trajectoires prévisibles.
3. **Lourd (résistant) :** lent, résistant et dangereux au contact.
4. **Attaquant à distance ou de zone :** oblige à lire les télégraphes et à se déplacer.

*Sélection proposée, à valider (§23.2) :* Coulure (poursuivant), Masque Fêlé (rapide, charge brève), Chevalet Brisé (lourd), Éclat de Toile (distance). Le Masque Fêlé est de toute façon requis par le boss recommandé, qui en invoque.

### 10.3 Archétypes de rôle (pistes futures)

Les archétypes se différencient par leur **rôle tactique**, pas uniquement par leurs points de vie.

- **Gribouilleur :** rapide et fragile ; force le joueur à se déplacer.
- **Bloc de Marbre :** lent et résistant ; menace l'espace proche.
- **Œil du Conservateur :** à distance ; oblige à interrompre ou modifier sa trajectoire.
- **Coloriste :** soutien ; renforce ses alliés ou modifie temporairement la peinture du sol.

Chaque ennemi a une fiche normalisée (§18.5).

### 10.4 Élites

**Exigences du premier élite (MVP) :**

- reconnaissable à distance ;
- au moins une attaque annoncée ;
- une variation de comportement, pas seulement plus de points de vie ;
- une table de récompenses définie ;
- soumis à la priorité de simulation même loin du joueur (§12.5).

**Élites envisagés (futur) :**

- **Masque Royal :** longue charge suivie d'une onde de choc.
- **Conservateur :** protège temporairement les ennemis proches.
- **Tache Couronnée :** laisse des zones d'encre dangereuses.
- **Copiste :** reproduit temporairement le comportement d'un ennemi.
- **Portrait Inachevé :** alterne entre forme vulnérable et forme agressive.

Pour la V1, un seul élite est retenu ; son choix reste ouvert (§23.2).

### 10.5 Mini-boss

Les mini-boss sont des objectifs de combat intermédiaires qui récompensent la maîtrise et offrent l'occasion de transformer un Instrument. Ils ne remplacent pas le boss final.

- La victoire donne un **Sceau d'Évolution garanti**, plus une **récompense secondaire** (Pigment, soin, coffre ou ressource de run ; à définir).
- Le mini-boss est annoncé, ses attaques sont télégraphiées, et sa difficulté dépasse celle d'un élite ordinaire.
- La victoire doit toujours sembler rentable, même si aucun Instrument n'a encore le rang requis. Le Sceau est alors conservé (§5.6).
- L'interface explique clairement pourquoi un Instrument est ou n'est pas éligible (§14.6).
- Pour la première version jouable : un seul mini-boss et au moins une évolution testable sur un Instrument.
- L'évolution n'est jamais obligatoire pour terminer une run.

### 10.6 Boss

**Règles communes :**

- barre de santé visible ;
- au moins trois motifs d'attaque distincts ;
- attaques majeures télégraphiées visuellement ou par le son ;
- attaques évitables par déplacement ou esquive, sauf exception annoncée ;
- effets du joueur ne masquant pas les zones dangereuses ;
- conditions de victoire fiables et vérifiables ;
- les apparitions ordinaires cessent dès le début du combat (§11.6) ;
- phases d'invulnérabilité longues à éviter : le boss teste le déplacement, la lecture des attaques et la construction du joueur.

**Boss envisagés :**

- **Le Portrait Sans Visage (boss du MVP) :** masque géant flottant, projectiles, zones explosives et invocation de Masques Fêlés.
- **Le Conservateur Déchu :** assemblage de cadres et de chevalets, frappes au sol et protection de points faibles.
- **L'Œuvre Inachevée :** entité humanoïde abstraite qui modifie les zones dangereuses ; boss final futur.
- Pistes alternatives : **Le Conservateur** (contrôle l'espace, à rapprocher du Conservateur Déchu), **La Toile Déchirée** (modifie les zones sûres), **Le Maître des Pigments** (change temporairement les propriétés de la peinture).

**Structure d'un combat de boss** (Portrait Sans Visage comme modèle) :

- phase de lecture et d'apprentissage ;
- phase d'attaque principale ;
- transition annoncée ;
- phase plus exigeante combinant les mécaniques déjà apprises ;
- fenêtre de vulnérabilité si le design en a besoin.

**Chaque attaque de boss précise :** son signal visuel et sonore, son délai avant impact, sa zone et sa durée, si le saut peut l'éviter, si l'esquive peut l'éviter, ses dégâts et effets, sa récupération et ses cas limites de collision.

Le boss est testé avec tous les profils de personnage prévus, notamment celui dont le saut est le plus bas et celui dont l'esquive est la plus courte. Aucune attaque obligatoire n'est impossible à éviter avec les statistiques de base d'un personnage. Les fiches de boss suivent le modèle du §18.6.

---

## 11. Vagues et difficulté

### 11.1 Durée et phases d'une run

Durée cible : 20 à 30 minutes, boss compris ; le prototype peut être plus court pour itérer. Le directeur de vague est piloté par un **chronomètre de run et des paliers explicites**, jamais par un nombre d'images ni par le niveau seul.

| Phase | Temps indicatif | Objectif | Contenu et repères |
|---|---|---|---|
| A | 0–4 min | Apprendre et se construire | Poursuivants simples (type Gribouilleur ou Coulure). 0–1 min : entrée en jeu, tutoriel léger intégré à l'action, premières cibles faciles. 1–4 min : premier choix d'amélioration, rythme d'attaque |
| B | 4–8 min | Ajouter une décision tactique | Groupes mixtes simples, second rôle ennemi ; première synergie possible |
| C | 8–12 min | Tester la construction | Première élite ou événement de récompense ; le joueur commence à spécialiser sa construction |
| D | 12–18 min | Pression et priorités | Groupes mixtes, tireurs et ennemis résistants ; choix de priorités |
| E | 18–20 min | Préparer le boss | Menaces lisibles, annonce du boss, possibilité de ramasser le Pigment restant et de finir un choix ; aucune récompense gratuite garantie juste avant le boss |
| F | 20–25 min | Épreuve finale | Boss seul avec ses attaques et ses phases ; aucune apparition ordinaire |

Ces temps sont des repères ajustés par les tests. En fin de run : bilan, calcul des récompenses permanentes, enregistrement des déblocages, puis recommencer ou retour au menu (§2.4).

### 11.2 Directeur de vague

Le directeur décide :

- du temps écoulé et de la phase ;
- des familles d'ennemis autorisées ;
- du budget de menace disponible ;
- du nombre maximal d'ennemis actifs ;
- des événements et élites disponibles ;
- des périodes de transition ;
- du moment du combat de boss.

Il tient aussi compte du type de zone et de la proximité du boss. Il ne fait pas apparaître d'ennemis si la limite globale est atteinte. Pendant un écran de choix, le temps de combat est réellement en pause.

### 11.3 Budget de menace et d'apparition

Chaque ennemi reçoit un **coût de menace**. Le directeur choisit une combinaison dont le coût total ne dépasse pas le budget de la phase : la difficulté ne repose pas uniquement sur les points de vie.

Le directeur respecte :

- une limite d'ennemis actifs ;
- une limite de créations par intervalle ;
- une distance minimale de sécurité autour du joueur ;
- des points d'apparition hors caméra ou clairement télégraphiés ;
- les contraintes de navigation et de collision ;
- une réserve de budget pour les élites et les événements importants.

Les nombres exacts d'ennemis actifs sont fixés après les tests de performance sur la machine cible. Les 1 000 ennemis du §12.1 sont une cible de stress test, pas une population de jeu.

### 11.4 Paramètres de difficulté

La difficulté peut augmenter par :

- le budget de menace et la fréquence ou la cadence d'apparition ;
- la composition et la diversité des familles d'ennemis ;
- la santé et les dégâts, avec modération et dans une plage limitée ;
- les nouvelles attaques ou variantes, et le nombre d'attaques simultanées ;
- les événements et la fréquence des élites ;
- la pression spatiale et le contrôle de zones.

Elle ne se contente pas d'augmenter toutes les statistiques de façon uniforme. La vitesse des projectiles et la taille des télégraphes restent équitables. Une difficulté lisible demande au joueur de mieux se déplacer, sauter, esquiver et choisir ses améliorations.

Des options de difficulté supplémentaires sont une décision ouverte (§23.2).

### 11.5 Transitions

Les changements de phase sont perceptibles : annonce, son, changement de musique ou de lumière, puis arrivée des nouvelles menaces. Pas d'apparition soudaine au contact du joueur.

### 11.6 Apparition du boss

Au début de l'introduction du boss :

- arrêter les nouvelles apparitions ordinaires ;
- désactiver les événements qui créeraient d'autres ennemis ;
- retirer proprement les ennemis ordinaires déjà présents, avec une courte animation de dissolution, pour éviter que le boss soit entouré d'une horde non prévue (règle retenue pour le MVP) ;
- nettoyer ou laisser expirer les projectiles ordinaires selon une durée courte et prévisible ;
- afficher le nom du boss, sa barre de santé et un avertissement sonore ;
- démarrer le combat seulement lorsque l'arène et le boss sont prêts.

---

## 12. Hordes, simulation et performance

### 12.1 Objectif

Construire une base capable de gérer une horde importante dans Unity 6 URP tout en préservant :

- des déplacements lisibles et réactifs du joueur ;
- des ennemis qui suivent le joueur sans traverser les murs ;
- des collisions stables et des interactions de combat cohérentes ;
- des animations dont le coût dépend de la distance et de l'importance de l'ennemi ;
- une fréquence d'images aussi stable que possible ;
- une architecture accessible à un développeur débutant et extensible après le prototype.

**Le nombre de 1 000 ennemis est une valeur de test de charge, pas une densité de jeu.** Le jeu n'est pas conçu pour afficher normalement 1 000 ennemis simultanés. Ce nombre sert à mesurer les limites, détecter les pics de coût et vérifier que les systèmes ne se dégradent pas brutalement. Le nombre réel de la horde est défini après les tests de lisibilité, de gameplay et de performances, et aucune capacité de 1 000 ennemis n'est annoncée avant mesure sur la machine cible.

### 12.2 Architecture de simulation

**Trois responsabilités séparées :**

1. **Données de l'ennemi :** santé, vitesse, dégâts, type, état, cible, récompense, paramètres.
2. **Simulation :** décision, direction, navigation, déplacement, collisions nécessaires, combat.
3. **Présentation :** mesh, animation, matériau, effets visuels, niveau de détail.

La présence visuelle d'un ennemi n'impose pas une simulation complète. Inversement, un ennemi ne devient pas invulnérable ni étranger au gameplay parce qu'il est loin de la caméra.

**Contrôleur central.** Un `EnemySimulationManager` organise les mises à jour des ennemis actifs, réparties en lots ou en ticks selon la priorité de chaque ennemi. Il :

- conserve une collection d'ennemis actifs sans rechercher leurs composants dans la scène à chaque image ;
- permet l'enregistrement et le retrait propres des ennemis ;
- répartit les mises à jour coûteuses dans le temps pour éviter les pics ;
- privilégie les ennemis proches, visibles, en combat ou sur le point d'interagir avec le joueur ;
- diminue la fréquence de calcul des ennemis éloignés lorsque le gameplay n'en souffre pas ;
- gère pause, mort, désactivation, réutilisation par pooling et changement de scène ;
- expose des compteurs de diagnostic : ennemis actifs, ennemis proches, fréquence de simulation, durée approximative des traitements.

La taille des lots est configurable et testée ; aucun lot arbitrairement grand n'est exécuté sans mesure.

**Pas d'ECS/DOTS au départ.** Le prototype utilise des composants C# et une simulation centralisée simple. Entities/DOTS, Job System et Burst ne sont envisagés que si le Profiler montre que le prototype classique ne répond pas aux objectifs. Éviter d'ajouter prématurément une dépendance et une complexité d'outillage.

### 12.3 Déplacement du joueur et physique des ennemis

**Joueur.** Il utilise initialement un `CharacterController` pour un contrôle direct du déplacement, du saut et de l'esquive. Les ennemis ne peuvent pas le pousser ou le bloquer de façon imprévisible par accumulation de forces physiques.

**Ennemis ordinaires.** Ne pas imposer un `Rigidbody` dynamique à chacun juste pour rendre la foule « physique ».

- Mouvement cinématique ou contrôlé pour les ennemis qui poursuivent le joueur.
- Colliders simples pour empêcher la traversée des murs et détecter les interactions nécessaires.
- `Rigidbody` dynamique seulement quand la simulation physique est utile (recul, chute, impulsion, mort physique).
- Désactiver ou simplifier les interactions physiques qui ne contribuent pas au gameplay.
- Les ennemis ordinaires n'entrent pas nécessairement en collision physique entre eux : leur espacement est géré par évitement local et règles de séparation.

**Boss, élites et réactions importantes.** Ils peuvent utiliser un mouvement et des collisions plus riches, car leur lecture et leur précision priment, tout en restant soumis aux mêmes exigences de stabilité et de profilage.

**Cohérence avec la physique Unity.** Ne pas déplacer un `Rigidbody` dynamique en écrivant directement sa position à chaque image. Choisir une méthode de mouvement compatible avec son mode physique et respecter la séparation `Update` / `FixedUpdate`. La fréquence de l'IA et celle de la physique peuvent différer, mais leurs échanges de données restent cohérents.

### 12.4 Navigation

La poursuite directe par le vecteur « ennemi vers joueur » ne suffit pas : elle est complétée par une navigation qui comprend les obstacles.

**Solution recommandée pour la horde :** une grille de navigation ou un **champ de direction partagé**.

1. Représenter les zones praticables et les obstacles fixes.
2. Calculer une destination ou un champ de coût depuis la position du joueur.
3. Laisser de nombreux ennemis consulter une direction commune selon leur cellule.
4. Actualiser le champ à fréquence contrôlée quand le joueur change de zone.
5. Compléter la direction globale par un évitement local (piliers, angles, autres ennemis).

Cette approche évite de calculer un chemin complet par ennemi et convient à une arène dont la destination principale est le joueur.

**Alternative de prototype :** un `NavMeshAgent` valide rapidement la navigation avec quelques ennemis. Ne pas supposer que 1 000 agents avec recalculs fréquents seront performants : mesurer avant de choisir.

**Évitement local.** Raycasts, sondes ou steering aident à contourner un obstacle proche mais ne remplacent pas une navigation globale (échec possible dans les culs-de-sac ou contre un mur qui bloque tout). La solution garantit :

- aucune traversée des murs statiques ;
- pas de téléportation de correction pour sortir d'un obstacle ;
- un comportement raisonnable dans les couloirs et autour des piliers ;
- une récupération contrôlée si un ennemi reste bloqué ;
- la distinction entre collision de décor, séparation de foule et collision de combat.

Murs et piliers utilisent des colliders adaptés ; les couches de collision sont configurées explicitement.

### 12.5 Animation adaptative à la distance

La distance au joueur choisit **la qualité et la fréquence de la représentation visuelle**, jamais les règles de gameplay. Seuils de départ :

| Niveau | Distance indicative | Animation recommandée |
|---|---:|---|
| LOD 0 — proche | 0 à 12 m | Animation complète, transitions fluides, effets et réactions détaillées |
| LOD 1 — moyenne | 12 à 28 m | Animation simplifiée, évaluation réduite, effets secondaires limités |
| LOD 2 — lointaine | 28 à 45 m | Animation très simplifiée ou poses pré-calculées, mise à jour visuelle moins fréquente |
| LOD 3 — très lointaine / hors écran | Plus de 45 m ou non visible durablement | Pas d'animation détaillée ; pose ou animation à faible fréquence, voire présentation réduite si l'ennemi n'a pas besoin d'être rendu |

La visibilité, la taille projetée à l'écran, l'état de combat et le type d'ennemi peuvent modifier le niveau choisi.

**Priorités qui passent avant la distance :**

- boss et élites : animations et télégraphes de combat lisibles ;
- attaque imminente, attaque en cours, étourdissement ou mort : conserver la pose et le timing nécessaires à la lecture ;
- ennemi touché par le joueur ou en interaction importante : ne pas retarder la réaction au point de rendre l'impact confus ;
- ennemi ordinaire éloigné et hors écran : niveau de détail minimal.

Santé, dégâts, récompenses et état de combat restent corrects même si l'animation est réduite.

**Fréquence visuelle de départ :** LOD 0 à la fréquence normale ; LOD 1 environ une évaluation sur deux, avec interpolation ou maintien de pose ; LOD 2 environ 5 à 10 fois par seconde ; LOD 3 à faible fréquence, à l'entrée dans une zone pertinente ou lors d'un changement d'état important. Répartir les mises à jour entre les ennemis pour qu'ils ne changent pas tous de pose sur la même image.

**Choix de technologie :** commencer simplement.

- `Animator` classique pour le joueur, le boss et les ennemis proches.
- Réduction de fréquence ou simplification pour les ennemis ordinaires à distance.
- Tester des poses pré-calculées en meshes pour les types d'ennemis nombreux et répétitifs.
- GPU instancing ou rendu groupé seulement après vérification de la compatibilité des meshes, matériaux et shaders URP.

Les meshes pré-calculés sont une optimisation, pas une obligation ; leur coût mémoire, le nombre de poses, la qualité des transitions et le gain réel sont mesurés sur un ennemi test avant de construire une bibliothèque. Le choix final entre animation classique, poses pré-calculées et rendu instancié est pris après comparaison dans la test map (§12.7).

**Calcul du LOD :**

- éviter les calculs de distance coûteux et les recherches de composants répétées à chaque image ;
- comparer des distances au carré face aux seuils ;
- recalculer les niveaux à fréquence contrôlée, par groupes décalés dans le temps ;
- ajouter une marge de transition (hystérésis) pour éviter les allers-retours près d'un seuil ;
- envisager la distance à la caméra et la taille à l'écran en complément ;
- garder les seuils configurables par type d'ennemi, pas dispersés dans les scripts.

### 12.6 Rendu, pooling et effets

- **Pooling** des ennemis ordinaires, projectiles et effets fréquents. À la mort d'un ennemi, remettre à zéro états, événements, effets, santé, cibles et données de simulation avant réutilisation ; éviter les abonnements d'événements en double.
- Réutiliser matériaux et meshes ; utiliser LOD et GPU instancing quand ils sont compatibles.
- Limiter matériaux uniques, transparences coûteuses, ombres inutiles et effets superposés.
- Un ennemi éloigné n'utilise pas autant de particules et de détails qu'un ennemi au premier plan.
- Conserver une silhouette claire et des couleurs de danger lisibles (§1.3).
- Réduire ou supprimer les effets cosmétiques à distance sans supprimer les signaux essentiels d'une attaque dangereuse. Limiter les sons simultanés répétitifs de la foule.

### 12.7 Test map de performance

Une scène séparée, `TestMap_EnemySandbox`, contient :

- un sol plat et des limites solides ;
- des piliers, murs, angles et couloirs ;
- quelques rampes pour tester le joueur ;
- des points d'apparition configurables ;
- un affichage de diagnostic : ennemis actifs, niveau d'animation, temps de simulation ;
- un moyen de sélectionner rapidement une charge de test : 10, 50, 100, 250, 500 et 1 000 ennemis.

Les tests sont reproductibles : même scène, même configuration graphique, mêmes comportements, même matériel.

### 12.8 Méthode de profilage

Mesurer séparément :

- le temps CPU de l'IA et du contrôleur central ;
- le temps CPU de la physique ;
- le temps CPU des animations ;
- le temps GPU et les draw calls ;
- les allocations mémoire et les pics de garbage collection ;
- le nombre d'objets actifs et de collisions ;
- la fluidité du joueur lorsque la foule est proche.

Tester dans l'éditeur pour itérer, puis confirmer dans une build Windows de développement. L'objectif prioritaire est une expérience fluide avec le nombre d'ennemis réellement visé, pas un record de foule.

---

## 13. Cartes et environnement

### 13.1 Principes de conception

Chaque carte contient :

- une aire de déplacement assez large pour esquiver les hordes ;
- des obstacles qui créent des décisions sans bloquer le déplacement en permanence ;
- des couloirs assez larges pour que les contrôles restent fiables ;
- des zones de respiration et des zones plus dangereuses ;
- des repères visuels permettant de se réorienter ;
- une zone de combat de boss adaptée aux attaques prévues.

Les sauts ont une utilité concrète, mais aucun saut de précision n'est requis pour terminer une partie standard. Les murs et piliers ont des colliders clairs ; les objets purement visuels ne créent pas de collisions invisibles.

### 13.2 V1 — Musée des Ombres

Grande galerie avec tableaux déchirés, statues, colonnes, cadres corrompus, sol de pierre et flaques d'encre.

- Établir une boucle principale facile à comprendre.
- Utiliser salles, cadres, vitrines et piliers comme éléments de composition ; les obstacles varient les déplacements sans piéger le joueur ni bloquer constamment la caméra.
- Éviter les petits accessoires qui piègent le `CharacterController`.
- Conserver des chemins de contournement.
- Prévoir une arène suffisamment ouverte pour esquiver et lire les attaques, avec un espace de boss assez dégagé.
- Introduire progressivement les deux pigments du MVP : noir d'encre et vermillon.

Dimensions, limites, obstacles et position du boss sont à définir (§23.2).

### 13.3 Événements de carte

Un événement explique clairement : son point ou sa zone d'activation, sa condition de déclenchement, le risque encouru, la récompense, et s'il est facultatif ou requis.

Pas de téléporteurs ni de portails. Les transitions entre cartes passent par le menu ou par un mécanisme de progression explicite, sans portail en jeu.

### 13.4 Cartes futures

1. **Atelier des Damnés :** chevalets, pots de peinture, toiles inachevées et zones d'encre.
2. **Corridor Impossible :** architecture répétitive et cadres flottants. *(La version d'origine mentionnait des portails : à retirer, §23.2.)*
3. **Galerie des Portraits :** miroirs, statues sans visage et apparitions surprises.
4. **Bassin des Pigments :** réserve souterraine, rivières de peinture et zones instables.
5. **Toile Originelle :** monde abstrait peint, changements visuels pendant le boss final.

Pistes d'arènes pour la progression future :

- **Atelier abandonné :** arène relativement ouverte pour apprendre les systèmes.
- **Galerie brisée :** piliers et cadres qui influencent les trajectoires.
- **Musée englouti :** espace plus contraint avec zones de peinture dangereuses.

Ces pistes recoupent en partie les cartes ci-dessus ; la liste définitive est à arbitrer. Des événements temporaires pourront s'y ajouter : chute de cadres, débordement de pigment, apparition d'une surface rare, vague d'élite.

Varier d'abord la disposition et la lumière plutôt que de produire des modèles uniques pour chaque carte. Ces contenus ne modifient pas le périmètre du MVP.

---

## 14. Interface et expérience du joueur

### 14.1 Intention visuelle et principes

L'interface évoque un **atelier de restauration interdit** : une toile encore humide, des pigments qui réagissent à la présence du joueur, des cadres fendus, du papier ancien et de l'encre vivante. Elle est organique, élégante et inquiétante, mais les informations utiles priment sur la décoration.

Principes directeurs :

1. **Lisibilité avant ornement.** Textures et animations ne masquent jamais une valeur, une icône, un télégraphe ennemi ou le personnage.
2. **Une cause visuelle identifiable.** Toute éclaboussure ou déformation importante est liée à un événement identifiable : dégâts reçus, montée de niveau, choix confirmé, arrivée d'un boss, déblocage.
3. **Un langage cohérent.** Les mêmes formes, couleurs et animations gardent le même sens dans tous les menus.
4. **Une intensité contextuelle.** L'interface peut s'agiter quand la menace augmente, puis se calmer dès que la pression retombe.
5. **Séparation entre décor et gameplay.** Les effets décoratifs de l'interface ne se confondent pas avec les attaques, zones dangereuses ou projectiles du jeu.

Priorité de production : **fonctionnalité et lisibilité d'abord**, puis identité « atelier de restauration interdit » par couches. Les éléments décoratifs ne retardent pas la validation du déplacement, du saut, de l'esquive, du combat, de la progression, de la pause et des écrans de fin (ordre de production au §21.3).

### 14.2 Palette et matériaux

**Palette fonctionnelle de l'interface :**

| Couleur | Code | Usage principal |
|---|---|---|
| Noir d'encre | `#171615` | Fond, panneaux sombres, silhouettes d'interface |
| Ivoire papier | `#F0E6D2` | Texte principal, papier, cartes claires |
| Vermillon | `#A9272D` | Danger, dégâts, alerte, sélection urgente |
| Vert pigment | `#66745D` | Confirmation positive, soin, amélioration bénéfique |

Le vermillon n'est pas une couleur décorative omniprésente : il signale le danger, l'alerte ou une action importante. Le vert pigment signale un retour positif sans être le seul moyen de comprendre l'information.

Les couleurs de rareté (vert, bleu, violet, jaune) restent distinctes de cette palette fonctionnelle et sont toujours accompagnées d'un libellé ou d'un symbole.

**Matériaux visuels :**

- **Papier fibreux :** cartes de choix, archives, fiches de personnages, descriptions.
- **Encre liquide :** transitions, jauges animées, confirmations.
- **Pigment sec :** icônes, marques, éléments de collection.
- **Cadre brisé :** élément sélectionné, contenu rare ou événement important.
- **Trait de pinceau :** séparateurs, surlignages, indicateurs de progression.

Les panneaux porteurs d'une information essentielle gardent un fond suffisamment opaque et un contraste élevé : les textures sont des finitions, jamais un obstacle à la lecture.

### 14.3 Menu principal et navigation

Le menu principal ressemble à une toile encore vierge dans un atelier inquiétant. Le personnage peut y apparaître, avec un lent écoulement d'encre. L'ambiance est contemplative et discrète ; les animations ne retardent pas l'accès au jeu.

Entrées :

- **Nouvelle toile :** lancer une partie.
- **Les artistes :** sélectionner un personnage et consulter son passif ; les personnages verrouillés indiquent leur condition de déblocage.
- **Le catalogue :** consulter les Archives (Instruments, évolutions, Reliques et ennemis découverts).
- **Options :** commandes, audio, affichage et accessibilité.
- **Quitter :** fermer le jeu.

Toute option non implémentée est masquée ou clairement indiquée comme indisponible. Au clic ou au survol, une trace de pinceau temporaire peut souligner le cadre ; la sélection reste clairement visible à la souris, au clavier et à la manette. Les transitions sont courtes et interrompables : le joueur n'attend jamais une longue animation pour confirmer ou annuler.

### 14.4 HUD en jeu

Le HUD reste lisible pendant les combats denses sans prendre inutilement de place.

**À afficher en permanence :**

- santé actuelle et maximale, bouclier éventuel, effets négatifs actifs ;
- niveau et jauge de Pigment ;
- chronomètre de run ;
- icônes d'Instruments (avec rangs et état d'évolution) et de Reliques équipés ;
- état de recharge de l'esquive, immédiatement compréhensible ;
- santé du boss pendant son combat, avec nom et indication de phase si cela améliore la lecture ;
- avertissements de danger, près du personnage ou dans le monde.

Les temps de recharge et jauges d'Instrument ne sont affichés que s'ils servent à une décision. Éviter les chiffres permanents pour chaque dégât s'ils masquent le combat.

**Organisation recommandée (à tester en résolution cible) :**

- **Bas de l'écran :** grande jauge de Pigment dessinée comme un trait d'encre liquide ; niveau du joueur à proximité.
- **Haut gauche :** santé, bouclier éventuel, effets négatifs ; Instruments équipés, rangs et état d'évolution dans la zone adjacente.
- **Haut droit :** Reliques et passifs actifs, en icônes compactes.
- **Haut centre :** chronomètre, phase ou vague, annonces de boss.
- **Sous le chronomètre ou en haut à droite :** niveau de menace et objectif actif, si la phase en cours en a besoin.
- **Boss :** barre de santé dédiée.

**Feedback de danger.** Quand le joueur subit des dégâts importants, un débordement vermillon bref peut apparaître sur les bords de l'écran ; il peut devenir plus marqué à faible santé mais ne masque jamais le centre de l'action. Ordre de priorité des alertes :

1. attaque dangereuse imminente ;
2. dégâts reçus et état critique du joueur ;
3. arrivée ou changement de phase d'un boss ;
4. gain de niveau et choix disponible ;
5. récompenses et informations secondaires.

Les animations décoratives cèdent la place aux alertes de gameplay.

Lors du premier lancement, ou dans un écran d'aide, afficher les commandes de saut et d'esquive. Distinguer par des télégraphes cohérents une attaque évitable par le saut d'une attaque évitable par l'esquive.

### 14.5 Choix d'amélioration

Le jeu est en **pause réelle** et présente trois cartes (ou le nombre d'options valides disponibles, §7.2). Chaque carte affiche :

- nom et icône ;
- catégorie (Instrument, Relique, bonus ou évolution) et élément concerné ;
- rang actuel et nouveau rang, si applicable ;
- rareté (couleur, nom et icône) pour les améliorations statistiques d'Instrument ;
- description courte et concrète ;
- bonus chiffré et, si possible, valeurs avant/après (qui correspondent au résultat réel) ;
- conditions manquantes si une évolution est verrouillée.

La rareté n'annonce jamais une nouvelle mécanique : une carte légendaire qui ne fait qu'augmenter les dégâts ne suggère ni nouvelle attaque ni transformation.

Les cartes apparaissent par traits de pinceau ; le survol peut révéler un léger relief. Après confirmation, la carte est absorbée par l'encre, l'effet est appliqué **une seule fois**, puis le jeu reprend avec un feedback bref. Le curseur est visible et déverrouillé. Jamais d'amélioration invalide à l'écran.

### 14.6 Écran d'évolution

La rareté décrit la valeur d'un bonus statistique ; **une évolution est une transformation mécanique dédiée**. Elle possède un nom distinct, une illustration ou un aperçu distinct, une description de la nouvelle mécanique, des prérequis affichés et ses synergies principales lorsqu'elles sont connues. L'interface ne fait jamais passer une amélioration statistique rare pour une évolution.

Parcours (conditions au §5.6) :

1. annoncer qu'une possibilité d'évolution est disponible ;
2. montrer les Instruments éligibles et leurs prérequis, et expliquer pourquoi un Instrument ne l'est pas ;
3. permettre de choisir un Instrument ;
4. présenter au moins deux branches distinctes pour cet Instrument ;
5. afficher clairement les effets et synergies de chaque branche ;
6. demander une confirmation unique avant d'appliquer la transformation.

Si aucun Instrument n'est éligible au palier atteint, le joueur reçoit une récompense alternative ; le Sceau n'est pas consommé sans transformation obtenue.

### 14.7 Pause, victoire, défaite et bilan

**Pause.** L'écran apparaît comme une feuille sombre posée sur la toile ; **Reprendre** est l'action la plus visible. Échap ouvre et ferme le menu. Le jeu n'inflige aucun dégât et ne fait pas progresser le combat pendant la pause. Le curseur est visible et déverrouillé, et la caméra ne le recapture pas tant que le menu est ouvert. Toute sortie de run qui entraîne la perte d'une progression temporaire demande une confirmation ; les boutons désactivés ont une explication.

**Victoire.** La composition visuelle se stabilise, les pigments cessent de se disperser et le cadre de l'œuvre se reconstitue progressivement, assez brièvement pour que le bilan soit vite accessible.

**Défaite.** L'encre envahit progressivement la toile, puis laisse apparaître le bilan et les options. L'effet peut être réduit ou désactivé via les options d'accessibilité.

**Bilan de run (minimum) :**

- durée de survie ;
- niveau atteint ;
- nombre d'ennemis éliminés ;
- Instruments utilisés et évolutions obtenues ;
- Reliques collectées ;
- découvertes ou déblocages obtenus ;
- récompenses permanentes, si applicables.

Les commandes **Recommencer** et **Retour au menu** fonctionnent et réinitialisent l'état temporaire de la run (§2.4).

### 14.8 Jauges et feedbacks visuels

- **Santé :** trait d'encre dans une forme lisible ; les variations restent faciles à estimer.
- **Expérience :** le Pigment se répand dans la jauge jusqu'au prochain niveau.
- **Menace :** un cadre peut se fissurer ou se tendre à mesure que la pression monte, sans devenir difficile à lire.
- **Récompense importante :** accent de pigment doré, réservé aux événements significatifs.
- **Acquisition de Relique :** tampon visuel bref, avec le nom et l'effet.
- **Dégâts reçus :** flash HUD bref, sans flash blanc plein écran obligatoire.

Chaque animation communique une information identifiable et reste secondaire par rapport aux télégraphes ennemis.

### 14.9 Micro-interactions

Durées de départ, à tester :

| Interaction | Durée cible initiale |
|---|---:|
| Survol d'un bouton : trace de pinceau | 100–160 ms |
| Confirmation d'un choix : absorption par l'encre | 180–280 ms |
| Montée de niveau : éclaboussure de pigment | 250–400 ms |
| Acquisition de Relique : tampon | 300–450 ms |
| Feedback HUD de dégâts | 100–180 ms |
| Arrivée d'un boss : bords d'encre et titre | 600–900 ms |
| Déblocage permanent : trait doré | 350–600 ms |

Les durées n'empêchent jamais une action immédiate ; les animations sont interrompables ou abrégées quand une alerte plus importante survient.

Règles d'animation :

- respecter une impression de viscosité, de direction et de masse de l'encre ;
- relier les effets majeurs à leur origine visuelle ;
- arrêter ou réduire les animations décoratives quand l'écran est inactif ;
- éviter que les effets d'arrière-plan rivalisent avec les attaques ;
- limiter le nombre de particules et d'effets simultanés ;
- ne jamais transmettre une information essentielle par une animation seule.

### 14.10 Accessibilité et ergonomie

À prévoir dès le prototype :

- taille d'interface réglable ; texte de taille suffisante, contraste contrôlé, lisible à la résolution cible ;
- icônes accompagnées de libellés ou d'infobulles utiles ;
- **information jamais transmise par la couleur seule** (rareté, statut, danger) ; les télégraphes ennemis ont des formes et animations distinctes ;
- réglages de sensibilité caméra, volume musique et effets sonores, tremblement de caméra, flashs, éclaboussures, particules, nombres flottants et intensité des effets plein écran ;
- navigation complète au clavier et à la manette ;
- pause réelle pendant les choix d'amélioration ;
- comparaison explicite des statistiques avant/après ;
- commandes affichées selon le périphérique utilisé, si possible ;
- joueur toujours visible dans les effets de foule.

Les options d'accessibilité modifient réellement le comportement visuel concerné : une option affichée mais non fonctionnelle n'est pas présentée comme disponible. Les effets de peinture sont testés sur un écran chargé pour vérifier que le personnage et les attaques ennemies restent visibles.

### 14.11 Mise en œuvre technique de l'interface (Unity 6 URP)

**Couches séparées :**

1. **HUD fonctionnel :** chiffres, barres, icônes, alertes prioritaires.
2. **Décoration papier :** cadres, fibres, bordures, fonds.
3. **Effets d'encre :** masques, éclaboussures, transitions.
4. **Texte :** hiérarchie, contraste, tailles cohérentes.

La décoration et les effets ne contrôlent jamais la logique du jeu : les systèmes restent fonctionnels si les effets visuels sont désactivés.

**Shaders et effets.** Construire les effets d'encre progressivement (masques, gradients, bruit, seuils), en commençant par des solutions simples et mesurables. Priorités : transitions de panneaux, remplissage de jauges, apparition des cartes, feedback de dégâts, annonce d'arrivée de boss.

**Composants réutilisables :** cartes de choix, jauges, icônes d'Instrument et de Relique, alertes, infobulles, notifications de découverte, annonces de boss. Les données affichées viennent des systèmes de jeu et des assets de données, sans duplication manuelle entre écrans.

**Performance :**

- éviter les mises à jour coûteuses à chaque image quand elles ne sont pas nécessaires ;
- limiter le surdessin, les transparences superposées et les effets plein écran ;
- pooler les effets répétés si les tests le justifient ;
- tester l'interface pendant une vague dense, pas seulement dans le menu ;
- garder le HUD lisible aux résolutions cibles et avec les options d'accessibilité activées.

---

## 15. Effets et audio

**Effets prioritaires de la V1 :**

- impact d'arme ;
- projectile ;
- zone dangereuse ;
- collecte de Pigment ;
- montée de niveau ;
- télégraphe de boss ;
- dégâts reçus et mort.

**Effets futurs :** évolutions d'Instruments, Fractures, réactions complexes entre pigments, effets propres aux personnages, transformations visuelles de carte.

**Audio de la V1 :** sons de coups, projectiles, dégâts reçus, collecte, montée de niveau, signal d'attaque du boss, musique d'ambiance et de boss, victoire et défaite. Les sons restent lisibles sans saturation pendant les hordes.

Règle de priorité : les signaux essentiels d'une attaque dangereuse passent avant tout effet cosmétique. Les contraintes de rendu et de performance sont aux §9.6 et §12.6.

---

## 16. Progression permanente et sauvegarde

### 16.1 Séparer run et méta-progression

**État temporaire de run** (réinitialisé à chaque nouvelle run, sauf fonction de reprise ajoutée explicitement plus tard) :

- santé actuelle, position et équipement temporaire ;
- niveau et Pigment ;
- Instruments, rangs, évolutions, Sceaux ;
- Reliques équipées ;
- minuteur, ennemis, boss et événements ;
- surfaces et statuts actifs.

**État permanent :**

- personnages débloqués ;
- Instruments, Reliques, variantes et cartes débloqués ;
- contenu découvert dans les Archives ;
- défis accomplis et succès, si retenus ;
- paramètres ;
- statistiques globales ;
- Fragments de Toile ;
- version de schéma de sauvegarde.

La sauvegarde ne restaure jamais par erreur la position, la santé ou l'équipement temporaire d'une ancienne run, et les données temporaires ne sont jamais sauvegardées comme progression permanente.

### 16.2 Récompenses et déblocages permanents

- Victoires et défaites peuvent donner une ressource permanente selon la durée ou les objectifs atteints. Nom provisoire : **Fragments de Toile**.
- Usages possibles : débloquer des personnages, ajouter des objets aux pools de récompenses, acheter de petits bonus, compléter les Archives.
- Le nom, les gains, les coûts et les bonus restent à définir (§23.2).
- Une même récompense permanente n'est jamais attribuée deux fois, y compris après une défaite.
- Les conditions de déblocage sont visibles dans les Archives.

Chaque déblocage a : un identifiant stable, une condition, une récompense, une progression visible, une notification et une règle de sauvegarde.

**Équilibrage.** Les bonus permanents de puissance restent **modestes**, pour préserver l'importance des décisions de la run et ne pas rendre les premières difficultés triviales. Prioriser l'ouverture de nouvelles options de construction (personnages, Instruments, Reliques, évolutions, défis, éléments de lore) plutôt que des augmentations de dégâts permanentes.

### 16.3 Règles de sauvegarde

- Format **versionné**, avec un numéro de version de schéma et une migration simple dès le début.
- Écrire la progression permanente après une fin de run et après chaque déblocage important.
- Vérifier les données lues avant de les appliquer ; prévoir des valeurs par défaut si la sauvegarde manque ou si un champ est absent ou inconnu.
- Éviter d'écraser une sauvegarde valide par un fichier partiellement écrit.
- En cas de fichier corrompu : conserver une copie de secours si possible et afficher un message compréhensible.
- Ne jamais effacer silencieusement une sauvegarde sans action explicite du joueur.
- Séparer les données de progression des paramètres audio/vidéo lorsque cela simplifie la maintenance.
- Ne pas enregistrer de références Unity brutes comme identifiants persistants.
- Prévoir une sauvegarde de développement réinitialisable pour tester les déblocages.
- La sauvegarde complète est requise avant la première version publique ; elle peut être différée pendant le prototype.

---

## 17. Architecture technique Unity 6 URP

### 17.1 Principes

- Séparer **données, logique de gameplay, présentation et sauvegarde**.
- Les fiches de contenu sont configurables sans modifier la logique centrale.
- Les systèmes centraux ne référencent pas directement des objets de scène temporaires après la fin de leur cycle de vie.
- Les règles de dégâts, de rareté et de progression sont testables de manière isolée.
- Les événements sont désabonnés correctement à la destruction ou à la réutilisation des objets.
- Ne pas créer un système générique complexe tant que le prototype n'en a pas besoin.
- Garder les comportements dans des composants C# faciles à comprendre. Les responsabilités du §17.3 ne sont pas une obligation de créer immédiatement une classe par ligne.

À éviter : un script géant, les recherches d'objets répétées à chaque image sans nécessité, les valeurs d'équilibrage dispersées, les références Inspector manquantes, les `GameManager` en double, et les changements de noms de champs sérialisés qui cassent les références.

### 17.2 Organisation des dossiers

```text
Assets/
  _Project/
    Art/
      Characters/
      Enemies/
      Environments/
      Materials/
      VFX/
    Audio/
      Music/
      SFX/
    Prefabs/
      Characters/
      Enemies/
      Instruments/
      Relics/
      Pickups/
      UI/
    Scenes/
      Boot/
      MainMenu/
      MuseumOfShadows/
      Sandbox/            (TestMap_EnemySandbox, voir §12.7)
    Scripts/
      Core/
      Player/
      Camera/
      Combat/
      Instruments/
      Relics/
      Enemies/
      Bosses/
      Waves/
      Progression/
      Paint/
      UI/
      Save/
    Data/
      Characters/
      Instruments/
      Evolutions/
      Relics/
      Enemies/
      Bosses/
      Waves/
      Balance/
    Settings/
    Tests/
      EditMode/
      PlayMode/
```

### 17.3 Responsabilités des systèmes

| Système | Responsabilité |
|---|---|
| GameManager / RunController (RunState) | État général de la run, pause, transitions, victoire, défaite, redémarrage |
| RunManager | Durée, phases et progression temporelle |
| PlayerController | Entrées, déplacement, orientation, saut, esquive |
| CameraController | Rotation, suivi, collision caméra |
| Health / DamageReceiver | Santé, réception des dégâts, invulnérabilité, mort |
| DamageResolver | Résolution des dégâts, critiques, résistances et sources (§8.2) |
| CharacterData | Données du personnage et passif |
| InstrumentDefinition (WeaponData) | Données statiques d'un Instrument : statistiques, rangs, statistiques éligibles |
| InstrumentController (WeaponController) | Instruments équipés, rangs, déclenchement des attaques automatiques |
| RelicData / RelicController | Données et effets des Reliques équipées |
| ExperienceSystem | Pigment, seuils et niveaux |
| UpgradeGenerator (UpgradeManager) | Éligibilité, catégories de récompense, tirages, application des choix |
| EvolutionSystem | Préconditions et choix d'évolution |
| StatusEffectController | Application, durée, cumul et expiration des statuts |
| PaintSurfaceSystem | Création, durée, chevauchement et limite des surfaces |
| WaveDirector (EnemySpawner) | Budget de menace, sélection des apparitions, arrêt des apparitions |
| EnemySimulationManager | Mise à jour centralisée des ennemis actifs (§12.2) |
| EnemyBrain (EnemyAI) | Comportement individuel d'un ennemi |
| BossController | Phases, télégraphes, attaques, transitions et état du boss |
| SaveService (SaveSystem) | Lecture et écriture de la progression permanente |
| UI Presenter (UIManager) | HUD, pause, menus et écrans de fin ; affiche les données sans posséder les règles de gameplay |

Les noms sont indicatifs. Avant d'implémenter, inspecter les scripts réels et préserver les noms et références Inspector existants lorsque c'est nécessaire.

### 17.4 Données configurables

Utiliser des **ScriptableObjects** pour les définitions statiques lorsque c'est utile : statistiques de base, icône, prefab, statistiques éligibles, poids de récompense, prérequis. Les valeurs qui changent pendant la run restent dans des états d'exécution séparés : ne jamais modifier l'asset de définition pour stocker l'état d'une run. Pour les personnages, voir aussi le §4.3.

### 17.5 Pooling et performances

Le pooling peut être utilisé pour projectiles, impacts, ennemis communs, objets de Pigment et effets de surface si les tests montrent son utilité. Tout objet réutilisé réinitialise santé, cible, minuteries, propriétaire, statuts, vitesse et abonnements aux événements.

Ne pas fixer d'objectif de FPS irréaliste sans connaître le matériel cible. Baseline à tester : **60 FPS** sur le PC de référence choisi, dans une scène avec le nombre cible d'ennemis, de projectiles et de surfaces. Le matériel minimum est défini avant la sortie publique (§23.2).

---

## 18. Fiches de contenu normalisées

Chaque contenu a une fiche stable. Les identifiants internes restent en anglais ou en PascalCase, stables entre versions ; les noms affichés peuvent rester en français.

### 18.1 Fiche Personnage

- **ID :**
- **Nom affiché :**
- **Rôle :**
- **Difficulté :**
- **Instrument de départ :**
- **Statistiques de base :** santé, vitesse, puissance, récupération, autres valeurs pertinentes.
- **Passif :** déclencheur, effet, durée, limite et contrepartie.
- **Compétence / signature :** condition d'activation, effet, durée, recharge ou coût.
- **Interaction avec le terrain :**
- **Forces :**
- **Faiblesses :**
- **Synergies recommandées :**
- **Feedback visuel / audio :**
- **Conditions de déblocage :**
- **Critères de test :**

### 18.2 Fiche Instrument

- **ID :**
- **Nom affiché :**
- **Type d'attaque :**
- **Cible :** plus proche, plus dangereuse, directionnelle ou règle spécifique.
- **Statistiques de base :** dégâts, cadence ou recharge, portée, taille, vitesse de projectile, durée, critique.
- **Rang maximum :** 5 par défaut.
- **Statistiques améliorables :**
- **Comportement à chaque rang :** valeurs ou formule.
- **Raretés compatibles :** bonus statistiques possibles, jamais d'effet mécanique unique.
- **Évolution A :** nom, prérequis, comportement, limites, synergies.
- **Évolution B :** nom, prérequis, comportement, limites, synergies.
- **Interaction de terrain :**
- **Effets visuels / audio :**
- **Cas limites :**
- **Critères de test :**

### 18.3 Fiche Évolution

- **ID :**
- **Instrument requis :**
- **Nom affiché :**
- **Palier de joueur :** 10, 15 ou 20, selon la branche.
- **Rang minimum de l'Instrument :**
- **Sceau ou événement requis :**
- **Conditions de synergie :**
- **Comportement avant / après :**
- **Statistiques modifiées :**
- **Nouvelles règles de déclenchement :**
- **Limites anti-boucle :**
- **Feedback visuel / audio :**
- **Méthode de choix :**
- **Critères de test :**

### 18.4 Fiche Relique

- **ID :**
- **Nom affiché :**
- **Catégorie :** offensive, défensive ou technique.
- **Effet :**
- **Condition de déclenchement :**
- **Valeur et durée :**
- **Règle de cumul :**
- **Limite :**
- **Interactions :**
- **Contrepartie éventuelle :**
- **Conditions de déblocage :**
- **Feedback :**
- **Critères de test :**

### 18.5 Fiche Ennemi

- **ID :**
- **Nom affiché :**
- **Rôle tactique :** poursuivant, bloqueur, tireur, soutien, contrôleur.
- **Santé :**
- **Vitesse :**
- **Dégâts :**
- **Portée et cadence d'attaque :**
- **Télégraphe :** durée, forme, couleur et son.
- **Comportement IA :** états, distances et priorités.
- **Résistances / immunités :**
- **Pigment donné :**
- **Récompense éventuelle :**
- **Coût de menace :**
- **Règles d'apparition :**
- **Niveau d'animation par distance :** seuils spécifiques éventuels (§12.5).
- **Feedback visuel / audio :**
- **Critères de test :**

### 18.6 Fiche Boss

- **ID :**
- **Nom affiché :**
- **Arène :**
- **Santé et résistances :**
- **Phase d'introduction :**
- **Phases de combat :**
- **Attaques :** télégraphe, délai avant impact, dégâts, zone, durée, récupération, évitable par saut ou par esquive, contre-jeu.
- **Transitions de phase :**
- **Règles d'invulnérabilité :** à éviter ou à limiter à des transitions courtes.
- **Gestion des ennemis présents :**
- **Conditions de victoire :**
- **Récompenses :**
- **Effets visuels / audio :**
- **Critères de test :**

### 18.7 Exemple de fiche — Exilé

- **ID :** `Exile`
- **Nom affiché :** L'Exilé
- **Rôle :** polyvalent, personnage de référence pour l'équilibrage.
- **Instrument de départ :** Pinceau du Dernier Trait.
- **Passif :** les éliminations augmentent temporairement les dégâts de l'Instrument ; la durée, le bonus maximal et la règle de rafraîchissement sont des paramètres configurables.
- **Signature proposée :** une esquive parfaitement synchronisée déclenche une vague d'encre ou renforce le prochain trait ; à activer seulement si cette signature est retenue pour le kit final.
- **Terrain :** aucune interaction spéciale obligatoire dans le MVP.
- **Forces :** prise en main, polyvalence, référence d'équilibrage.
- **Faiblesses :** moins spécialisé que les personnages futurs.
- **Tests :** le bonus ne s'empile pas sans limite ; le joueur comprend visuellement quand le passif est actif.

### 18.8 Exemple de fiche — Patineur (futur)

- **ID :** `Skater`
- **Nom affiché :** Le Patineur
- **Rôle :** mobilité, trajectoire et inertie.
- **Instrument de départ proposé :** Rouleau de l'Atelier.
- **Passif :** glisse sur des surfaces noires compatibles ; la vitesse augmente jusqu'à un plafond.
- **Signature :** dérapage directionnel qui trace une ligne de peinture et peut toucher les ennemis traversés.
- **Coûts et limites :** les virages serrés réduisent l'inertie ; s'arrêter fait perdre le bonus.
- **Téléportation :** aucune.
- **Tests :** vitesse maximale, collision contre les murs, virage, arrêt, contrôle au clavier, lisibilité de la trace.

---

## 19. Tests et critères d'acceptation

### 19.1 Niveaux de test

- **EditMode :** formules de dégâts, probabilités, seuils d'expérience, règles de cumul, éligibilité.
- **PlayMode :** déplacement, esquive, combat, apparition des vagues, interfaces, transitions d'état.
- **Tests de scène :** une run complète avec un contenu représentatif.
- **Tests de régression :** rejouer les scénarios après chaque modification d'un système central.
- **Tests de performance :** densité maximale de combat, projectiles, surfaces et effets (§19.10).
- **Tests utilisateurs :** lisibilité, compréhension des cartes, rythme et difficulté perçue.

Les contrôles sont d'abord testés dans une scène dédiée, puis dans la carte réelle.

### 19.2 Critères d'acceptation et de sortie du MVP

La première version jouable est acceptée lorsque :

- [ ] Le joueur se déplace à la troisième personne, à une seule vitesse.
- [ ] Le saut fonctionne au sol, avec gravité correcte et sans double saut accidentel.
- [ ] L'esquive manuelle possède une courte fenêtre d'invulnérabilité.
- [ ] La caméra suit le joueur et libère le curseur dans les menus, la pause et les écrans de fin.
- [ ] Une attaque automatique fonctionne sans commande d'attaque.
- [ ] Les ennemis poursuivent et attaquent le joueur.
- [ ] Le joueur reçoit des dégâts, peut mourir et voit l'écran de défaite.
- [ ] Le redémarrage restaure un état de run propre.
- [ ] Les ennemis laissent du Pigment et le joueur peut le collecter.
- [ ] La montée de niveau propose trois choix valides.
- [ ] Le jeu est en pause pendant les choix et reprend après sélection.
- [ ] Les limites de six Instruments et de six Reliques sont respectées lorsque le système est complet.
- [ ] Les Instruments ne dépassent pas cinq rangs de base, et les rangs modifient réellement les paramètres ou les effets.
- [ ] Vaincre un mini-boss attribue sa récompense une seule fois et donne un Sceau d'Évolution.
- [ ] Un Instrument n'évolue que si les conditions de rang, de palier et de Sceau sont satisfaites.
- [ ] Si aucune arme n'est éligible, le joueur conserve le Sceau et reçoit quand même la récompense secondaire.
- [ ] Les apparitions ordinaires cessent au début du combat de boss.
- [ ] Le boss a des attaques lisibles et peut être vaincu.
- [ ] La victoire déclenche un écran distinct de la défaite.
- [ ] Une run peut être terminée de bout en bout sans erreur bloquante.
- [ ] Les déblocages permanents sont sauvegardés avant la sortie publique complète.
- [ ] Les performances restent acceptables avec un nombre représentatif d'ennemis et d'effets.

Le MVP est **prêt pour une première session de test externe** lorsque, de plus :

- les améliorations sont appliquées une seule fois et affichent les bons chiffres ;
- au moins trois testeurs peuvent expliquer la boucle principale sans aide après une partie ;
- les bugs bloquants et critiques connus sont corrigés ou documentés.

### 19.3 Déplacement, saut, esquive et caméra

**Déplacement**

- Tester les quatre directions et la diagonale ; les diagonales ne sont pas plus rapides.
- Le personnage n'a qu'une vitesse sélectionnable, cohérente avec ses données : aucune course ni marche alternative.
- Relâcher la direction arrête le personnage sans glisse excessive.
- Pas de traversée des murs ; pas de blocage sur de petites irrégularités.
- Clavier et manette fonctionnent.

**Saut**

- Test sur sol plat et près des obstacles.
- Un saut au sol démarre de façon fiable ; maintenir le bouton ne provoque pas de sauts répétés.
- L'atterrissage est reconnu de façon stable sur terrain plat et sur pentes autorisées.
- Le contrôle aérien correspond au profil du personnage.
- Le saut ne donne pas d'invulnérabilité non prévue.

**Esquive**

- Test dans plusieurs directions ; la direction choisie est cohérente et reproductible (y compris l'esquive arrière sans direction).
- Durée, distance, invulnérabilité et récupération correspondent aux données.
- Les murs bloquent l'esquive ; les limites de carte ne sont pas franchissables.
- Une attaque ne touche pas le joueur pendant la fenêtre d'invulnérabilité, sauf exception documentée.
- La récupération empêche le spam ; l'esquive en l'air respecte le même délai.
- Les différences entre personnages sont perceptibles mais restent équilibrables.

**Caméra**

- Tests près des murs et dans les coins.
- Le suivi n'a pas de tremblement continu ; les collisions ne cachent pas durablement le personnage.
- Le boss et les attaques importantes restent lisibles.
- Sensibilité et inversion sont appliquées correctement.

### 19.4 Combat et ennemis

- Chaque Instrument acquiert une cible valide ; cadence, portée et ligne de vue sont respectées.
- Dégâts normaux et critiques corrects ; un impact n'inflige les dégâts qu'une fois, même si un projectile touche plusieurs colliders d'un même ennemi.
- Une zone de dégâts applique sa fréquence configurée, pas des dégâts à chaque image.
- Une cible meurt exactement une fois, ne donne ses récompenses qu'une fois, et ne peut plus être ciblée ni recevoir de dégâts une fois morte.
- Attaque interrompue par la mort de sa source ; ennemi mort pendant un effet périodique.
- Statuts qui expirent ou se renouvellent ; dégâts de zone et chevauchement des zones.
- Immunité du boss aux contrôles non autorisés.
- Pas de propagation récursive infinie ; pas de projectile orphelin après redémarrage.
- Les apparitions ordinaires cessent au lancement du boss ; la défaite et la victoire ne se déclenchent qu'une fois ; le redémarrage ne laisse aucun état résiduel.

### 19.5 Progression

- Collecter du Pigment et monter plusieurs niveaux ; l'expérience excédentaire n'est pas perdue.
- Passage de niveau et pause ; trois cartes toutes valides ; aucun bonus appliqué deux fois.
- Emplacements pleins ; Instrument au rang maximum ; aucune option unique disponible.
- Paliers 10, 15 et 20 sans évolution éligible : récompense alternative.
- Rangs, prérequis d'évolution et Sceaux respectés ; Sceau conservé si aucune évolution n'est possible, consommé uniquement lors d'une évolution réussie.
- Une évolution n'est sélectionnée qu'une fois par Instrument.
- État propre après recommencer ; récompenses permanentes jamais attribuées en double.

### 19.6 Probabilités de rareté

Pour une table 60 / 25 / 12 / 3 :

- la somme des probabilités vaut 100 % ;
- chaque résultat possible peut être tiré ;
- une simulation automatisée d'au moins 100 000 tirages compare les fréquences observées aux fréquences attendues, avec une tolérance statistique définie ;
- les modificateurs de Chance respectent leurs plafonds ;
- la protection contre la malchance, si elle est activée, se réinitialise correctement ;
- la rareté ne modifie pas le comportement de l'arme.

Un test statistique vérifie le comportement sur un grand échantillon ; il ne garantit pas que chaque petite série reflète exactement les pourcentages.

### 19.7 Peinture au sol

- Surface créée puis supprimée à l'expiration ; limite du nombre de surfaces ; renouvellement.
- Joueur entrant et sortant rapidement ; ennemi restant dans une surface.
- Plusieurs colliders de la même cible.
- Deux pigments qui se chevauchent.
- Surface supprimée lorsque sa source disparaît.
- Une surface ne masque pas les télégraphes de boss.
- Pas de croissance illimitée des effets actifs.

### 19.8 Interface

**Lisibilité en combat**

- Santé, niveau et progression du Pigment identifiables en quelques secondes.
- Télégraphes du boss visibles malgré les effets du HUD.
- Dégâts reçus perceptibles sans masquer le centre de l'action.
- Instruments et Reliques identifiables sur un écran chargé.

**Choix et progression**

- Le jeu est réellement en pause pendant les choix ; une sélection ne s'applique qu'une fois.
- Les valeurs avant/après correspondent au résultat réel.
- La rareté ne laisse pas croire à une nouvelle mécanique.
- Les évolutions affichent leurs prérequis et leurs différences.
- L'absence d'évolution valide déclenche une alternative sans bloquer la progression.

**Navigation et fin de partie**

- Tous les menus sont utilisables à la souris, au clavier et à la manette ; le curseur est libre dans les menus et la pause.
- Pause et reprise, Reprendre, Recommencer, Retour au menu et Quitter déclenchent les bonnes actions.
- Mort du joueur et victoire du boss, puis redémarrage après les deux résultats ; remise à zéro du temps et des états temporaires.
- Les éléments indisponibles sont clairement identifiés.

**Accessibilité et performance**

- La réduction des animations diminue effectivement les effets concernés.
- L'information reste compréhensible sans distinguer les couleurs.
- L'interface reste lisible pendant une vague dense.
- Les effets décoratifs ne provoquent pas de baisse de performance inacceptable sur la configuration cible.

### 19.9 Sauvegarde

- Première création de sauvegarde ; sauvegarde avec plusieurs déblocages.
- Fermeture et relancement : déblocages et paramètres persistent.
- Run temporaire non restaurée par erreur.
- Sauvegarde absente ; champ manquant ; version de schéma antérieure ou future.
- Données corrompues ; écriture interrompue.
- Bouton de réinitialisation avec confirmation.

### 19.10 Hordes et performances

**Critères de validation du prototype de foule :**

1. Le joueur se déplace, saute et esquive sans être immobilisé par la foule.
2. Les ennemis ne traversent pas les murs et contournent les obstacles de façon acceptable.
3. La navigation ne recalcule pas inutilement un chemin complet par ennemi et par image.
4. Le système central ajoute et retire des ennemis sans références invalides.
5. Le pooling fonctionne après de nombreux cycles apparition / mort / réapparition.
6. Les animations deviennent progressivement moins coûteuses avec la distance.
7. Les attaques importantes des boss et élites restent lisibles.
8. Les ennemis éloignés continuent de participer correctement au gameplay.
9. Les tests de 10 à 1 000 ennemis permettent d'identifier le goulot d'étranglement.
10. Les résultats sont consignés avec la configuration matérielle et graphique utilisée.

**Scénarios de performance minimaux :** scène vide (référence), densité moyenne, densité maximale prévue, maximum de projectiles, maximum de surfaces de peinture, combat de boss avec effets, menu et écran de choix, transitions et nettoyage de fin de run.

Mesurer FPS, temps CPU et GPU, allocations mémoire par image et nombre d'objets actifs. Le seuil final est défini sur le PC de référence ; pour le prototype, viser 60 FPS et éviter les chutes prolongées sous 50 FPS sur la scène cible, puis ajuster selon le matériel choisi.

### 19.11 Gravité des bugs

- **Bloquant :** impossible de démarrer, terminer ou redémarrer une run.
- **Critique :** perte de sauvegarde, progression dupliquée, combat impossible ou crash.
- **Majeur :** mécanique centrale incorrecte, amélioration appliquée deux fois, boss bloqué.
- **Mineur :** problème visuel ou sonore sans impact majeur sur le gameplay.
- **Cosmétique :** alignement, animation ou texte sans effet fonctionnel.

Aucun bug bloquant ou critique connu n'est accepté pour une version publique. Les bugs majeurs sont corrigés ou acceptés explicitement avec un plan de correction.

---

## 20. Équilibrage

### 20.1 Mesures à collecter

- durée moyenne et médiane des runs ;
- taux de victoire et minute de décès ;
- dégâts reçus par minute ;
- dégâts infligés par Instrument ;
- fréquence de choix par rareté et par catégorie ;
- taux d'utilisation de chaque personnage ;
- Reliques et synergies les plus choisies ;
- fréquence d'évolution ;
- durée du combat de boss ;
- dégâts évités par esquive ;
- nombre d'ennemis simultanés et temps d'image.

### 20.2 Procédure d'itération

1. Définir une hypothèse précise (par exemple « les attaques à distance sont trop sûres »).
2. Modifier un petit nombre de paramètres liés à cette hypothèse.
3. Exécuter les mêmes scénarios de test.
4. Comparer les métriques et observer des parties réelles.
5. Conserver, annuler ou ajuster la modification.
6. Inscrire la décision et sa raison dans le journal d'équilibrage (§23.3).

Ne pas changer simultanément dégâts, santé des ennemis, chances de rareté et cadence d'apparition : il deviendrait impossible de savoir quelle modification a produit le résultat. Ajuster une variable à la fois.

### 20.3 Ordre de validation

1. Valider déplacement, saut et esquive sans ennemis.
2. Tester les mêmes actions avec un ennemi poursuivant.
3. Ajouter un ennemi rapide et un ennemi lourd.
4. Tester les attaques automatiques et le ciblage.
5. Ajouter l'élite et ses télégraphes.
6. Tester les attaques du boss contre les extrêmes de statistiques des personnages.
7. Tester une partie complète de 20 à 30 minutes.
8. Comparer les personnages avec un équipement de départ identique lorsque c'est possible.
9. Ajuster une variable à la fois et consigner le résultat.
10. Vérifier ensuite la performance en build Windows.

Ne pas chercher à équilibrer six personnages, dix Instruments, dix Reliques et plusieurs cartes avant d'avoir validé le premier personnage et le premier combat de boss.

### 20.4 Seuils de départ à surveiller

Objectifs de test, pas garanties de conception :

- durée d'une run gagnée : 20 à 30 minutes ;
- le joueur voit au moins une amélioration tôt dans la partie ;
- aucun Instrument n'est clairement dominant sur toutes les cartes et tous les personnages ;
- le boss reste lisible quand le joueur a une construction puissante ;
- les améliorations légendaires sont rares et plaisantes mais pas nécessaires pour gagner ;
- la construction reste viable sans obtenir une rareté spécifique ;
- une défaite s'explique par les décisions ou les menaces, pas par une absence prolongée de choix valides.

---

## 21. Plan de production

### 21.1 Socle minimal (première tâche après création du projet)

1. Créer un projet Unity 6 URP et la structure de dossiers (§17.2).
2. Créer une scène de test simple.
3. Configurer le New Input System.
4. Créer un personnage provisoire capable de se déplacer, sauter et esquiver.
5. Ajouter la caméra à la troisième personne.
6. Ajouter une attaque automatique et un ennemi.
7. Ajouter santé, dégâts, mort, écran de défaite et redémarrage.
8. Tester les contrôles, collisions, caméra et écrans de fin.
9. Valider ce socle avant d'ajouter Pigment, niveaux et équipement.

**Condition de passage :** aucune progression complexe n'est ajoutée avant que déplacement, saut, esquive, caméra, combat élémentaire, mort et redémarrage soient fiables.

### 21.2 Étapes de production

**Étape A — Contrôle jouable**

- actions du New Input System ;
- `CharacterController` ;
- déplacement à vitesse unique ;
- saut et contrôle aérien ;
- esquive et invulnérabilité ;
- caméra et collision ;
- configuration de personnage en ScriptableObject.

**Étape B — Combat de base**

- vie du joueur et des ennemis ;
- réception et émission de dégâts ;
- cible automatique ;
- un Instrument de test ;
- projectiles ou zones avec pooling ;
- retours visuels et sonores.

**Étape C — Boucle de partie**

- expérience et niveaux ;
- interface de choix ;
- trois Instruments et quatre Reliques du MVP ;
- directeur de vague ;
- quatre ennemis communs ;
- élite, mini-boss et Sceau d'Évolution ;
- premier boss et transition de combat.

**Étape D — Carte et sauvegarde**

- boucle de navigation du Musée des Ombres ;
- événements de carte ;
- victoire, défaite et résultats ;
- ressource permanente ;
- déblocages et sauvegarde versionnée.

**Étape E — Validation**

- tests d'acceptation (chapitre 19) ;
- profils de personnages extrêmes ;
- profilage CPU/GPU et allocations ;
- build Windows ;
- correction des problèmes avant extension du contenu.

### 21.3 Piste interface

Développer l'interface par étapes vérifiables, chacune utilisable avant de passer à la suivante :

1. HUD fonctionnel simple, sans effets décoratifs coûteux.
2. Choix d'amélioration en pause réelle.
3. Pause, victoire, défaite et bilan de run.
4. Écran d'évolution dédié.
5. Menu principal et sélection de personnage.
6. Transitions d'encre et matériaux papier.
7. Micro-interactions et finitions visuelles.
8. Options d'accessibilité et optimisation.

Ne pas commencer par les shaders ou les animations complexes au détriment des systèmes fonctionnels.

### 21.4 Piste hordes

1. Construire la test map grise (§12.7).
2. Valider le déplacement du joueur et les collisions de décor.
3. Implémenter un ennemi qui poursuit le joueur sans traverser les murs.
4. Ajouter l'évitement local, puis une navigation globale adaptée aux hordes.
5. Centraliser les mises à jour de simulation et ajouter les diagnostics.
6. Ajouter le pooling et tester les cycles de vie.
7. Ajouter les niveaux d'animation par distance sur un seul type d'ennemi.
8. Mesurer Animator classique, poses pré-calculées et rendu instancié avant de choisir.
9. Augmenter progressivement le nombre d'ennemis et profiler chaque palier.
10. Réutiliser l'architecture validée dans le Musée des Ombres.

---

## 22. Règles de travail pour l'assistant IA

### 22.1 Règles générales

1. Lire ce cahier des charges avant toute tâche importante.
2. Inspecter les scripts et assets réels avant de proposer ou d'effectuer des modifications ; ne jamais inventer le contenu d'un script non consulté.
3. Séparer les faits confirmés des hypothèses, et signaler les valeurs provisoires.
4. Préférer des changements ciblés : ne pas remplacer un fichier entier si une modification locale suffit, et ne pas modifier plusieurs systèmes sans expliquer pourquoi.
5. Conserver les noms et références sérialisées déjà utilisés (champs Inspector), sauf migration documentée.
6. Ne jamais prétendre qu'un script a été compilé, qu'un test a réussi ou qu'une performance est atteinte sans l'avoir exécuté et mesuré.
7. Expliquer à un débutant les étapes Unity dans l'Inspector et la Hierarchy : références à assigner, réglages, couches de collision.
8. Demander confirmation avant une refonte importante ou l'ajout de dépendances. Ne pas introduire ECS/DOTS, une navigation complexe ou une dépendance externe sans justifier le besoin par des mesures.
9. Fournir après chaque changement une liste de tests et les résultats attendus ; inclure des tests ou une procédure de test pour les règles critiques.
10. Ne pas développer les fonctionnalités futures avant validation des priorités.
11. Répondre en français, en gardant les noms techniques C# et Unity en anglais.
12. Garder une architecture modulaire sans dépendances circulaires, et éviter toute complexité inutile pour le prototype.
13. Utiliser le New Input System et centraliser les valeurs d'équilibrage dans des données ou configurations.
14. Respecter les règles d'or : pas de sprint ni de marche/course, pas de téléportation ni de portail ; les différences de saut, d'esquive et de statistiques viennent des données du personnage et restent testables.
15. Pour les systèmes de foule, commencer par le cas minimal et le tester avant d'étendre.

### 22.2 Format de réponse pour une tâche technique non triviale

Présenter : objectif, fichiers à inspecter, cause confirmée ou hypothèses, modification minimale, étapes Inspector/Hierarchy, tests manuels, limites restantes.

### 22.3 Checklist avant de coder un système

- [ ] La règle est formulée sans ambiguïté.
- [ ] Les données nécessaires sont identifiées.
- [ ] Les préconditions et les cas limites sont documentés.
- [ ] Les interactions avec les autres systèmes sont listées.
- [ ] Le comportement en cas de mort, pause, victoire et redémarrage est défini.
- [ ] Les effets visuels et sonores utiles sont indiqués.
- [ ] Les critères d'acceptation sont testables.
- [ ] Les valeurs sont identifiées comme provisoires ou validées.
- [ ] Les éléments hors MVP sont marqués comme futurs.
- [ ] Aucun comportement de téléportation ou de portail n'est introduit.

---

## 23. Décisions et journal des changements

### 23.1 Registre des décisions retenues

| Sujet | Décision retenue | Section |
|---|---|---|
| Vitesse de déplacement | Une seule vitesse par personnage ; ni sprint, ni marche/course | §3.2 |
| Téléportation | Interdite : aucun portail ni déplacement instantané | §1.4 |
| Pause pendant les choix de niveau | Pause complète | §2.1, §7.2 |
| Esquive en l'air | Autorisée, avec la même récupération | §3.4 |
| Esquive sans direction | Vers l'arrière par rapport à l'orientation du personnage | §3.4 |
| Ennemis présents à l'entrée du boss | Retirés proprement avec une animation de dissolution | §11.6 |
| Emplacements pleins | Remplacer ou refuser ; conversion en ressource = futur | §5.2, §6.2 |
| Ciblage automatique | Règle définie par Instrument dans ses données, départage stable | §5.5 |
| Rareté | Quantité de statistiques uniquement ; probabilités fixes 60/25/12/3 au prototype | §7.3 |
| Rang d'Instrument vs palier de joueur | Deux progressions distinctes (rangs 1–5 ; paliers 10/15/20) | §5.6 |
| Sceau d'Évolution | Consommé seulement lors d'une évolution appliquée ; conservé sinon | §5.6 |
| Évolutions | Au moins deux branches mutuellement exclusives par Instrument | §5.6 |
| Pigments de terrain du MVP | Encre noire et Vermillon uniquement | §9.4 |
| Les 1 000 ennemis | Palier de stress test, pas une densité de jeu | §12.1 |
| Architecture de foule | Composants C# et simulation centralisée ; pas d'ECS/DOTS au départ | §12.2 |
| Joueur et physique | `CharacterController` pour le joueur ; ennemis ordinaires sans `Rigidbody` dynamique par défaut | §12.3 |
| Navigation de la horde | Grille ou champ de direction partagé, avec évitement local | §12.4 |
| Phases d'une run | Six phases A à F pilotées par le chronomètre de run | §11.1 |
| Noms du menu principal | Nouvelle toile, Les artistes, Le catalogue, Options, Quitter | §14.3 |
| Sauvegarde | Format versionné ; run temporaire jamais restaurée | §16.3 |
| Bonus permanents | Modestes ; priorité aux déblocages | §16.2 |
| Périmètre du MVP | 1 personnage, 1 carte, 3 Instruments, 4 Reliques, 4 ennemis communs, 1 élite, 1 mini-boss, 1 boss | §1.6 |
| Boss du MVP | Le Portrait Sans Visage | §10.6 |
| Organisation du document | Document unique, sans addendum ; journal au §23.3 | Début du document |

### 23.2 Décisions encore ouvertes

Les propositions ci-dessous sont des points de départ, pas des décisions. Quand une décision est prise, la reporter dans la section concernée et déplacer sa ligne vers le registre (§23.1).

| # | Sujet | Recommandation actuelle ou candidats | Section |
|---|---|---|---|
| 1 | Touche finale d'esquive | Maj gauche (provisoire) ; bouton secondaire de la souris envisageable | §3.1 |
| 2 | Valeurs finales de vitesse, saut, esquive, caméra et dégâts | Plages de départ aux chapitres 3 et 4 | §3, §4.3 |
| 3 | Les Instruments attaquent-ils pendant le saut ? | Ouvert : à trancher et tester | §3.3 |
| 4 | Certains Instruments peuvent-ils tirer à travers les obstacles ? | Ouvert | §5.5 |
| 5 | Choix des trois Instruments du MVP | Pinceau du Dernier Trait (départ de l'Exilé) + un projectile + une zone persistante, par exemple Arbalète à Encre et Flacon de Vermillon (cohérent avec le pigment vermillon) | §1.6 |
| 6 | Choix des quatre Reliques du MVP | Ouvert | §6.2 |
| 7 | Sélection des quatre ennemis communs | Coulure, Masque Fêlé, Chevalet Brisé, Éclat de Toile | §10.2 |
| 8 | Élite du MVP | Candidat : Masque Royal (cohérent avec le Masque Fêlé) | §10.4 |
| 9 | Poids des catégories de choix de niveau | Valeurs configurables à équilibrer | §7.2 |
| 10 | Formule d'expérience et rythme de montée de niveau | À calibrer sur une run de 20 à 30 minutes | §7.1 |
| 11 | Les Reliques ont-elles des rangs ? | Ouvert | §6.2 |
| 12 | Mini-boss et évolutions : rang requis, nombre de Sceaux, récompense secondaire, nombre d'évolutions par Instrument, Sceau consommable ou non | Rang 5 proposé ; un Sceau par mini-boss | §5.6, §10.5 |
| 13 | **Conflit :** l'évolution B du Cadre Fracturé (« Portail de la Toile ») téléporte des projectiles, ce qui contredit l'interdiction de téléportation et de portail. Les mentions de portails dans la Miniature Fracturée et le Corridor Impossible sont signalées pour la même raison | Reformuler l'évolution (par exemple un tir qui traverse l'arène par un trajet visible) et retirer les portails des textes | §5.1, §6.1, §13.4 |
| 14 | Règles de cumul précises de chaque statut | Règles disponibles au §8.6 ; à choisir statut par statut | §8.6 |
| 15 | Effectif final de personnages | Six prévus ; trois pistes d'extension (Patineur, Encré, Marionnettiste) | §4.4 |
| 16 | Liste définitive des cartes futures | Deux listes se recoupent partiellement | §13.4 |
| 17 | Nom définitif, gains, coûts et effets de la ressource permanente | « Fragments de Toile » provisoire | §16.2 |
| 18 | Format et emplacement du fichier de sauvegarde | Format versionné requis ; format exact à choisir | §16.3 |
| 19 | Dimensions du Musée des Ombres, limites, obstacles, position du boss | À définir après la test map | §13.2 |
| 20 | Équilibrage : santé, dégâts, vitesse, XP, fréquence d'apparition, nombre maximal d'ennemis actifs | Après tests de performance et de lisibilité | §11.3, chapitre 20 |
| 21 | Performances cibles et configuration PC minimale | Baseline 60 FPS sur le PC de référence | §17.5 |
| 22 | Options de difficulté | Ouvert | §11.4 |
| 23 | Le boss est-il toujours obligatoire pour gagner ? | Ouvert | §10.6 |
| 24 | Technologie d'animation des ennemis : classique, poses pré-calculées ou rendu instancié | Décision après comparaison dans la test map | §12.5 |
| 25 | Contrats de restauration | Concept à définir | §1.6 |

### 23.3 Journal des changements

Pour chaque changement important, ajouter une ligne : version, date, sujet, ancienne règle, nouvelle règle, raison, systèmes impactés, statut (proposé, prototypé, testé, accepté ou rejeté). Les décisions ouvertes ne sont jamais effacées : elles sont marquées comme résolues avec la règle retenue.

| Version | Date | Changement |
|---|---|---|
| Base | — | Cahier des charges initial en 25 sections (vision, contrôles, personnages, Instruments, Reliques, ennemis, boss, cartes, interface, architecture, tests) |
| 1.3 | — | Règles de jeu détaillées : améliorations et raretés, peinture au sol, synergies, vagues, statuts, sauvegarde, MVP, fiches de contenu, plan de test |
| 1.4 | — | Direction artistique, UX/UI, HUD, accessibilité, intégration technique de l'interface |
| 1.5 | — | Hordes : simulation centralisée, navigation, animations adaptatives à la distance, test map |
| 1.6 | — | Contrôles et sensations de jeu : vitesse unique, saut, esquive, caméra, combat, profils de personnages, boss du MVP, cartes, validation |
| 2.0 | 2026-10-18 | **Consolidation** en un document unique organisé par thème. Les règles répétées dans plusieurs versions sont fusionnées en un seul endroit. Contradictions tranchées : nombre d'ennemis présents au boss (dissolution retenue), pause complète retenue, esquive en l'air et direction par défaut retenues, découpage des phases de run unifié en six phases A à F, conditions d'évolution unifiées (rang + Sceau + palier), probabilités de rareté fixes au prototype, noms du menu alignés sur l'interface (« Nouvelle toile », etc.), deux arborescences de dossiers fusionnées en une. Conflit signalé et laissé ouvert : évolution « Portail de la Toile » contre l'interdiction de téléportation (§23.2, n° 13). Mentions de portails retirées de la Miniature Fracturée et du Corridor Impossible |
