# BROKEN CANVAS — Cahier des charges

- **Version :** 3.3 — simplification du périmètre livrable et clarification technique ; document unique, il remplace toutes les versions précédentes
- **Statut :** référence de conception proposée. Les valeurs chiffrées sont des valeurs de départ à confirmer par prototype et tests. Aucune fonctionnalité n'est réputée implémentée. La priorité est un jeu court, amusant et livrable par une personne débutante ; les systèmes non indispensables sont reportés.
- **Moteur :** Unity 6, URP, C#
- **Plateforme :** PC Windows
- **Mode :** solo
- **Genre :** survivor roguelite 3D à la troisième personne, avec combats de boss exigeants

## Sommaire

- [1. Présentation du projet](#1-présentation-du-projet)
- [2. Structure du jeu](#2-structure-du-jeu)
- [3. Contrôles, déplacement et caméra](#3-contrôles-déplacement-et-caméra)
- [4. Combat, dégâts et statuts](#4-combat-dégâts-et-statuts)
- [5. Personnages](#5-personnages)
- [6. Instruments](#6-instruments)
- [7. Reliques](#7-reliques)
- [8. Synergies](#8-synergies)
- [9. Progression en partie](#9-progression-en-partie)
- [10. Peinture au sol](#10-peinture-au-sol)
- [11. Ennemis communs, mini-boss et boss](#11-ennemis-communs-mini-boss-et-boss)
- [12. Vagues et difficulté](#12-vagues-et-difficulté)
- [13. Cartes et navigation](#13-cartes-et-navigation)
- [14. Objectifs, événements et récompenses](#14-objectifs-événements-et-récompenses)
- [15. Progression permanente, archives et statistiques](#15-progression-permanente-archives-et-statistiques)
- [16. Direction artistique, effets et audio](#16-direction-artistique-effets-et-audio)
- [17. Interface et expérience du joueur](#17-interface-et-expérience-du-joueur)
- [18. Menu Options](#18-menu-options)
- [19. Architecture technique Unity 6 URP et sauvegarde](#19-architecture-technique-unity-6-urp-et-sauvegarde)
- [20. Hordes, simulation et performance](#20-hordes-simulation-et-performance)
- [21. Fiches de contenu normalisées](#21-fiches-de-contenu-normalisées)
- [22. Tests et critères d'acceptation](#22-tests-et-critères-dacceptation)
- [23. Équilibrage](#23-équilibrage)
- [24. Plan de production](#24-plan-de-production)
- [25. Règles de travail pour l'assistant IA](#25-règles-de-travail-pour-lassistant-ia)
- [26. Simplification de production et décisions de cohérence](#26-simplification-de-production-et-décisions-de-cohérence)
- [27. Décisions et journal des changements](#27-décisions-et-journal-des-changements)

## Comment lire et mettre à jour ce document

Ce document est la **référence unique** du projet. Il n'y a ni addendum ni « version 1.7 » à ajouter en fin de fichier.

- **Une règle, un seul endroit.** Chaque règle est écrite dans le chapitre de son sujet. Les autres chapitres y renvoient (→ §x.y) sans la recopier.
- **Pour changer une règle**, modifier la section concernée à sa place, puis noter le changement dans le journal (§27.3) : date, ancienne règle, nouvelle règle, raison.
- **Pour trancher une décision ouverte** (§26.2), reporter la règle retenue dans le chapitre concerné, puis déplacer la ligne vers le registre des décisions (§27.1).
- **Pour ajouter du contenu** (personnage, Instrument, Relique, ennemi, boss), remplir la fiche normalisée correspondante (chapitre 21) et l'ajouter à la liste du chapitre concerné.
- **Valeurs chiffrées :** toute valeur est une valeur de départ à tester, sauf mention contraire. Aucune n'est validée avant un test jouable.

**Organisation du document :**

- **Chapitres 1 et 2 — Le projet :** vision, périmètre, boucle de jeu, modes, fin de run.
- **Chapitres 3 à 8 — Le joueur et le combat :** contrôles, combat, personnages, Instruments, Reliques, synergies.
- **Chapitres 9 à 15 — Progression, monde et récompenses :** progression en partie, peinture, ennemis et boss, vagues, cartes, objectifs et événements, progression permanente.
- **Chapitres 16 à 18 — Présentation :** direction artistique, effets, audio, interface, options.
- **Chapitres 19 et 20 — Technique :** architecture, sauvegarde, hordes et performance.
- **Chapitres 21 à 26 — Production :** fiches de contenu, tests, équilibrage, plan de production, règles pour l'assistant IA, décisions et journal.

## Résumé exécutif

BROKEN CANVAS est un survivor roguelite 3D solo sur PC Windows, développé sous Unity 6 URP. Le joueur contrôle un personnage à la troisième personne ; il se déplace, saute et esquive manuellement tandis que ses Instruments attaquent automatiquement. Pendant une run de 20 à 30 minutes, il collecte du Pigment, monte de niveau et compose un équipement de six Instruments et six Reliques au maximum. La pression augmente jusqu'à un combat de boss dédié, pendant lequel les apparitions ordinaires cessent. Objectifs secondaires, défis facultatifs, événements et coffres rythment l'exploration de grandes cartes. Le jeu propose deux modes : Normal, et Infini (survie sans fin, débloqué après une première victoire).

Les Instruments ont cinq rangs de base. Dans la vision complète, une évolution se débloque grâce à un Sceau d'Évolution, un Instrument au rang 5 et un niveau de joueur minimal. Pour le MVP simplifié, implémenter une seule évolution de démonstration avec une condition simple et visible ; les paliers 10/15/20 et les multiples branches sont ajoutés après validation de la boucle.

La première version livrable est volontairement plus simple que la vision complète : un personnage, une petite arène du Musée des Ombres, deux Instruments au départ (un seul au tout premier prototype), deux Reliques, trois ennemis communs, le Colosse de Marbre et une version simplifiée du Conservateur Sans Visage. La run complète est d'abord raccourcie à 8–12 minutes pour tester le plaisir de jeu ; la cible de 20–30 minutes ne sera rétablie qu'après validation. Les événements, Fractures, contrats, mini-carte, surfaces complexes, progression permanente, mode Infini et contenu supplémentaire sont reportés.

La priorité est de construire une base stable, lisible et testable, puis d'ajouter le contenu par étapes sans perdre l'identité artistique du jeu.

## Règles d'or

- **La rareté mesure une quantité de statistiques, jamais une transformation de gameplay.**
- **L'évolution change le comportement d'un Instrument et a des prérequis explicites.**
- **Les niveaux de joueur et les rangs d'Instrument sont deux progressions distinctes dans la version complète ; le MVP peut réduire le nombre de rangs et utiliser un seul palier d'évolution.**
- **Une seule vitesse de déplacement par personnage** : ni sprint, ni marche/course.
- **Aucune téléportation, aucun portail, aucun déplacement instantané.**
- **Les dégâts et les statuts suivent des règles communes, déterministes et testables.**
- **La difficulté monte par la composition, le budget de menace et le rythme, pas seulement par les points de vie.**
- **Les surfaces de peinture ont une limite, une durée et une règle de chevauchement.**
- **La sauvegarde permanente ne restaure jamais l'équipement ni l'état temporaire d'une ancienne run.**
- **Le MVP prouve une boucle courte et amusante avant toute run longue ou tout ajout massif de contenu.**
- **Un système n'est ajouté que s'il améliore directement le plaisir, la lisibilité ou la stabilité du prototype.**
- **La cible de foule initiale est modeste ; 1 000 ennemis est un test de stress futur, jamais une exigence de livraison.**
- **Tout chiffre non validé par un test reste une proposition.**

---

## 1. Présentation du projet

### 1.1 Objectif du document et statuts

Ce document sert de référence pour le développeur et les assistants IA. Il décrit ce que le jeu doit faire ; il ne demande aucune programmation en tant que tel.

Lecture des statuts :

- **V1 / MVP :** nécessaire à la première version jouable.
- **Futur :** à réaliser après validation de la boucle principale.
- **Valeur de départ :** chiffre proposé, à tester avant d'être figé.

Les règles de travail (inspection du code existant, préservation des références, vérification des tests, simplicité des systèmes) sont regroupées au chapitre 25.

### 1.2 Vision et piliers

Le joueur incarne un personnage lié à l'art et à la peinture maudite. Il explore des lieux contaminés par des œuvres vivantes, combat des hordes de créatures peintes, collecte du Pigment pour monter de niveau, compose une combinaison d'Instruments et de Reliques, puis affronte un boss final.

Piliers :

1. **Construire sa puissance :** choix de niveau et synergies qui changent réellement les attaques.
2. **Survivre par le mouvement :** placement, saut et esquive ont une utilité claire.
3. **Lire le danger :** les attaques importantes possèdent des indices visuels ou sonores.
4. **Identité artistique :** tableaux vivants, encre surnaturelle et pigments corrompus.
5. **Contenu évolutif :** ajouter des personnages, objets et cartes sans réécrire les systèmes centraux.

### 1.3 Fiche de référence

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
| Difficulté | Une seule difficulté progressive : aucun choix de difficulté (§12.4) |
| Modes de jeu | Normal (V1) et Infini (Futur) (§2.3) |
| Première carte | Musée des Ombres |
| Emplacements maximum | 6 Instruments et 6 Reliques ; le MVP peut utiliser une limite plus basse |
| Rang maximum d'un Instrument | 5 |
| Évolutions | Deux branches par Instrument, mutuellement exclusives par run (§6.6) |
| Raretés | Commun, Peu commun, Épique, Légendaire ; quantité statistique uniquement (§9.3) |
| Pigments de terrain du MVP | Encre noire et Vermillon uniquement (chapitre 10) |
| Boss | Les apparitions ordinaires cessent à son entrée (§12.6) |
| Progression permanente | Déblocages et bonus de puissance modestes (chapitre 15) |
| Téléportation | Interdite : aucun portail ni déplacement instantané |
| Données | Contenus configurables via assets de données ; pas de valeurs de gameplay dispersées dans les scripts |
| Contenu initial ciblé | Voir §1.5 |

### 1.4 Vocabulaire

- **Instrument :** arme principale qui déclenche une attaque automatique.
- **Relique :** équipement secondaire qui modifie les statistiques, les attaques ou les synergies.
- **Pigment :** expérience collectée pendant une run.
- **Rang :** niveau d'amélioration d'un Instrument (1 à 5) ou, si cette règle est retenue, d'une Relique.
- **Palier :** niveau de joueur (10, 15 ou 20) auquel une évolution peut être proposée. Ne pas confondre avec un rang.
- **Évolution :** transformation spéciale d'un Instrument, débloquée par un Sceau d'Évolution, un rang requis et un palier de joueur.
- **Sceau d'Évolution :** récompense garantie par la victoire contre un mini-boss ou un événement prévu ; condition d'une évolution.
- **Surface :** zone de peinture temporaire sur le sol, avec des effets de gameplay (chapitre 10).
- **Fracture :** anomalie dans un tableau qui propose un contrat facultatif ; fonctionnalité future (§14.3).
- **Contrat :** défi facultatif proposé par une Fracture, avec une meilleure récompense.
- **Acte :** tranche de 5 minutes du mode Infini après la 20e minute (§2.3).
- **Retouche :** petite amélioration permanente achetée avec des Fragments de Toile (§15.2).
- **Run :** partie complète jusqu'à la victoire ou la défaite.
- **Directeur de vague :** système qui choisit les apparitions d'ennemis selon le temps et un budget de menace (chapitre 12).
- **Fragments de Toile :** nom provisoire de la ressource permanente (chapitre 15).
- **Archives / catalogue :** collection permanente des découvertes et déblocages, consultable depuis le menu (§17.3).

Les noms d'interface peuvent être artistiques ; les noms de classes, méthodes et variables C# doivent rester explicites et maintenables.

### 1.5 Périmètre : MVP et contenu futur

**Objectif du MVP.** Prouver qu'une run complète est amusante, compréhensible et techniquement stable avant d'élargir le contenu. Le MVP n'est pas le jeu complet : c'est une tranche jouable qui valide le cœur du projet.

**Contenu inclus dans le MVP simplifié :**

- 1 personnage jouable : l'Exilé ;
- 1 petite arène fermée ou semi-ouverte du Musée des Ombres ; aucune grande carte d'exploration pour la première version ;
- 2 Instruments au maximum dans la première tranche jouable : le Pinceau du Dernier Trait et un Instrument à projectile ; le troisième n'est ajouté qu'après validation ;
- 2 Reliques simples au départ ; le total peut monter à 4 si le système de choix fonctionne déjà ;
- 3 ennemis communs au premier objectif jouable : Coulure, Masque Fêlé et Éclat de Toile ; le Chevalet Brisé est ajouté après validation des trois premiers ;
- aucun ennemi « élite » distinct et aucun Essaim dans le MVP ;
- 1 mini-boss : Colosse de Marbre ;
- 1 boss final simplifié : Conservateur Sans Visage ;
- déplacement, caméra, saut et esquive ; les quatre actions doivent être agréables avant d'ajouter du contenu ;
- attaques automatiques, santé, dégâts, Pigment, montée de niveau et choix parmi trois améliorations ;
- un seul système de récompense : choix de niveau ; récompenses de mini-boss uniquement si le flux de choix est stable ;
- une seule évolution démonstrative, avec une branche sélectionnable ; les deux branches par Instrument seront développées après validation ;
- pause, victoire, défaite, bilan minimal et redémarrage fiable ;
- interface fonctionnelle simple, sans effets décoratifs complexes ;
- sauvegarde minimale uniquement si un déblocage permanent est déjà inclus ; sinon, sauvegarde de paramètres reportée ;
- durée de test initiale de 8 à 12 minutes. La cible de 20 à 30 minutes est réintroduite uniquement après validation du rythme et du plaisir de jeu.

**Reporté après validation du prototype amusant :**

- troisième Instrument et quatrième Relique ;
- deuxième branche d'évolution, puis évolutions des autres Instruments ;
- quatrième ennemi commun et tous les autres ennemis ;
- grandes cartes, mini-carte, marqueurs avancés et navigation d'exploration ;
- surfaces de peinture avec effets de gameplay ; elles sont désactivées au départ sauf si le prototype manque de son identité sans elles ;
- événements aléatoires, Fractures, contrats, coffres variés et objectifs secondaires ;
- progression permanente, Fragments de Toile, retouches, Archives complètes et statistiques avancées ;
- mode Infini, actes et récompenses d'acte ;
- autres personnages, cartes, boss, pigments et arsenal ;
- options graphiques avancées, remappage des commandes, shaders complexes et effets d'interface élaborés ;
- architecture de foule spécialisée, simulation centralisée avancée, animation LOD et optimisation pour 1 000 ennemis.

Les critères de sortie du MVP sont au §22.2.

---

## 2. Structure du jeu

### 2.1 Boucle générale d'une run

1. Choisir un personnage disponible.
2. Lancer une run : le joueur entre dans la carte avec son équipement de départ.
3. Explorer et combattre. Les Instruments attaquent automatiquement ; les ennemis apparaissent selon le directeur de vague (chapitre 12).
4. Se déplacer, sauter et esquiver manuellement.
5. Ramasser le Pigment laissé par les ennemis.
6. Monter de niveau et choisir une amélioration parmi trois options. Le jeu est en **pause complète** pendant le choix.
7. Équiper et améliorer des Instruments et des Reliques dans la limite des emplacements.
8. Survivre à une difficulté croissante. Un mini-boss ou un événement explicitement défini peut fournir un Sceau d'Évolution (§6.6).
9. Arrêter les apparitions ordinaires lorsque le boss commence (§12.6).
10. Vaincre le boss ou perdre tous ses points de vie.
11. Afficher le bilan, attribuer les récompenses permanentes, puis permettre de recommencer ou de revenir au menu.

En combat ordinaire, la boucle doit rester dynamique : la pause n'intervient que pour les choix, le menu pause et les écrans de fin.

### 2.2 Boucle seconde par seconde

1. Le joueur lit la position des ennemis et les avertissements d'attaque.
2. Il se déplace pour maintenir une distance utile, éviter les zones dangereuses ou regrouper les ennemis.
3. Les Instruments évaluent leurs conditions de déclenchement et attaquent automatiquement.
4. Les ennemis se déplacent selon leur rôle et déclenchent leurs attaques télégraphiées.
5. Le joueur saute ou esquive pour éviter une menace ; le coût de récupération de l'esquive empêche l'esquive permanente.
6. Les impacts appliquent dégâts, critiques, statuts et interactions de terrain selon un ordre de résolution unique (§4.2).
7. Les ennemis vaincus peuvent laisser du Pigment, des objets ou une récompense spécifique.
8. Le joueur arbitre entre continuer à attaquer, récupérer du Pigment et se repositionner.

Le rythme par minute (phases A à F) est défini au §12.1.

### 2.3 Modes de jeu

Le jeu propose deux modes. Il n'y a pas de choix de difficulté : chaque mode suit sa propre progression (§12.4). Aucun mode journalier ni run à graine n'est prévu.

**Mode Normal (V1).** C'est la run décrite au §2.1 : 20 à 30 minutes, phases A à F, boss final. La run se termine par la victoire contre le boss ou par la défaite.

**Mode Infini (Futur).** Une run de survie sans fin.

- **Déblocage :** après une première victoire en mode Normal.
- Mêmes personnage, carte, équipement de départ, commandes et systèmes que le mode Normal.
- Les 20 premières minutes suivent les phases A à E (§12.1) ; la phase E annonce l'escalade au lieu d'un boss.
- À partir de 20:00, il n'y a **ni boss final programmé ni victoire** : la run entre dans des **actes de 5 minutes** (valeur de départ). Chaque acte est annoncé (§12.5), change la composition des ennemis, augmente le budget de menace, introduit une rencontre de menace renforcée composée d’ennemis communs et, si le rythme le permet, d’un mini-boss ; une fois terminé, l’acte donne un coffre d’acte garanti (§14.4).
- La difficulté monte par la composition, la cadence des vagues et la fréquence des rencontres majeures (§12.4), pas par les points de vie seuls. Les plafonds d'ennemis actifs, de projectiles et de surfaces restent ceux du mode Normal (§12.3) : la run ne doit pas se dégrader techniquement avec le temps.
- Quand tous les Instruments équipés sont au rang maximum et que les options uniques sont épuisées, les montées de niveau proposent des bonus génériques plafonnés (§9.5).
- La run ne se termine que par la mort ou l'abandon (§2.5). Le score comprend le temps de survie, le nombre d'actes terminés et le nombre d'ennemis éliminés, enregistrés comme records (§15.5).
- **Fragments de Toile :** comme une défaite, mais les gains sont réduits de moitié après 30 minutes de run (valeur de départ), pour éviter de farmer sans fin.
- La présence éventuelle d'un boss en mode Infini est une décision ouverte (§26.2).

### 2.4 Machine à états

Pour le MVP, utiliser une machine à états volontairement petite : `MainMenu`, `RunStarting`, `Combat`, `LevelUpChoice`, `BossIntro`, `BossCombat`, `Victory` et `Defeat`. `CharacterSelect` et `EventReward` ne sont ajoutés que lorsqu'ils sont réellement nécessaires. La pause est un état d'interface superposé à l'état courant, et non un état qui efface celui-ci.

**Règles techniques obligatoires :**

- Un seul `RunController` est propriétaire de l'état de la run et des transitions.
- Une seule transition de récompense peut être active à la fois. Si plusieurs récompenses sont déclenchées, elles attendent dans une file simple.
- `isPaused` bloque la simulation de gameplay ; l'interface continue de répondre. Ne pas dépendre uniquement de `Time.timeScale = 0` : les systèmes qui utilisent des mises à jour manuelles doivent aussi respecter l'état de pause.
- Fermer le menu Pause restaure l'état précédent. Fermer un choix ne peut pas reprendre le combat tant que le choix n'est pas résolu.
- La mort, la victoire, la défaite et l'attribution des récompenses sont des opérations idempotentes : chaque résultat n'est appliqué qu'une fois.
- Le redémarrage crée un nouvel état de run et réinitialise tous les objets temporaires.

Chaque transition est définie et testable. Exemples :

- `Combat -> LevelUpChoice -> Combat` met les systèmes de jeu en pause une seule fois, conserve les données de run et reprend sans dupliquer une amélioration.
- `BossCombat -> Victory` ne peut se produire qu'une fois.
- La fermeture d'un menu restaure l'état précédent ; si l'écran précédent était un écran de choix, c'est lui qui est restauré, pas le combat.

### 2.5 Mort, victoire, abandon et résultats

À zéro point de vie du joueur :

- interrompre immédiatement les nouvelles attaques du joueur ;
- arrêter les apparitions et désactiver les entrées de combat ;
- laisser les effets visuels se terminer brièvement, sans infliger de nouveaux dégâts au joueur ;
- afficher le bilan de run (§17.7) ;
- calculer les Fragments de Toile **une seule fois** ;
- enregistrer les déblocages et les statistiques de run ;
- permettre `Recommencer` ou `Menu principal`.

Règles communes à la victoire et à la défaite :

- une mort ne déclenche pas plusieurs écrans de défaite et ne distribue pas plusieurs fois les récompenses ;
- la victoire et la défaite ne sont déclenchées qu'une seule fois ;
- le redémarrage crée un **nouvel état de run** : il ne réutilise ni anciens ennemis, ni projectiles, ni effets, ni minuteries, ni buffs temporaires, ni objets de Pigment, et il réinitialise santé, statistiques temporaires et temps de jeu ;
- le retour au menu permet de choisir un autre personnage.

**Récompenses de fin de run.** Les Fragments de Toile (§15.2) sont calculés une seule fois à partir du total gagné pendant la run, selon l'issue :

| Issue | Fragments | Progression |
|---|---|---|
| Victoire | Total de la run × 1,5 | Déblocages, découvertes, défis et statistiques enregistrés |
| Défaite | Total de la run × 1 | Déblocages, découvertes, défis et statistiques enregistrés |
| Abandon | Total de la run × 1 | Identique à la défaite ; la run est marquée « abandonnée » et ne compte pas comme victoire |

L'abandon n'est pas puni : la récompense dépend de ce qui a été accompli, donc quitter tôt rapporte peu. Il se fait depuis le menu pause, avec confirmation (§17.7), et mène au même bilan que la défaite. En mode Infini, il n'y a ni victoire ni multiplicateur (§2.3).

---

## 3. Contrôles, déplacement et caméra

### Contrat de contrôle du MVP

Le premier objectif est un personnage qui répond de façon prévisible, pas un système de mouvement réaliste. Les valeurs numériques des autres sections restent provisoires, mais le prototype doit choisir une seule configuration de référence et l'enregistrer dans `PlayerMovementConfig`.

- Le déplacement est relatif à la caméra, sur le plan horizontal ; la vitesse est constante hors accélération/freinage courts.
- Le saut est déclenché au sol, applique une impulsion verticale et utilise une gravité explicite. Le MVP n'inclut pas le double saut, l'escalade ni les attaques aériennes spéciales.
- L'esquive a une durée, une distance, un délai de récupération et une fenêtre d'invulnérabilité distincts. Une esquive ne peut pas être relancée tant que sa récupération n'est pas terminée.
- Une esquive sans direction va vers l'arrière par rapport à l'orientation du personnage, conformément à la décision retenue.
- Le joueur ne peut pas être déplacé par un menu ou un curseur de souris capturé ; les menus libèrent le curseur et bloquent les entrées de combat.
- Les attaques automatiques fonctionnent pendant le saut uniquement si le choix ouvert correspondant est tranché ; pour le premier prototype, les attaques restent autorisées en l'air afin de limiter les exceptions.
- Les collisions du joueur et la caméra sont testées dans une arène simple avant de construire la carte finale.


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
- La remappabilité des commandes est prévue dans une version ultérieure ; la centralisation des actions la rend possible. Les options de commandes sont décrites au §18.
- Le bouton secondaire de la souris peut être envisagé pour l'esquive ; la touche finale reste à confirmer (§26.2).

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
- Le saut ne permet pas de franchir les limites de la carte ni d'éviter toutes les mécaniques d'un boss. La possibilité de sauter par-dessus une attaque de boss est décidée attaque par attaque (§11.6).
- Les attaques pouvant toucher le joueur en l'air sont clairement lisibles ; les attaques au sol peuvent avoir des règles distinctes si leur télégraphe l'indique.
- Les attaques automatiques pendant le saut suivent une règle explicite et testée (décision ouverte, §26.2).
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
- Feedback visuel ou sonore à chaque esquive ; l'état de recharge est immédiatement compréhensible (§17.4).
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
- garder les attaques dangereuses, les mini-boss et le boss visibles autant que possible ;
- limiter le mal des transports : accélération de caméra modérée, pas de secousses constantes, sensibilité réglable.

Comportement :

- vue à la troisième personne, suivi stable, rotation à la souris (stick droit à la manette) ;
- limites verticales raisonnables ;
- curseur **capturé pendant le jeu** et **libéré** dans les menus, la pause, les écrans de choix et de fin ; la caméra ne recapture pas la souris tant que le jeu est en pause ou terminé.

Réglages exposés dans une configuration caméra : sensibilité horizontale et verticale, distance de suivi, hauteur et angle, limites verticales, amortissement du suivi, comportement en collision avec le décor, inversion facultative des axes vertical et horizontal, champ de vision, intensité des secousses (réglable et désactivable). Les réglages accessibles au joueur sont au §18.

Point de départ à tester : caméra légèrement surélevée, angle plongeant modéré, distance suffisante pour lire les menaces proches. Les chiffres exacts dépendent de la taille des personnages, de la carte et du champ de vision ; ils ne sont pas figés avant un test dans le Musée des Ombres.

**Collision caméra.** La caméra ne traverse ni murs ni piliers. En cas d'obstruction : rapprochement temporaire, pas de changement brusque de distance, retour progressif à la distance normale, et le joueur ne disparaît jamais durablement. Les tests incluent coins de murs, couloirs étroits, piliers, plafonds bas et combat contre un boss volumineux.

Ne pas faire trembler la caméra à chaque impact mineur (§4.4).

---

## 4. Combat, dégâts et statuts

### Règles techniques simplifiées obligatoires pour le MVP

Tous les dégâts passent par un seul point d'entrée logique (`DamageResolver` ou une méthode équivalente clairement identifiée). Un impact contient au minimum : source, cible, valeur de dégâts et type. Le système applique les règles dans cet ordre :

1. vérifier que la source et la cible sont valides et vivantes ;
2. vérifier l'invulnérabilité de la cible ;
3. appliquer la réduction de dégâts éventuelle ;
4. appliquer les dégâts une seule fois ;
5. déclencher une seule fois les réactions liées à l'impact ;
6. déclencher la mort si la santé atteint zéro.

**Règles MVP :**

- Les attaques ordinaires ne touchent une même cible qu'une fois par impact, sauf si leur fiche décrit explicitement plusieurs impacts.
- L'esquive accorde une seule fenêtre d'invulnérabilité définie par le contrôleur du joueur ; elle ne rend pas le joueur invulnérable pendant toute l'animation par défaut.
- Les statuts complexes, les critiques combinés, les résistances multiples et les réactions en chaîne sont reportés. Le MVP commence avec des dégâts directs et, au maximum, un effet simple de ralentissement ou de zone.
- Un effet de dégâts sur la durée doit posséder une durée, un intervalle et une règle de cumul explicites. À défaut, il ne peut pas être ajouté au jeu.
- Les dégâts de mort, de zone, de projectile et de surface doivent tous respecter le même contrôle d'invulnérabilité.
- Aucun système de dégâts ne modifie directement les points de vie en contournant ce circuit.


### 4.1 Architecture du combat

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

### 4.2 Application des dégâts

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

### 4.3 Règles de combat

- Une attaque n'inflige pas de dégâts plusieurs fois par image à une même cible, sauf effet explicitement conçu pour cela.
- Une zone persistante a une fréquence de dégâts explicite.
- Les dégâts du joueur ne sont pas appliqués à un ennemi déjà mort. Les ennemis morts ne bloquent plus le ciblage et ne sont plus des menaces actives.
- La logique de dégâts suit la collision, pas seulement une animation : collision et visuel restent cohérents.
- Les nombres de dégâts, de critiques et d'effets sont réglables dans les données.
- Les effets de statut ont une durée, une intensité et une règle de cumul définies (§4.6).

### 4.4 Retours de combat

Chaque impact important fournit une combinaison proportionnée de :

- réaction d'impact ou animation ;
- effet visuel lisible ;
- son adapté ;
- nombre de dégâts facultatif (éviter les chiffres permanents s'ils masquent le combat) ;
- léger recul si le type d'ennemi l'autorise.

Pas de tremblement de caméra sur chaque impact mineur. Les effets importants ne masquent ni le sol, ni les projectiles, ni les avertissements de boss.

### 4.5 Points de vie, soins et défaite

- Les points de vie maximum viennent des données du personnage, puis des modificateurs applicables.
- Les soins ne dépassent pas le maximum, sauf mécanisme explicitement défini.
- Le joueur reçoit un retour clair lorsqu'il subit un dégât important.
- L'invulnérabilité de l'esquive est distincte d'une éventuelle invulnérabilité temporaire après impact ; si les deux existent, leur interaction est documentée.
- Le prototype commence avec une règle simple de soins et de dégâts. Résurrection, bouclier, vol de vie et armure temporaire ne sont ajoutés que si un Instrument, une Relique ou un personnage les justifie.
- La défaite suit la procédure du §2.5.

### 4.6 Statuts

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
| Étourdissement | Interrompt une action non protégée | Durée réduite ou immunité temporaire sur mini-boss et boss |

Le MVP n'implémente que les statuts requis par ses personnages, Instruments et ennemis. Les règles de cumul précises de chaque statut sont à confirmer par les tests (§26.2).

---

## 5. Personnages

### 5.1 Règles communes

La vision finale prévoit six personnages. Pour la V1, seul l'Exilé est nécessaire afin de valider les systèmes communs.

- Les personnages partagent le contrôleur, la caméra, la santé et les règles de combat. Les règles de déplacement, de saut et d'esquive du chapitre 3 s'appliquent à tous.
- Les différences viennent des **données** : statistiques de base, passif, Instrument de départ.
- Le passif fonctionne automatiquement dès le début de la run.
- Chaque personnage a une force, une faiblesse et des synergies distinctes.
- Chaque personnage reste jouable avec les commandes de base. Aucun profil ne rend le saut ou l'esquive inutilisables sur une carte standard.
- Les personnages verrouillés indiquent leur condition de déblocage.
- Les cinq personnages supplémentaires sont du contenu futur, pas un prérequis du prototype.

### 5.2 Les six personnages

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

Chaque personnage démarre avec l'Instrument qui porte son identité (liste complète au §6.1). Les fiches détaillées suivent le modèle du §21.1.

### 5.3 Statistiques de base et profils

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

### 5.4 Personnages futurs à prototyper après la V1

Ces trois personnages sont des pistes d'extension, pas des prérequis. Le Patineur est le premier candidat au prototypage après validation du combat de base. Aucun ne doit utiliser de portail ou de téléportation : la mobilité s'appuie sur le déplacement physique, l'inertie, les esquives et les interactions avec le terrain.

- **Le Patineur :** personnage axé sur l'inertie. Il accélère en glissant sur des traces compatibles, perd de la vitesse dans les virages serrés et peut effectuer un dérapage qui dépose une ligne de peinture. À haute vitesse, une collision offensive peut produire une onde de choc. *Force :* maîtrise de la trajectoire. *Faiblesses :* espaces encombrés et arrêts fréquents.
- **L'Encré :** peut se transformer brièvement en flux d'encre au ras du sol. La transformation consomme une jauge ; traverser ses propres flaques peut contribuer à la restaurer ; reprendre forme près d'une flaque peut provoquer une explosion. La transformation reste un déplacement continu : ni téléportation ni déplacement instantané.
- **Le Marionnettiste :** relie plusieurs ennemis avec des fils d'encre ; une fraction des dégâts infligés à un ennemi lié peut être propagée aux autres. Une compétence peut rapprocher les ennemis liés. Plus faible contre une cible isolée.

L'effectif final de personnages (six, ou davantage avec ces pistes) reste à décider (§26.2). Les interactions de ces personnages avec les surfaces sont au §10.5.

---

## 6. Instruments

### 6.1 Liste de référence et évolutions

Dix Instruments sont prévus. Chacun a deux évolutions (A et B) mutuellement exclusives par run (§6.6). Les synergies citées sont des propositions à tester.

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
    - **Évolution B — Toile Réfléchissante :** le cadre dévie physiquement les projectiles qui le traversent vers une cible marquée. La trajectoire de déviation reste visible et se termine par une petite explosion. Aucun portail, aucune brèche et aucune téléportation. Synergie : l’Arbalète à Encre marque les cibles prioritaires et la Palette Catalytique ajoute un statut à l’explosion.

### 6.2 Règles des Instruments

- Maximum de six Instruments équipés, Instrument de départ inclus.
- Améliorer un Instrument équipé ne consomme pas un nouvel emplacement.
- **Emplacements pleins :** une montée de niveau ne propose jamais de nouvel Instrument (§9.5). Quand une récompense d'objet précise (coffre d’acte, récompense de coffre explicitement prévue, mini-boss ou contrat) en offre un, le joueur peut **remplacer** un Instrument équipé, avec confirmation (l'Instrument remplacé perd ses rangs et son évolution), ou **refuser** et recevoir une petite compensation (valeur de départ : soin de 10 % des points de vie maximum).
- Les attaques automatiques sélectionnent des cibles valides et respectent la cadence, la portée et, si applicable, la ligne de vue.
- Projectiles, zones et effets persistants ont des règles de collision et de durée explicites.
- Un Instrument déjà possédé n'est pas proposé en doublon, sauf règle explicite.

### 6.3 Rangs

- **Rang 1 :** version de base.
- **Rangs 2 à 4 :** amélioration progressive de paramètres ou d'effets.
- **Rang 5 :** version de base pleinement améliorée. Le rang 5 ne suffit pas, à lui seul, à rendre un Instrument éligible à une évolution.
- Les rangs peuvent améliorer dégâts, cadence, portée, nombre de projectiles, taille de zone ou durée d'effet.
- Les valeurs sont assez perceptibles pour que chaque rang soit utile.
- Un Instrument au rang maximum ne reçoit plus de rang statistique ordinaire mais reste éligible aux évolutions si les prérequis sont remplis.

### 6.4 Statistiques compatibles par type d'Instrument

Chaque Instrument déclare explicitement les statistiques compatibles. Baseline recommandée :

| Type d'Instrument | Dégâts | Cadence | Portée | Taille | Vitesse projectile | Durée | Critique |
|---|---|---|---|---|---|---|---|
| Projectile direct | Oui | Oui | Oui | Oui | Oui | Non, sauf effet persistant | Oui si applicable |
| Attaque de mêlée | Oui | Oui | Oui | Oui | Non | Non | Oui |
| Zone persistante | Oui | Oui ou recharge | Oui | Oui | Selon projectile | Oui | Selon design |
| Aura / effet autour du joueur | Oui | Selon cadence | Oui, rayon d'aura | Oui | Non | Selon effet | Selon design |
| Fil / lien | Oui | Selon activation | Oui | Selon nombre de cibles ou rayon | Non | Oui si lien temporaire | Selon design |
| Objet invoqué | Oui | Cadence d'attaque | Rayon de détection | Oui | Selon projectile | Durée d'invocation | Selon design |

La matrice est un guide, pas une permission automatique : la fiche de l'Instrument (§21.2) est la source de vérité.

### Règles de ciblage du MVP

- Chaque Instrument possède une portée, une cadence et une règle de ciblage simples.
- Le prototype n'effectue pas une recherche de cible complète à chaque image : il actualise la cible à intervalle régulier, par exemple 0,2 à 0,3 seconde, puis conserve la cible valide.
- Départage stable : priorité à la cible la plus proche ; en cas d'égalité pratique, conserver la cible précédente.
- Une cible morte, désactivée ou hors portée est abandonnée au prochain contrôle de ciblage.
- Les tirs à travers les murs sont interdits par défaut. Les exceptions futures doivent être déclarées dans les données de l'Instrument et testées.
- Le MVP n'inclut pas de visée manuelle, de verrouillage de cible ni de système de précision complexe.

### 6.5 Ciblage et cadence

**Ciblage.** Chaque Instrument définit sa règle de ciblage dans ses données :

- ennemi valide le plus proche ;
- ennemi le plus proche dans une portée ;
- ennemi dans une direction ou un cône ;
- point de sol ou zone autour du joueur ;
- tir dans la direction du déplacement ou de la caméra, si le design de l'Instrument l'exige.

Le ciblage évite les ennemis morts, invulnérables ou hors de portée. En cas d'égalité, la sélection est stable, pour que le tir ne saute pas d'une cible à l'autre à chaque image.

**Paramètres séparés par Instrument :** dégâts de base, temps entre attaques, portée, nombre de projectiles ou de cibles, vitesse et durée de vie des projectiles, taille de zone, règles de perforation ou de rebond, type de ciblage, effets de contrôle.

Tous les Instruments ne sont pas des projectiles : mêlée, zones, lignes et effets orbitaux partagent les règles générales de dégâts (§4.2) sans être forcés dans une seule implémentation.

**Limites techniques.**

- Pooler les projectiles et effets fréquemment créés.
- Ne pas rechercher tous les ennemis de la scène à chaque image pour chaque Instrument ; réutiliser les résultats de ciblage lorsque c'est sûr, à une fréquence adaptée.
- Les attaques de boss et les événements critiques conservent leur priorité de simulation.
- Les optimisations ne font pas disparaître une cible à l'écran et ne rendent pas une attaque injuste.

### 6.6 Évolutions : conditions et déroulement

Une évolution transforme le comportement d'un Instrument ; elle n'est ni un rang ni une rareté.

**Conditions requises (toutes) :**

1. L'Instrument est équipé et a atteint le **rang minimum** défini dans sa fiche (proposé : rang 5).
2. Le joueur possède un **Sceau d'Évolution**, obtenu en vainquant un mini-boss ou lors d'un événement prévu (§11.5).
3. Le joueur atteint le prochain **palier d’évolution** : niveau 10 pour la première évolution de la run, niveau 15 pour la deuxième et niveau 20 pour la troisième. Le palier dépend du nombre d’évolutions déjà choisies pendant la run, pas de la branche choisie.
4. Aucune évolution n'a déjà été choisie pour cet Instrument.

**Règles :**

- Les paliers 10, 15 et 20 correspondent respectivement à la première, deuxième et troisième évolution choisie pendant la run ; ils ouvrent une possibilité, pas une évolution automatique. Les rangs 1 à 5 et les paliers de joueur sont deux progressions distinctes.
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

L'interface d'évolution est décrite au §17.6.

---

## 7. Reliques

### 7.1 Liste proposée

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

### 7.2 Règles

- Maximum de six Reliques équipées. Le contenu du MVP n'en a besoin que de quatre pour tester les interactions.
- Les Reliques modifient les statistiques ou ajoutent des effets passifs.
- **Emplacements pleins :** même règle que pour les Instruments (§6.2). Une Relique offerte par une récompense précise peut remplacer une Relique équipée, avec confirmation (l'ancienne est perdue), ou être refusée contre la même compensation.
- Les règles de cumul des effets similaires sont explicites (§9.4).
- Les effets affichent leur valeur et, si pertinent, leur durée.
- Les modificateurs de vitesse d'une Relique respectent la règle de vitesse unique (§3.2) : ils modifient la vitesse de base uniquement si leur effet le précise.
- Le système de rangs des Reliques reste à décider (§26.2). S'il est activé, les Reliques suivent les mêmes règles d'éligibilité que les Instruments (§9.5).

---

## 8. Synergies

Les synergies décrivent comment les Instruments, les Reliques, les personnages, le terrain et les bonus se combinent. Elles reposent sur des tags et des paires nommées, avec des garde-fous contre les boucles infinies.

### 8.1 Catégories

1. **Instrument + Instrument :** les attaques se complètent ou déclenchent un effet secondaire limité.
2. **Instrument + Relique :** la Relique amplifie une statistique ou une condition de l'Instrument.
3. **Instrument + personnage :** le passif favorise un style de jeu sans rendre les autres Instruments inutiles.
4. **Instrument + terrain :** une attaque exploite une surface ou un pigment.
5. **Statut + statut :** réaction explicite entre deux statuts différents, si elle est autorisée.
6. **Personnage + terrain :** le terrain modifie déplacement ou ressource selon une règle propre au personnage.

### 8.2 Tags partagés et paires nommées

 Chaque Instrument, Relique, bonus et statut porte des tags (par exemple Encre, Vermillon, Zone, Projectile, Critique, Statut, Lien, Cadre, Impact). Un effet peut cibler un tag plutôt qu'un contenu précis, ce qui rend les synergies larges et plus faciles à équilibrer. Les paires nommées (deux contenus précis) restent possibles, notamment pour les synergies d'évolution (§6.1). Une synergie découverte est inscrite dans les Archives (§15.4).

**Mélange de pigments (Futur).** Une construction qui rassemble plusieurs contenus du même tag pourrait activer un petit bonus de palette ; les seuils et les valeurs restent à définir (§26.2).

### 8.3 Lisibilité

 À l'écran de choix (§17.5), les icônes des contenus déjà possédés qui forment une synergie avec la carte proposée s'illuminent.

### 8.4 Contrat d'une synergie

 Chaque synergie définit :

- les identifiants des contenus requis ;
- les conditions d'activation ;
- l'effet exact et sa durée ;
- la priorité si plusieurs synergies se déclenchent ensemble ;
- la fréquence maximale et les limites de propagation ;
- le feedback visuel et sonore ;
- les règles de cumul avec les autres bonus ;
- le comportement quand un prérequis est retiré ou remplacé.

### 8.5 Règles anti-boucle

- Une réaction déclenchée par une autre réaction ne se réactive pas indéfiniment.
- Les dégâts propagés ne déclenchent pas une propagation identique en cascade, sauf règle explicite avec profondeur maximale.
- Chaque élimination ne déclenche qu'une seule récompense d'élimination.
- Une explosion qui tue plusieurs ennemis ne génère pas une quantité illimitée de surfaces ou de projectiles.
- La propagation d'un statut est plafonnée par cible et par seconde.
- Les effets d'une même source conservent un identifiant d'origine pour éviter les doubles déclenchements.

### 8.6 Exemples à tester

Propositions, non équilibrées :

| Combinaison | Effet proposé | Garde-fou |
|---|---|---|
| Flacon de Vermillon + surface de vermillon | Augmente légèrement la durée ou le rayon des surfaces créées | Durée et rayon plafonnés |
| Éclats de Fusain + ennemis marqués | Les projectiles peuvent privilégier les cibles marquées | Priorité de ciblage, pas de dégâts gratuits infinis |
| Palette Catalytique + statut existant | Peut déclencher une réaction de pigment | Temps de recharge par cible |
| Fil de Restauration + ennemis liés | Une partie limitée des dégâts se transmet aux cibles reliées | Propagation sans chaîne récursive |
| Maillet de Marbre Noir + groupe d'ennemis | Onde de choc plus efficace lorsque plusieurs ennemis sont proches | Rayon et multiplicateur plafonnés |
| Bottes du Restaurateur + surfaces noires | Bonus de mobilité réduit mais régulier sur surface compatible | Ne dépasse pas le plafond de vitesse |

---

## 9. Progression en partie

### 9.1 Pigment et niveaux

- Les ennemis vaincus peuvent laisser du Pigment ; les quantités sont configurables par type d'ennemi, et la récompense n'est attribuée qu'une seule fois.
- Le joueur le récupère au contact ou dans un rayon de ramassage configurable.
- Le seuil d'expérience suit une **formule centrale et configurable**, calibrée sur une partie complète de 20 à 30 minutes, le nombre moyen d'éliminations et la distribution des récompenses, et non choisie parce qu'elle est populaire ailleurs.
- Une montée de niveau ne perd pas l'expérience excédentaire.
- Si plusieurs niveaux sont gagnés d'un coup, chaque niveau est résolu correctement, sans dupliquer les récompenses ni écraser un choix.
- L'interface affiche le Pigment courant, le seuil suivant et le niveau (§17.4).
- Les objets de Pigment sont nettoyés correctement après collecte.

### 9.2 Offres d'amélioration

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
6. évolution d'Instrument, uniquement dans l'interface spéciale et si les conditions sont remplies (§6.6).

Les évolutions n'occupent jamais silencieusement une carte ordinaire : elles ont une présentation spéciale et des prérequis explicites.

**Ordre du générateur :**

1. Construire la liste des récompenses éligibles selon l'inventaire, les emplacements et les prérequis.
2. Éliminer les options déjà maximisées, incompatibles ou bloquées.
3. Tirer les catégories selon des poids configurables.
4. Pour chaque amélioration statistique d'Instrument, tirer la rareté (§9.3).
5. Choisir une statistique autorisée pour cet Instrument.
6. Calculer le montant du bonus selon la rareté et la statistique (§9.4).
7. Éliminer les doublons visuels ou mécaniques parmi les trois cartes.
8. S'il reste moins de trois options valides, compléter avec des bonus génériques autorisés.
9. Si aucune option n'est disponible, utiliser une option de secours définie (par exemple un petit bonus de santé ou une ressource de run), sans bloquer le joueur.

Règles complémentaires :

- Le joueur dispose toujours d'au moins une option valide. Les options impossibles sont exclues ou clairement expliquées ; une carte qui ne peut pas être prise n'est jamais affichée.
- Le générateur utilise une graine aléatoire de run pour reproduire les bugs.
- Le choix appliqué est validé une seule fois, puis l'écran se ferme.
- Les poids des catégories (fréquence des Instruments, Reliques et bonus) sont des données configurables, à équilibrer par les tests.
- Les relances et bannissements débloqués par la progression permanente (§15.2) s'appliquent à ce générateur : une relance remplace les trois options par un nouveau tirage, un bannissement retire une option du pool pour le reste de la run. Leur nombre par run est limité.

### 9.3 Raretés des améliorations ordinaires

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
- La couleur n'est jamais le seul indicateur : afficher aussi le nom de la rareté et une icône (§17.11).

### 9.4 Montants et cumul des bonus

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

### 9.5 Éligibilité

- Un nouvel Instrument n'est proposé que si un emplacement est libre et qu'il n'est pas déjà possédé (sauf règle explicite de doublon).
- Un Instrument possédé reçoit des améliorations jusqu'au rang maximum.
- Si tous les Instruments sont au rang maximum, les options correspondantes sont retirées du pool.
- Les options uniques déjà obtenues ne sont pas proposées une seconde fois.
- Le système affiche un motif clair quand une récompense n'est pas disponible, sans remplir l'écran de messages techniques.
- **Emplacements pleins :** aux montées de niveau, aucun nouvel Instrument ni nouvelle Relique n'est proposé ; seules des améliorations (rangs, bonus, évolutions) le sont. Le choix remplacer ou refuser ne concerne que les récompenses d'objet précises (§6.2).

---

## 10. Peinture au sol

La peinture au sol est une mécanique de déplacement et de combat, pas seulement un effet visuel. Pour préserver la lisibilité et limiter le coût de production, le MVP commence avec deux types de surface ; le système est étendu après validation.

### 10.1 Règles générales

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

### 10.2 Encre noire (terrain initial)

- Le Patineur (futur) gagne de l'accélération sur une surface noire compatible, jusqu'à un plafond de vitesse.
- L'Encré (futur) peut récupérer une petite quantité de ressource en traversant ses propres surfaces, si cette mécanique est activée.
- Les ennemis ne sont pas ralentis par défaut : l'encre noire ne devient pas un contrôle universel.
- Les surfaces noires ne modifient pas la friction du moteur physique de façon imprévisible ; le bonus est appliqué via le contrôleur de déplacement.

### 10.3 Vermillon (terrain initial)

- Peut appliquer un effet de marquage ou de dégâts aux ennemis qui entrent dans la zone, selon la source qui a créé la surface.
- Un même ennemi ne reçoit le même déclenchement qu'une fois par intervalle défini (valeur de départ : 0,75 s).
- Les déclenchements sont indépendants du nombre de colliders qui se chevauchent.
- Les dégâts périodiques ne se cumulent pas sans limite : renouveler la durée ou appliquer une intensité plafonnée.

### 10.4 Superposition et pigments futurs

Pour le MVP, seuls l'encre noire et le vermillon existent en gameplay. Si les deux se chevauchent :

- les deux effets coexistent s'ils sont compatibles ;
- un même effet n'est jamais appliqué plusieurs fois par image ;
- les dégâts périodiques d'un même type ne se cumulent pas sans plafond ;
- l'effet de déplacement est résolu séparément de l'effet offensif ;
- le rendu affiche les deux couleurs de façon lisible, sans couvrir le personnage ni les télégraphes ennemis.

Pigments futurs (hors périmètre tant que les deux premiers ne sont pas validés, et jamais codés comme systèmes cachés dans le MVP) : un pigment bleu qui ralentit les ennemis, un pigment or, une peinture contaminée qui transmet un statut aux ennemis qui la traversent.

### 10.5 Interactions entre surface et personnages

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

### 10.6 Performance et lisibilité

- Séparer les surfaces de gameplay des particules décoratives.
- Réutiliser les objets visuels par pooling si le profilage confirme un gain.
- Éviter un système de physique par goutte de peinture.
- Limiter les déclenchements de statut par cible et par intervalle.
- Réduire les particules décoratives avant de réduire les télégraphes d'attaque.
- Les avertissements de boss restent toujours visibles au-dessus des effets de terrain.
- Tester l'arène avec la densité maximale d'ennemis, de projectiles et de surfaces prévue.

---

## 11. Ennemis communs, mini-boss et boss

### 11.1 Principes

Les modèles sont stylisés, simples et lisibles. Plusieurs ennemis peuvent partager des animations ou des composants. Une variante apporte un comportement ou un rôle distinct, pas seulement une couleur. Chaque ennemi configure ses points de vie, sa vitesse, ses dégâts, sa portée et ses délais d'attaque. Les récompenses ne sont attribuées qu'une seule fois. Les noms, formes, animations et attaques s'intègrent à l'esthétique du jeu.

### 11.2 Ennemis communs et familles de comportement

Le jeu distingue trois niveaux de menace : **ennemis communs → mini-boss → boss**. Il n’existe pas de catégorie séparée d’« élites » dans la conception actuelle. Les ennemis communs utilisent des comportements simples, réutilisables et lisibles. Chaque ennemi doit idéalement avoir trois états maximum et un télégraphe principal.

**Familles de comportement**

| Famille | Comportement | Télégraphe et réponse du joueur | Exemples |
|---|---|---|---|
| **Suiveur** | Avance vers le joueur et attaque au contact. | Lire la densité de la nuée et se repositionner. | Coulure, Gribouilleur, Bloc de Marbre |
| **Charge** | S’arrête environ 0,6 s, charge en ligne puis récupère environ 1,2 s. | Esquive latérale ; punir la récupération. | Masque Fêlé, Tache Vorace |
| **Explosion** | S’approche, gonfle environ 1 s, puis explose en zone. | S’éloigner avant l’explosion. | Goutte Vermillon, Bulle Bombée |
| **Traînée** | Suit le joueur en laissant de l’encre poisseuse pendant environ 4 s. | Changer de trajectoire et éviter la surface. | Traînard, Suinteur |
| **Artilleur** | S’arrête à 18–25 m et lance un projectile en arc. L’impact survient 1,4 s plus tard et laisse une flaque poisseuse pendant 3 s. Cadence : un tir toutes les 3 s. | Cercle de 2 m qui se remplit au sol ; le joueur se déplace, sans visée manuelle. | Éclat de Toile (rôle à confirmer en prototype) |
| **Essaim** | Groupe de 6 à 10 petits ennemis fragiles qui avancent en nuée avec de légères dispersions. | Pas de télégraphe individuel : le danger vient du nombre. Regrouper puis balayer avec des attaques de zone. | Essaim de petites taches |
| **Bouclier** | Suit le joueur et bloque les coups venant de face. | Le contourner pour attaquer de côté. | Cadre Ambulant |
| **Soutien** | Suit à distance et renforce les alliés proches, par exemple +15 % de vitesse. | Repérer et prioriser le soutien. | Lampion |

**Autres concepts d’ennemis communs proposés** (à utiliser selon le budget de contenu, pas tous dans le MVP) :

- **Goutte Errante :** poursuivant simple, lisible et peu résistant.
- **Bulle Bombée :** approche puis explose après un gonflement annoncé.
- **Suinteur :** laisse des flaques derrière lui.
- **Trait Fugitif :** charge rapide en ligne droite.
- **Œil du Cadre :** marque le joueur puis tire vers sa position annoncée.
- **Débris Vivant :** petite cible fragile qui zigzague.
- **Tache Rampante :** ralentit brièvement au contact.
- **Semencier Noir :** pose au maximum deux mines annoncées.
- **Cadre Fendu :** protège son front, encourage le contournement.
- **Papillon de Pigment :** ennemi volant qui laisse une zone temporairement dangereuse.
- **Buste Désaxé :** tank lent avec frappe annoncée.
- **Bulle d’Encre :** variante qui laisse une flaque en mourant.
- **Portrait Effacé :** devient translucide et intouchable tout en continuant à se déplacer lentement ; sa silhouette reste visible au sol. Il ne disparaît pas et ne réapparaît pas instantanément.

**Règles de composition :**

- Au moins 60 % des ennemis actifs devraient être des Suiveurs ou des unités d’Essaim simples.
- Quatre familles de comportement au maximum sont utilisées simultanément sur une carte dans le MVP.
- Une composition complète peut comprendre trois profils de suiveurs (fragile, standard, lourd) et deux familles spéciales.
- Les apparitions se font idéalement dans un anneau de 25 à 40 m, jamais à moins de 15 m, sur une zone accessible et généralement hors champ de vision ; environ 70 % peuvent provenir de la direction avant du joueur. Ces valeurs sont des hypothèses de test.
- Les attaques et télégraphes ne doivent pas être masqués par les effets visuels.

**Ennemis communs de la vision complète ; MVP simplifié : les trois premiers ennemis ci-dessous, puis le quatrième après validation :**

1. **Coulure** — poursuivant de mêlée simple.
2. **Masque Fêlé** — rapide, avec une charge annoncée.
3. **Chevalet Brisé** — ennemi lourd, lent et résistant.
4. **Éclat de Toile** — rôle d’artilleur à distance, avec le tir en arc défini ci-dessus.

L’Essaim est reporté après validation du jeu de base. Il ne fait pas partie du MVP simplifié et son intérêt sera évalué lors d'un test ultérieur consacré aux attaques de zone.

### 11.3 Répartition proposée par carte

| Carte | Suiveurs | Ennemis spéciaux |
|---|---|---|
| Musée des Ombres (MVP) | Gribouilleur, Coulure, Bloc de Marbre | Masque Fêlé, Éclat de Toile/Artilleur |
| Atelier des Damnés | Pinceau Errant, Tube Éventré, Chevalet Brisé | Traînard, Goutte Vermillon |
| Corridor Impossible | Mannequin, Marionnette du Peintre, Pilier Animé | Cadre Ambulant, Essaim |
| Galerie des Portraits | Ombre de Cadre, Visage Pâle, Buste Fendu | Portrait Effacé reformulé, Œil du Conservateur |
| Bassin des Pigments | Goutte Filante, Flaque Rampante, Colosse de Cire | Traînard de Pigment, Lampion |

Les noms et associations sont des propositions de contenu, pas des exigences de production immédiates. Le MVP ne produit que les quatre ennemis listés ci-dessus.

### 11.4 Mini-boss

Un mini-boss est un ennemi nommé, nettement plus dangereux qu’un ennemi commun et associé à une récompense importante. Il n’existe pas de palier « élite » séparé. Un mini-boss comporte au moins deux attaques et un changement de comportement vers 50 % de ses points de vie. Durée cible : 45 à 75 s ; toutes les attaques majeures sont télégraphiées et aucune phase d’invulnérabilité prolongée n’est autorisée.

La victoire donne un **Sceau d’Évolution garanti** et **une Relique à choisir parmi trois** (§14.4). Si aucun Instrument n’est éligible, le Sceau est conservé ; une récompense alternative peut être proposée sans consommer le Sceau. Pour le MVP : un seul mini-boss.

| Mini-boss | Famille principale | Attaques | Changement vers 50 % |
|---|---|---|---|
| **Colosse de Marbre** (MVP) | Suiveur lourd | Balayage frontal en cône (télégraphe 0,9 s) ; piétinement en cercle (télégraphe 1,2 s) | Enchaîne les deux attaques et appelle 3 Gribouilleurs |
| **Maître du Fusain** (futur) | Attaques aériennes plongeantes, à retravailler sans téléportation | Plongeon en ligne annoncé par une ombre au sol pendant 1 s ; volée d’éclats | Invoque un groupe d’ennemis volants ou un Essaim |
| **Coulée Mère** (futur) | Traînée + explosion | Traînée d’encre poisseuse ; Gouttes Vermillon qui explosent après 2 s | Traînée plus large et davantage de gouttes |
| **Matrice des Taches** (futur) | Zones au sol | Poursuite lente, flaques successives, explosion télégraphiée | Plus de flaques, suivies d’une fenêtre de récupération |
| **Gardien Défiguré** (futur) | Lourd / onde au sol | Piétinement créant une onde basse à sauter ; frappe horizontale | Enchaîne les attaques avec une récupération toujours lisible |
| **Portrait Exécuteur** (futur) | Tir à distance | Zone marquée puis salve de projectiles ; attaque en ligne | Salves plus rapides, sans supprimer les fenêtres de réaction |
| **Serpent de Vermillon** (futur) | Charge / traînée | Charge, traînée dangereuse, émergence dans une zone annoncée | Traînées plus nombreuses, jamais de déplacement instantané non télégraphié |
| **Peintre Bourreau** (futur) | Attaques de zone | Deux coups de pinceau, arc de peinture, rectangle dangereux | Enchaînements plus serrés avec zones distinctes |
| **Conservateur Brisé** (futur) | Bouclier / projectiles | Garde frontale, éventail d’éclats, zones multiples | Récupération vulnérable après une attaque lourde |

### 11.5 Principes des mini-boss et choix du MVP

- Le mini-boss doit tester une compétence apprise : déplacement, saut, esquive, lecture d’une zone ou contournement.
- Chaque attaque doit avoir un signal visuel, un délai, une zone, une durée, des dégâts et une récupération documentés.
- Les renforts invoqués sont limités et ne doivent pas rendre le combat illisible.
- Le Colosse de Marbre est le mini-boss retenu pour le MVP. Le Conservateur Sans Visage est désormais le boss final du MVP ; le Portrait Sans Visage devient un concept de boss futur à retravailler.
- Les anciens concepts initialement nommés élites (Masque Royal, Conservateur protecteur, Tache Couronnée, Copiste, Portrait Inachevé) peuvent être réutilisés comme variantes d’ennemis communs ou concepts de mini-boss ; ils ne constituent pas une catégorie de progression distincte.

### 11.6 Boss

**Règles communes :**

- barre de santé visible ;
- au moins trois motifs d'attaque distincts ;
- attaques majeures télégraphiées visuellement ou par le son ;
- attaques évitables par déplacement ou esquive, sauf exception annoncée ;
- effets du joueur ne masquant pas les zones dangereuses ;
- conditions de victoire fiables et vérifiables ;
- les apparitions ordinaires cessent dès le début du combat (§12.6) ;
- phases d'invulnérabilité longues à éviter : le boss teste le déplacement, la lecture des attaques et la construction du joueur.

**Boss envisagés :**

- **Le Conservateur Sans Visage (boss du MVP) :** boss final de la première version jouable. Baseline à prototyper : balayage frontal de pinceau annoncé au sol, ligne de projectiles d’encre, flaques persistantes temporaires et attaque circulaire de zone. Trois phases lisibles ; la dernière accélère les enchaînements sans téléportation. Chaque attaque doit avoir un télégraphe, une réponse de déplacement/saut/esquive et une récupération identifiable. Ce kit est une proposition de production à valider par prototype, pas un ensemble d’attaques déjà testé.
- **Le Portrait Sans Visage (boss futur) :** masque géant flottant, projectiles, zones explosives et invocation de Masques Fêlés.
- **Le Conservateur Déchu :** assemblage de cadres et de chevalets, frappes au sol et protection de points faibles.
- **L'Œuvre Inachevée :** entité humanoïde abstraite qui modifie les zones dangereuses ; boss final futur.
- Pistes alternatives : **Le Conservateur** (contrôle l'espace, à rapprocher du Conservateur Déchu), **La Toile Déchirée** (modifie les zones sûres), **Le Maître des Pigments** (change temporairement les propriétés de la peinture).
- **Le Roi de Marbre** (Atelier des Damnés, futur) : coups de maillet et ondes sismiques, anneaux à sauter, éclats de marbre et fenêtres de vulnérabilité après une frappe lourde.
- **Le Portrait aux Mille Yeux** (Galerie des Portraits, futur) : visée laser annoncée, pluie de pigments, lignes qui se croisent et point faible lisible ; éviter les faux signaux injustes.
- **Le Serpent Vermillon** (Bassin des Pigments, futur) : charges, traînées d’encre et morsure depuis une zone annoncée ; les zones sûres restent toujours accessibles.
- **L’Horloger des Perspectives** (Corridor Impossible, futur) : attaques en lignes, vagues rythmées et cadres qui tournent ; la cadence augmente en fin de combat sans rendre les signaux illisibles.
- **L’Œuvre Originelle** (Toile Originelle, futur) : combine progressivement des mécaniques déjà apprises et des zones de pigments, plutôt que d’introduire de nombreuses règles inconnues à la dernière phase.

**Structure d'un combat de boss** (modèle générique ; adapter au Conservateur Sans Visage pour le MVP) :

- phase de lecture et d'apprentissage ;
- phase d'attaque principale ;
- transition annoncée ;
- phase plus exigeante combinant les mécaniques déjà apprises ;
- fenêtre de vulnérabilité si le design en a besoin.

**Chaque attaque de boss précise :** son signal visuel et sonore, son délai avant impact, sa zone et sa durée, si le saut peut l'éviter, si l'esquive peut l'éviter, ses dégâts et effets, sa récupération et ses cas limites de collision.

Le boss est testé avec tous les profils de personnage prévus, notamment celui dont le saut est le plus bas et celui dont l'esquive est la plus courte. Aucune attaque obligatoire n'est impossible à éviter avec les statistiques de base d'un personnage. Les fiches de boss suivent le modèle du §21.6.

---

## 12. Vagues et difficulté

### 12.1 Durée et phases d'une run

Durée cible : 20 à 30 minutes, boss compris. Le prototype peut être raccourci pour itérer. Le directeur de vague est piloté par le chronomètre de run et des paliers explicites.

**Chronologie de départ proposée pour le mode Normal** (les horaires seront ajustés par les tests) :

| Temps | Phase | Menace et événements | Récompenses / progression |
|---|---|---|---|
| 0:00–1:00 | A | Coulures, environ 6 à 10 ennemis actifs ; conseil de déplacement vers 0:10 | Premier niveau vers 0:50 |
| 1:00–2:00 | A | Coulures plus nombreuses ; conseil de saut au premier obstacle | Niveaux 2–3 |
| 2:00–3:00 | A | Introduction des Masques Fêlés et de leur charge ; conseil d’esquive | Niveau 4 |
| 3:00–4:00 | A | Groupes plus larges ; premier tirage d’événement | Niveau 5 vers 4:00 |
| 4:00–6:00 | B | Introduction des Artilleurs / Éclats de Toile | Niveaux 5–7 ; première synergie possible |
| 6:00–8:00 | B | Ajout des ennemis lourds ; événements espacés | Niveaux 7–9 |
| 8:00 | C | Rencontre de vague renforcée, sans mini-boss supplémentaire garanti | Récompense à choix possible via coffre ou événement identifié |
| 9:45 | C | Annonce du mini-boss principal | Niveau 10 visé vers 9:00 |
| 10:00–11:15 | C | Mini-boss MVP (combat cible de 45 à 75 s) | Sceau d’Évolution + une Relique parmi trois |
| 11:15–13:00 | C | Retour des vagues | Première évolution possible si les prérequis sont remplis |
| 13:00–18:00 | D | Pression accrue, groupes mixtes, Artilleurs et Essaims possibles après le MVP | Niveau 15 visé vers 14:00 ; événements facultatifs limités |
| 18:00 | E | Arrêt des événements et des nouveaux défis facultatifs | Préparation finale |
| 18:30 | E | Annonce sonore et marqueur du boss | Derniers choix de niveau |
| 19:45 | E | Fin des apparitions ordinaires ; Pigment restant attiré vers le joueur | Pas de récompense gratuite garantie |
| 20:00–20:10 | F | Dissolution des ennemis ordinaires et introduction du boss | Arène nettoyée |
| 20:10–24:00 | F | Boss, durée cible 3 à 5 min | Victoire puis bilan |

Les repères de niveau sont des cibles d’équilibrage : niveau 10 vers 9:00, niveau 15 vers 14:00 et niveau 20 vers 19:00. Le boss du mode Normal arrive à 20:00 ; l’annonce commence à 18:30 et les apparitions ordinaires cessent à 19:45. Le mini-boss supplémentaire à 15:30 reste hors MVP et pourra être testé plus tard.

### 12.2 Directeur de vague

Le directeur décide :

- du temps écoulé et de la phase ;
- des familles d'ennemis autorisées ;
- du budget de menace disponible ;
- du nombre maximal d'ennemis actifs ;
- des événements et rencontres majeures disponibles ;
- des périodes de transition ;
- du moment du combat de boss.

Il tient aussi compte du type de zone et de la proximité du boss. Il ne fait pas apparaître d'ennemis si la limite globale est atteinte. Pendant un écran de choix, le temps de combat est réellement en pause.

### 12.3 Budget de menace et d'apparition

Chaque ennemi reçoit un **coût de menace**. Le directeur choisit une combinaison dont le coût total ne dépasse pas le budget de la phase : la difficulté ne repose pas uniquement sur les points de vie.

Le directeur respecte :

- une limite d'ennemis actifs ;
- une limite de créations par intervalle ;
- une distance minimale de sécurité autour du joueur ;
- des points d'apparition hors caméra ou clairement télégraphiés ;
- les contraintes de navigation et de collision ;
- une réserve de budget pour les mini-boss et les événements importants.

Les nombres exacts d'ennemis actifs sont fixés après les tests de performance sur la machine cible. Les 1 000 ennemis du §20.1 sont une cible de stress test, pas une population de jeu.

**Règle de placement initiale à tester :** apparition à 25–40 m du joueur, jamais à moins de 15 m, sur une surface accessible et généralement hors champ ; environ 70 % des apparitions peuvent provenir de l’hémisphère avant. Les Artilleurs cherchent une distance de 18–25 m. Aucun ennemi ne doit apparaître directement dans une zone d’impact imminente.

### 12.4 Paramètres de difficulté

La difficulté peut augmenter par :

- le budget de menace et la fréquence ou la cadence d'apparition ;
- la composition et la diversité des familles d'ennemis ;
- la santé et les dégâts, avec modération et dans une plage limitée ;
- les nouvelles attaques ou variantes, et le nombre d'attaques simultanées ;
- les événements et la fréquence des rencontres majeures ;
- la pression spatiale et le contrôle de zones.

Elle ne se contente pas d'augmenter toutes les statistiques de façon uniforme. La vitesse des projectiles et la taille des télégraphes restent équitables. Une difficulté lisible demande au joueur de mieux se déplacer, sauter, esquiver et choisir ses améliorations.

Il n'existe **qu'une seule difficulté progressive** : le jeu ne propose aucun choix de difficulté.

### 12.5 Transitions

Les changements de phase sont perceptibles : annonce, son, changement de musique ou de lumière, puis arrivée des nouvelles menaces. Pas d'apparition soudaine au contact du joueur.

### 12.6 Apparition du boss

Au début de l'introduction du boss :

- arrêter les nouvelles apparitions ordinaires ;
- désactiver les événements qui créeraient d'autres ennemis ;
- retirer proprement les ennemis ordinaires déjà présents, avec une courte animation de dissolution, pour éviter que le boss soit entouré d'une horde non prévue (règle retenue pour le MVP) ;
- nettoyer ou laisser expirer les projectiles ordinaires selon une durée courte et prévisible ;
- afficher le nom du boss, sa barre de santé et un avertissement sonore ;
- démarrer le combat seulement lorsque l'arène et le boss sont prêts.

---

## 13. Cartes et navigation

### 13.1 Principes de conception

Les cartes sont **grandes** : elles se parcourent et s'explorent, elles ne se réduisent pas à une arène d'un seul écran. Valeur de départ : environ 300 à 500 m de côté, soit 1 à 1,5 minute pour les traverser à la vitesse de référence (§3.2). Cette taille impose une navigation lisible (§13.4), une navigation d'ennemis locale (§20.4) et des apparitions hors caméra à bonne distance du joueur ; elle est validée dans la test map (§20.7).

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

Les dimensions suivent la valeur de départ du §13.1 ; limites, obstacles et position du boss sont à définir (§26.2).

### 13.3 Cartes futures

1. **Atelier des Damnés :** chevalets, pots de peinture, toiles inachevées et zones d'encre.
2. **Corridor Impossible :** architecture répétitive et cadres flottants. *(La version d'origine mentionnait des portails : à retirer, §26.2.)*
3. **Galerie des Portraits :** miroirs, statues sans visage et apparitions surprises.
4. **Bassin des Pigments :** réserve souterraine, rivières de peinture et zones instables.
5. **Toile Originelle :** monde abstrait peint, changements visuels pendant le boss final.

Pistes d'arènes pour la progression future :

- **Atelier abandonné :** arène relativement ouverte pour apprendre les systèmes.
- **Galerie brisée :** piliers et cadres qui influencent les trajectoires.
- **Musée englouti :** espace plus contraint avec zones de peinture dangereuses.

Ces pistes recoupent en partie les cartes ci-dessus ; la liste définitive est à arbitrer. Les événements aléatoires sont décrits au §14.1.

Varier d'abord la disposition et la lumière plutôt que de produire des modèles uniques pour chaque carte. Ces contenus ne modifient pas le périmètre du MVP.

### 13.4 Navigation

Les cartes étant grandes (§13.1), le joueur dispose de deux aides de navigation.

- **Mini-carte (V1) :** un plan du musée en vue de dessus, simplifié, qui montre la position et l'orientation du joueur ainsi que les marqueurs d’objectifs (mini-boss, coffres, Fractures, autels, événement actif). Elle ne montre pas les ennemis ordinaires, pour préserver la lisibilité. Son emplacement dans le HUD est au §17.4.
- **Marqueurs d'objectifs en bord d'écran (V1) :** de discrètes flèches indiquent la direction des objectifs importants situés hors du champ de vision. Pour le MVP : le mini-boss et le boss.
- **Révélation progressive (Futur) :** le plan se peint à mesure que le joueur explore.
- **Présentation proposée :** mini-carte fixe orientée nord en haut ; seule la flèche du joueur tourne. Rayon représenté d’environ 80 m, taille proche de 15 % de la hauteur d’écran, opacité de 75 %, coin inférieur droit. Ces valeurs sont des points de départ.
- Le joueur est toujours affiché ; les ennemis ordinaires ne le sont jamais. Mini-boss et boss sont marqués dès leur annonce ; les marqueurs d’objectif, événements et récompenses apparaissent lorsqu’ils sont découverts ou actifs. Les coffres communs futurs peuvent être révélés dans un rayon de 40 m.
- La mémoire des zones déjà explorées est une fonctionnalité future ; aucune option de zoom, taille ou opacité n’est prévue en V1, mais le masquage de la mini-carte est possible.
- Les deux aides se désactivent séparément dans les options (§18).
- Technique : la mini-carte utilise une texture pré-calculée ou une caméra orthographique dédiée en basse résolution ; son coût est mesuré (§20.8).
- Aucun déplacement rapide via la carte (§1.3).

---

## 14. Objectifs, événements et récompenses

### 14.1 Événements de carte

Un événement explique clairement : son point ou sa zone d'activation, sa condition de déclenchement, le risque encouru, la récompense, et s'il est facultatif ou requis.

Pas de téléporteurs ni de portails. Les transitions entre cartes passent par le menu ou par un mécanisme de progression explicite, sans portail en jeu.

**Événements aléatoires (Futur ; un événement simple peut être testé dans le MVP) :**

| Événement | Effet | Risque / récompense |
|---|---|---|
| Pluie de Pigment | Zone riche en Pigment pendant 30 s | Les ennemis sont attirés vers la zone |
| Chute de cadres | Zones de danger télégraphées | Coffre à la fin si le joueur survit |
| Débordement de pigment | Surface dangereuse sur une zone | Pigment doublé à l'intérieur |
| Vague d'encre | Afflux soudain d'ennemis | Récompense si le joueur survit |
| Surface rare | Surface bénéfique temporaire | À exploiter rapidement |
| Rencontre itinérante | Un mini-boss ou une rencontre spéciale apparaît sur la carte | Récompense annoncée et adaptée à la rencontre |

**Fréquence de départ :** un tirage toutes les 90 s à partir de 3:00, avec 35 % de chance ; au moins 2 minutes entre deux événements ; un seul événement actif à la fois ; 4 à 5 événements par run au maximum ; aucun événement après 18:00 ni pendant le boss. En mode Infini, au plus un événement par acte. Chaque événement est annoncé avant d'agir (§12.5), respecte le budget de menace (§12.3) et se nettoie complètement à sa fin.

### 14.2 Objectifs secondaires

Pendant une run, le joueur peut faire autre chose que tuer des ennemis. Ces objectifs sont facultatifs et aucun n'est requis pour gagner. Chacun précise sa zone, sa condition, son risque et sa récompense (§14.1). Phase : Futur.

| Objectif | Principe | Récompense proposée |
|---|---|---|
| Restaurer un tableau | Rester dans la zone d'un autel de restauration pendant une durée (valeur de départ : 20 s) ; la progression redescend lentement si le joueur sort | Un bonus de run (§14.5) |
| Briser des toiles marquées | Détruire un nombre défini de toiles destructibles dispersées dans une zone | Pigment |
| Vaincre une cible spéciale désignée | Une cible ou un groupe est marqué sur la carte ; le vaincre dans le temps imparti | Coffre doré du contrat (§14.4) |
| Repeindre une zone | La zone se peint par la présence du joueur, sans exiger un Instrument particulier | Pigment ou bonus de run |
| Tenir sans dégât | Survivre une durée définie sans subir de dégâts, avec compteur visible | Pigment |

Aucun objectif n'est impossible pour un personnage donné (§5.1).

### 14.3 Défis facultatifs : Fractures et contrats

Une **Fracture** est une anomalie visible dans un tableau de la carte. Elle propose un **contrat** : un défi facultatif, plus risqué, avec de meilleures récompenses. Phase : Futur.

- Le joueur accepte en entrant dans la zone de la Fracture et peut ignorer sans pénalité.
- Un seul contrat est actif à la fois ; son compteur est affiché dans le HUD (§17.4).
- Récompense : un coffre doré (§14.4) et 2 Fragments de Toile (§15.2).
- En cas d'échec, le contrat disparaît, sans autre effet.
- Apparition : 2 à 3 Fractures par run (valeur de départ), à des endroits variés, jamais pendant le boss.
- Exemples de contrats : éliminer 30 ennemis marqués en 60 s ; tenir 45 s dans une zone ; vaincre une cible spéciale désignée ; tenir 60 s sans subir de dégâts.
- Une Fracture n'ouvre ni portail ni déplacement (§1.3).

### 14.4 Coffres et récompenses

Les récompenses sont garanties, aléatoires ou à choix selon leur source. Aucune récompense ne dépend d’une catégorie « élite » : chaque gain doit être relié à un mini-boss, un coffre, un événement ou un contrat précis.

| Source | Type | Contenu | Phase |
|---|---|---|---|
| Mini-boss | Garantie | Sceau d'Évolution et 1 Relique parmi 3 | V1 |
| Coffre de récompense de rencontre | À choix, uniquement lorsqu’une rencontre ou un événement le prévoit explicitement | 1 récompense parmi 3 : Relique, Instrument ou bonus | Selon le contenu |
| Coffre commun (carte) | Aléatoire | Une récompense tirée : Pigment, soin ou bonus de run | Futur |
| Coffre doré (contrat) | Garantie, à choix | 1 parmi 3, avec un pool amélioré (§14.3) | Futur |

Règles :

- Un coffre s'ouvre par contact : aucune commande d'interaction supplémentaire n'est ajoutée (§3.1).
- Les choix à trois options suivent le générateur du §9.2 et l'éligibilité du §9.5 ; une option impossible n'est jamais affichée.
- Une récompense n'est attribuée qu'une seule fois.
- Emplacements pleins : remplacer ou refuser (§6.2).
- En mode Normal, le joueur reçoit au moins une récompense à choix entre 8:00 et 18:00. Aucun coffre commun n'apparaît après 18:00 ni dans l'espace du boss.
- En mode Infini, chaque acte terminé donne un coffre d’acte garanti (§2.3).

### 14.5 Bonus de run

Les récompenses hors montée de niveau sont classées en quatre catégories, afin que leur durée et leur cumul soient prévisibles.

| Catégorie | Exemples proposés | Durée | Règle de cumul |
|---|---|---|---|
| **Instantané** | Fiole de soin (+25 % des PV max), aimant à Pigment (30 m), éclat de Pigment doublé | Instantanée | Le soin ne dépasse pas les PV max ; au plus un ramassable de soin par minute |
| **Temporaire** | Cadence d’encre (+20 % pendant 12 s), Armure d’encre (−20 % dégâts reçus pendant 10 s), Pigment éclatant (+50 % pendant 15 s) | 10–15 s | Même effet renouvelable mais non empilable ; au plus deux effets temporaires différents actifs |
| **De run** | Cartes génériques et bonus d’interactifs | Jusqu’à la fin de la run | Plafond par statistique, voir §9.4 |
| **Évolution** | Transformation d’un Instrument | Jusqu’à la fin de la run | Une évolution par Instrument ; trois évolutions maximum par run, selon les paliers de niveau |

**Bonus génériques de run proposés :**

| Bonus | Par carte | Plafond |
|---|---:|---:|
| Points de vie max | +10 % | 5 cartes |
| Dégâts | +5 % | 5 cartes |
| Cadence | +4 % | 5 cartes |
| Rayon de ramassage | +15 % | 4 cartes |
| Chance de critique | +3 points de pourcentage | 5 cartes |
| Pigment collecté | +8 % | 4 cartes |
| Réduction de dégâts | +3 % | 5 cartes, soit 15 % maximum |

Les bonus temporaires affichent une icône et un anneau de durée dans le HUD. Leur durée repart à zéro en cas de renouvellement ; ils ne peuvent pas être prolongés au-delà de la durée prévue par accumulation. Le Sablier du Dernier Regard ne modifie que les statuts appliqués aux ennemis, pas les bonus du joueur.

**Vitesse :** tous les bonus et Reliques qui modifient la vitesse utilisent l’unique vitesse de déplacement du personnage, avec un plafond global proposé de +10 % (§3.2). Aucun bonus n’ajoute une commande marche/course ou sprint.

**Obtention :** niveaux (cartes, Instruments, Reliques), coffres (équipement, bonus de run, soutien), mini-boss (Sceau et Relique), événements et vases (temporaires), ramassables (instantanés), interactifs (bonus de run), contrats (coffre doré). Les valeurs sont des bases de test, non des valeurs définitivement équilibrées.

---

## 15. Progression permanente, archives et statistiques

### 15.1 Séparer run et méta-progression

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

### 15.2 Récompenses et déblocages permanents

- Victoires et défaites peuvent donner une ressource permanente selon la durée ou les objectifs atteints. Nom provisoire : **Fragments de Toile**.
- Usages possibles : débloquer des personnages, ajouter des objets aux pools de récompenses, acheter de petits bonus, compléter les Archives.
- Le nom est provisoire ; les gains ci-dessous sont des valeurs de départ, et les coûts et valeurs finales des retouches restent à définir (§26.2).
- Une même récompense permanente n'est jamais attribuée deux fois, y compris après une défaite.
- Les conditions de déblocage sont visibles dans les Archives.

Chaque déblocage a : un identifiant stable, une condition, une récompense, une progression visible, une notification et une règle de sauvegarde.

**Gains de départ de Fragments de Toile :**

| Source | Gain |
|---|---|
| Minute survécue | 1 |
| Rencontre spéciale terminée | 3 |
| Mini-boss vaincu | 10 |
| Boss vaincu | 25 |
| Contrat réussi | 2 |
| Première découverte (Archives) | 5 |

**Exemples de gains à tester** (avec les valeurs ci-dessus et les multiplicateurs de fin de run) : victoire Normal d’environ 24 minutes avec trois rencontres majeures, deux mini-boss et deux contrats : environ 123 Fragments ; défaite à 12 minutes : environ 25 ; défaite à 18 minutes : environ 51. L’Infini doit rester moins rentable par heure que la victoire Normal ; cible indicative de test : environ 100 Fragments/heure en Infini contre environ 300/heure pour un Normal gagné. Ces chiffres doivent être recalculés après validation des sources de récompenses et leur équilibrage.

**Coûts provisoires des Retouches :** dix Retouches de trois rangs à 30 / 60 / 120 Fragments, soit 2 100 Fragments pour tout acheter. À environ 60 Fragments par run en moyenne, la complétion totale représenterait environ 30 à 45 runs. Un joueur sans Retouche doit pouvoir gagner.

**Déblocages de personnages proposés :** Graveur — atteindre le niveau 15 ; Alchimiste — appliquer 500 statuts cumulés ; Sculpteur — vaincre trois mini-boss cumulés ; Virtuose — vaincre le boss une fois ; Restauratrice — vaincre le boss avec deux personnages. Ordre et difficulté à tester ; le premier déblocage est visé après 3 à 5 runs.

Le total de la run est ensuite multiplié selon l'issue (§2.5). Objectif de test : le premier déblocage de personnage arrive en 3 à 5 runs.

**Déblocages par accomplissements.** Les personnages, Instruments, Reliques et cartes se débloquent par des accomplissements (§15.3), pas par achat.

**Retouches.** Les Fragments de Toile s'échangent contre une dizaine de petites améliorations permanentes, chacune avec plusieurs rangs et un plafond. Ordres de grandeur à tester : +10 % de points de vie maximum, +10 % de rayon de ramassage, une relance du choix de niveau par run, un bannissement d'option par run (§9.2).

**Musée à restaurer (Futur).** Le menu principal montre un musée qui se restaure visuellement à mesure des déblocages ; cet effet est purement cosmétique.

**Équilibrage.** Les bonus permanents de puissance restent **modestes**, pour préserver l'importance des décisions de la run et ne pas rendre les premières difficultés triviales. Prioriser l'ouverture de nouvelles options de construction (personnages, Instruments, Reliques, évolutions, défis, éléments de lore) plutôt que des augmentations de dégâts permanentes.

### 15.3 Défis permanents

Les défis permanents sont suivis entre les parties. Le MVP n'en a pas besoin (Futur).

- **Catégories :** découverte, maîtrise, boss, Instruments (rang maximum, évolutions), cumul.
- **Suivi :** chaque défi a un identifiant, une progression, un état « terminé » et un état « récompense reçue », enregistrés dans la sauvegarde (§19.6).
- **Attribution :** une notification apparaît pendant la run quand un défi est terminé, mais la récompense n'est validée **qu'au bilan de run**, une seule fois.
- **Récompenses :** déblocages (personnages, Instruments, Reliques, cartes), Fragments de Toile et éléments cosmétiques.

Exemples de défis de déblocage de personnages (à valider) :

| Personnage | Défi |
|---|---|
| La Virtuose | Vaincre le boss une fois |
| Le Graveur | Atteindre le niveau 20 |
| L'Alchimiste | Appliquer 500 statuts cumulés |
| Le Sculpteur | Vaincre 3 mini-boss cumulés |
| La Restauratrice | Vaincre le boss avec deux personnages différents |

### 15.4 Archives

Les Archives sont accessibles par « Le catalogue » (§17.3).

- **Onglets :** Personnages, Instruments (et leurs évolutions), Reliques, Ennemis (ennemis communs, mini-boss et boss), Synergies, Cartes, Statistiques.
- **États d'un contenu :** *inconnu* (« ??? » avec sa condition de déblocage), *découvert* (description et notes), *maîtrisé* (critère propre à chaque contenu, par exemple un Instrument porté à son rang maximum).
- Une évolution reste une silhouette jusqu'à sa première obtention, pour ne pas dévoiler la surprise.
- Les découvertes sont enregistrées à la fin de la run (§19.6).
- Pour le MVP : une liste simple des découvertes, sans onglets.

### 15.5 Statistiques

- **Par run :** le bilan de run (§17.7).
- **Globales (Futur) :** meilleur temps de survie par mode, ennemis éliminés au total, boss vaincus (par boss), runs jouées et victoires, plus gros coup, niveau maximum atteint, défis réussis, records du mode Infini (actes et temps).
- **Détail (Futur) :** par personnage et par Instrument (temps équipé, dégâts, victoires).
- **Technique :** les compteurs sont accumulés en mémoire pendant la run et écrits à la fin, sans allocation à chaque coup, pour rester compatible avec les hordes (chapitre 20).

### 15.6 Fin de jeu

Quand le joueur a débloqué tous les personnages et terminé toutes les cartes :

- le mode Infini et ses records (§2.3) ;
- des défis de maîtrise : gagner avec chaque personnage, obtenir chaque évolution, remplir le catalogue à 100 % ;
- des éléments cosmétiques : cadres de portrait et titres ;
- des rangs de maîtrise par personnage, purement cosmétiques ;
- les Fragments restants continuent de servir aux retouches (§15.2).

---

## 16. Direction artistique, effets et audio

### 16.1 Direction artistique

Palette principale : noir d'encre, charbon, ivoire, toile vieillie et rouge vermillon. Les autres couleurs de pigments sont réservées aux effets de gameplay et aux objets particuliers. La palette et les matériaux de l'interface sont détaillés au §17.2.

Privilégier les silhouettes lisibles, les modèles stylisés réutilisables, les effets reconnaissables et les télégraphes de boss qui ne se confondent pas avec les attaques du joueur. Réutiliser matériaux, accessoires, éclairages et éléments de décor d'une carte à l'autre.

### 16.2 Effets

**Effets prioritaires de la V1 :**

- impact d'arme ;
- projectile ;
- zone dangereuse ;
- collecte de Pigment ;
- montée de niveau ;
- télégraphe de boss ;
- dégâts reçus et mort.

**Effets futurs :** évolutions d'Instruments, Fractures, réactions complexes entre pigments, effets propres aux personnages, transformations visuelles de carte.

Règle de priorité : les signaux essentiels d'une attaque dangereuse passent avant tout effet cosmétique. Les contraintes de rendu et de performance sont aux §10.6 et §20.6.

### 16.3 Audio

**Audio de la V1 :** sons de coups, projectiles, dégâts reçus, collecte, montée de niveau, signal d'attaque du boss, musique d'ambiance et de boss, victoire et défaite. Les sons restent lisibles sans saturation pendant les hordes.

---

## 17. Interface et expérience du joueur

### 17.1 Intention visuelle et principes

L'interface évoque un **atelier de restauration interdit** : une toile encore humide, des pigments qui réagissent à la présence du joueur, des cadres fendus, du papier ancien et de l'encre vivante. Elle est organique, élégante et inquiétante, mais les informations utiles priment sur la décoration.

Principes directeurs :

1. **Lisibilité avant ornement.** Textures et animations ne masquent jamais une valeur, une icône, un télégraphe ennemi ou le personnage.
2. **Une cause visuelle identifiable.** Toute éclaboussure ou déformation importante est liée à un événement identifiable : dégâts reçus, montée de niveau, choix confirmé, arrivée d'un boss, déblocage.
3. **Un langage cohérent.** Les mêmes formes, couleurs et animations gardent le même sens dans tous les menus.
4. **Une intensité contextuelle.** L'interface peut s'agiter quand la menace augmente, puis se calmer dès que la pression retombe.
5. **Séparation entre décor et gameplay.** Les effets décoratifs de l'interface ne se confondent pas avec les attaques, zones dangereuses ou projectiles du jeu.

Priorité de production : **fonctionnalité et lisibilité d'abord**, puis identité « atelier de restauration interdit » par couches. Les éléments décoratifs ne retardent pas la validation du déplacement, du saut, de l'esquive, du combat, de la progression, de la pause et des écrans de fin (ordre de production au §24.3).

### 17.2 Palette et matériaux

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

### 17.3 Menu principal et navigation

Le menu principal ressemble à une toile encore vierge dans un atelier inquiétant. Le personnage peut y apparaître, avec un lent écoulement d'encre. L'ambiance est contemplative et discrète ; les animations ne retardent pas l'accès au jeu.

Entrées :

- **Nouvelle toile :** lancer une partie.
- **Les artistes :** sélectionner un personnage et consulter son passif ; les personnages verrouillés indiquent leur condition de déblocage.
- **Le catalogue :** consulter les Archives (Instruments, évolutions, Reliques et ennemis découverts).
- **Options :** cinq onglets — Jeu, Vidéo, Commandes, Audio et Effets (§18).
- **Quitter :** fermer le jeu.

Toute option non implémentée est masquée ou clairement indiquée comme indisponible. Au clic ou au survol, une trace de pinceau temporaire peut souligner le cadre ; la sélection reste clairement visible à la souris, au clavier et à la manette. Les transitions sont courtes et interrompables : le joueur n'attend jamais une longue animation pour confirmer ou annuler.

### 17.4 HUD en jeu

Le HUD reste lisible pendant les combats denses sans prendre inutilement de place.

**À afficher en permanence :**

- santé actuelle et maximale, bouclier éventuel, effets négatifs actifs ;
- niveau et jauge de Pigment ;
- chronomètre de run ;
- icônes d'Instruments (avec rangs et état d'évolution) et de Reliques équipés ;
- état de recharge de l'esquive, immédiatement compréhensible ;
- santé du boss pendant son combat, avec nom et indication de phase si cela améliore la lecture ;
- compteur du contrat actif, le cas échéant (§14.3) ;
- marqueurs d'objectifs en bord d'écran (§13.4) ;
- avertissements de danger, près du personnage ou dans le monde.

Les temps de recharge et jauges d'Instrument ne sont affichés que s'ils servent à une décision. Éviter les chiffres permanents pour chaque dégât s'ils masquent le combat.

**Organisation recommandée (à tester en résolution cible) :**

- **Bas de l'écran :** grande jauge de Pigment dessinée comme un trait d'encre liquide ; niveau du joueur à proximité.
- **Haut gauche :** santé, bouclier éventuel, effets négatifs ; Instruments équipés, rangs et état d'évolution dans la zone adjacente.
- **Haut droit :** Reliques et passifs actifs, en icônes compactes.
- **Haut centre :** chronomètre, phase ou vague, annonces de boss.
- **Sous le chronomètre ou en haut à droite :** niveau de menace et objectif actif, si la phase en cours en a besoin.
- **Boss :** barre de santé dédiée.
- **Bas droit :** mini-carte (§13.4), assez petite pour ne pas masquer l'action ; emplacement à tester en résolution cible.

**Feedback de danger.** Quand le joueur subit des dégâts importants, un débordement vermillon bref peut apparaître sur les bords de l'écran ; il peut devenir plus marqué à faible santé mais ne masque jamais le centre de l'action. Ordre de priorité des alertes :

1. attaque dangereuse imminente ;
2. dégâts reçus et état critique du joueur ;
3. arrivée ou changement de phase d'un boss ;
4. gain de niveau et choix disponible ;
5. récompenses et informations secondaires.

Les animations décoratives cèdent la place aux alertes de gameplay.

Lors du premier lancement, ou dans un écran d'aide, afficher les commandes de saut et d'esquive, tant que l'option « Rappel des commandes à l'écran » est activée (§18). Distinguer par des télégraphes cohérents une attaque évitable par le saut d'une attaque évitable par l'esquive.

### 17.5 Choix d'amélioration

Le jeu est en **pause réelle** et présente trois cartes (ou le nombre d'options valides disponibles, §9.2). Chaque carte affiche :

- nom et icône ;
- catégorie (Instrument, Relique, bonus ou évolution) et élément concerné ;
- rang actuel et nouveau rang, si applicable ;
- rareté (couleur, nom et icône) pour les améliorations statistiques d'Instrument ;
- description courte et concrète ;
- bonus chiffré et, si possible, valeurs avant/après (qui correspondent au résultat réel) ;
- conditions manquantes si une évolution est verrouillée.

La rareté n'annonce jamais une nouvelle mécanique : une carte légendaire qui ne fait qu'augmenter les dégâts ne suggère ni nouvelle attaque ni transformation.

Les cartes apparaissent par traits de pinceau ; le survol peut révéler un léger relief. Après confirmation, la carte est absorbée par l'encre, l'effet est appliqué **une seule fois**, puis le jeu reprend avec un feedback bref. Le curseur est visible et déverrouillé. Jamais d'amélioration invalide à l'écran.

### 17.6 Écran d'évolution

La rareté décrit la valeur d'un bonus statistique ; **une évolution est une transformation mécanique dédiée**. Elle possède un nom distinct, une illustration ou un aperçu distinct, une description de la nouvelle mécanique, des prérequis affichés et ses synergies principales lorsqu'elles sont connues. L'interface ne fait jamais passer une amélioration statistique rare pour une évolution.

Parcours (conditions au §6.6) :

1. annoncer qu'une possibilité d'évolution est disponible ;
2. montrer les Instruments éligibles et leurs prérequis, et expliquer pourquoi un Instrument ne l'est pas ;
3. permettre de choisir un Instrument ;
4. présenter au moins deux branches distinctes pour cet Instrument ;
5. afficher clairement les effets et synergies de chaque branche ;
6. demander une confirmation unique avant d'appliquer la transformation.

Si aucun Instrument n'est éligible au palier atteint, le joueur reçoit une récompense alternative ; le Sceau n'est pas consommé sans transformation obtenue.

### 17.7 Pause, victoire, défaite et bilan

**Pause.** L'écran apparaît comme une feuille sombre posée sur la toile ; **Reprendre** est l'action la plus visible. Échap ouvre et ferme le menu. Le jeu n'inflige aucun dégât et ne fait pas progresser le combat pendant la pause. Le curseur est visible et déverrouillé, et la caméra ne le recapture pas tant que le menu est ouvert. Toute sortie de run qui entraîne la perte d'une progression temporaire demande une confirmation ; les boutons désactivés ont une explication. L'option « Abandonner la run » (avec confirmation) mène au bilan de run (§2.5).

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

Les commandes **Recommencer** et **Retour au menu** fonctionnent et réinitialisent l'état temporaire de la run (§2.5).

### 17.8 Jauges et feedbacks visuels

- **Santé :** trait d'encre dans une forme lisible ; les variations restent faciles à estimer.
- **Expérience :** le Pigment se répand dans la jauge jusqu'au prochain niveau.
- **Menace :** un cadre peut se fissurer ou se tendre à mesure que la pression monte, sans devenir difficile à lire.
- **Récompense importante :** accent de pigment doré, réservé aux événements significatifs.
- **Acquisition de Relique :** tampon visuel bref, avec le nom et l'effet.
- **Dégâts reçus :** flash HUD bref, sans flash blanc plein écran obligatoire.

Chaque animation communique une information identifiable et reste secondaire par rapport aux télégraphes ennemis.

### 17.9 Micro-interactions

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

### 17.10 Apprentissage

Il n'y a pas de tutoriel séparé : l'apprentissage prolonge la minute d'introduction intégrée à l'action (§12.1). Chaque conseil :

- apparaît **une seule fois**, déclenché par la situation ;
- est non bloquant : il ne met pas le jeu en pause et n'interrompt pas le combat ;
- se retrouve ensuite dans l'écran d'aide.

| Conseil | Déclencheur |
|---|---|
| Déplacement et caméra | Dans les 10 premières secondes |
| Saut | Premier obstacle bas sur le parcours |
| Esquive | Première attaque télégraphée |
| Cartes de niveau | Premier choix d'amélioration |
| Évolution | Obtention du premier Sceau d'Évolution |
| Surfaces de peinture | Première apparition d'une surface |

Les conseils de commandes (déplacement, saut, esquive) dépendent de l'option « Rappel des commandes à l'écran » (§18) et affichent les touches du périphérique utilisé. Les astuces du menu sont distinctes : ce sont de courts conseils affichés dans les menus. Le MVP implémente au minimum les trois premiers conseils. Une salle d'entraînement dans le musée (mannequins et attaques télégraphées) est une piste Futur.

### 17.11 Accessibilité et ergonomie

À prévoir dès le prototype :

- texte de taille suffisante, contraste contrôlé et lisibilité à la résolution cible, sans option de réglage de la taille de l'interface, de la taille du texte ni du contraste ;
- icônes accompagnées de libellés ou d'infobulles utiles ;
- **information jamais transmise par la couleur seule** (rareté, statut, danger) ; les télégraphes ennemis ont des formes et animations distinctes ;
- réglages de sensibilité caméra, volume musique et effets sonores, tremblement de caméra, flashs, éclaboussures, particules, nombres flottants et intensité des effets plein écran ;
- navigation complète au clavier et à la manette ;
- pause réelle pendant les choix d'amélioration ;
- comparaison explicite des statistiques avant/après ;
- commandes affichées selon le périphérique utilisé, si possible ;
- joueur toujours visible dans les effets de foule.

Les réglages correspondants sont regroupés dans le menu Options (§18). Les options d'accessibilité modifient réellement le comportement visuel concerné : une option affichée mais non fonctionnelle n'est pas présentée comme disponible. Les effets de peinture sont testés sur un écran chargé pour vérifier que le personnage et les attaques ennemies restent visibles.

### 17.12 Mise en œuvre technique de l'interface (Unity 6 URP)

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

## 18. Menu Options

Le menu Options est accessible depuis le menu principal (§17.3) et depuis le menu pause ; le jeu reste en pause pendant qu'il est ouvert. Il comporte **cinq onglets : Jeu, Vidéo, Commandes, Audio et Effets**. Il n'y a **aucune option de difficulté** : le jeu propose une seule difficulté progressive (§12.4).

### 18.1 Règles communes

- Une option affichée fonctionne réellement. Une option marquée **Futur** est masquée ou indiquée comme indisponible (§17.3).
- Les réglages de Vidéo et d'Effets ne suppriment jamais l'information de gameplay : les télégraphes d'attaque et les surfaces de peinture de gameplay restent visibles à tous les réglages ; seules les décorations sont réduites (§10.6, §20.6).
- Les réglages sont enregistrés avec les paramètres, séparément de la progression permanente (§19.6), et conservés d'une session à l'autre.
- Un changement de Vidéo se valide par « Appliquer ». Sans confirmation après quelques secondes, le jeu revient automatiquement à l'ancien réglage, pour ne jamais bloquer le joueur sur une résolution invalide.
- Les valeurs par défaut sont des valeurs de départ à tester.

### 18.2 Onglet Jeu

| Option | Valeurs | Défaut | Phase |
|---|---|---|---|
| Langue | Français ; autres langues ajoutées plus tard | Français | Futur |
| Afficher les dégâts | Désactivé / Critiques seulement / Tous | Critiques seulement | V1 |
| Afficher les dégâts sur la durée | Oui / Non | Non | V1 |
| Vignette de dégâts | Oui / Non | Oui | V1 |
| Montrer les astuces dans le menu | Oui / Non | Oui | V1 |
| Rappel des commandes à l'écran | Oui / Non | Oui | V1 |
| Afficher la mini-carte | Oui / Non | Oui | V1 |
| Marqueurs d'objectifs | Oui / Non | Oui | V1 |

### 18.3 Onglet Vidéo

| Option | Valeurs | Défaut | Phase |
|---|---|---|---|
| Moniteur | Liste des écrans détectés | Écran principal | V1 |
| Résolution | Liste des résolutions prises en charge | Résolution du bureau | V1 |
| Mode d'affichage | Plein écran / Fenêtré sans bordure / Fenêtré | Plein écran | V1 |
| Synchronisation verticale | Oui / Non | Oui | V1 |
| Limite d'images par seconde | 30 / 60 / 120 / 144 / Illimitée | 60 | V1 |
| Champ de vision (FOV) | Plage bornée autour de la valeur de référence de la caméra | Valeur de référence | V1 |
| Anti-crénelage | Désactivé / FXAA / MSAA 2x / MSAA 4x | FXAA | V1 |
| Qualité des ombres | Désactivées / Basse / Moyenne / Élevée | Moyenne | V1 |
| Post-traitement (lueur, vignettage d'objectif, etc.) | Désactivé / Réduit / Complet | Complet | V1 |

### 18.4 Onglet Commandes

| Option | Valeurs | Défaut | Phase |
|---|---|---|---|
| Sensibilité de la caméra à la souris | Curseur | Moyenne | V1 |
| Sensibilité de la caméra à la manette | Curseur | Moyenne | V1 |
| Inverser l'axe vertical | Oui / Non | Non | V1 |
| Inverser l'axe horizontal | Oui / Non | Non | V1 |
| Remappage des touches et boutons du clavier | Par action, avec détection des conflits | Commandes par défaut (§3.1) | Futur |
| Remappage des touches et boutons de la manette | Par action, avec détection des conflits | Commandes par défaut (§3.1) | Futur |
| Rétablir les commandes par défaut | Bouton, avec confirmation | — | Futur |

### 18.5 Onglet Audio

| Option | Valeurs | Défaut | Phase |
|---|---|---|---|
| Volume principal | 0 % à 100 % | 80 % | V1 |
| Volume de la musique | 0 % à 100 % | 70 % | V1 |
| Volume des effets sonores | 0 % à 100 % | 80 % | V1 |

### 18.6 Onglet Effets

| Option | Valeurs | Défaut | Phase |
|---|---|---|---|
| Tremblement de caméra | 0 % à 100 % | 100 % | V1 |
| Flashs d'écran et débordement | Désactivé / Réduit / Complet | Complet | V1 |
| Effets d'encre plein écran et transitions | Désactivés / Réduits / Complets | Complets | V1 |
| Éclaboussures et particules décoratives | Minimales / Réduites / Complètes | Complètes | V1 |
| Nombres de dégâts flottants | Réduits (fixes) / Complets (animés) | Complets | V1 |
| Animations d'interface | Réduites / Complètes | Complètes | V1 |
| Télégraphes d'attaque à contraste renforcé | Oui / Non | Non | V1 |
| Flou de mouvement | Oui / Non | Non | Futur |

### 18.7 Précisions pour éviter les recoupements

- **Dégâts affichés :** « Afficher les dégâts » et « Afficher les dégâts sur la durée » décident *si* les nombres apparaissent ; « Nombres de dégâts flottants » (Effets) règle seulement leur animation.
- **Vignette de dégâts :** l'option de l'onglet Jeu active l'indicateur de dégâts reçus sur les bords de l'écran (§17.4). « Flashs d'écran et débordement » (Effets) règle l'intensité des flashs et de ce débordement. Si la vignette est désactivée, seuls les flashs restent réglables. Elle est distincte du vignettage d'objectif du post-traitement.
- **Rappel des commandes :** affiche les commandes de saut et d'esquive du périphérique utilisé (clavier ou manette), au premier lancement et dans l'écran d'aide (§17.4).
- **Mini-carte et marqueurs :** « Afficher la mini-carte » et « Marqueurs d'objectifs » (§13.4) se règlent séparément.
- **Tremblement de caméra :** un seul réglage, dans Effets.
- **Champ de vision :** un seul réglage, dans Vidéo.
- **Sensibilité :** un curseur par périphérique ; le rapport entre axes horizontal et vertical est fixé dans la configuration de la caméra (§3.5).
- **Inversion des axes :** s'applique à la souris et à la manette.
- **Remappage :** l'action Pause ne peut jamais rester sans touche assignée.

---

## 19. Architecture technique Unity 6 URP et sauvegarde

### 19.1 Principes

- Séparer **données, logique de gameplay, présentation et sauvegarde**.
- Les fiches de contenu sont configurables sans modifier la logique centrale.
- Les systèmes centraux ne référencent pas directement des objets de scène temporaires après la fin de leur cycle de vie.
- Les règles de dégâts, de rareté et de progression sont testables de manière isolée.
- Les événements sont désabonnés correctement à la destruction ou à la réutilisation des objets.
- Ne pas créer un système générique complexe tant que le prototype n'en a pas besoin.
- Garder les comportements dans des composants C# faciles à comprendre. Les responsabilités du §19.3 ne sont pas une obligation de créer immédiatement une classe par ligne.

À éviter : un script géant, les recherches d'objets répétées à chaque image sans nécessité, les valeurs d'équilibrage dispersées, les références Inspector manquantes, les `GameManager` en double, et les changements de noms de champs sérialisés qui cassent les références.

### 19.2 Organisation des dossiers

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
      Sandbox/            (TestMap_EnemySandbox, voir §20.7)
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

### 19.3 Responsabilités des systèmes

| Système | Responsabilité |
|---|---|
| GameManager / RunController (RunState) | État général de la run, pause, transitions, victoire, défaite, redémarrage |
| RunManager | Durée, phases et progression temporelle |
| PlayerController | Entrées, déplacement, orientation, saut, esquive |
| CameraController | Rotation, suivi, collision caméra |
| Health / DamageReceiver | Santé, réception des dégâts, invulnérabilité, mort |
| DamageResolver | Résolution des dégâts, critiques, résistances et sources (§4.2) |
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
| EnemySimulationManager | Mise à jour centralisée des ennemis actifs (§20.2) |
| EnemyBrain (EnemyAI) | Comportement individuel d'un ennemi |
| BossController | Phases, télégraphes, attaques, transitions et état du boss |
| SaveService (SaveSystem) | Lecture et écriture de la progression permanente |
| UI Presenter (UIManager) | HUD, pause, menus et écrans de fin ; affiche les données sans posséder les règles de gameplay |

Les noms sont indicatifs. Avant d'implémenter, inspecter les scripts réels et préserver les noms et références Inspector existants lorsque c'est nécessaire.

### 19.4 Données configurables

Utiliser des **ScriptableObjects** pour les définitions statiques lorsque c'est utile : statistiques de base, icône, prefab, statistiques éligibles, poids de récompense, prérequis. Les valeurs qui changent pendant la run restent dans des états d'exécution séparés : ne jamais modifier l'asset de définition pour stocker l'état d'une run. Pour les personnages, voir aussi le §5.3.

### 19.5 Pooling et performances

Le pooling peut être utilisé pour projectiles, impacts, ennemis communs, objets de Pigment et effets de surface si les tests montrent son utilité. Tout objet réutilisé réinitialise santé, cible, minuteries, propriétaire, statuts, vitesse et abonnements aux événements.

Ne pas fixer d'objectif de FPS irréaliste sans connaître le matériel cible. Baseline à tester : **60 FPS** sur le PC de référence choisi, dans une scène avec le nombre cible d'ennemis, de projectiles et de surfaces. Le matériel minimum est défini avant la sortie publique (§26.2).

### 19.6 Règles de sauvegarde

- Format **versionné**, avec un numéro de version de schéma et une migration simple dès le début.
- Écrire la progression permanente après une fin de run, après chaque déblocage important et à la fermeture du jeu, **jamais en pleine run**.
- Vérifier les données lues avant de les appliquer ; prévoir des valeurs par défaut si la sauvegarde manque ou si un champ est absent ou inconnu.
- Écriture **atomique** : écrire dans un fichier temporaire, puis le renommer. Conserver une copie de secours tournante des trois dernières sauvegardes valides.
- Ajouter une **somme de contrôle** pour détecter une corruption accidentelle (ce n'est pas un système anti-triche). En cas de fichier corrompu : charger la copie de secours la plus récente et afficher un message compréhensible.
- Ne jamais effacer silencieusement une sauvegarde sans action explicite du joueur.
- Séparer les données de progression des paramètres audio/vidéo lorsque cela simplifie la maintenance.
- Ne pas enregistrer de références Unity brutes comme identifiants persistants.
- Chaque récompense permanente a un identifiant et un état « reçue » : elle ne peut pas être attribuée deux fois, même après un crash ou une relecture du bilan.
- Dépenser des Fragments et appliquer la retouche achetée se fait en **une seule écriture**.
- Prévoir une sauvegarde de développement réinitialisable pour tester les déblocages.
- La sauvegarde complète est requise avant la première version publique ; elle peut être différée pendant le prototype.

---

## 20. Hordes, simulation et performance

### 20.1 Objectif

Construire une base capable de gérer une horde importante dans Unity 6 URP tout en préservant :

- des déplacements lisibles et réactifs du joueur ;
- des ennemis qui suivent le joueur sans traverser les murs ;
- des collisions stables et des interactions de combat cohérentes ;
- des animations dont le coût dépend de la distance et de l'importance de l'ennemi ;
- une fréquence d'images aussi stable que possible ;
- une architecture accessible à un développeur débutant et extensible après le prototype.

**Le nombre de 1 000 ennemis est une valeur de test de charge, pas une densité de jeu.** Le jeu n'est pas conçu pour afficher normalement 1 000 ennemis simultanés. Ce nombre sert à mesurer les limites, détecter les pics de coût et vérifier que les systèmes ne se dégradent pas brutalement. Le nombre réel de la horde est défini après les tests de lisibilité, de gameplay et de performances, et aucune capacité de 1 000 ennemis n'est annoncée avant mesure sur la machine cible.

### 20.2 Architecture de simulation

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

### 20.3 Déplacement du joueur et physique des ennemis

**Joueur.** Il utilise initialement un `CharacterController` pour un contrôle direct du déplacement, du saut et de l'esquive. Les ennemis ne peuvent pas le pousser ou le bloquer de façon imprévisible par accumulation de forces physiques.

**Ennemis ordinaires.** Ne pas imposer un `Rigidbody` dynamique à chacun juste pour rendre la foule « physique ».

- Mouvement cinématique ou contrôlé pour les ennemis qui poursuivent le joueur.
- Colliders simples pour empêcher la traversée des murs et détecter les interactions nécessaires.
- `Rigidbody` dynamique seulement quand la simulation physique est utile (recul, chute, impulsion, mort physique).
- Désactiver ou simplifier les interactions physiques qui ne contribuent pas au gameplay.
- Les ennemis ordinaires n'entrent pas nécessairement en collision physique entre eux : leur espacement est géré par évitement local et règles de séparation.

**Boss, mini-boss et réactions importantes.** Ils peuvent utiliser un mouvement et des collisions plus riches, car leur lecture et leur précision priment, tout en restant soumis aux mêmes exigences de stabilité et de profilage.

**Cohérence avec la physique Unity.** Ne pas déplacer un `Rigidbody` dynamique en écrivant directement sa position à chaque image. Choisir une méthode de mouvement compatible avec son mode physique et respecter la séparation `Update` / `FixedUpdate`. La fréquence de l'IA et celle de la physique peuvent différer, mais leurs échanges de données restent cohérents.

### 20.4 Navigation

La poursuite directe par le vecteur « ennemi vers joueur » ne suffit pas : elle est complétée par une navigation qui comprend les obstacles.

**Solution recommandée pour la horde :** une grille de navigation ou un **champ de direction partagé**.

1. Représenter les zones praticables et les obstacles fixes.
2. Calculer une destination ou un champ de coût depuis la position du joueur.
3. Laisser de nombreux ennemis consulter une direction commune selon leur cellule.
4. Actualiser le champ à fréquence contrôlée quand le joueur change de zone.
5. Compléter la direction globale par un évitement local (piliers, angles, autres ennemis).

Cette approche évite de calculer un chemin complet par ennemi et convient à une arène dont la destination principale est le joueur. Sur une grande carte (§13.1), le champ n'est calculé que dans une zone active autour du joueur, pas sur toute la carte.

**Alternative de prototype :** un `NavMeshAgent` valide rapidement la navigation avec quelques ennemis. Ne pas supposer que 1 000 agents avec recalculs fréquents seront performants : mesurer avant de choisir.

**Évitement local.** Raycasts, sondes ou steering aident à contourner un obstacle proche mais ne remplacent pas une navigation globale (échec possible dans les culs-de-sac ou contre un mur qui bloque tout). La solution garantit :

- aucune traversée des murs statiques ;
- pas de téléportation de correction pour sortir d'un obstacle ;
- un comportement raisonnable dans les couloirs et autour des piliers ;
- une récupération contrôlée si un ennemi reste bloqué ;
- la distinction entre collision de décor, séparation de foule et collision de combat.

Murs et piliers utilisent des colliders adaptés ; les couches de collision sont configurées explicitement.

### 20.5 Animation adaptative à la distance

La distance au joueur choisit **la qualité et la fréquence de la représentation visuelle**, jamais les règles de gameplay. Seuils de départ :

| Niveau | Distance indicative | Animation recommandée |
|---|---:|---|
| LOD 0 — proche | 0 à 12 m | Animation complète, transitions fluides, effets et réactions détaillées |
| LOD 1 — moyenne | 12 à 28 m | Animation simplifiée, évaluation réduite, effets secondaires limités |
| LOD 2 — lointaine | 28 à 45 m | Animation très simplifiée ou poses pré-calculées, mise à jour visuelle moins fréquente |
| LOD 3 — très lointaine / hors écran | Plus de 45 m ou non visible durablement | Pas d'animation détaillée ; pose ou animation à faible fréquence, voire présentation réduite si l'ennemi n'a pas besoin d'être rendu |

La visibilité, la taille projetée à l'écran, l'état de combat et le type d'ennemi peuvent modifier le niveau choisi.

**Priorités qui passent avant la distance :**

- boss et mini-boss : animations et télégraphes de combat lisibles ;
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

Les meshes pré-calculés sont une optimisation, pas une obligation ; leur coût mémoire, le nombre de poses, la qualité des transitions et le gain réel sont mesurés sur un ennemi test avant de construire une bibliothèque. Le choix final entre animation classique, poses pré-calculées et rendu instancié est pris après comparaison dans la test map (§20.7).

**Calcul du LOD :**

- éviter les calculs de distance coûteux et les recherches de composants répétées à chaque image ;
- comparer des distances au carré face aux seuils ;
- recalculer les niveaux à fréquence contrôlée, par groupes décalés dans le temps ;
- ajouter une marge de transition (hystérésis) pour éviter les allers-retours près d'un seuil ;
- envisager la distance à la caméra et la taille à l'écran en complément ;
- garder les seuils configurables par type d'ennemi, pas dispersés dans les scripts.

### 20.6 Rendu, pooling et effets

- **Pooling** des ennemis ordinaires, projectiles et effets fréquents. À la mort d'un ennemi, remettre à zéro états, événements, effets, santé, cibles et données de simulation avant réutilisation ; éviter les abonnements d'événements en double.
- Réutiliser matériaux et meshes ; utiliser LOD et GPU instancing quand ils sont compatibles.
- Limiter matériaux uniques, transparences coûteuses, ombres inutiles et effets superposés.
- Un ennemi éloigné n'utilise pas autant de particules et de détails qu'un ennemi au premier plan.
- Conserver une silhouette claire et des couleurs de danger lisibles (§16.1).
- Réduire ou supprimer les effets cosmétiques à distance sans supprimer les signaux essentiels d'une attaque dangereuse. Limiter les sons simultanés répétitifs de la foule.

### 20.7 Test map de performance

Une scène séparée, `TestMap_EnemySandbox`, contient :

- un sol plat et des limites solides ;
- des piliers, murs, angles et couloirs ;
- quelques rampes pour tester le joueur ;
- des points d'apparition configurables ;
- un affichage de diagnostic : ennemis actifs, niveau d'animation, temps de simulation ;
- un moyen de sélectionner rapidement une charge de test : 10, 50, 100, 250, 500 et 1 000 ennemis ;
- une variante à l'échelle d'une grande carte (§13.1), pour vérifier la navigation des ennemis, l'apparition hors caméra et la mini-carte sur de longues distances.

Les tests sont reproductibles : même scène, même configuration graphique, mêmes comportements, même matériel.

### 20.8 Méthode de profilage

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

## 21. Fiches de contenu normalisées

Chaque contenu a une fiche stable. Les identifiants internes restent en anglais ou en PascalCase, stables entre versions ; les noms affichés peuvent rester en français.

### 21.1 Fiche Personnage

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

### 21.2 Fiche Instrument

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

### 21.3 Fiche Évolution

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

### 21.4 Fiche Relique

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

### 21.5 Fiche Ennemi

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
- **Niveau d'animation par distance :** seuils spécifiques éventuels (§20.5).
- **Feedback visuel / audio :**
- **Critères de test :**

### 21.6 Fiche Boss

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

### 21.7 Exemple de fiche — Exilé

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

### 21.8 Exemple de fiche — Patineur (futur)

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

## 22. Tests et critères d'acceptation

### 22.1 Niveaux de test

- **EditMode :** formules de dégâts, probabilités, seuils d'expérience, règles de cumul, éligibilité.
- **PlayMode :** déplacement, esquive, combat, apparition des vagues, interfaces, transitions d'état.
- **Tests de scène :** une run complète avec un contenu représentatif.
- **Tests de régression :** rejouer les scénarios après chaque modification d'un système central.
- **Tests de performance :** densité maximale de combat, projectiles, surfaces et effets (§22.10).
- **Tests utilisateurs :** lisibilité, compréhension des cartes, rythme et difficulté perçue.

Les contrôles sont d'abord testés dans une scène dédiée, puis dans la carte réelle.

### 22.2 Critères d'acceptation et de sortie du MVP

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

### 22.3 Déplacement, saut, esquive et caméra

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

### 22.4 Combat et ennemis

- Chaque Instrument acquiert une cible valide ; cadence, portée et ligne de vue sont respectées.
- Dégâts normaux et critiques corrects ; un impact n'inflige les dégâts qu'une fois, même si un projectile touche plusieurs colliders d'un même ennemi.
- Une zone de dégâts applique sa fréquence configurée, pas des dégâts à chaque image.
- Une cible meurt exactement une fois, ne donne ses récompenses qu'une fois, et ne peut plus être ciblée ni recevoir de dégâts une fois morte.
- Attaque interrompue par la mort de sa source ; ennemi mort pendant un effet périodique.
- Statuts qui expirent ou se renouvellent ; dégâts de zone et chevauchement des zones.
- Immunité du boss aux contrôles non autorisés.
- Pas de propagation récursive infinie ; pas de projectile orphelin après redémarrage.
- Les apparitions ordinaires cessent au lancement du boss ; la défaite et la victoire ne se déclenchent qu'une fois ; le redémarrage ne laisse aucun état résiduel.

### 22.5 Progression

- Collecter du Pigment et monter plusieurs niveaux ; l'expérience excédentaire n'est pas perdue.
- Passage de niveau et pause ; trois cartes toutes valides ; aucun bonus appliqué deux fois.
- Emplacements pleins ; Instrument au rang maximum ; aucune option unique disponible.
- Paliers 10, 15 et 20 sans évolution éligible : récompense alternative.
- Rangs, prérequis d'évolution et Sceaux respectés ; Sceau conservé si aucune évolution n'est possible, consommé uniquement lors d'une évolution réussie.
- Une évolution n'est sélectionnée qu'une fois par Instrument.
- État propre après recommencer ; récompenses permanentes jamais attribuées en double.

### 22.6 Probabilités de rareté

Pour une table 60 / 25 / 12 / 3 :

- la somme des probabilités vaut 100 % ;
- chaque résultat possible peut être tiré ;
- une simulation automatisée d'au moins 100 000 tirages compare les fréquences observées aux fréquences attendues, avec une tolérance statistique définie ;
- les modificateurs de Chance respectent leurs plafonds ;
- la protection contre la malchance, si elle est activée, se réinitialise correctement ;
- la rareté ne modifie pas le comportement de l'arme.

Un test statistique vérifie le comportement sur un grand échantillon ; il ne garantit pas que chaque petite série reflète exactement les pourcentages.

### 22.7 Peinture au sol

- Surface créée puis supprimée à l'expiration ; limite du nombre de surfaces ; renouvellement.
- Joueur entrant et sortant rapidement ; ennemi restant dans une surface.
- Plusieurs colliders de la même cible.
- Deux pigments qui se chevauchent.
- Surface supprimée lorsque sa source disparaît.
- Une surface ne masque pas les télégraphes de boss.
- Pas de croissance illimitée des effets actifs.

### 22.8 Interface

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

**Menu Options**

- Chaque option modifie réellement le comportement concerné ; les options marquées Futur sont masquées ou indisponibles.
- Un réglage de Vidéo invalide ou non confirmé revient automatiquement à l'ancien réglage.
- Les réglages sont conservés après fermeture et relancement du jeu, indépendamment de la progression permanente.
- Aucun réglage d'Effets ou de Vidéo ne masque un télégraphe d'attaque ou une surface de peinture de gameplay.
- Quand le remappage est disponible : conflits détectés, action Pause toujours assignée, rétablissement des commandes par défaut avec confirmation.

### 22.9 Sauvegarde

- Première création de sauvegarde ; sauvegarde avec plusieurs déblocages.
- Fermeture et relancement : déblocages et paramètres persistent.
- Run temporaire non restaurée par erreur.
- Sauvegarde absente ; champ manquant ; version de schéma antérieure ou future.
- Données corrompues ; écriture interrompue.
- Coupure pendant l'écriture : la copie de secours est chargée au démarrage suivant.
- Somme de contrôle invalide : la copie de secours est chargée et un message s'affiche.
- Une récompense permanente n'est attribuée qu'une fois, même si le bilan est relu ou le jeu relancé juste après.
- Dépense de Fragments : retouche et solde enregistrés en une seule écriture.

### 22.10 Hordes et performances

**Critères de validation du prototype de foule :**

1. Le joueur se déplace, saute et esquive sans être immobilisé par la foule.
2. Les ennemis ne traversent pas les murs et contournent les obstacles de façon acceptable.
3. La navigation ne recalcule pas inutilement un chemin complet par ennemi et par image.
4. Le système central ajoute et retire des ennemis sans références invalides.
5. Le pooling fonctionne après de nombreux cycles apparition / mort / réapparition.
6. Les animations deviennent progressivement moins coûteuses avec la distance.
7. Les attaques importantes des boss et mini-boss restent lisibles.
8. Les ennemis éloignés continuent de participer correctement au gameplay.
9. Les tests de 10 à 1 000 ennemis permettent d'identifier le goulot d'étranglement.
10. Les résultats sont consignés avec la configuration matérielle et graphique utilisée.

**Scénarios de performance minimaux :** scène vide (référence), densité moyenne, densité maximale prévue, maximum de projectiles, maximum de surfaces de peinture, combat de boss avec effets, menu et écran de choix, transitions et nettoyage de fin de run.

Mesurer FPS, temps CPU et GPU, allocations mémoire par image et nombre d'objets actifs. Le seuil final est défini sur le PC de référence ; pour le prototype, viser 60 FPS et éviter les chutes prolongées sous 50 FPS sur la scène cible, puis ajuster selon le matériel choisi.

### 22.11 Gravité des bugs

- **Bloquant :** impossible de démarrer, terminer ou redémarrer une run.
- **Critique :** perte de sauvegarde, progression dupliquée, combat impossible ou crash.
- **Majeur :** mécanique centrale incorrecte, amélioration appliquée deux fois, boss bloqué.
- **Mineur :** problème visuel ou sonore sans impact majeur sur le gameplay.
- **Cosmétique :** alignement, animation ou texte sans effet fonctionnel.

Aucun bug bloquant ou critique connu n'est accepté pour une version publique. Les bugs majeurs sont corrigés ou acceptés explicitement avec un plan de correction.

### 22.12 Modes, récompenses et objectifs

- Le mode Infini n'est accessible qu'après une victoire en mode Normal ; il n'a ni boss programmé ni victoire ; les performances restent stables après 40 minutes (pas de croissance illimitée d'objets, d'ennemis ou de surfaces).
- Un coffre ne s'ouvre qu'une fois ; les offres sont valides ; avec des emplacements pleins, remplacer, refuser et la compensation fonctionnent.
- Un seul événement est actif à la fois ; la fréquence respecte les limites du §14.1 ; aucun événement après 18:00 ni pendant le boss ; chaque événement se nettoie à sa fin.
- Un seul contrat est actif ; un échec n'a aucun effet ; la récompense n'est attribuée qu'une fois.
- La mini-carte et les marqueurs indiquent des positions correctes, disparaissent quand l'objectif est accompli et respectent leurs options.
- Les Fragments sont attribués une seule fois selon l'issue (victoire, défaite, abandon) ; une run abandonnée est marquée comme telle.
- Les défis permanents progressent, persistent entre deux sessions et ne valident leur récompense qu'au bilan.
- Les découvertes des Archives persistent ; une évolution non obtenue reste une silhouette.
- Chaque conseil du tutoriel n'apparaît qu'une fois et ne bloque pas le combat.

---

## 23. Équilibrage

### 23.1 Mesures à collecter

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

### 23.2 Procédure d'itération

1. Définir une hypothèse précise (par exemple « les attaques à distance sont trop sûres »).
2. Modifier un petit nombre de paramètres liés à cette hypothèse.
3. Exécuter les mêmes scénarios de test.
4. Comparer les métriques et observer des parties réelles.
5. Conserver, annuler ou ajuster la modification.
6. Inscrire la décision et sa raison dans le journal d'équilibrage (§26.3).

Ne pas changer simultanément dégâts, santé des ennemis, chances de rareté et cadence d'apparition : il deviendrait impossible de savoir quelle modification a produit le résultat. Ajuster une variable à la fois.

### 23.3 Ordre de validation

1. Valider déplacement, saut et esquive sans ennemis.
2. Tester les mêmes actions avec un ennemi poursuivant.
3. Ajouter un ennemi rapide et un ennemi lourd.
4. Tester les attaques automatiques et le ciblage.
5. Ajouter le mini-boss et ses télégraphes.
6. Tester les attaques du boss contre les extrêmes de statistiques des personnages.
7. Tester une partie complète de 20 à 30 minutes.
8. Comparer les personnages avec un équipement de départ identique lorsque c'est possible.
9. Ajuster une variable à la fois et consigner le résultat.
10. Vérifier ensuite la performance en build Windows.

Ne pas chercher à équilibrer six personnages, dix Instruments, dix Reliques et plusieurs cartes avant d'avoir validé le premier personnage et le premier combat de boss.

### 23.4 Seuils de départ à surveiller

Objectifs de test, pas garanties de conception :

- durée d'une run gagnée : 20 à 30 minutes ;
- le joueur voit au moins une amélioration tôt dans la partie ;
- aucun Instrument n'est clairement dominant sur toutes les cartes et tous les personnages ;
- le boss reste lisible quand le joueur a une construction puissante ;
- les améliorations légendaires sont rares et plaisantes mais pas nécessaires pour gagner ;
- la construction reste viable sans obtenir une rareté spécifique ;
- une défaite s'explique par les décisions ou les menaces, pas par une absence prolongée de choix valides.

### 23.5 Équilibrer les combinaisons

Pour éviter qu'une combinaison de contenus soit beaucoup plus puissante que toutes les autres :

- **Plafonds** par statistique : dégâts, cadence, critique, durée (§9.4).
- **Rendements décroissants** quand plusieurs bonus de même source s'empilent.
- **Banc de simulation automatique** (EditMode) : il estime les dégâts par seconde de chaque Instrument au rang maximum et évolué, et signale tout contenu au-dessus de 130 % de la médiane (valeur de départ).
- **Matrice de tests** Instrument × Relique sur les combinaisons les plus probables.
- **Pas de difficulté adaptative :** le budget de menace suit le temps, pas la puissance du joueur (§12.3), pour rester lisible.
- Les synergies respectent le contrat et les règles anti-boucle des §8.4 et §8.5 ; on ajuste une variable à la fois (§23.2).

---

## 24. Plan de production

### 24.1 Socle minimal (première tâche après création du projet)

1. Créer un projet Unity 6 URP et la structure de dossiers (§19.2).
2. Créer une scène de test simple.
3. Configurer le New Input System.
4. Créer un personnage provisoire capable de se déplacer, sauter et esquiver.
5. Ajouter la caméra à la troisième personne.
6. Ajouter une attaque automatique et un ennemi.
7. Ajouter santé, dégâts, mort, écran de défaite et redémarrage.
8. Tester les contrôles, collisions, caméra et écrans de fin.
9. Valider ce socle avant d'ajouter Pigment, niveaux et équipement.

**Condition de passage :** aucune progression complexe n'est ajoutée avant que déplacement, saut, esquive, caméra, combat élémentaire, mort et redémarrage soient fiables.

### 24.2 Étapes de production

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
- mini-boss et Sceau d'Évolution ;
- premier boss et transition de combat.

**Étape D — Carte et sauvegarde**

- boucle de navigation du Musée des Ombres ;
- événements de carte ;
- victoire, défaite et résultats ;
- ressource permanente ;
- déblocages et sauvegarde versionnée.

**Étape E — Validation**

- tests d'acceptation (chapitre 22) ;
- profils de personnages extrêmes ;
- profilage CPU/GPU et allocations ;
- build Windows ;
- correction des problèmes avant extension du contenu.

### 24.3 Piste interface

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

### 24.4 Piste ennemis et performance — version débutant

1. Construire une arène de test grise.
2. Valider le déplacement du joueur et les collisions du décor.
3. Implémenter un seul ennemi qui poursuit le joueur sans traverser les murs, avec une solution Unity simple adaptée à l'arène.
4. Limiter le nombre d'ennemis actifs au départ à une valeur modeste (par exemple 20–30) et vérifier le ressenti.
5. Ajouter les autres ennemis un par un, sans ajouter de système générique avant d'en avoir besoin.
6. Réutiliser les projectiles et effets seulement quand les créations/destructions répétées deviennent mesurables.
7. Profiler une scène chargée avant toute optimisation spécialisée.
8. Ne développer ni navigation de foule personnalisée, ni simulation centralisée avancée, ni animation LOD tant que le profilage ne démontre pas un problème réel.

---

## 25. Règles de travail pour l'assistant IA

### 25.1 Règles générales

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
16. Construire un prototype jouable avant de produire beaucoup de contenu.

### 25.2 Format de réponse pour une tâche technique non triviale

Présenter : objectif, fichiers à inspecter, cause confirmée ou hypothèses, modification minimale, étapes Inspector/Hierarchy, tests manuels, limites restantes.

### 25.3 Checklist avant de coder un système

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

## 26. Simplification de production et décisions de cohérence

### 26.1 Principe directeur : livrer un jeu amusant avant un jeu vaste

Le projet est développé par une personne débutante en game development. La spécification doit donc réduire le nombre de systèmes qui doivent fonctionner simultanément. Le critère de priorité est : **est-ce que cette fonctionnalité rend la boucle de combat immédiatement plus amusante ou plus compréhensible ?** Si la réponse n'est pas clairement oui, elle est reportée.

**Ordre de priorité :**

1. Le déplacement, la caméra, le saut et l'esquive sont agréables.
2. Un ennemi est satisfaisant à combattre.
3. Une attaque automatique est claire et plaisante.
4. Le joueur devient progressivement plus puissant grâce aux choix.
5. Le mini-boss et le boss sont lisibles et amusants.
6. Une run peut être terminée, perdue, redémarrée et jouée à nouveau sans bug.
7. Seulement ensuite : contenu, exploration, progression permanente et effets avancés.

### 26.2 Décisions techniques à appliquer au MVP

**États et pause**

- Le `RunController` est l'unique propriétaire de l'état de la run.
- L'état de jeu et l'affichage des menus sont séparés : la pause est une surcouche et mémorise l'état précédent.
- Une seule récompense à choix est ouverte à la fois ; les autres attendent.
- Tous les systèmes de gameplay vérifient l'autorisation de simuler avant d'avancer.
- Victoire, défaite, mort, récompenses et redémarrage ne peuvent être exécutés qu'une fois par événement.

**Dégâts**

- Tous les dégâts passent par un seul résolveur.
- Le résolveur vérifie cible valide, invulnérabilité, réduction, dégâts, réactions puis mort, dans cet ordre.
- Le MVP utilise surtout les dégâts directs. Les statuts et réactions complexes sont reportés.
- Les attaques multi-impacts doivent annoncer leur nombre d'impacts ; aucune répétition implicite à chaque image n'est autorisée.

**Mouvement**

- `CharacterController`, déplacement relatif à la caméra et une vitesse de base par personnage.
- Pas de sprint, double saut, escalade, téléportation ou déplacement instantané.
- Saut et esquive possèdent des durées et limites configurées à un seul endroit.
- L'esquive a une récupération et une fenêtre d'invulnérabilité explicites.
- Les valeurs exactes sont fixées par test, puis centralisées dans une configuration de mouvement ; elles ne sont pas dispersées dans les scripts.

**Ciblage automatique**

- Portée et cadence par Instrument.
- Recherche de cible périodique, pas à chaque image.
- Cible la plus proche par défaut ; en cas d'égalité, garder la cible actuelle si elle reste valide.
- Aucun tir à travers les murs par défaut.
- Pas de visée manuelle ni de système de verrouillage pour le MVP.

**Apparitions et foule**

- Commencer avec un maximum de 20 à 30 ennemis actifs, puis augmenter uniquement si le jeu est lisible et performant.
- Les apparitions ne se font pas à portée de contact immédiate du joueur.
- Les ennemis utilisent une navigation simple validée dans l'arène de test. Pas de champ de direction personnalisé, d'évitement de foule complexe ou de système d'animation adaptatif avant d'avoir mesuré un problème.
- Le test de 1 000 ennemis est futur et ne constitue pas un critère de livraison du premier jeu.

**Boss**

- Le boss n'est activé qu'après le nettoyage de l'arène et une annonce courte.
- La transition est déclenchée une seule fois par le contrôleur de run.
- Le combat du boss est indépendant des apparitions ordinaires ; aucune nouvelle vague ordinaire n'est créée pendant ce combat.
- Chaque attaque doit avoir un signal, un délai, une zone, une réponse possible et une récupération. Les attaques impossibles à éviter avec les statistiques de base sont rejetées ou redessinées.

**Données et sauvegarde**

- Les `ScriptableObject` décrivent le contenu statique ; ils ne stockent pas les valeurs temporaires modifiées pendant la run.
- L'état de run est conservé dans des données runtime.
- La progression permanente est reportée ; tant qu'elle n'est pas requise, ne pas construire de système de sauvegarde complexe.
- Une fois la sauvegarde ajoutée, elle est versionnée et ne restaure jamais l'équipement temporaire d'une ancienne run.

### 26.3 Cohérence des systèmes de gameplay : règles retenues

| Système | Règle de cohérence |
|---|---|
| Instruments | Une attaque possède une cadence, une portée, une règle de ciblage et une seule voie d'application des dégâts. |
| Reliques | Les bonus s'appliquent une seule fois et sont affichés avec leur effet réel. Les rangs de Reliques sont reportés tant qu'ils ne sont pas nécessaires. |
| Rareté | Modifie uniquement les valeurs statistiques ; elle ne crée pas de nouvelle mécanique. Peut être reportée du tout premier prototype. |
| Progression | Le Pigment est la seule ressource nécessaire au premier système de niveau. Pas de monnaie permanente dans le MVP initial. |
| Évolution | Une évolution démonstrative suffit pour prouver le principe. Le système complet Sceau + rang 5 + paliers 10/15/20 attend la validation du combat. |
| Statuts | Au plus un statut simple dans le MVP ; durée, intervalle et cumul définis avant intégration. |
| Surfaces de peinture | Désactivées dans le premier prototype si elles ne sont pas indispensables. Si activées, elles sont plafonnées en nombre et durée, et ne peuvent pas infliger des dégâts chaque image sans intervalle. |
| Événements et contrats | Reportés. Aucun flux de récompense concurrent du choix de niveau dans le MVP initial. |
| Coffres | Reportés ; la récompense du mini-boss peut utiliser l'écran de choix existant plutôt qu'un nouveau système. |
| Mini-carte | Reportée. L'arène initiale est assez petite pour être lisible sans navigation avancée. |
| Mode Infini | Reporté jusqu'à ce qu'une run Normal soit complète et amusante. |
| Progression permanente | Reportée jusqu'à ce que le joueur ait une raison claire de relancer une deuxième run. |
| Options | Ne présenter que les options réellement fonctionnelles. Remappage, paramètres graphiques avancés et effets complexes sont reportés. |
| Performance | Profiler avant optimisation ; limiter la foule avant d'introduire une architecture avancée. |

### 26.4 Critères de passage entre les étapes

Ne pas passer à l'étape suivante tant que les conditions de l'étape actuelle ne sont pas satisfaites.

**Étape 1 — Contrôle :** le joueur peut se déplacer, tourner la caméra, sauter et esquiver sans se bloquer dans le décor. Le contrôle reste compréhensible pendant plusieurs minutes.

**Étape 2 — Combat :** un ennemi peut poursuivre le joueur, recevoir des dégâts et mourir. Les dégâts ne sont ni doublés ni appliqués après la mort.

**Étape 3 — Plaisir de jeu :** une attaque automatique et une petite horde donnent envie de continuer à bouger. Si le combat est ennuyeux, ajuster les sensations avant d'ajouter des systèmes.

**Étape 4 — Progression :** le Pigment permet de gagner un niveau et de choisir une amélioration ; le choix s'applique une seule fois et le jeu reprend correctement.

**Étape 5 — Run complète :** le mini-boss, le boss, la victoire, la défaite et le redémarrage fonctionnent dans le même build.

**Étape 6 — Extension :** seulement après plusieurs runs de test, ajouter les systèmes futurs un par un, avec un objectif clair et un test associé.

### 26.5 Décisions ouvertes à fermer avant le prototype concerné

- Valeurs finales de vitesse, saut, esquive, santé et dégâts : à déterminer par tests de sensation, pas par théorie.
- Les Instruments attaquent pendant le saut : **oui pour le premier prototype**, afin de limiter les exceptions.
- Tir à travers les obstacles : **non par défaut**.
- Instruments initiaux : Pinceau du Dernier Trait seul au premier prototype ; ajouter un Instrument à projectile après validation.
- Reliques : commencer avec deux effets simples, sans rangs ni déclenchements complexes.
- Nombre maximal d'ennemis actifs : commencer à 20–30 et ajuster selon le ressenti et le profilage.
- Durée du prototype : 8–12 minutes ; cible 20–30 minutes après validation.
- Boss du prototype : Conservateur Sans Visage dans une version simplifiée ; sa durée et ses attaques seront mesurées.
- Surfaces de peinture : désactivées au tout début, sauf si le prototype perd trop de son identité artistique sans elles.
- Sauvegarde : aucune progression permanente requise pour le premier prototype.
- Les autres décisions de contenu futur restent ouvertes tant qu'elles ne bloquent pas le MVP.

## 27. Décisions et journal des changements

### 27.1 Registre des décisions retenues

| Sujet | Décision retenue | Section |
|---|---|---|
| Vitesse de déplacement | Une seule vitesse par personnage ; ni sprint, ni marche/course | §3.2 |
| Téléportation | Interdite : aucun portail ni déplacement instantané | §1.3 |
| Pause pendant les choix de niveau | Pause complète | §2.1, §9.2 |
| Esquive en l'air | Autorisée, avec la même récupération | §3.4 |
| Esquive sans direction | Vers l'arrière par rapport à l'orientation du personnage | §3.4 |
| Ennemis présents à l'entrée du boss | Retirés proprement avec une animation de dissolution | §12.6 |
| Emplacements pleins | Aux niveaux, seules des améliorations sont proposées ; remplacer ou refuser (avec compensation) uniquement pour les récompenses d'objet précises | §6.2, §7.2, §9.5 |
| Ciblage automatique | Règle définie par Instrument dans ses données, départage stable | §6.5 |
| Rareté | Quantité de statistiques uniquement ; probabilités fixes 60/25/12/3 au prototype | §9.3 |
| Rang d'Instrument vs palier de joueur | Deux progressions distinctes (rangs 1–5 ; paliers 10/15/20) | §6.6 |
| Sceau d'Évolution | Consommé seulement lors d'une évolution appliquée ; conservé sinon | §6.6 |
| Évolutions | Au moins deux branches mutuellement exclusives par Instrument | §6.6 |
| Pigments de terrain du MVP | Encre noire et Vermillon uniquement | §10.4 |
| Modes de jeu | Normal et Infini ; ni mode journalier ni run à graine | §2.3 |
| Structure de la run | Pilotée par le chronomètre et les phases ; aucune jauge ni zone de corruption | §12.1 |
| Taille des cartes | Grandes cartes (valeur de départ : 300 à 500 m de côté) | §13.1 |
| Navigation | Mini-carte et marqueurs d'objectifs, réglables séparément | §13.4, §18 |
| Coffres et récompenses | Mini-boss et récompense associée garantis ; coffre d’acte en Infini ; coffre commun aléatoire ; coffre doré pour les contrats ; ouverture par contact | §14.4 |
| Événements aléatoires | Un tirage toutes les 90 s dès 3:00, 35 %, 4 à 5 par run, aucun après 18:00 | §14.1 |
| Fin de run | Victoire ×1,5 ; défaite ×1 ; abandon ×1 sans pénalité | §2.5 |
| Fragments de Toile | Gains de départ par source ; retouches plafonnées ; déblocages par accomplissements | §15.2 |
| Les 1 000 ennemis | Palier de stress test, pas une densité de jeu | §20.1 |
| Architecture de foule | Composants C# et simulation centralisée ; pas d'ECS/DOTS au départ | §20.2 |
| Joueur et physique | `CharacterController` pour le joueur ; ennemis ordinaires sans `Rigidbody` dynamique par défaut | §20.3 |
| Navigation de la horde | Grille ou champ de direction partagé, avec évitement local | §20.4 |
| Phases d'une run | Six phases A à F pilotées par le chronomètre de run | §12.1 |
| Noms du menu principal | Nouvelle toile, Les artistes, Le catalogue, Options, Quitter | §17.3 |
| Difficulté | Une seule difficulté progressive ; aucune option de difficulté | §12.4 |
| Menu Options | Cinq onglets : Jeu, Vidéo, Commandes, Audio, Effets | §18 |
| Réglages d'interface | Pas d'option de taille d'interface, de taille du texte, de contraste élevé ni de réinitialisation de la sauvegarde ; rappel des commandes à l'écran : option disponible | §17.11, §18 |
| Sauvegarde | Format versionné ; run temporaire jamais restaurée | §19.6 |
| Bonus permanents | Modestes ; priorité aux déblocages | §15.2 |
| Périmètre du MVP simplifié | 1 personnage, petite arène, 2 Instruments au maximum après prototype initial, 2 Reliques, 3 ennemis communs, 1 mini-boss, 1 boss simplifié ; run initiale 8–12 min | §1.5 |
| Boss du MVP | Le Conservateur Sans Visage | §11.6 |
| Mini-boss du MVP | Colosse de Marbre | §11.4 |
| Ennemis communs du MVP | Coulure, Masque Fêlé, Chevalet Brisé, Éclat de Toile ; Essaim reporté à un test ultérieur | §11.2 |
| Boss du mode Normal | Arrivée à 20:00 ; annonce à 18:30 ; arrêt des apparitions ordinaires à 19:45 | §12.1 |
| Paliers d’évolution | Niveau 10, 15 et 20 selon le nombre d’évolutions déjà choisies pendant la run | §6.6 |
| Catégorie « élite » | Supprimée ; chaque récompense est associée à un mini-boss, coffre, événement ou contrat identifié | §11, §14 |
| Organisation du document | Document unique, sans addendum ; journal au §26.3 | Début du document |

### 27.2 Décisions encore ouvertes

Les propositions ci-dessous sont des points de départ, pas des décisions. Quand une décision est prise, la reporter dans la section concernée et déplacer sa ligne vers le registre (§26.1).

| # | Sujet | Recommandation actuelle ou candidats | Section |
|---|---|---|---|
| 1 | Touche finale d'esquive | Maj gauche (provisoire) ; bouton secondaire de la souris envisageable | §3.1 |
| 2 | Valeurs finales de vitesse, saut, esquive, caméra et dégâts | Plages de départ aux chapitres 3 et 5 | §3, §5.3 |
| 3 | Les Instruments attaquent-ils pendant le saut ? | Ouvert : à trancher et tester | §3.3 |
| 4 | Certains Instruments peuvent-ils tirer à travers les obstacles ? | Ouvert | §6.5 |
| 5 | Choix des trois Instruments du MVP | Pinceau du Dernier Trait (départ de l'Exilé) + un projectile + une zone persistante, par exemple Arbalète à Encre et Flacon de Vermillon (cohérent avec le pigment vermillon) | §1.5 |
| 6 | Choix des quatre Reliques du MVP | Ouvert | §7.2 |
| 7 | Sélection des quatre ennemis communs | Coulure, Masque Fêlé, Chevalet Brisé, Éclat de Toile | §11.2 |
| 8 | Choix du mini-boss MVP | **Résolu : Colosse de Marbre** ; son rythme et ses attaques restent à tester | §11.4, §26.1 |
| 9 | Poids des catégories de choix de niveau | Valeurs configurables à équilibrer | §9.2 |
| 10 | Formule d'expérience et rythme de montée de niveau | À calibrer sur une run de 20 à 30 minutes | §9.1 |
| 11 | Les Reliques ont-elles des rangs ? | Ouvert | §7.2 |
| 12 | Mini-boss et évolutions : rang requis, nombre de Sceaux, nombre d'évolutions par Instrument, Sceau consommable ou non | Rang 5 proposé ; un Sceau par mini-boss | §6.6, §11.5 |
| 13 | Évolution du Cadre Fracturé | Toile Réfléchissante : déviation physique de projectiles sur une trajectoire visible, sans portail ni téléportation | §6.1, §7.1 |
| 14 | Règles de cumul précises de chaque statut | Règles disponibles au §4.6 ; à choisir statut par statut | §4.6 |
| 15 | Effectif final de personnages | Six prévus ; trois pistes d'extension (Patineur, Encré, Marionnettiste) | §5.4 |
| 16 | Liste définitive des cartes futures | Deux listes se recoupent partiellement | §13.3 |
| 17 | Nom définitif, coûts et valeurs finales des retouches de la ressource permanente | « Fragments de Toile » provisoire ; gains de départ au §15.2 | §15.2 |
| 18 | Format et emplacement du fichier de sauvegarde | Format versionné requis ; format exact à choisir | §19.6 |
| 19 | Dimensions exactes du Musée des Ombres, limites, obstacles, position du boss | Valeur de départ : 300 à 500 m de côté (§13.1) ; à valider dans la test map | §13.1, §13.2 |
| 20 | Équilibrage : santé, dégâts, vitesse, XP, fréquence d'apparition, nombre maximal d'ennemis actifs | Après tests de performance et de lisibilité | §12.3, chapitre 23 |
| 21 | Performances cibles et configuration PC minimale | Baseline 60 FPS sur le PC de référence | §19.5 |
| 22 | Le boss est-il toujours obligatoire pour gagner ? | Résolu pour le mode Normal : vaincre le boss est requis pour la victoire ; l’Infini n’a pas de victoire programmée | §2.3, §11.6 |
| 23 | Technologie d'animation des ennemis : classique, poses pré-calculées ou rendu instancié | Décision après comparaison dans la test map | §20.5 |
| 24 | Un boss apparaît-il en mode Infini ? | Recommandation : non dans une première version de ce mode (survie par actes) | §2.3 |
| 25 | Mélange de pigments : bonus de palette liés aux tags | Idée Futur ; seuils et valeurs à définir | §8.2 |

### 27.3 Journal des changements

Pour chaque changement important, ajouter une ligne : version, date, sujet, ancienne règle, nouvelle règle, raison, systèmes impactés, statut (proposé, prototypé, testé, accepté ou rejeté). Les décisions ouvertes ne sont jamais effacées : elles sont marquées comme résolues avec la règle retenue.

| Version | Date | Changement |
|---|---|---|
| Base | — | Cahier des charges initial en 25 sections (vision, contrôles, personnages, Instruments, Reliques, ennemis, boss, cartes, interface, architecture, tests) |
| 1.3 | — | Règles de jeu détaillées : améliorations et raretés, peinture au sol, synergies, vagues, statuts, sauvegarde, MVP, fiches de contenu, plan de test |
| 1.4 | — | Direction artistique, UX/UI, HUD, accessibilité, intégration technique de l'interface |
| 1.5 | — | Hordes : simulation centralisée, navigation, animations adaptatives à la distance, test map |
| 1.6 | — | Contrôles et sensations de jeu : vitesse unique, saut, esquive, caméra, combat, profils de personnages, boss du MVP, cartes, validation |
| 2.0 | 2026-10-18 | **Consolidation** en un document unique organisé par thème. Les règles répétées dans plusieurs versions sont fusionnées en un seul endroit. Contradictions tranchées : nombre d'ennemis présents au boss (dissolution retenue), pause complète retenue, esquive en l'air et direction par défaut retenues, découpage des phases de run unifié en six phases A à F, conditions d'évolution unifiées (rang + Sceau + palier), probabilités de rareté fixes au prototype, noms du menu alignés sur l'interface (« Nouvelle toile », etc.), deux arborescences de dossiers fusionnées en une. Conflit signalé et laissé ouvert : évolution « Portail de la Toile » contre l'interdiction de téléportation (§26.2, n° 13). Mentions de portails retirées de la Miniature Fracturée et du Corridor Impossible |
| 2.1 | 2026-10-19 | Ajout du menu Options (§18) : cinq onglets Jeu, Vidéo, Commandes, Audio, Effets. Une seule difficulté progressive : l'option de difficulté et la décision ouverte correspondante sont supprimées. Doublons de la liste d'origine fusionnés (champ de vision, tremblement de caméra) ou distingués (dégâts affichés, vignette de dégâts). Réglages d'accessibilité du §17.11 absents de la liste (tranché en 2.2) |
| 2.2 | 2026-10-19 | Décision : aucune option de taille d'interface, de taille du texte, de contraste élevé ni de réinitialisation de la sauvegarde (exigences retirées des §17.11 et §22.9 ; la sauvegarde de développement du §19.6 reste un outil interne). Ajout de l'option « Rappel des commandes à l'écran » dans l'onglet Jeu (§18). Décision ouverte n° 25 supprimée |
| 2.3 | 2026-10-20 | Intégration des propositions validées : modes Normal et Infini sans corruption (§2.3), grandes cartes (§13.1), navigation (§13.4), objectifs secondaires (§14.2), défis facultatifs et contrats (§14.3), événements aléatoires (§14.1), coffres (§14.4), bonus de run (§14.5), synergies par tags (§8), emplacements pleins (§6.2, §7.2, §9.5), fin de run (§2.5), progression permanente (§15.2), défis permanents, Archives, statistiques et fin de jeu (§15.3 à §15.6), sauvegarde (§19.6), apprentissage (§17.10), équilibrage des combinaisons (§23.5), options mini-carte et marqueurs (§18). Incohérence entre §6.2, §7.2 et §9.5 sur les emplacements pleins résolue. Décision ouverte « contrats de restauration » remplacée par deux nouvelles (n° 24 et 25) |
| 3.0 | 2026-10-20 | **Réorganisation complète** en 26 chapitres, du général au particulier : projet (1–2), joueur et combat (3–8), progression, monde et récompenses (9–15), présentation (16–18), technique (19–20), production (21–26). Principaux déplacements : le combat passe avant les personnages et les Instruments (4) ; les synergies deviennent un chapitre (8) ; progression en partie, peinture, ennemis et vagues se suivent (9 à 12) ; objectifs, événements, coffres et bonus de run sont réunis (14) ; progression permanente, Archives et statistiques sont réunies (15) ; direction artistique, effets et audio sont réunis (16) ; l'interface (17) et le menu Options (18) sont séparés ; la sauvegarde rejoint l'architecture technique (19.6) ; les hordes et la performance passent côté technique (20) ; les principes de production de l'ancien §1.1 sont fusionnés avec les règles pour l'assistant IA (25). Tous les renvois sont renumérotés. Aucune règle ni valeur de gameplay n'est modifiée |
| 3.1 | 2026-10-22 | Intégration des propositions détaillées de chronologie de run, bonus instantanés/temporaires/de run, économie permanente, navigation, contrats et contenu ennemi. Ajout des familles Artilleur et Essaim, remplacement de la famille Plongeon, reformulation du Portrait Effacé pour interdire la téléportation, et clarification de la hiérarchie ennemis communs → mini-boss → boss sans catégorie d’élites distincte. Ajout de propositions de mini-boss et de règles de placement. Les chiffres restent à tester. |
| 3.2 | 2026-10-22 | Décisions validées : mini-boss MVP = Colosse de Marbre ; boss MVP = Conservateur Sans Visage ; quatre ennemis communs au MVP et Essaim reporté à un test ultérieur ; boss Normal à 20:00 ; paliers d’évolution 10, 15 et 20 selon le nombre d’évolutions déjà choisies ; suppression de la catégorie « élite » et rattachement des récompenses à une source explicite. Harmonisation du résumé, du périmètre MVP, des ennemis, de la chronologie, des récompenses, du mode Infini, des Archives, des critères d’acceptation et du registre de décisions. Les valeurs de combat du boss restent à tester. |
| 3.3 | 2026-10-23 | Audit de cohérence et simplification pour une production solo débutante : MVP ramené à une petite arène, une run prototype de 8–12 minutes, deux Instruments au maximum après le prototype initial, deux Reliques, trois ennemis communs et une évolution démonstrative. Clarification de la pause et des états, du circuit unique de dégâts, du contrat de déplacement, du ciblage automatique et des transitions de boss. Navigation de foule avancée, mini-carte, surfaces complexes, événements, contrats, Infini et méta-progression sont reportés. Plan de production réordonné autour du plaisir de jeu et d'un build Windows complet. |
