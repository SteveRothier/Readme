**# BROKEN CANVAS — Cahier des charges complet**



**\*\*Version :\*\*** 1.3 — spécification de conception détaillée  

**\*\*Statut :\*\*** référence de conception proposée ; les valeurs marquées « à valider » doivent être confirmées par prototype et tests. Aucune fonctionnalité n'est réputée implémentée.  

**\*\*Moteur :\*\*** Unity 6, URP, C#  

**\*\*Plateforme :\*\*** PC Windows  

**\*\*Mode :\*\*** solo  

**\*\*Genre :\*\*** survivor roguelite 3D à la troisième personne, avec combats de boss exigeants.



**---**



**## 1. Objectif et règles de production**



Ce document sert de référence pour le développeur et les assistants IA. Les éléments « V1 » sont nécessaires à la première version jouable ; les éléments « Futur » viennent après validation de la boucle principale. Les chiffres non confirmés sont des valeurs de départ à tester.



Principes :

\- Construire un prototype jouable avant de produire beaucoup de contenu.

\- Inspecter les scripts réels avant toute modification ; ne pas supposer que le projet correspond parfaitement à ce document.

\- Préserver les références Inspector et les systèmes existants sauf nécessité expliquée.

\- Préférer des systèmes simples, réutilisables et testables.

\- Ne jamais prétendre qu'un script a été compilé ou testé si ce n'est pas le cas.

\- Aucune programmation n'est demandée par ce document : il s'agit d'une spécification de conception.



**## 2. Vision du jeu**



Le joueur incarne un personnage lié à l'art et à la peinture maudite. Il explore des lieux contaminés par des œuvres vivantes, combat des hordes de créatures peintes, collecte du Pigment pour monter de niveau, compose une combinaison d'Instruments et de Reliques, puis affronte un boss final.



**### Piliers**

1\. **\*\*Construire sa puissance :\*\*** choix de niveau et synergies qui changent réellement les attaques.

2\. **\*\*Survivre par le mouvement :\*\*** placement, saut et esquive ont une utilité claire.

3\. **\*\*Lire le danger :\*\*** les attaques importantes possèdent des indices visuels ou sonores.

4\. **\*\*Identité artistique :\*\*** tableaux vivants, encre surnaturelle et pigments corrompus.

5\. **\*\*Contenu évolutif :\*\*** ajouter des personnages, objets et cartes sans réécrire les systèmes centraux.



**### Direction artistique**

Palette principale : noir d'encre, charbon, ivoire, toile vieillie et rouge vermillon. Les autres couleurs de pigments sont réservées aux effets de gameplay et aux objets particuliers. Privilégier les silhouettes lisibles, les modèles stylisés réutilisables, les effets reconnaissables et les télégraphes de boss qui ne se confondent pas avec les attaques du joueur.



**## 3. Fiche de référence**



\| Élément | Décision |

\|---|---|

\| Perspective | Troisième personne |

\| Caméra | Derrière le personnage, rotation à la souris |

\| Mode | Solo |

\| Plateforme | PC Windows |

\| Moteur | Unity 6 avec URP |

\| Entrées | New Input System |

\| Combat courant | Attaques automatiques |

\| Actions manuelles | Déplacement, saut, esquive et caméra |

\| Esquive | Manuelle, avec courte invulnérabilité |

\| Durée cible | 20 à 30 minutes, boss compris |

\| Première carte | Musée des Ombres |

\| Emplacements maximum | 6 Instruments et 6 Reliques |

\| Rang maximum d'un Instrument | 5 |

\| Boss | Les apparitions ordinaires cessent pendant son combat |

\| Progression permanente | Déblocages et bonus de puissance modestes |

\| Contenu initial ciblé | 1 personnage, 1 carte, 3 Instruments, 4 Reliques, 4 ennemis communs, 1 élite, 1 boss |



**## 4. Vocabulaire**



\- **\*\*Instrument :\*\*** arme principale qui déclenche une attaque automatique.

\- **\*\*Relique :\*\*** équipement secondaire qui modifie les statistiques, les attaques ou les synergies.

\- **\*\*Pigment :\*\*** expérience collectée pendant une run.

\- **\*\*Rang :\*\*** niveau d'amélioration d'un Instrument ou, si cette règle est retenue, d'une Relique.

\- **\*\*Évolution :\*\*** transformation spéciale d'un Instrument, débloquée par la victoire contre un mini-boss et par un rang d'arme requis.

\- **\*\*Fracture :\*\*** anomalie dans un tableau qui peut déclencher un événement facultatif ; fonctionnalité future.

\- **\*\*Run :\*\*** partie complète jusqu'à la victoire ou la défaite.

\- **\*\*Élite :\*\*** ennemi plus dangereux qui donne une récompense supérieure.

\- **\*\*Archives :\*\*** collection permanente des découvertes et déblocages.



Les noms d'interface peuvent être artistiques ; les noms de classes, méthodes et variables C# doivent rester explicites et maintenables.



**## 5. Boucle principale**



1\. Choisir un personnage disponible.

2\. Lancer une run.

3\. Explorer et combattre ; les Instruments attaquent automatiquement.

4\. Se déplacer, sauter et esquiver manuellement.

5\. Ramasser le Pigment laissé par les ennemis.

6\. Monter de niveau et choisir une amélioration parmi trois options.

7\. Équiper et améliorer des Instruments et des Reliques dans la limite des emplacements.

8\. Survivre à une difficulté croissante.

9\. Arrêter les apparitions ordinaires lorsque le boss commence.

10\. Vaincre le boss ou perdre tous ses points de vie.

11\. Afficher le bilan, attribuer les récompenses permanentes et permettre de recommencer ou de revenir au menu.



Le jeu peut se mettre en pause pendant les choix d'amélioration. En combat ordinaire, la boucle doit rester dynamique.



**## 6. Contrôles, déplacement et caméra**



**### Contrôles proposés**



\| Action | Contrôle initial proposé |

\|---|---|

\| Déplacement | ZQSD ou WASD selon la disposition |

\| Caméra | Souris |

\| Saut | Espace |

\| Esquive | Maj gauche ou bouton souris secondaire, à confirmer |

\| Pause | Échap |

\| Attaque | Automatique |

\| Navigation UI | Souris et clavier |



Les actions doivent être centralisées dans le New Input System pour permettre leur remappage ultérieur.



**### Déplacement**

Le déplacement se fait sur le plan de jeu en fonction de l'orientation de la caméra. La vitesse diagonale est normalisée. Les collisions ne doivent pas permettre de traverser les murs ni de rester coincé dans la géométrie.



**### Saut — exigence V1**

Le saut fait partie des déplacements principaux dès la première version jouable :

\- déclenchement manuel ;

\- saut autorisé lorsque le personnage est au sol ;

\- gravité cohérente et retour au sol fiable ;

\- pas de double saut par défaut ;

\- hauteur adaptée à l'échelle de l'arène ;

\- le saut ne doit pas permettre de franchir les limites de carte ou d'éviter toutes les mécaniques de boss ;

\- les attaques automatiques pendant le saut doivent suivre une règle explicite et testée ;

\- animations provisoires acceptées pendant le prototype.



**\*\*Recommandation :\*\*** le saut sert surtout à la mobilité et au franchissement léger. Il ne donne pas d'invulnérabilité par défaut et ne remplace pas l'esquive. La possibilité de sauter par-dessus une attaque de boss se décide attaque par attaque.



**### Esquive**

\- Déclenchement manuel et impulsion de déplacement dans une direction déterminée par le joueur.

\- Courte fenêtre d'invulnérabilité.

\- Délai de récupération pour éviter une esquive continue.

\- Feedback visuel ou sonore.

\- Aucun dégât antérieur au déclenchement ne doit être annulé.

\- Les durées exactes, la distance et le délai sont à équilibrer par les tests.



**### Caméra**

\- Vue à la troisième personne, suivi stable et rotation souris.

\- Limites verticales raisonnables.

\- Gestion des obstacles entre caméra et personnage.

\- Sensibilité réglable ; inversion verticale facultative.

\- Curseur capturé pendant le jeu et libéré dans les menus, la pause et les écrans de fin.

\- La caméra ne doit pas recapturer la souris lorsque le jeu est en pause ou terminé.



**## 7. Personnages jouables**



La vision finale prévoit six personnages. Pour la V1, seul l'Exilé est nécessaire afin de valider les systèmes communs.



**### 1. L'Exilé — départ**

\- **\*\*Instrument :\*\*** Pinceau du Dernier Trait.

\- **\*\*Passif — Trait fondateur :\*\*** les éliminations augmentent temporairement les dégâts de l'Instrument ; le bonus expire après une période sans élimination.

\- **\*\*Style :\*\*** équilibré, adapté à la découverte.



**### 2. La Virtuose**

\- **\*\*Instrument :\*\*** Rapière Vermillon.

\- **\*\*Passif — Coup de maître :\*\*** les coups critiques marquent les ennemis ; une prochaine attaque critique contre une cible marquée reçoit un bonus.

\- **\*\*Style :\*\*** attaques rapides et dégâts concentrés.



**### 3. Le Graveur**

\- **\*\*Instrument :\*\*** Arbalète à Encre.

\- **\*\*Passif — Perspective :\*\*** les attaques à distance gagnent en puissance jusqu'à une limite.

\- **\*\*Style :\*\*** portée et précision.



**### 4. L'Alchimiste**

\- **\*\*Instrument :\*\*** Palette Catalytique.

\- **\*\*Passif — Mélange instable :\*\*** les effets de pigments peuvent déclencher une réaction de zone, avec une probabilité limitée.

\- **\*\*Style :\*\*** effets de statut et synergies.



**### 5. Le Sculpteur**

\- **\*\*Instrument :\*\*** Maillet de Marbre Noir.

\- **\*\*Passif — Œuvre monumentale :\*\*** les niveaux renforcent légèrement sa résistance avec un compromis de mobilité.

\- **\*\*Style :\*\*** robuste, lent et puissant.



**### 6. La Restauratrice**

\- **\*\*Instrument :\*\*** Fil de Restauration.

\- **\*\*Passif — Retouche vitale :\*\*** éliminer des ennemis marqués peut rendre un peu de santé, avec une limite de fréquence.

\- **\*\*Style :\*\*** survie et récupération.



Règles communes :

\- Les personnages partagent le contrôleur, la caméra, la santé et les règles de combat.

\- Les différences viennent des données, statistiques, passifs et Instruments de départ.

\- Le passif fonctionne automatiquement dès le début de la run.

\- Chaque personnage doit avoir une force, une faiblesse et des synergies distinctes.

\- Les personnages verrouillés indiquent leur condition de déblocage.

\- Les cinq personnages supplémentaires sont du contenu futur, pas un prérequis du prototype.



**## 8. Instruments — armes principales**



**### Liste de référence**



1\. **\*\*Pinceau du Dernier Trait — zone rapprochée :\*\*** dessine un arc d'encre devant le personnage.
   - **Évolution A — Grande Marée :** l'arc devient une vague qui avance et traverse les ennemis. Synergie : les ennemis ralentis par la Palette Catalytique restent plus longtemps dans la vague.
   - **Évolution B — Fresque Tourbillonnante :** trois arcs tournent brièvement autour du joueur après l'attaque. Synergie : les Éclats de Fusain ajoutent une explosion d'encre lorsqu'un fragment touche un ennemi déjà marqué.
2\. **\*\*Rapière Vermillon — critiques :\*\*** frappe une cible proche.
   - **Évolution A — Estoc de Rubis :** les coups critiques appliquent une marque ; après un court délai, la marque explose. Synergie : la Broche du Critique augmente les déclenchements, et le Flacon de Vermillon amplifie l'explosion dans ses flaques.
   - **Évolution B — Danse des Entailles :** chaque élimination déclenche une entaille secondaire vers une cible proche, avec une limite de rebonds. Synergie : le Fil de Restauration propage la marque aux ennemis reliés.
3\. **\*\*Arbalète à Encre — longue portée :\*\*** tire un projectile précis.
   - **Évolution A — Trait Perforant :** le projectile traverse plusieurs ennemis et inflige davantage de dégâts aux cibles alignées. Synergie : la Cloche du Vernissage regroupe ou interrompt les ennemis pour faciliter l'alignement.
   - **Évolution B — Pointe de Contamination :** le projectile se plante dans la première cible, puis éclate en éclaboussures après un délai. Synergie : la Palette Catalytique ajoute un effet de statut aux éclaboussures.
4\. **\*\*Flacon de Vermillon — zone persistante :\*\*** éclate et laisse une flaque dangereuse.
   - **Évolution A — Marée Vermillon :** la flaque se divise en petites flaques qui se propagent vers les ennemis proches. Synergie : le Sablier du Dernier Regard prolonge leur durée.
   - **Évolution B — Fournaise Écarlate :** les flaques s'enflamment après quelques secondes et explosent, infligeant des dégâts de zone. Synergie : la Palette Catalytique transforme les ennemis contaminés dans la zone en source d'une réaction secondaire.
5\. **\*\*Éclats de Fusain — défense orbitale :\*\*** fragments tournant autour du personnage.
   - **Évolution A — Couronne de Charbon :** les fragments sont plus nombreux et repoussent légèrement les ennemis au contact. Synergie : le Maillet de Marbre Noir inflige des dégâts bonus aux ennemis repoussés ou déséquilibrés.
   - **Évolution B — Éclipse de Fusain :** à intervalles réguliers, les fragments quittent l'orbite et plongent sur les ennemis proches avant de revenir. Synergie : la Fiole d'Encre Vive réduit le délai entre ces salves.
6\. **\*\*Palette Catalytique — effets de statut :\*\*** applique brûlure, ralentissement ou contamination.
   - **Évolution A — Réaction en Chaîne :** l'application d'un statut sur une cible déjà affectée déclenche une réaction de zone. Synergie : le Flacon de Vermillon fournit des zones persistantes où la réaction peut se propager.
   - **Évolution B — Prisme Instable :** les statuts alternent entre ralentissement, brûlure et contamination ; deux statuts différents sur une même cible déclenchent une décharge. Synergie : le Fil de Restauration transmet une partie de la décharge aux ennemis reliés.
7\. **\*\*Maillet de Marbre Noir — impact lourd :\*\*** frappe une zone avec cadence lente et dégâts élevés.
   - **Évolution A — Faille du Musée :** l'impact crée des fissures qui explosent en ligne après un bref délai. Synergie : la Cloche du Vernissage immobilise brièvement les ennemis légers dans la trajectoire.
   - **Évolution B — Sceau du Sculpteur :** chaque impact laisse une zone de gravité qui ralentit les ennemis avant une frappe secondaire. Synergie : le Pinceau Miniature augmente la taille de la zone et la portée de l'onde.
8\. **\*\*Fil de Restauration — ricochets :\*\*** relie plusieurs ennemis et transmet une partie des dégâts.
   - **Évolution A — Réseau de Restauration :** davantage d'ennemis sont reliés et une partie des dégâts reçus par l'un est répercutée sur les autres. Synergie : la Palette Catalytique fait se propager les statuts à travers les liens.
   - **Évolution B — Suture Vitale :** les éliminations d'ennemis reliés rendent une petite quantité de santé, avec un délai interne et un plafond par vague. Synergie : le Cœur de Porcelaine augmente la marge de survie nécessaire pour rester au contact.
9\. **\*\*Cloche du Vernissage — contrôle :\*\*** onde périodique repoussant ou interrompant les ennemis légers.
   - **Évolution A — Résonance Fragile :** l'onde marque les ennemis touchés ; ils subissent davantage de dégâts lors de leur prochain impact puissant. Synergie : le Maillet de Marbre Noir consomme la marque pour infliger une onde secondaire.
   - **Évolution B — Silence de Galerie :** l'onde crée une zone de ralentissement et interrompt plus efficacement les attaques légères, sans neutraliser les boss. Synergie : le Flacon de Vermillon inflige davantage de dégâts aux ennemis ralentis dans ses flaques.
10\. **\*\*Cadre Fracturé — invocation :\*\*** ouvre un cadre qui tire des éclats d'encre.
   - **Évolution A — Galerie Jumelle :** un second cadre apparaît à une distance limitée et les deux cadres peuvent viser une même cible. Synergie : la Miniature Fracturée augmente la fréquence de tir des cadres.
   - **Évolution B — Portail de la Toile :** le cadre ouvre périodiquement une brèche qui téléporte ses projectiles vers une cible marquée et provoque une petite explosion à l'arrivée. Synergie : l'Arbalète à Encre marque les cibles prioritaires et la Palette Catalytique ajoute un statut à l'explosion.



**### Règles des Instruments**

\- Maximum de six Instruments équipés, Instrument de départ inclus.

\- Chaque Instrument a cinq rangs maximum dans le système de base.

\- Améliorer un Instrument équipé ne consomme pas un nouvel emplacement.

\- Si les emplacements sont pleins, le joueur peut remplacer un Instrument ou refuser le choix.

\- La conversion d'un objet refusé en ressource est une option future.

\- Les attaques automatiques doivent sélectionner des cibles valides et respecter la cadence, la portée et, si applicable, la ligne de vue.

\- Projectiles, zones et effets persistants ont des règles de collision et de durée explicites.



**### Rangs**

\- Rang 1 : version de base.

\- Rangs 2 à 4 : amélioration progressive de paramètres ou d'effets.

\- Rang 5 : version de base pleinement améliorée. Le rang 5 ne suffit plus, à lui seul, à rendre une arme éligible à une évolution.

\- Les améliorations peuvent toucher dégâts, cadence, portée, nombre de projectiles, taille de zone ou durée d'effet.

\- Les valeurs doivent être assez perceptibles pour que chaque rang soit utile.

### Rareté des améliorations ordinaires

La rareté sert uniquement à déterminer la puissance statistique d'une amélioration ordinaire. Elle ne change pas le comportement de l'Instrument et ne crée pas de nouvelle mécanique. Les transformations de gameplay restent réservées aux évolutions dédiées, aux passifs des personnages et aux systèmes explicitement conçus pour cela.

| Rareté | Couleur | Chance de base proposée | Puissance et contenu |
|---|---|---:|---|
| Commun | Vert | 60 % | Petit bonus sur une statistique : dégâts, cadence, portée, taille, durée, vitesse de projectile ou statistique critique selon l'Instrument. |
| Peu commun | Bleu | 25 % | Bonus statistique plus élevé, généralement sur une seule statistique. |
| Épique | Violet | 12 % | Bonus élevé, éventuellement réparti sur deux statistiques compatibles. |
| Légendaire | Jaune | 3 % | Bonus statistique exceptionnel, pouvant combiner plusieurs statistiques compatibles. |

- Ces probabilités sont un point de départ et doivent être testées en jeu. Leur somme doit rester égale à 100 % avant application d'éventuels modificateurs de chance.
- Exemples indicatifs à équilibrer : Commun +10 % de dégâts ; Peu commun +18 % de dégâts ; Épique +25 % de dégâts et +15 % de taille ; Légendaire +40 % de dégâts et +20 % de cadence.
- Les chiffres sont des exemples de conception, pas des valeurs finales garanties. Chaque Instrument ne propose que les statistiques pertinentes pour son fonctionnement.
- Les probabilités peuvent être ajustées selon la phase de la run ou une statistique de Chance, mais aucun bonus ne doit rendre les améliorations légendaires courantes.
- Une protection contre la malchance peut être testée : après une longue série sans amélioration rare, les chances d'obtenir une rareté supérieure augmentent progressivement puis se réinitialisent après son obtention.
- La couleur ne doit jamais être le seul indicateur : afficher aussi le nom de la rareté et une icône, avec une présentation lisible et compatible avec les daltonismes.
- Les améliorations ordinaires conservent le même type d'effet à toutes les raretés ; seule la quantité statistique change. Une amélioration légendaire n'est donc pas une évolution d'arme.


\- **\*\*L'évolution est distincte des cinq rangs de base\*\*** : elle transforme le comportement de l'Instrument sans consommer un emplacement supplémentaire.

\- Condition proposée : posséder l'Instrument au rang 5, atteindre le niveau de joueur requis (niveau 10 recommandé, avec variantes d'équilibrage possibles aux niveaux 15 ou 20) et obtenir un Sceau d'Évolution en vainquant un mini-boss. Le niveau du joueur est un seuil de progression distinct du rang de l'arme.

\- Après la victoire, si une ou plusieurs armes éligibles sont disponibles, le jeu propose un choix d'évolution. Le joueur choisit l'arme à faire évoluer puis une évolution valide pour celle-ci.

\- Si aucune arme n'est éligible, le Sceau est conservé pour plus tard ; la victoire du mini-boss doit tout de même donner une récompense utile.

\- Une évolution est appliquée une seule fois par Instrument, sauf si une future mécanique indique explicitement le contraire.

\- L'évolution doit avoir un effet de gameplay visible et une identité visuelle/audio distincte, pas seulement un bonus de dégâts.
- Les raretés des améliorations ordinaires ne sont pas utilisées pour définir la puissance ou l'accès aux évolutions. Une évolution est un choix spécial distinct, obtenu selon les conditions de progression et de récompense définies ci-dessus.
\- Chaque Instrument dispose d'au moins deux évolutions mutuellement exclusives par arme et par run. Elles doivent proposer des styles distincts et chacune créer au moins deux interactions lisibles avec d'autres Instruments ou Reliques ; éviter les synergies qui ne font qu'ajouter des dégâts sans changer les décisions du joueur.

\- Les effets précis, le nombre d'évolutions possibles par Instrument et le caractère consommable ou non du Sceau pourront être équilibrés pendant le prototypage.



**## 9. Reliques — équipements secondaires**



**### Liste proposée**

1\. **\*\*Masque de l'Inspiration :\*\*** augmente les dégâts au prix d'une légère réduction de résistance.

2\. **\*\*Fiole d'Encre Vive :\*\*** réduit le délai entre les attaques.

3\. **\*\*Bottes du Restaurateur :\*\*** augmentent la vitesse et améliorent légèrement l'esquive.

4\. **\*\*Miroir Brisé :\*\*** renvoie une partie des dégâts reçus aux ennemis proches, selon un délai interne.

5\. **\*\*Pinceau Miniature :\*\*** augmente la portée et la taille des zones.

6\. **\*\*Sablier du Dernier Regard :\*\*** augmente la durée des effets de statut.

7\. **\*\*Cœur de Porcelaine :\*\*** augmente les points de vie maximum.

8\. **\*\*Boussole du Cadre :\*\*** augmente la quantité de Pigment, avec un plafond.

9\. **\*\*Broche du Critique :\*\*** augmente les chances de coup critique.

10\. **\*\*Miniature Fracturée :\*\*** améliore les Instruments liés aux cadres et portails.



**### Règles**

\- Maximum de six Reliques équipées.

\- Les Reliques modifient les statistiques ou ajoutent des effets passifs.

\- Si les emplacements sont pleins, le joueur peut remplacer une Relique ou refuser la nouvelle.

\- Les règles de cumul des effets similaires doivent être explicites.

\- Les effets affichent leur valeur et leur durée si pertinent.

\- La V1 n'a besoin que de quatre Reliques pour tester les interactions.

\- Le système de rangs des Reliques reste à décider.



**## 10. Pigment et montée de niveau**



\- Les ennemis vaincus peuvent laisser du Pigment.

\- Le joueur le récupère au contact ou dans un rayon de ramassage défini.

\- Les quantités gagnées sont configurables par type d'ennemi.

\- La jauge affiche le Pigment courant, le seuil suivant et le niveau.

\- Un niveau présente trois choix.
- Lorsqu'un choix améliore un Instrument équipé, son niveau de rareté est déterminé selon les probabilités définies dans la section « Rareté des améliorations ordinaires ».
- Les raretés Vert/Commun, Bleu/Peu commun, Violet/Épique et Jaune/Légendaire indiquent uniquement l'ampleur du bonus statistique. Elles ne transforment pas l'attaque et ne remplacent jamais les évolutions d'Instrument.
- Les choix doivent afficher clairement le nom de l'Instrument, la rareté, les statistiques modifiées et les valeurs avant/après lorsque cela est possible.

\- Le jeu se met en pause pendant la sélection.

\- Les choix peuvent proposer un nouvel Instrument, une Relique, l'amélioration d'un objet équipé ou un bonus de statistiques.

\- Les options impossibles doivent être exclues ou clairement expliquées.

\- Le joueur doit toujours disposer d'au moins une option valide.

\- Le seuil d'expérience progresse selon une formule configurable, à équilibrer pendant les tests.

\- Les objets de Pigment doivent être nettoyés correctement après collecte.



**## 10 bis. Systèmes de peinture au sol et pistes d'extension**

La peinture au sol doit être une mécanique de déplacement et de combat, pas seulement un effet visuel. Pour préserver la lisibilité et limiter le coût de production, commencer avec deux types de surface, puis étendre le système après validation.

- **Encre noire :** peut favoriser la glisse du Patineur et la récupération de ressource de l'Encré.
- **Vermillon :** peut endommager ou marquer les ennemis et participer à certaines réactions.
- **Pigments futurs :** un pigment bleu pourrait ralentir les ennemis ; une peinture contaminée pourrait transmettre un statut aux ennemis qui la traversent. Ces idées restent hors du périmètre initial tant que les deux premiers types ne sont pas validés.
- Les flaques doivent avoir une durée, une taille maximale, des règles de cumul et une limite de nombre pour préserver les performances et la lisibilité.
- Les surfaces ne doivent pas enfermer le joueur ni rendre les déplacements imprévisibles.

### Personnages futurs à prototyper après la V1

- **Le Patineur :** personnage axé sur l'inertie. Il accélère en glissant sur des traces compatibles, perd de la vitesse dans les virages serrés et peut effectuer un dérapage qui dépose une ligne de peinture. À haute vitesse, une collision offensive peut produire une onde de choc. La maîtrise de la trajectoire est sa force ; les espaces encombrés et les arrêts fréquents sont ses faiblesses.
- **L'Encré :** personnage capable de se transformer brièvement en flux d'encre au ras du sol. La transformation consomme une jauge ; traverser ses propres flaques peut contribuer à la restaurer. Reprendre forme près d'une flaque peut provoquer une explosion. Cette transformation doit rester un déplacement continu : aucune téléportation ni aucun déplacement instantané.
- **Le Marionnettiste :** relie plusieurs ennemis avec des fils d'encre ; une fraction des dégâts infligés à un ennemi lié peut être propagée aux autres. Une compétence peut rapprocher les ennemis liés. Il est plus faible contre une cible isolée.
- Ces trois personnages sont des pistes de contenu futur, pas des prérequis de la première version jouable. Le Patineur est le premier candidat au prototypage après validation du combat de base.
- Aucun personnage ni système ne doit utiliser de portails ou de téléportation. La mobilité doit s'appuyer sur le déplacement physique, l'inertie, les esquives et les interactions avec le terrain.

### Archétypes d'ennemis et boss futurs

- **Gribouilleur :** ennemi rapide et fragile qui force le joueur à se déplacer.
- **Bloc de Marbre :** ennemi lent et résistant qui menace l'espace proche.
- **Œil du Conservateur :** ennemi à distance qui oblige à interrompre ou modifier sa trajectoire.
- **Coloriste :** ennemi de soutien qui renforce ses alliés ou modifie temporairement la peinture du sol.
- Les archétypes doivent se différencier par leur rôle tactique, et non uniquement par leurs points de vie.
- Pistes de boss : **Le Conservateur**, qui contrôle l'espace ; **La Toile Déchirée**, qui modifie les zones sûres ; **Le Maître des Pigments**, qui change temporairement les propriétés de la peinture.
- Chaque attaque dangereuse doit avoir une indication lisible. Éviter les phases d'invulnérabilité longues ; les boss doivent tester le déplacement, la lecture des attaques et la construction du joueur.

### Arènes et progression future

- **Atelier abandonné :** arène relativement ouverte pour apprendre les systèmes.
- **Galerie brisée :** piliers et cadres qui influencent les trajectoires.
- **Musée englouti :** espace plus contraint avec zones de peinture dangereuses.
- Des événements temporaires peuvent être ajoutés plus tard : chute de cadres, débordement de pigment, apparition d'une surface rare ou vague d'élite.
- La méta-progression doit prioritairement débloquer des personnages, Instruments, Reliques, évolutions, défis et éléments de lore. Les bonus permanents bruts doivent rester modestes afin de ne pas trivialiser les premières difficultés.
- Ces contenus sont des propositions d'extension : ils ne modifient pas le périmètre réduit de la V1.

**## 11. Ennemis**



**### Principes**

Les modèles doivent être stylisés, simples et lisibles. Plusieurs ennemis peuvent partager des animations ou des composants. Une variante doit apporter un comportement ou un rôle distinct, pas seulement une couleur. Chaque ennemi configure ses points de vie, vitesse, dégâts, portée et délais d'attaque. Les récompenses ne sont attribuées qu'une seule fois.



**### Liste**

\| Ennemi | Silhouette et comportement |

\|---|---|

\| **\*\*Coulure\*\*** | Masse noire arrondie ; avance lentement |

\| **\*\*Masque Fêlé\*\*** | Masque flottant ; poursuit et charge brièvement |

\| **\*\*Chevalet Brisé\*\*** | Assemblage de bois ; lent, résistant, attaque au contact |

\| **\*\*Éclat de Toile\*\*** | Fragment flottant ; tire à distance |

\| **\*\*Tache Vorace\*\*** | Petite masse à bouche ; bondit puis se replie |

\| **\*\*Marionnette du Peintre\*\*** | Humanoïde simple ; approche et attaque lisiblement |

\| **\*\*Cadre Ambulant\*\*** | Cadre animé ; bloque le passage et protège ses alliés |

\| **\*\*Corbeau de Fusain\*\*** | Oiseau noir ; tourne autour du joueur et plonge |

\| **\*\*Portrait Effacé\*\*** | Humanoïde pâle ; disparaît puis réapparaît |

\| **\*\*Goutte Vermillon\*\*** | Sphère rouge ; explose au contact ou après un délai |



**### Élites futures**

\- **\*\*Masque Royal :\*\*** longue charge suivie d'une onde de choc.

\- **\*\*Conservateur :\*\*** protège temporairement les ennemis proches.

\- **\*\*Tache Couronnée :\*\*** laisse des zones d'encre dangereuses.

\- **\*\*Copiste :\*\*** reproduit temporairement le comportement d'un ennemi.

\- **\*\*Portrait Inachevé :\*\*** alterne entre forme vulnérable et forme agressive.



Pour la V1, retenir quatre ennemis communs couvrant des rôles différents — mêlée lente, rapide, distance et résistant — et un seul élite.



**## 11 bis. Mini-boss et récompenses d'évolution**



Les mini-boss sont des objectifs de combat intermédiaires qui récompensent la maîtrise et donnent au joueur une occasion de transformer un Instrument puissant. Ils ne remplacent pas le boss final.



**### Règle de base proposée**

1\. Le joueur rencontre et vainc un mini-boss.

2\. La victoire donne un **\*\*Sceau d'Évolution\*\*** garanti, plus une récompense secondaire à définir (Pigment, soin, coffre ou ressource de run).

3\. Le jeu vérifie les Instruments équipés : un Instrument doit avoir atteint le rang requis, proposé à **\*\*rang 5\*\***, pour pouvoir évoluer.

4\. Si au moins un Instrument est éligible, une interface met en avant les choix valides. Le joueur sélectionne l'Instrument puis l'une de ses deux évolutions disponibles. Chaque évolution doit modifier le comportement de l'arme et ouvrir des synergies différentes.

5\. Si aucun Instrument n'est éligible, le Sceau est conservé et peut être utilisé quand une arme devient éligible. Le mini-boss accorde tout de même sa récompense secondaire.

6\. Une évolution transforme l'Instrument en une variante plus spécialisée et spectaculaire ; elle ne compte pas comme un nouvel Instrument dans la limite de six emplacements.



**### Garde-fous de conception**

\- La victoire du mini-boss doit toujours sembler rentable, même si aucune arme n'est encore au rang requis.

\- L'interface doit expliquer clairement pourquoi une arme est ou n'est pas éligible.

\- Les mini-boss doivent être annoncés et leurs attaques télégraphiées ; leur difficulté doit être supérieure à celle d'un ennemi d'élite ordinaire.

\- Pour la première version jouable, prévoir un seul mini-boss et au moins une évolution testable sur un Instrument.

\- Éviter de rendre l'évolution obligatoire pour terminer une run : elle est une récompense puissante, pas une condition de victoire.



**## 12. Boss**



**### Boss envisagés**

\- **\*\*Le Portrait Sans Visage :\*\*** masque géant flottant, projectiles, zones explosives et invocation de Masques Fêlés. Boss recommandé pour la V1.

\- **\*\*Le Conservateur Déchu :\*\*** assemblage de cadres et de chevalets, frappes au sol et protection de points faibles.

\- **\*\*L'Œuvre Inachevée :\*\*** entité humanoïde abstraite qui modifie les zones dangereuses ; boss final futur.



**### Règles**

\- Barre de santé visible.

\- Au moins trois motifs d'attaque distincts.

\- Attaques majeures télégraphiées visuellement ou par le son.

\- Attaques évitables par déplacement ou esquive, sauf exception annoncée.

\- Effets du joueur ne masquant pas les zones dangereuses.

\- Conditions de victoire fiables et vérifiables.

\- Les apparitions ordinaires cessent dès le début du combat de boss.

\- **\*\*Recommandation à valider :\*\*** éliminer les ennemis communs déjà présents au début du combat afin de conserver une arène lisible.



**## 13. Cartes et environnement**



**### V1 — Musée des Ombres**

Grande galerie avec tableaux déchirés, statues, colonnes, cadres corrompus, sol de pierre et flaques d'encre. Prévoir une arène suffisamment ouverte pour esquiver et lire les attaques. Les obstacles doivent varier les déplacements sans piéger le joueur ou bloquer constamment la caméra.



**### Cartes futures**

1\. **\*\*Atelier des Damnés :\*\*** chevalets, pots de peinture, toiles inachevées et zones d'encre.

2\. **\*\*Corridor Impossible :\*\*** architecture répétitive, cadres flottants et portails.

3\. **\*\*Galerie des Portraits :\*\*** miroirs, statues sans visage et apparitions surprises.

4\. **\*\*Bassin des Pigments :\*\*** réserve souterraine, rivières de peinture et zones instables.

5\. **\*\*Toile Originelle :\*\*** monde abstrait peint, changements visuels pendant le boss final.



Réutiliser matériaux, accessoires, éclairages et éléments de décor. Varier d'abord la disposition et la lumière plutôt que produire des modèles uniques pour chaque carte.



**## 14. Durée et difficulté d'une run**



Durée cible : 20 à 30 minutes, boss compris. Le prototype peut être plus court pour accélérer les tests.



\| Temps indicatif | Phase | Contenu |

\|---|---|---|

\| 00:00–04:00 | Découverte | Ennemis simples, Pigment, premières améliorations |

\| 04:00–11:00 | Montée en puissance | Nouveaux rôles et premières élites |

\| 11:00–18:00 | Pression élevée | Hordes plus denses et événements futurs |

\| 18:00–20:00 | Préparation | Derniers choix et annonce du boss |

\| 20:00–25:00 | Boss final | Combat dédié |



Le rythme sera ajusté par les tests. La difficulté doit augmenter par le nombre et la variété des ennemis, leurs motifs d'attaque et les décisions de placement, pas uniquement par des points de vie plus élevés.



**## 15. Progression permanente et sauvegarde**



\- Les victoires et les défaites peuvent donner une ressource permanente selon la durée ou les objectifs atteints.

\- Les personnages et objets peuvent être débloqués.

\- Les bonus permanents de puissance restent modestes afin de préserver l'importance des décisions prises pendant la run.

\- Les conditions de déblocage sont visibles dans les Archives.

\- Nom provisoire de la ressource : **\*\*Fragments de Toile\*\***.

\- Usages possibles : débloquer personnages, ajouter des objets aux pools de récompenses, acheter de petits bonus et compléter les Archives.

\- Le nom, les gains, les coûts et les bonus restent à définir.

\- Les déblocages doivent persister après fermeture du jeu.

\- La sauvegarde ne doit pas restaurer par erreur la position, la santé ou l'équipement temporaire d'une ancienne run.

\- La sauvegarde complète est requise avant la première version publique, mais peut être différée pendant le prototype.



**## 16. Interfaces**



**### Menu principal**

Nouvelle partie, sélection de personnage, Archives, paramètres et quitter. Les options non implémentées doivent être masquées ou indiquées comme indisponibles.



**### HUD**

Afficher au minimum les points de vie, la jauge de Pigment, le niveau, le chronomètre, les icônes d'Instruments et de Reliques, ainsi que la santé du boss.



**### Choix d'amélioration**

Trois options avec nom, type, rang, rareté et effet. Pour une amélioration statistique d'Instrument, afficher la couleur de rareté (vert, bleu, violet ou jaune), le bonus chiffré et, si possible, les valeurs avant/après. La rareté n'annonce jamais une nouvelle mécanique : elle indique seulement un bonus statistique plus ou moins important. Les évolutions d'Instrument sont présentées dans une interface distincte. Le jeu est en pause ; une sélection applique l'effet une seule fois et reprend la partie. Le curseur est visible et déverrouillé.



**### Pause et fin de partie**

\- Échap ouvre ou ferme le menu pause.

\- Le jeu ne continue pas à infliger des dégâts pendant la pause.

\- Les menus libèrent le curseur ; la caméra ne doit pas le recapturer.

\- Victoire et défaite affichent un bilan et des commandes fonctionnelles.

\- Le redémarrage réinitialise ennemis, objets, statistiques temporaires, santé et temps du jeu.

\- Le retour au menu permet de choisir un autre personnage.



**## 17. Architecture Unity recommandée**



**### Structure de dossiers**

\`\`\`text

Assets/

└── \_Project/

    ├── Art/

    │   ├── Characters/

    │   ├── Enemies/

    │   ├── Environments/

    │   ├── Materials/

    │   └── VFX/

    ├── Audio/

    ├── Data/

    │   ├── Characters/

    │   ├── Weapons/

    │   ├── Relics/

    │   ├── Enemies/

    │   └── Bosses/

    ├── Prefabs/

    │   ├── Player/

    │   ├── Weapons/

    │   ├── Enemies/

    │   ├── Pickups/

    │   └── UI/

    ├── Scenes/

    ├── Scripts/

    │   ├── Core/

    │   ├── Player/

    │   ├── Combat/

    │   ├── Weapons/

    │   ├── Relics/

    │   ├── Enemies/

    │   ├── Bosses/

    │   ├── Progression/

    │   ├── UI/

    │   └── SaveSystem/

    └── Settings/

\`\`\`



**### Responsabilités proposées**

\- **\*\*GameManager :\*\*** état général, pause, victoire, défaite et redémarrage.

\- **\*\*RunManager :\*\*** durée, phases et progression temporelle.

\- **\*\*PlayerController :\*\*** déplacement, orientation, saut et esquive.

\- **\*\*PlayerHealth :\*\*** santé, dégâts, invulnérabilité et mort.

\- **\*\*CharacterData :\*\*** données du personnage et passif.

\- **\*\*WeaponData :\*\*** données des Instruments et rangs.

\- **\*\*WeaponController :\*\*** déclenchement des attaques automatiques.

\- **\*\*RelicData :\*\*** données et effets des Reliques.

\- **\*\*ExperienceSystem :\*\*** Pigment et progression de niveau.

\- **\*\*EnemyAI :\*\*** comportement des ennemis.

\- **\*\*EnemySpawner :\*\*** apparitions et arrêt des apparitions.

\- **\*\*BossController :\*\*** attaques, phases et état du boss.

\- **\*\*UpgradeManager :\*\*** génération et application des choix.

\- **\*\*UIManager :\*\*** HUD, pause, menus et écrans de fin.

\- **\*\*SaveSystem :\*\*** progression permanente.



Ce sont des responsabilités, pas une obligation de créer immédiatement une classe par ligne. Employer des \`ScriptableObject\` pour les données si cela simplifie l'ajout de contenu. Garder les comportements dans des composants C# faciles à comprendre.



Éviter un script géant, les recherches d'objets répétées à chaque frame sans nécessité, les valeurs d'équilibrage dispersées, les références Inspector manquantes, les GameManager en double et les changements de noms de champs sérialisés qui cassent les références.



**## 18. Art, effets et audio**



**### Effets prioritaires V1**

\- impact d'arme ;

\- projectile ;

\- zone dangereuse ;

\- collecte de Pigment ;

\- montée de niveau ;

\- télégraphe de boss ;

\- dégâts reçus et mort.



**### Effets futurs**

\- évolutions d'Instruments ;

\- Fractures ;

\- réactions complexes entre pigments ;

\- effets propres aux personnages ;

\- transformations visuelles de carte.



**### Audio V1**

Sons de coups, projectiles, dégâts reçus, collecte, montée de niveau, signal d'attaque du boss, musique d'ambiance et de boss, victoire et défaite. Les sons doivent rester lisibles sans saturation pendant les hordes.



**## 19. Fonctionnalités futures**



À réaliser après validation de la boucle principale :

1\. Les cinq personnages supplémentaires.

2\. Les Instruments et Reliques restants.

3\. Les évolutions d'Instruments.

4\. Les Fractures et événements facultatifs.

5\. Les contrats de restauration.

6\. Les autres cartes.

7\. Les deux boss supplémentaires.

8\. Les Archives et la sauvegarde complète.

9\. Les paramètres avancés et la remappabilité des commandes.

10\. Les modèles, animations, effets et sons finaux.

11\. Les modes ou options de difficulté supplémentaires, si les tests le justifient.



**## 20. Critères d'acceptation de la première version jouable**



\- [ ] Le joueur se déplace à la troisième personne.

\- [ ] Le saut fonctionne au sol, avec gravité correcte et sans double saut accidentel.

\- [ ] L'esquive manuelle possède une courte fenêtre d'invulnérabilité.

\- [ ] La caméra suit le joueur et libère le curseur dans les menus, la pause et les écrans de fin.

\- [ ] Une attaque automatique fonctionne sans commande d'attaque.

\- [ ] Les ennemis poursuivent et attaquent le joueur.

\- [ ] Le joueur reçoit des dégâts, peut mourir et voit l'écran de défaite.

\- [ ] Le redémarrage restaure un état de run propre.

\- [ ] Les ennemis laissent du Pigment et le joueur peut le collecter.

\- [ ] La montée de niveau propose trois choix valides.

\- [ ] Le jeu est en pause pendant les choix et reprend après sélection.

\- [ ] Les limites de six Instruments et six Reliques sont respectées lorsque le système est complet.

\- [ ] Les Instruments ne dépassent pas cinq rangs de base et les rangs modifient réellement les paramètres ou les effets.

\- [ ] Vaincre un mini-boss attribue sa récompense une seule fois et donne un Sceau d'Évolution.

\- [ ] Un Instrument n'évolue que si les conditions de rang et de récompense sont satisfaites.

\- [ ] Si aucune arme n'est éligible, le joueur conserve le Sceau et reçoit quand même la récompense secondaire.

\- [ ] Les apparitions ordinaires cessent au début du combat de boss.

\- [ ] Le boss a des attaques lisibles et peut être vaincu.

\- [ ] La victoire déclenche un écran distinct de la défaite.

\- [ ] Une run peut être terminée sans erreur bloquante.

\- [ ] Les déblocages permanents sont sauvegardés avant la sortie publique complète.

\- [ ] Les performances restent acceptables avec un nombre représentatif d'ennemis et d'effets.



**## 21. Plan de tests manuels**



**### Déplacement**

\- Tester les quatre directions et la diagonale.

\- Tester le saut sur sol plat et près des obstacles.

\- Vérifier collisions et gravité.

\- Tester l'esquive dans plusieurs directions.

\- Vérifier la durée d'invulnérabilité et le délai de récupération.

\- Tester la caméra près des murs et dans les coins.



**### Combat et ennemis**

\- Vérifier que chaque Instrument acquiert une cible valide.

\- Vérifier que les dégâts ne sont pas appliqués plusieurs fois par un même impact.

\- Vérifier cadence, portée et ligne de vue si applicable.

\- Vérifier qu'un ennemi ne donne ses récompenses qu'une fois.

\- Vérifier l'arrêt des apparitions au lancement du boss.



**### Progression**

\- Collecter du Pigment et monter plusieurs niveaux.

\- Choisir des améliorations et vérifier leur effet.

\- Tester les emplacements pleins.

\- Vérifier qu'aucun bonus n'est appliqué deux fois.



**### Interface et fin de partie**

\- Tester pause et reprise.

\- Vérifier que le curseur est utilisable dans les menus.

\- Tester la mort du joueur, la victoire du boss et le redémarrage après les deux résultats.

\- Vérifier la remise à zéro du temps et des états temporaires.



**### Sauvegarde**

\- Débloquer un élément, fermer l'application et relancer.

\- Vérifier que le déblocage persiste.

\- Vérifier qu'aucun état temporaire de la run précédente n'est restauré.



**## 22. Décisions encore ouvertes**



1\. Saut : hauteur, durée, contrôle en l'air et interactions avec les attaques de boss.

2\. Esquive : touche finale, distance, durée d'invulnérabilité et récupération.

3\. Les Instruments attaquent-ils pendant le saut ?

4\. Caméra : sensibilité, limites verticales et comportement près des murs.

5\. Ciblage automatique : ennemi le plus proche, le plus dangereux ou priorité propre à chaque Instrument.

6\. Certains Instruments peuvent-ils tirer à travers les obstacles ?

7\. Fréquence des Instruments, Reliques et bonus de statistiques dans les choix de niveau.

8\. Objets lorsque les emplacements sont pleins : remplacement ou conversion en ressource.

9\. Les Reliques ont-elles des rangs ?

10\. Équilibrage final du système de mini-boss : rang requis (rang 5 proposé), nombre de Sceaux, récompense secondaire et nombre d'évolutions par Instrument.

11\. Gestion exacte des ennemis présents lorsque le boss apparaît.

12\. Nom définitif, gains, coûts et effets de la ressource permanente.

13\. Format et emplacement de sauvegarde ; gestion d'une sauvegarde corrompue.

14\. Dimensions du Musée des Ombres, limites, obstacles et position du boss.

15\. Équilibrage : santé, dégâts, vitesse, XP, fréquence d'apparition et nombre maximal d'ennemis.

16\. Performances cibles et configuration PC minimale.

17\. Options de difficulté.

18\. Le boss est-il toujours obligatoire pour gagner ?



Les décisions prises doivent être consignées ici afin que le document reste la référence unique du projet.



**## 23. Règles de travail pour l'assistant IA**



1\. Lire ce cahier des charges avant toute tâche importante.

2\. Inspecter les scripts réels avant de proposer des modifications.

3\. Séparer les faits confirmés des hypothèses.

4\. Ne pas modifier plusieurs systèmes sans expliquer pourquoi.

5\. Préserver les références sérialisées et les noms de champs Inspector sauf nécessité.

6\. Préférer des changements ciblés et expliquer les fichiers concernés.

7\. Ne jamais prétendre avoir compilé ou testé le jeu si ce n'est pas le cas.

8\. Expliquer les étapes Unity Inspector et Hierarchy à un débutant.

9\. Demander confirmation avant une refonte importante ou l'ajout de dépendances.

10\. Fournir après chaque changement une liste de tests et les résultats attendus.

11\. Ne pas développer les fonctionnalités futures avant validation des priorités.

12\. Répondre en français, tout en gardant les noms techniques C# et Unity en anglais.

13\. Éviter une architecture trop complexe pour le prototype.

14\. Ne pas inventer le contenu d'un script qui n'a pas été consulté.



Pour toute tâche technique non triviale, présenter : objectif, fichiers à inspecter, cause confirmée ou hypothèses, modification minimale, étapes Inspector/Hierarchy, tests manuels et limites restantes.



**## 24. Première tâche après création du projet**



1\. Créer un projet Unity 6 URP et la structure de dossiers.

2\. Créer une scène de test simple.

3\. Configurer le New Input System.

4\. Créer un personnage provisoire capable de se déplacer, sauter et esquiver.

5\. Ajouter la caméra à la troisième personne.

6\. Ajouter une attaque automatique et un ennemi.

7\. Ajouter santé, dégâts, mort, écran de défaite et redémarrage.

8\. Tester les contrôles, collisions, caméra et écrans de fin.

9\. Valider ce socle avant d'ajouter Pigment, niveaux et équipement.



**\*\*Condition de passage :\*\*** aucune progression complexe ne doit être ajoutée avant que déplacement, saut, esquive, caméra, combat élémentaire, mort et redémarrage soient fiables.



**## 25. Résumé exécutif**



BROKEN CANVAS est un survivor roguelite 3D solo sur PC Windows, développé sous Unity 6 URP. Le joueur contrôle un personnage à la troisième personne, se déplace, saute et esquive manuellement tandis que ses Instruments attaquent automatiquement. Pendant une run de 20 à 30 minutes, il collecte du Pigment, monte de niveau et compose un équipement de six Instruments maximum et six Reliques maximum. Les Instruments ont cinq rangs de base ; deux évolutions distinctes sont prévues pour chaque Instrument. Une évolution spéciale se débloque grâce à un Sceau obtenu en vainquant un mini-boss, à une arme au rang 5 et à un niveau de joueur minimal (niveau 10 recommandé, avec 15 ou 20 comme options d'équilibrage). La pression augmente jusqu'à un combat de boss dédié, pendant lequel les apparitions ordinaires cessent.



La première version reste volontairement limitée : un personnage, le Musée des Ombres, trois Instruments, quatre Reliques, quatre ennemis communs, un élite et un boss. Après validation de la boucle principale, le projet pourra s'étendre aux six personnages, à l'arsenal complet, aux évolutions, aux Fractures, aux autres cartes et à la progression permanente.



La priorité est de construire une base stable, lisible et testable, puis d'ajouter le contenu par étapes sans perdre l'identité artistique de BROKEN CANVAS.



---

# ADDENDUM DE SPÉCIFICATION — VERSION 1.3

> **Statut de cet addendum :** proposition détaillée destinée à guider la production. Les valeurs numériques constituent une baseline de prototype, pas des résultats de tests. Toute modification doit être reportée dans le document afin de conserver une source de vérité unique.

## 26. Décisions de conception consolidées

Cette section rassemble les règles à appliquer de manière cohérente dans l'ensemble du projet.

| Sujet | Règle proposée pour le prototype |
|---|---|
| Perspective | Action 3D à la troisième personne, caméra derrière le personnage |
| Attaques | Les Instruments attaquent automatiquement ; le joueur contrôle déplacement, saut, esquive et caméra |
| Durée cible | 20–30 minutes, boss compris ; mode prototype raccourci pour itérer |
| Emplacements finaux | Jusqu'à 6 Instruments et 6 Reliques ; le MVP peut utiliser une limite plus basse |
| Rangs d'Instrument | 5 rangs statistiques de base au maximum |
| Paliers d'évolution | Niveaux de joueur 10, 15 et 20 : occasions de proposer une évolution si les conditions sont remplies |
| Conditions d'évolution | Rang d'Instrument requis + Sceau d'Évolution obtenu sur un mini-boss/événement prévu + palier de progression valide |
| Rareté | Commun vert 60 %, Peu commun bleu 25 %, Épique violet 12 %, Légendaire jaune 3 % |
| Fonction de la rareté | Augmente uniquement la valeur des bonus statistiques d'une amélioration ordinaire ; ne crée pas de nouvelle mécanique |
| Transformations | Les changements de comportement sont réservés aux évolutions explicites, passifs de personnages et mécaniques dédiées |
| Pigments de terrain MVP | Encre noire et Vermillon uniquement |
| Téléportation | Interdite : aucun portail ni déplacement instantané ; le mouvement reste continu et physique |
| Boss | À son entrée, les apparitions ordinaires cessent ; les ennemis déjà présents suivent une règle de nettoyage définie plus bas |
| Sauvegarde | Sauvegarde permanente séparée de l'état temporaire d'une run |
| Données | Contenus configurables via assets de données ; éviter les valeurs de gameplay dispersées dans les scripts |
| Plateforme | PC Windows, Unity 6, URP, C# et New Input System |

**Clarification sur les paliers d'évolution :** les niveaux 10, 15 et 20 sont des paliers du joueur pendant la run, tandis que les rangs 1 à 5 sont les rangs statistiques de l'Instrument. Ils ne sont pas la même progression. Une évolution ne doit pas apparaître automatiquement à chaque palier : elle est proposée uniquement si ses prérequis sont satisfaits. Si aucun Instrument n'est éligible, le palier produit une récompense de remplacement et ne doit pas bloquer la progression.

## 27. Boucle de gameplay détaillée

### 27.1 Boucle seconde par seconde

1. Le joueur lit la position des ennemis et les avertissements d'attaque.
2. Il se déplace pour maintenir une distance utile, éviter les zones dangereuses ou regrouper les ennemis.
3. Les Instruments évaluent leurs conditions de déclenchement et attaquent automatiquement.
4. Les ennemis se déplacent selon leur rôle et déclenchent leurs attaques télégraphiées.
5. Le joueur saute ou esquive pour éviter une menace, avec un coût de récupération qui empêche l'esquive permanente.
6. Les impacts appliquent dégâts, critiques, statuts et interactions de terrain selon un ordre de résolution unique.
7. Les ennemis vaincus peuvent laisser du Pigment, des objets ou une récompense spécifique.
8. Le joueur choisit entre continuer à attaquer, récupérer du Pigment et se repositionner.

### 27.2 Boucle par minute

- **0–1 min :** entrée en jeu, tutoriel léger intégré à l'action, premières cibles faciles et premier gain d'expérience.
- **1–4 min :** premier choix d'amélioration ; présentation des ennemis de base et du rythme d'attaque.
- **4–8 min :** apparition progressive d'un second rôle ennemi ; la première synergie devient possible.
- **8–12 min :** élite ou événement de récompense ; le joueur commence à spécialiser sa construction.
- **12–18 min :** pression accrue, groupes mixtes et nécessité de choisir ses priorités.
- **18–20 min :** avertissement du boss, possibilité de ramasser le Pigment restant et de finir un choix d'amélioration ; aucune récompense gratuite ne doit être garantie juste avant le boss.
- **20–25 min :** combat de boss, sans apparition ordinaire supplémentaire.
- **Fin :** écran de résultats, calcul des récompenses permanentes, enregistrement des déblocages, puis recommencer ou retourner au menu.

Ces temps sont des repères. Le directeur de vague doit être piloté par un chronomètre de run et des paliers explicites, pas par un nombre de frames ou par le niveau seul.

### 27.3 État de jeu

Le jeu doit utiliser une machine à états explicite, avec au minimum :

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

Chaque transition doit être définie et testable. Par exemple, `Combat -> LevelUpChoice -> Combat` doit mettre en pause les systèmes de jeu une seule fois, conserver les données de run et reprendre sans dupliquer une amélioration. `BossCombat -> Victory` ne peut se produire qu'une fois.

### 27.4 Mort et résultats

À zéro point de vie :
- interrompre immédiatement les nouvelles attaques du joueur ;
- arrêter les apparitions et désactiver les entrées de combat ;
- laisser les effets visuels se terminer brièvement sans infliger de nouveaux dégâts au joueur ;
- afficher le bilan de run ;
- calculer les Fragments de Toile une seule fois ;
- enregistrer les déblocages et statistiques de run ;
- permettre `Recommencer` ou `Menu principal`.

Le redémarrage doit créer un nouvel état de run. Il ne doit pas réutiliser les anciens ennemis, projectiles, effets, minuteries, buffs temporaires ou objets de Pigment.

## 28. Système d'améliorations : règles de tirage complètes

### 28.1 Nombre et catégories de choix

À chaque niveau, présenter **3 choix**. Le générateur construit trois options valides à partir des catégories suivantes :
1. amélioration d'un Instrument possédé ;
2. nouvel Instrument si un emplacement est libre ;
3. amélioration d'une Relique possédée, si les rangs de Relique sont activés ;
4. nouvelle Relique si un emplacement est libre ;
5. bonus statistique générique prévu par la table de récompenses ;
6. évolution d'Instrument, uniquement dans l'interface spéciale et si les conditions sont remplies.

Les évolutions ne doivent pas occuper silencieusement un emplacement d'amélioration ordinaire : elles utilisent une présentation spéciale et des prérequis explicites.

### 28.2 Ordre du générateur

1. Construire la liste des récompenses éligibles en fonction de l'inventaire, des emplacements et des prérequis.
2. Éliminer les options déjà maximisées, incompatibles ou bloquées.
3. Tirer les catégories selon des poids configurables.
4. Pour chaque amélioration statistique d'Instrument, tirer la rareté.
5. Choisir une statistique autorisée pour cet Instrument.
6. Calculer le montant du bonus selon la rareté et la statistique.
7. Éliminer les doublons visuels ou mécaniques dans les trois cartes.
8. Si moins de trois options valides subsistent, compléter avec des bonus génériques autorisés.
9. Si aucune option n'est disponible, utiliser une option de secours définie (par exemple un petit bonus de santé ou une ressource de run), sans bloquer le joueur.

Le générateur doit utiliser une graine aléatoire de run pour faciliter la reproduction des bugs. Le choix appliqué est validé une seule fois, puis l'écran se ferme.

### 28.3 Probabilités de rareté

| Rareté | Couleur | Probabilité de base | Règle |
|---|---|---:|---|
| Commun | Vert | 60 % | Bonus statistique modeste |
| Peu commun | Bleu | 25 % | Bonus statistique moyen |
| Épique | Violet | 12 % | Bonus statistique élevé, éventuellement sur deux stats |
| Légendaire | Jaune | 3 % | Bonus statistique exceptionnel, sans mécanique nouvelle |

Ces probabilités s'appliquent aux améliorations statistiques d'Instrument, pas à toutes les cartes du niveau. Une nouvelle arme, une Relique, un soin ou une évolution ne reçoit pas artificiellement une rareté d'arme si ce n'est pas prévu par sa catégorie.

Pour le prototype, conserver des probabilités fixes. La chance de personnage ou de Relique pourra modifier les probabilités plus tard, avec une limite explicite et des tests. Ne pas augmenter automatiquement la rareté simplement parce que la run avance tant que ce comportement n'a pas été testé.

### 28.4 Montants de bonus proposés

Les valeurs ci-dessous sont des points de départ pour les statistiques **de base de l'Instrument**, avant multiplicateurs de personnage ou de Relique.

| Rareté | Dégâts | Cadence | Portée | Taille / rayon | Vitesse projectile |
|---|---:|---:|---:|---:|---:|
| Commun | +10 % | +8 % | +10 % | +10 % | +10 % |
| Peu commun | +18 % | +14 % | +18 % | +18 % | +18 % |
| Épique | +25 % | +20 % | +25 % | +25 % | +25 % |
| Légendaire | +40 % | +25 % | +35 % | +35 % | +35 % |

Règles :
- Les valeurs doivent être ajustées par Instrument ; ne proposer que les statistiques réellement utiles.
- Une carte peut donner une statistique principale et, pour les raretés Épique ou Légendaire, une statistique secondaire plus petite.
- Les améliorations de cadence ne doivent pas dépasser la limite technique définie par arme.
- Les bonus de durée s'appliquent uniquement aux effets persistants.
- Les bonus de critique sont exprimés en points de pourcentage, pas en pourcentage relatif, et doivent être plafonnés.
- Une amélioration ne peut jamais produire une valeur négative ou NaN.
- La carte doit afficher la valeur du bonus et la valeur finale estimée.

### 28.5 Cumul des statistiques

Règle de calcul recommandée :
- **Bonus additifs d'une même famille :** les bonus de dégâts d'améliorations sont additionnés dans un même multiplicateur d'amélioration.
- **Multiplicateurs de familles distinctes :** les multiplicateurs de personnage, d'amélioration et de synergie se multiplient entre eux, si cette séparation est nécessaire.
- **Cadence :** utiliser un intervalle entre attaques ou une fréquence par seconde, mais pas mélanger les deux représentations.
- **Critique :** `Dégâts moyens = dégâts normaux × (1 - chanceCritique) + dégâtsCritiques × chanceCritique`.
- **Limites :** chaque statistique qui peut casser l'équilibrage possède une limite supérieure documentée.
- **Arrondi :** arrondir uniquement pour l'affichage ; conserver la précision interne.

L'interface doit expliquer les valeurs finales sans exposer au joueur des formules internes complexes.

### 28.6 Éligibilité d'un Instrument

- Un nouvel Instrument n'est proposé que si le joueur dispose d'un emplacement libre et que cet Instrument n'est pas déjà possédé, sauf règle explicite de doublon.
- Un Instrument déjà possédé peut recevoir une amélioration jusqu'au rang maximum.
- Un Instrument au rang maximum ne peut plus recevoir de rang statistique ordinaire, mais peut rester éligible aux évolutions si les prérequis sont remplis.
- Si tous les Instruments sont au rang maximum, les options correspondantes sont retirées du pool.
- Les Reliques suivent la même règle d'éligibilité si leurs rangs sont activés.
- Les options uniques déjà obtenues ne peuvent pas être proposées une seconde fois.
- Le système doit afficher un motif clair lorsqu'une récompense n'est pas disponible, mais ne pas remplir l'écran de messages techniques.

### 28.7 Rangs d'Instrument et paliers d'évolution

Les rangs 1 à 5 sont des améliorations de base de l'Instrument. Les niveaux de joueur 10, 15 et 20 ouvrent des paliers d'évolution. Une évolution requiert :
- un Instrument éligible ;
- le rang minimum de l'Instrument défini dans sa fiche ;
- le Sceau d'Évolution ou la récompense de boss/mini-boss exigée par la fiche ;
- le palier de niveau du joueur approprié ;
- une évolution pas déjà choisie.

Si plusieurs évolutions sont éligibles, proposer uniquement celles compatibles avec les conditions et les synergies. Le joueur choisit entre les branches disponibles ; le système ne doit pas choisir automatiquement pour lui. Les deux évolutions d'un Instrument sont des transformations distinctes, pas des raretés.

Si le joueur atteint le niveau 10, 15 ou 20 sans évolution éligible, afficher une récompense alternative. Ne pas supprimer ni consommer un Sceau sans récompense.

## 29. Matrice de statistiques par type d'Instrument

Chaque Instrument doit déclarer explicitement les statistiques compatibles. Baseline recommandée :

| Type d'Instrument | Dégâts | Cadence | Portée | Taille | Vitesse projectile | Durée | Critique |
|---|---|---|---|---|---|---|---|
| Projectile direct | Oui | Oui | Oui | Oui | Oui | Non, sauf effet persistant | Oui si applicable |
| Attaque de mêlée | Oui | Oui | Oui | Oui | Non | Non | Oui |
| Zone persistante | Oui | Oui ou recharge | Oui | Oui | Selon projectile | Oui | Selon design |
| Aura / effet autour du joueur | Oui | Selon cadence | Oui, rayon d'aura | Oui | Non | Selon effet | Selon design |
| Fil / lien | Oui | Selon activation | Oui | Selon nombre de cibles ou rayon | Non | Oui si lien temporaire | Selon design |
| Objet invoqué | Oui | Cadence d'attaque | Rayon de détection | Oui | Selon projectile | Durée d'invocation | Selon design |

La matrice sert de guide, pas de permission automatique : la fiche d'un Instrument est la source de vérité.

## 30. Peinture au sol : spécification de gameplay

### 30.1 Règles générales

Une surface de peinture est une zone de gameplay temporaire créée par une arme, un personnage, un ennemi ou un événement. Elle possède :
- identifiant de type de pigment ;
- position et rayon ;
- durée restante ;
- source de création ;
- faction / propriétaires affectés ;
- effets activés ;
- priorité de superposition ;
- budget d'effets et limite de cumul.

Valeurs de prototype :
- durée standard : 5 secondes ;
- durée maximale après extension : 8 secondes ;
- rayon initial typique : 1,5 mètre, ajustable selon la source ;
- nombre maximal de surfaces actives du joueur : 20 ;
- au-delà de la limite, remplacer ou rafraîchir la surface la plus ancienne du même type selon la règle de la source ;
- pas de création d'un nouveau collider physique pour chaque éclaboussure visuelle ; utiliser une représentation de zone légère et un contrôle de chevauchement.

Ces valeurs doivent être testées dans l'arène réelle. L'effet visuel peut s'estomper avant la disparition logique, à condition que l'état de gameplay reste lisible.

### 30.2 Encre noire — type de terrain initial

- Le Patineur gagne de l'accélération sur une surface noire compatible, jusqu'à un plafond de vitesse.
- L'Encré peut récupérer une petite quantité de ressource en traversant ses propres surfaces si cette mécanique est activée.
- Les ennemis ne sont pas ralentis par défaut : l'encre noire ne doit pas devenir un contrôle universel.
- Les surfaces noires ne doivent pas modifier la friction du moteur physique de manière imprévisible ; appliquer le bonus via le contrôleur de déplacement.

### 30.3 Vermillon — type de terrain initial

- Peut appliquer un effet de marquage ou de dégâts sur les ennemis qui entrent dans la zone, selon la source qui a créé la surface.
- Un même ennemi ne peut recevoir le même déclenchement qu'une fois par intervalle défini, proposé à 0,75 seconde.
- Les déclenchements doivent être indépendants du nombre de colliders qui se chevauchent.
- Les dégâts périodiques ne se cumulent pas sans limite : renouveler la durée ou appliquer un niveau d'intensité plafonné.

### 30.4 Superposition des pigments

Pour le MVP, seules l'encre noire et le vermillon existent en gameplay. Si les deux se chevauchent :
- les deux effets peuvent coexister si leurs effets sont compatibles ;
- un même effet ne peut pas être appliqué plusieurs fois par frame ;
- les dégâts périodiques d'un même type ne se cumulent pas sans plafond ;
- l'effet de déplacement est résolu séparément de l'effet offensif ;
- le rendu doit afficher les deux couleurs de manière lisible sans couvrir le personnage ou les télégraphes ennemis.

Les pigments bleu, or ou contaminé sont futurs et ne doivent pas être codés comme des systèmes cachés dans le MVP.

### 30.5 Interactions entre surface et personnages

| Personnage / rôle | Interaction proposée |
|---|---|
| Exilé | Aucun bonus spécial de terrain dans le MVP ; sert de référence de contrôle |
| Virtuose | Peut traverser les surfaces sans ralentissement spécial ; ses attaques de précision restent lisibles |
| Graveur | Les lignes de tir et les projectiles doivent rester lisibles au-dessus des surfaces |
| Alchimiste | Peut déclencher les réactions de pigment définies par sa compétence, avec un plafond de fréquence |
| Sculpteur | Ne doit pas perdre le contrôle de ses attaques lourdes sur les surfaces |
| Restauratrice | Les fils et marquages doivent rester lisibles sur les surfaces |
| Patineur (futur) | Utilise l'encre noire pour conserver ou gagner de l'inertie |
| Encré (futur) | Récupère une ressource sur ses surfaces compatibles |
| Marionnettiste (futur) | Peut exploiter les surfaces comme support visuel, mais aucun bonus n'est implicite |

### 30.6 Performance et lisibilité

- Séparer les surfaces de gameplay des particules décoratives.
- Réutiliser les objets visuels par pooling si le profilage confirme un gain.
- Éviter un système de physique par goutte de peinture.
- Limiter les déclenchements de statut par cible et par intervalle.
- Réduire les particules décoratives avant de réduire les télégraphes d'attaque.
- Les avertissements de boss doivent toujours être visibles au-dessus des effets de terrain.
- Tester l'arène avec la densité maximale d'ennemis, projectiles et surfaces prévue.

## 31. Synergies : règles et taxonomie

### 31.1 Catégories de synergie

1. **Instrument + Instrument :** les attaques peuvent se compléter ou déclencher un effet secondaire limité.
2. **Instrument + Relique :** la Relique amplifie une statistique ou une condition de l'Instrument.
3. **Instrument + personnage :** le passif du personnage favorise un style de jeu sans rendre les autres Instruments inutiles.
4. **Instrument + terrain :** une attaque exploite une surface ou un pigment.
5. **Statut + statut :** réaction explicite entre deux statuts différents, si elle est autorisée.
6. **Personnage + terrain :** le terrain modifie déplacement ou ressource selon une règle propre au personnage.

### 31.2 Contrat d'une synergie

Chaque synergie doit définir :
- identifiants des contenus requis ;
- conditions d'activation ;
- effet exact et durée ;
- priorité si plusieurs synergies se déclenchent ensemble ;
- fréquence maximale ;
- limites de propagation ;
- feedback visuel et sonore ;
- règles de cumul avec les autres bonus ;
- comportement lorsque l'un des prérequis est retiré ou remplacé.

### 31.3 Règles anti-boucle

- Une réaction déclenchée par une autre réaction ne peut pas se réactiver indéfiniment.
- Les dégâts propagés ne peuvent pas déclencher une propagation identique en cascade, sauf règle explicite avec profondeur maximale.
- Chaque élimination ne déclenche qu'une seule récompense d'élimination.
- Une explosion qui tue plusieurs ennemis ne doit pas générer une quantité illimitée de surfaces ou de projectiles.
- La propagation d'un statut est plafonnée par cible et par seconde.
- Les effets issus d'une même source conservent un identifiant d'origine pour éviter les doubles déclenchements.

### 31.4 Exemples de synergies à tester

| Combinaison | Effet proposé | Garde-fou |
|---|---|---|
| Flacon de Vermillon + surface de vermillon | Augmente légèrement la durée ou le rayon des surfaces créées | Durée et rayon plafonnés |
| Éclats de Fusain + ennemis marqués | Les projectiles peuvent privilégier les cibles marquées | Priorité de ciblage, pas de dégâts gratuits infinis |
| Palette Catalytique + statut existant | Peut déclencher une réaction de pigment | Temps de recharge par cible |
| Fil de Restauration + ennemis liés | Une partie limitée des dégâts se transmet aux cibles reliées | Propagation sans chaîne récursive |
| Maillet de Marbre Noir + groupe d'ennemis | Onde de choc plus efficace lorsque plusieurs ennemis sont proches | Rayon et multiplicateur plafonnés |
| Bottes du Restaurateur + surfaces noires | Bonus de mobilité réduit mais régulier sur surface compatible | Ne dépasse pas le plafond de vitesse |

Ces combinaisons sont des propositions de design à tester ; elles ne doivent pas être considérées comme déjà équilibrées.

## 32. Système de vagues et difficulté dynamique

### 32.1 Directeur de vague

Le directeur de vague utilise le temps de run comme source principale de difficulté. Il sélectionne les groupes selon :
- phase de la run ;
- budget de menace disponible ;
- nombre d'ennemis vivants ;
- type de zone ;
- présence d'un événement ou d'un élite ;
- proximité du boss final.

Il ne doit pas apparaître d'ennemis si la limite globale est atteinte. Si le joueur se trouve près d'un écran de choix, le temps de combat doit être réellement en pause.

### 32.2 Budget de menace

Chaque ennemi reçoit un coût de menace. Le directeur choisit une combinaison dont le coût total ne dépasse pas le budget de la phase. Cette méthode évite de gérer uniquement la difficulté par les points de vie.

| Phase | Temps indicatif | Objectif | Composition indicative |
|---|---|---|---|
| A | 0–4 min | Apprendre et se construire | Gribouilleurs / poursuivants simples |
| B | 4–8 min | Ajouter une décision tactique | Groupes mixtes simples |
| C | 8–12 min | Tester la construction | Première élite ou événement de récompense |
| D | 12–18 min | Pression et priorités | Groupes mixtes, tireurs et ennemis résistants |
| E | 18–20 min | Préparer le boss | Menaces lisibles, annonce de boss |
| F | 20–25 min | Épreuve finale | Boss seul avec ses attaques et phases |

### 32.3 Paramètres de difficulté

La difficulté peut ajuster :
- budget de menace ;
- cadence d'apparition ;
- proportion de chaque archétype ;
- santé et dégâts dans une plage limitée ;
- nombre d'attaques simultanées ;
- fréquence des élites.

Elle ne doit pas se contenter d'augmenter toutes les statistiques de façon uniforme. La vitesse des projectiles et la taille des télégraphes doivent rester équitables.

### 32.4 Apparition du boss

Au début de l'introduction du boss :
- arrêter les nouvelles apparitions ordinaires ;
- désactiver les événements qui créeraient d'autres ennemis ;
- appliquer une règle fixe aux ennemis déjà présents : ils sont retirés proprement ou terminent leur cycle puis disparaissent avant l'activation du boss ;
- nettoyer ou laisser expirer les projectiles ordinaires selon une durée courte et prévisible ;
- afficher le nom du boss, sa barre de santé et un avertissement sonore ;
- démarrer le combat seulement quand l'arène et le boss sont prêts.

Le choix recommandé pour le MVP est de retirer proprement les ennemis ordinaires restants au lancement de l'introduction, avec une courte animation de dissolution, afin d'éviter que le boss soit entouré d'une horde non prévue.

## 33. Architecture Unity 6 URP

### 33.1 Principes d'architecture

- Séparer données, logique de gameplay, présentation et sauvegarde.
- Les fiches de contenu doivent être configurables sans modifier la logique centrale.
- Les systèmes centraux ne doivent pas référencer directement des objets de scène temporaires après la fin de leur cycle de vie.
- Les règles de dégâts, de rareté et de progression doivent être testables de manière isolée.
- Les événements doivent être désabonnés correctement lors de la destruction ou de la réutilisation des objets.
- Ne pas créer un système générique complexe tant que le prototype n'en a pas besoin.

### 33.2 Organisation de dossiers proposée

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
    Scripts/
      Core/
      Player/
      Camera/
      Combat/
      Instruments/
      Relics/
      Enemies/
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
    Tests/
      EditMode/
      PlayMode/
```

### 33.3 Responsabilités proposées

| Système | Responsabilité |
|---|---|
| RunState / RunController | État de la run, transitions, victoire/défaite |
| PlayerController | Déplacement, saut, esquive, réception des entrées |
| CameraController | Rotation, suivi, collision caméra |
| Health / DamageReceiver | Santé et réception des dégâts |
| DamageResolver | Résolution des dégâts, critiques, résistances et sources |
| InstrumentController | Gestion des Instruments équipés et de leurs rangs |
| InstrumentDefinition | Données statiques d'un Instrument |
| UpgradeGenerator | Éligibilité, catégories de récompense et tirages |
| ExperienceSystem | Pigment, seuils et niveaux |
| EvolutionSystem | Préconditions et choix d'évolution |
| RelicController | Reliques équipées et effets |
| StatusEffectController | Application, durée, cumul et expiration des statuts |
| PaintSurfaceSystem | Création, durée, chevauchement et limite des surfaces |
| WaveDirector | Budget de menace et sélection des apparitions |
| EnemyBrain | Comportement individuel d'un ennemi |
| BossController | Phases, télégraphes, attaques et transitions |
| SaveService | Lecture/écriture de la progression permanente |
| UI Presenter | Affichage des données sans posséder les règles de gameplay |

Les noms sont indicatifs. Avant d'implémenter, inspecter les scripts réels et préserver les noms/références Inspector existants lorsque cela est nécessaire.

### 33.4 Données configurables

Utiliser des ScriptableObjects pour les définitions statiques lorsque c'est utile : statistiques de base, icône, prefab, statistiques éligibles, poids de récompense et prérequis. Les valeurs qui changent pendant la run restent dans des états d'exécution séparés ; ne pas modifier l'asset de définition pour stocker l'état d'une run.

### 33.5 Gestion des dégâts

Ordre recommandé :
1. valider que la source et la cible sont actives ;
2. vérifier immunité / invulnérabilité ;
3. calculer les dégâts de base ;
4. appliquer les bonus additifs de la famille ;
5. appliquer les multiplicateurs de famille ;
6. résoudre le critique ;
7. appliquer résistance et réduction ;
8. appliquer les règles de bouclier ;
9. réduire la santé ;
10. déclencher les effets d'impact, statuts et élimination une seule fois.

Cet ordre doit être stable et documenté. Les exceptions, par exemple dégâts fixes ou dégâts qui ignorent l'armure, doivent être explicitement déclarées.

### 33.6 Pooling et performances

Le pooling peut être utilisé pour projectiles, impacts, ennemis communs, objets de Pigment et effets de surface si les tests démontrent son utilité. Tout objet réutilisé doit réinitialiser santé, cible, timers, propriétaire, statuts, vitesse et abonnements aux événements.

Ne pas fixer un objectif de FPS irréaliste sans connaître le matériel cible. Baseline à tester : 60 FPS sur le PC de référence choisi, dans une scène avec le nombre cible d'ennemis, projectiles et surfaces. Définir le matériel minimum avant la sortie publique.

## 34. Interface et expérience du joueur

### 34.1 HUD en combat

Toujours afficher :
- santé actuelle et maximale ;
- niveau et jauge de Pigment ;
- chronomètre de run ;
- icônes d'Instruments et de Reliques équipés ;
- temps de recharge ou jauges uniquement si elles sont utiles à la décision ;
- santé du boss pendant le combat ;
- avertissements de danger proches du personnage ou dans le monde.

Éviter les chiffres permanents pour chaque dégât si cela masque le combat. Prévoir des options pour réduire le tremblement de caméra, les flashs, les particules et les nombres flottants.

### 34.2 Cartes d'amélioration

Chaque carte doit afficher :
- nom et icône ;
- catégorie (Instrument, Relique, bonus ou évolution) ;
- rareté et couleur pour les améliorations statistiques d'Instrument ;
- description courte ;
- bonus chiffré ;
- valeurs avant/après quand disponibles ;
- conditions manquantes si une évolution est verrouillée.

La rareté est uniquement un indicateur de quantité statistique pour les améliorations ordinaires. Une carte légendaire ne doit pas suggérer une nouvelle attaque ou une transformation si elle ne fait qu'augmenter les dégâts.

### 34.3 Pause et menus

- Échap ouvre la pause pendant la run.
- Les choix d'amélioration mettent réellement en pause le monde.
- Le curseur est visible et utilisable dans les menus.
- La fermeture d'un menu restaure l'état précédent, pas automatiquement le combat si l'écran précédent était un autre écran de choix.
- Les boutons désactivés doivent avoir une explication.
- Toute action de sortie de run doit être confirmée si elle entraîne une perte de progression temporaire.

### 34.4 Accessibilité et lisibilité

- Ne jamais utiliser la couleur seule pour distinguer rareté, statut ou danger.
- Les télégraphes d'ennemis doivent avoir une forme et une animation distinctes.
- Ajouter des réglages de sensibilité caméra, volume musique/SFX, tremblement et intensité des effets.
- Les textes doivent rester lisibles à la résolution cible.
- Tester les effets de peinture avec un écran chargé pour vérifier que le personnage et les attaques ennemies restent visibles.

## 35. Statuts : contrat commun

Chaque statut doit définir les champs suivants :
- identifiant stable ;
- nom affiché ;
- durée de base ;
- source autorisée ;
- cible autorisée ;
- règle de cumul ;
- fréquence de tick ;
- résistance ou immunité éventuelle ;
- feedback visuel et audio ;
- effets de fin ;
- interactions avec les autres statuts.

### 35.1 Règles de cumul recommandées

- **Renouveler la durée :** réapplique la durée sans augmenter l'intensité.
- **Intensité plafonnée :** augmente une valeur jusqu'à un plafond explicite.
- **Instances indépendantes :** seulement si l'effet exige réellement plusieurs sources ; limiter le nombre d'instances par cible.
- **Remplacement :** un statut plus fort remplace un statut plus faible du même type.
- **Immunité temporaire :** après certains contrôles de foule, une cible peut recevoir une courte résistance afin d'éviter les verrouillages permanents.

Ne pas mélanger ces règles implicitement. Chaque statut choisit une règle et l'indique dans sa fiche.

### 35.2 Catalogue initial recommandé

| Statut | Effet proposé | Règle de cumul |
|---|---|---|
| Marqué | Rend une cible éligible aux effets qui exigent un marquage | Renouvelle la durée |
| Contamination vermillon | Dégâts périodiques modérés | Intensité plafonnée, cadence fixe |
| Ralentissement | Réduit la vitesse de déplacement | La valeur la plus forte prévaut, durée renouvelable |
| Vulnérabilité | Augmente les dégâts reçus | Valeur plafonnée, ne se cumule pas sans limite |
| Étourdissement | Interrompt une action non protégée | Durée réduite ou immunité temporaire sur élites/boss |

Le catalogue complet peut évoluer, mais le MVP doit n'implémenter que les statuts requis par ses personnages, Instruments et ennemis.

## 36. Sauvegarde et déblocages permanents

### 36.1 Séparer run et méta-progression

**État temporaire de run :**
- santé actuelle ;
- niveau et Pigment ;
- Instruments, rangs, évolutions ;
- Reliques équipées ;
- minuteur, ennemis et boss ;
- surfaces et statuts actifs.

Cet état est réinitialisé au lancement d'une nouvelle run, sauf si une fonction de reprise de run est ajoutée explicitement plus tard.

**État permanent :**
- personnages débloqués ;
- contenu découvert dans les Archives ;
- défis accomplis ;
- paramètres ;
- statistiques globales ;
- Fragments de Toile ;
- version de schéma de sauvegarde.

### 36.2 Règles de sauvegarde

- Écrire la progression permanente après une fin de run et après chaque déblocage important.
- Ne pas écrire l'état temporaire de run dans la sauvegarde permanente par accident.
- Vérifier les données lues avant de les appliquer.
- Prévoir des valeurs par défaut si la sauvegarde manque ou si un champ est inconnu.
- Prévoir un numéro de version de schéma pour les migrations.
- En cas de fichier corrompu, conserver une copie de secours si possible et afficher un message compréhensible.
- Ne jamais effacer silencieusement la sauvegarde sans action explicite du joueur.

### 36.3 Déblocages

Chaque déblocage doit avoir :
- un identifiant stable ;
- une condition ;
- une récompense ;
- une progression visible ;
- une notification ;
- une règle de sauvegarde.

Les bonus permanents de puissance doivent rester modestes. Prioriser l'ouverture de nouvelles options de construction plutôt que des augmentations de dégâts permanentes qui rendraient le contenu trivial.

## 37. Périmètre de la première version jouable (MVP)

### 37.1 Objectif

Prouver qu'une run complète est amusante, compréhensible et techniquement stable avant d'élargir le contenu. Le MVP n'est pas le jeu complet ; c'est une tranche jouable permettant de valider le cœur du projet.

### 37.2 Contenu inclus

- 1 personnage jouable : l'Exilé ;
- 3 Instruments de départ ou déblocables pendant la run ;
- 4 Reliques maximum dans le contenu du MVP, même si la limite finale reste 6 ;
- 4 ennemis communs aux rôles distincts ;
- 1 élite ;
- 1 boss final avec au moins 3 attaques lisibles ;
- 1 arène : Musée des Ombres ;
- déplacement, saut, esquive et caméra ;
- attaques automatiques ;
- Pigment, expérience et choix à 3 options ;
- raretés d'amélioration statistiques ;
- au moins une évolution d'Instrument testable ;
- une version limitée des surfaces encre noire / vermillon si elle est nécessaire à l'identité du prototype ;
- écran de pause, victoire, défaite et bilan ;
- sauvegarde minimale des paramètres et du résultat permanent si nécessaire au test.

### 37.3 Contenu explicitement reporté

- personnages supplémentaires non nécessaires à la validation ;
- toutes les évolutions des dix Instruments ;
- pigments supplémentaires ;
- plusieurs cartes ;
- événements aléatoires complexes ;
- méta-progression étendue ;
- défis quotidiens ou contenu en ligne ;
- systèmes de téléportation/portails (interdits par la direction retenue) ;
- tout système qui ne contribue pas à prouver le combat, la progression ou la lisibilité.

### 37.4 Critères de sortie du MVP

Le MVP est prêt pour une première session de test externe lorsque :
- une run peut être terminée de bout en bout ;
- aucune erreur bloquante n'empêche de gagner ou de perdre ;
- les améliorations sont appliquées une seule fois et affichent les bons chiffres ;
- le boss est lisible et vainquable ;
- les performances sont acceptables dans la scène cible ;
- au moins trois testeurs peuvent expliquer la boucle principale sans aide après une partie ;
- les bugs bloquants et critiques connus sont corrigés ou documentés.

## 38. Fiches de contenu normalisées

Chaque contenu doit avoir une fiche stable. Les identifiants internes restent en anglais ou en PascalCase, stables entre versions ; les noms affichés peuvent rester en français.

### 38.1 Fiche Personnage

- **ID :**
- **Nom affiché :**
- **Rôle :**
- **Difficulté :**
- **Instrument de départ :**
- **Statistiques de base :** santé, vitesse, puissance, récupération, autres valeurs pertinentes.
- **Passif :** déclencheur, effet, durée, limite et contrepartie.
- **Compétence/signature :** condition d'activation, effet, durée, recharge ou coût.
- **Interaction avec le terrain :**
- **Forces :**
- **Faiblesses :**
- **Synergies recommandées :**
- **Feedback visuel/audio :**
- **Conditions de déblocage :**
- **Critères de test :**

### 38.2 Fiche Instrument

- **ID :**
- **Nom affiché :**
- **Type d'attaque :**
- **Cible :** plus proche, plus dangereuse, directionnelle ou règle spécifique.
- **Statistiques de base :** dégâts, cadence/recharge, portée, taille, vitesse projectile, durée, critique.
- **Rang maximum :** 5 par défaut.
- **Statistiques améliorables :**
- **Comportement à chaque rang :** valeurs ou formule.
- **Raretés compatibles :** bonus statistiques possibles, pas d'effet mécanique unique.
- **Évolution A :** nom, prérequis, comportement, limites, synergies.
- **Évolution B :** nom, prérequis, comportement, limites, synergies.
- **Interaction de terrain :**
- **Effets visuels/audio :**
- **Cas limites :**
- **Critères de test :**

### 38.3 Fiche Évolution

- **ID :**
- **Instrument requis :**
- **Nom affiché :**
- **Palier de joueur :** 10, 15 ou 20, selon la branche.
- **Rang minimum de l'Instrument :**
- **Sceau ou événement requis :**
- **Conditions de synergie :**
- **Comportement avant/après :**
- **Statistiques modifiées :**
- **Nouvelles règles de déclenchement :**
- **Limites anti-boucle :**
- **Feedback visuel/audio :**
- **Méthode de choix :**
- **Critères de test :**

### 38.4 Fiche Relique

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

### 38.5 Fiche Ennemi

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
- **Feedback visuel/audio :**
- **Critères de test :**

### 38.6 Fiche Boss

- **ID :**
- **Nom affiché :**
- **Arène :**
- **Santé et résistances :**
- **Phase d'introduction :**
- **Phases de combat :**
- **Attaques :** télégraphe, dégâts, zone, récupération, contre-jeu.
- **Transitions de phase :**
- **Règles d'invulnérabilité :** à éviter ou à limiter à des transitions courtes.
- **Gestion des ennemis présents :**
- **Conditions de victoire :**
- **Récompenses :**
- **Effets visuels/audio :**
- **Critères de test :**

### 38.7 Exemple de fiche — Exilé

- **ID :** `Exile`
- **Nom affiché :** L'Exilé
- **Rôle :** polyvalent, personnage de référence pour l'équilibrage.
- **Instrument de départ :** Pinceau du Dernier Trait.
- **Passif :** les éliminations augmentent temporairement les dégâts de l'Instrument ; la durée, le bonus maximal et la règle de rafraîchissement doivent être des paramètres configurables.
- **Signature proposée :** une esquive parfaitement synchronisée déclenche une vague d'encre ou renforce le prochain trait ; à activer seulement si cette signature est retenue pour le kit final.
- **Terrain :** aucune interaction spéciale obligatoire dans le MVP.
- **Forces :** prise en main, polyvalence, référence d'équilibrage.
- **Faiblesses :** moins spécialisé que les personnages futurs.
- **Tests :** le bonus ne doit pas s'empiler sans limite ; le joueur doit comprendre visuellement quand le passif est actif.

### 38.8 Exemple de fiche — Patineur (futur)

- **ID :** `Skater`
- **Nom affiché :** Le Patineur
- **Rôle :** mobilité, trajectoire et inertie.
- **Instrument de départ proposé :** Rouleau de l'Atelier.
- **Passif :** glisse sur des surfaces noires compatibles ; la vitesse augmente jusqu'à un plafond.
- **Signature :** dérapage directionnel qui trace une ligne de peinture et peut toucher les ennemis traversés.
- **Coûts/limites :** les virages serrés réduisent l'inertie ; s'arrêter fait perdre le bonus.
- **Téléportation :** aucune.
- **Tests :** vitesse maximale, collision contre murs, virage, arrêt, contrôle au clavier et lisibilité de la trace.

## 39. Plan de test détaillé et critères d'acceptation

### 39.1 Niveaux de test

- **EditMode :** formules de dégâts, probabilités, seuils d'expérience, règles de cumul et éligibilité.
- **PlayMode :** déplacement, esquive, combat, apparition des vagues, interfaces et transitions d'état.
- **Tests de scène :** une run complète avec contenu représentatif.
- **Tests de régression :** rejouer les scénarios après chaque modification d'un système central.
- **Tests de performance :** densité maximale de combat, projectiles, surfaces et effets.
- **Tests utilisateurs :** lisibilité, compréhension des cartes, rythme et difficulté perçue.

### 39.2 Tests des probabilités

Pour une table 60/25/12/3 :
- vérifier que la somme des probabilités vaut 100 % ;
- vérifier que chaque résultat possible peut être tiré ;
- exécuter une simulation automatisée d'au moins 100 000 tirages ;
- comparer les fréquences observées aux probabilités attendues avec une tolérance statistique définie ;
- vérifier que les modificateurs de Chance respectent les plafonds ;
- vérifier que la protection contre la malchance, si activée, se réinitialise correctement ;
- vérifier que la rareté ne modifie pas les comportements de l'arme.

Un test statistique ne garantit pas que chaque petite série de tirages reflète exactement les pourcentages ; il vérifie le comportement sur un grand échantillon.

### 39.3 Tests de combat

- dégâts normaux et critiques ;
- cible tuée exactement une fois ;
- projectile qui touche plusieurs colliders d'un même ennemi ;
- attaque interrompue par la mort de sa source ;
- ennemi mort pendant un effet périodique ;
- statuts qui expirent ou se renouvellent ;
- dégâts de zone et chevauchement des zones ;
- immunité du boss aux contrôles non autorisés ;
- pas de propagation récursive infinie ;
- pas de projectile orphelin après redémarrage.

### 39.4 Tests de progression

- passage de niveau et pause ;
- choix de trois cartes valides ;
- inventaire plein ;
- Instrument au rang maximum ;
- aucune option unique disponible ;
- niveau 10/15/20 sans évolution éligible ;
- Sceau d'Évolution conservé si aucune évolution n'est possible ;
- évolution sélectionnée une seule fois ;
- état propre après recommencer.

### 39.5 Tests de peinture

- surface créée et supprimée à l'expiration ;
- limite du nombre de surfaces ;
- renouvellement de surface ;
- joueur entrant et sortant rapidement ;
- ennemi restant dans une surface ;
- plusieurs colliders de la même cible ;
- deux pigments qui se chevauchent ;
- surface supprimée lorsque sa source disparaît ;
- surface qui ne masque pas les télégraphes de boss ;
- absence de croissance illimitée des effets actifs.

### 39.6 Tests de sauvegarde

- première création de sauvegarde ;
- fermeture et relancement du jeu ;
- déblocage persistant ;
- paramètres persistants ;
- run temporaire non restaurée par erreur ;
- champ manquant ;
- version de schéma antérieure ;
- données corrompues ;
- écriture interrompue ;
- bouton de réinitialisation avec confirmation.

### 39.7 Tests de performance

Tester au minimum :
- scène vide pour la référence ;
- densité moyenne d'ennemis ;
- densité maximale prévue ;
- maximum de projectiles ;
- maximum de surfaces de peinture ;
- combat de boss avec effets ;
- menu et écran de choix ;
- transitions et nettoyage de fin de run.

Mesurer FPS, temps CPU/GPU, allocations mémoire par frame et nombre d'objets actifs. Le seuil de réussite final doit être défini sur le PC de référence. Pour le prototype, viser 60 FPS et éviter les chutes prolongées en dessous de 50 FPS sur la scène cible, puis ajuster selon le matériel choisi.

### 39.8 Gravité des bugs

- **Bloquant :** impossible de démarrer, terminer ou redémarrer une run.
- **Critique :** perte de sauvegarde, progression dupliquée, combat impossible ou crash.
- **Majeur :** mécanique centrale incorrecte, amélioration appliquée deux fois, boss bloqué.
- **Mineur :** problème visuel ou sonore sans impact majeur sur le gameplay.
- **Cosmétique :** alignement, animation ou texte sans effet fonctionnel.

Aucun bug bloquant ou critique connu ne doit être accepté pour une version publique. Les bugs majeurs doivent être corrigés ou acceptés explicitement avec un plan de correction.

## 40. Plan d'équilibrage après prototype

### 40.1 Mesures à collecter

- durée moyenne et médiane des runs ;
- taux de victoire ;
- minute de décès ;
- dégâts reçus par minute ;
- dégâts infligés par Instrument ;
- fréquence de choix par rareté ;
- fréquence de choix par catégorie ;
- taux d'utilisation de chaque personnage ;
- Reliques et synergies les plus choisies ;
- fréquence d'évolution ;
- durée du combat de boss ;
- nombre de dégâts évités par esquive ;
- nombre d'ennemis simultanés et temps de frame.

### 40.2 Procédure d'itération

1. Définir une hypothèse précise, par exemple « les attaques à distance sont trop sûres ».
2. Modifier un petit nombre de paramètres liés à cette hypothèse.
3. Exécuter les mêmes scénarios de test.
4. Comparer les métriques et observer des parties réelles.
5. Conserver, annuler ou ajuster la modification.
6. Inscrire la décision et sa raison dans le journal d'équilibrage.

Ne pas changer simultanément les dégâts, la santé des ennemis, les chances de rareté et la cadence d'apparition, sinon il devient difficile de savoir quelle modification a produit le résultat.

### 40.3 Seuils de départ à surveiller

- durée d'une run gagnée : 20–30 minutes ;
- le joueur voit au moins une amélioration tôt dans la partie ;
- aucun Instrument ne doit être clairement dominant sur toutes les cartes et tous les personnages ;
- le boss doit rester lisible quand le joueur a une construction puissante ;
- les améliorations légendaires doivent être rares et plaisantes, mais ne doivent pas être nécessaires pour gagner ;
- la construction du joueur doit rester viable sans obtenir une rareté spécifique ;
- une défaite doit pouvoir être expliquée par les décisions ou les menaces, pas par une absence prolongée de choix valides.

Ces seuils sont des objectifs de test et non des garanties de conception.

## 41. Registre de décisions et journal de changements

Pour chaque décision importante, consigner :
- date / version ;
- sujet ;
- ancienne règle ;
- nouvelle règle ;
- raison ;
- systèmes impactés ;
- tests nécessaires ;
- statut : proposé, prototypé, testé, accepté ou rejeté.

Les décisions ouvertes ne doivent pas être effacées ; elles sont marquées comme résolues avec la règle retenue. Cela évite que les mêmes points soient réinterprétés différemment au fil du développement.

## 42. Checklist de préparation au développement

Avant de coder un système :
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

## 43. Références rapides pour l'équipe

- **Rareté = quantité de statistiques, pas transformation de gameplay.**
- **Évolution = changement de comportement, avec prérequis explicites.**
- **Niveaux de joueur 10/15/20 et rangs d'Instrument 1–5 sont deux systèmes distincts.**
- **Les surfaces de peinture ont une limite, une durée et une règle de chevauchement.**
- **Les dégâts et les statuts sont résolus par des règles communes et déterministes.**
- **La difficulté augmente par composition, budget de menace et rythme, pas seulement par les points de vie.**
- **La sauvegarde permanente ne restaure pas l'équipement temporaire d'une ancienne run.**
- **Le MVP prouve une run complète avant l'ajout massif de contenu.**
- **Aucune téléportation, aucun portail, aucun déplacement instantané.**
- **Tout chiffre non validé par test reste une proposition.**

