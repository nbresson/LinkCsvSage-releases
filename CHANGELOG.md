# Journal des modifications

Format inspiré de [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/),
versionnage [SemVer](https://semver.org/lang/fr/).

**Ce fichier est le texte des publications.** `tools/Version.ps1` déplace la
section *Non publié* sous le numéro de version au moment de livrer, et le contenu
devient la description de la *Release* GitHub. Un cycle sans note ne se clôt pas :
le script refuse une section vide, parce qu'une publication que personne ne sait
décrire n'aide personne à décider s'il doit mettre à jour.

## Non publié

## 1.6.2 — 2026-09-19

### Corrigé

- **L'application n'avait pas d'icône.** L'explorateur, le menu Démarrer et la
  barre des tâches la montraient avec l'icône générique de Windows — et la boîte
  de choix du programme externe de Sage aussi, là où c'est le seul repère pour la
  reconnaître au milieu d'une liste de programmes.

  Le fichier d'icône était mal formé d'une façon qui ne se voyait nulle part : sa
  table annonçait des images d'un octet alors que les images étaient bien là,
  entières. Ni la compilation, ni la préparation des paquets ne s'en plaignaient.
  Trois contrôles le vérifient désormais — à la génération de l'icône, à chaque
  exécution des tests, et sur l'exécutable réellement livré.

  Les fenêtres de l'application, elles, n'étaient pas concernées : leur icône est
  dessinée à partir du logo vectoriel, et elle a toujours été juste.

## 1.6.1 — 2026-09-18

### Corrigé

- **À la largeur minimale de la fenêtre, les cases à cocher de la grille
  disparaissaient.** La colonne était rognée jusqu'à ne plus rien montrer : on ne
  pouvait plus rien cocher ni décocher, alors que le compteur annonçait le nombre
  de pièces cochées et que le bouton d'export restait actif.

- La description du profil livré s'accentue. Un profil déjà enregistré garde
  la sienne : ce n'est qu'un libellé, et le réécrire sous vos pieds coûterait
  plus qu'il ne rapporte.

## 1.6.0 — 2026-09-18

### Ajouté

- **« Relancer les N pièces occupées »**, éprouvé : une pièce ouverte dans Sage
  pendant un lot en ressort *Occupée* ; une fois refermée, le bouton du bandeau
  ne recoche qu'elle et l'exporte, sans toucher au reste de la sélection.

- **Le détail d'un résultat s'ouvre depuis la grille**, au double-clic ou par la
  touche *Entrée* : le message complet, dans une zone qu'on peut sélectionner et
  copier, avec *Ouvrir le journal* à côté.

  L'info-bulle ne suffisait pas : elle ne se copie pas, ne s'atteint pas au
  clavier, et disparaît dès que la souris bouge — alors que c'est justement ce
  texte qu'on transmet quand on demande de l'aide.

- **Les informations libres de la société sont exportables.** Celles du document,
  celles des lignes, celles de la fiche client : elles apparaissent dans
  l'éditeur de profil, groupées à part et nommées comme dans Sage.

  Elles se déclarent **dossier par dossier** : l'éditeur propose celles de la
  société ouverte. Un profil qui en porte et qu'on applique ailleurs garde ses
  colonnes — avec leur en-tête —, simplement vides.

- **Quinze champs de la fiche client** au catalogue de colonnes : adresse
  complète, pays, téléphone, e-mail, SIRET, n° de TVA intracommunautaire, code
  APE, qualité, classement, encours autorisé. Ils viennent de la même lecture que
  l'intitulé du client — aucune requête supplémentaire.

  Quinze, et non les cent soixante et onze colonnes de la fiche : un catalogue
  est un choix qu'on vous offre. La liste s'allonge à la demande.

### Corrigé

- **Une colonne dont la clé est inconnue ne disparaît plus du fichier.** Elle
  garde sa place, son en-tête, et sort vide. Le fichier changeait de forme sans
  un mot, et une chaîne d'import qui le lit — par position ou par nom de colonne —
  cassait sans qu'on sache pourquoi.

## 1.5.0 — 2026-09-17

### Modifié

- **L'interface parle enfin la même langue que Sage.** Un document de vente est
  une **pièce** ; le fichier que nous y attachons est un **document rattaché**.
  Le mot « document » désignait les deux, parfois dans la même phrase : « 31
  documents trouvés. 31 portent déjà un document rattaché. »

  L'écran, les messages du mode silencieux, le compte rendu de lot et l'aide
  disent désormais « pièce ». Là où « document » désigne la chose de Sage —
  *Documents des ventes*, *Documents liés*, *type de document* —, il reste : c'est
  le mot que vous lisez dans Sage.

- **⚠ Les en-têtes des fichiers exportés changent.** Les libellés du catalogue
  s'accentuent : « N° de piece » devient « N° de pièce », « Intitule tiers »
  devient « Intitulé tiers », et de même pour *Référence*, *Désignation*,
  *Quantité*, *Dépôt*, *Représentant*, *Net à payer*.

  **Un profil qui n'avait pas personnalisé ses libellés suit automatiquement** :
  ses prochains fichiers porteront les nouveaux en-têtes. Si une chaîne d'import
  lit ces colonnes par leur nom, vérifiez-la avant de mettre à jour — ou fixez
  vos libellés dans l'éditeur de profil, ce qui les met à l'abri de toute
  évolution ultérieure.

  Les **clés** techniques (`DO_Piece`, `CT_Intitule`…) n'ont pas bougé : le format
  JSON, qui les emploie comme noms de champs, est inchangé.

- **Le panneau de critères tient dans l'écran.** Les critères de tous les jours
  restent visibles ; les neuf types de document et les critères rares —
  représentant, dépôt, souche, total HT — se replient, **chacun avec son
  compte** : *8 sur 9*, *2 renseignés*. Replié ne veut jamais dire caché, et le
  dernier état est celui que vous retrouvez à l'ouverture.

- **La gestion des profils passe dans un menu *Gérer***, à la place de cinq
  boutons permanents dont deux étaient presque toujours désactivés.

- **Des pictogrammes** accompagnent les états, les compteurs de fin de lot, les
  entrées du menu et les raccourcis de la barre d'état. Ils doublent la couleur
  et le mot — jamais ils ne les remplacent.

- Le bandeau est deux fois moins haut, les champs de saisie ont la hauteur des
  listes, les montants s'alignent sans changer de police, le client tient dans une
  colonne avec son code, et *Déjà rattaché* n'est plus une pastille grise répétée
  sur chaque ligne.

### Ajouté

- **Un bandeau de fin de lot**, au-dessus de la grille et jusqu'à la recherche
  suivante : « Lot terminé · 28 exportées · 3 occupées · 1 en erreur · en 12 s ».

  **Chaque chiffre filtre la grille** sur sa famille — « 3 occupées » n'est plus
  une information, c'est le chemin vers les trois pièces —, et le bandeau rappelle
  ce qu'il masque : « 3 sur 31 affichées ».

- **« Relancer les 3 pièces occupées »** : le bouton ne recoche que celles-là et
  relance. Il fallait auparavant les retrouver et les recocher une à une.

- **Une confirmation au-delà de vingt pièces**, rappelant le nombre, la société et
  le profil. Un lot écrit dans Sage, et rien ne se retire depuis l'application.

- **Une case à cocher en tête de la grille**, à la place des boutons *Tout cocher*
  et *Tout décocher* qui se trouvaient à l'autre bout de l'écran.

- **Un profil modifié ne se perd plus** : *Échap* et la croix demandent
  confirmation, et *Entrée* ne ferme plus la fenêtre pendant qu'on renomme une
  colonne.

### Corrigé

- **« Jamais exportées » écartait des pièces que nous n'avions jamais
  touchées.** Le filtre retenait n'importe quel rattachement : un PDF déposé à la
  main suffisait à faire disparaître une facture de la liste. Il reconnaît
  désormais notre propre signature. Vous verrez sans doute davantage de pièces
  qu'avant sous ce filtre — ce sont celles qui auraient dû y être.

- **Un critère illisible était ignoré en silence.** « 4 692,29 » recopié depuis la
  grille n'était pas reconnu, le filtre disparaissait, et la recherche ramenait
  plus de pièces que prévu — cochées d'office. La saisie fautive est maintenant
  refusée et signalée, et la recherche attend d'être corrigée.

- **Une pièce restée ouverte dans Sage s'affichait « À traiter »** dans la grille,
  indiscernable d'une pièce que le lot n'avait pas atteinte, alors que le compteur
  la disait occupée.

- **À la largeur minimale de la fenêtre, les colonnes rétrécissaient** au lieu de
  défiler : « 9 937,48 » s'affichait « 937,48 », amputé de son millier et sans
  rien pour le signaler.

- La silhouette du thème sombre n'existait pas : son tracé avait une aire nulle.

- Quelques textes de l'interface restaient sans accents, dont les types
  *Préparation de livraison* et *Facture comptabilisée*.

## 1.4.0 — 2026-07-26

### Ajouté

- **L'intégration continue construit désormais toutes les fenêtres** à chaque
  compilation, applique leurs gabarits et écoute les liaisons.

  C'est le contrôle qui manquait : une erreur d'analyse XAML ne se voit qu'à
  l'exécution, et c'est ainsi que la fenêtre de mise à jour est restée inerte
  pendant deux versions. Trois défauts introduits volontairement — liaison
  `TwoWay` sur une propriété en lecture seule, ressource renommée, propriété liée
  inexistante — sont tous détectés.

  Le dernier ne lève même pas : WPF se contente de l'écrire dans une trace que
  personne ne lit.

  **Rien ne change à l'usage.** C'est une garantie sur les versions à venir : une
  fenêtre qui ne s'ouvre pas ne peut plus être livrée sans que la compilation le
  refuse.

## 1.3.0 — 2026-07-26

### Modifié

- **Le logo devient vectoriel.** L'application l'affiche comme un tracé, et non
  plus comme une image de 256 px : il reste net quelle que soit la mise à
  l'échelle de Windows, de 100 à 200 %.

  `Assets/Logo.xaml` est désormais la source unique. L'icône `.ico` et l'image
  des README en sont **régénérées** par `tools/Regenerate-Logo.ps1` : elles ne
  peuvent plus diverger de ce que montre l'application.

  Le lettrage « CSV » y est tracé et non écrit — un texte aurait dépendu de la
  police installée sur le poste.

- **Toute l'interface porte désormais ses accents.** La convention existait
  depuis le début, mais n'était tenue que dans l'éditeur de profils : l'écran
  principal, la connexion et *À propos* affichaient « Criteres de selection »,
  « Connexion a la societe », « Fichiers exportes ».

  113 corrections, sur les écrans comme sur les messages — notifications, boîtes
  de dialogue, rapport de fin de lot, aide en ligne de commande. **Les journaux
  aussi** : ils sont écrits en UTF-8 avec marqueur d'encodage précisément pour
  que leurs accents survivent.

  Un test le vérifie désormais. Il ne couvre volontairement **pas** les libellés
  qui atterrissent dans des fichiers produits — colonnes des exports, jeton
  `{TypeLibelle}`, en-têtes du rapport : les accentuer changerait les fichiers
  existants, et pour le deuxième la clé de détection des doublons.

### Ajouté

- La fenêtre **À propos** nomme l'auteur, **Nicolas BRESSON**, et porte la
  mention de copyright.

  Les deux viennent des **métadonnées de l'assembly**, définies une seule fois
  dans `Directory.Build.props`. Elles figurent donc aussi dans les propriétés du
  fichier vues par l'explorateur Windows — là où l'on regarde quand on se demande
  d'où vient un exécutable.

## 1.2.1 — 2026-07-26

### Corrigé

- **La fenêtre de mise à jour ne s'ouvrait jamais.** Défaut présent en 1.1.0 et
  1.2.0 : la barre d'état signalait bien la version disponible, mais la
  proposition ne s'affichait pas au démarrage, et la demander depuis la barre
  d'état ou *À propos* **fermait l'application**.

  Cause : `ProgressBar.Value` se lie en `TwoWay` par défaut, sur une propriété
  qui n'a qu'un accesseur privé. WPF refusait alors de construire la fenêtre.

  **Conséquence pour les postes déjà installés en 1.1.0 ou 1.2.0** : ils ne
  peuvent pas recevoir cette correction par la mise à jour automatique, puisque
  c'est elle qui est en panne. Une mise à jour **manuelle** est nécessaire, une
  seule fois. Les versions suivantes se poseront normalement.

- Les notes de la version proposée s'affichaient en **Markdown brut** —
  `### Ajouté`, `**Aide intégrée**. Elles sont désormais mises en forme.

- Une erreur survenant à l'ouverture de la proposition est **journalisée**, et
  n'interrompt plus l'application : un message donne le lien de la publication
  pour une mise à jour manuelle. C'est l'absence de cette trace qui avait laissé
  le défaut ci-dessus passer deux versions.

### Sécurité

- `tools/Publier-Public.ps1` **refuse de publier si le dépôt public contient
  autre chose que de la présentation**.

  GitHub attache *Source code (zip)* et *Source code (tar.gz)* à toute
  publication, à partir du contenu du dépôt, et ces entrées ne peuvent pas être
  retirées. La règle ne peut donc se tenir qu'en amont, sur le dépôt lui-même.

  Le contrôle emploie une **liste blanche** d'extensions : une liste noire
  laisserait passer la prochaine extension à laquelle personne n'a pensé. Aucun
  contournement, `-Force` compris.

  Vérification faite au passage : les publications 1.1.0 et 1.2.0 déjà en ligne
  ne contiennent **aucun code** — archives téléchargées et inspectées.

## 1.2.0 — 2026-07-26

### Ajouté

- **Aide intégrée**, atteignable par **F1** — qui ouvre la rubrique de l'écran en
  cours, pas un sommaire — et par le bouton *Aide* de la barre d'état.

  Neuf rubriques : premiers pas, recherche, export et rattachement, profils
  d'export, déclenchement depuis Sage, multi-sociétés, rapport de lot, mise à
  jour, dépannage. Illustrées, avec des liens entre rubriques.

  Elle est **embarquée dans l'application** : elle s'ouvre sur un poste sans
  accès à Internet, et suit le thème clair ou sombre.

## 1.1.0 — 2026-07-26

### Ajouté

- **Mise à jour automatique.** Au lancement, une fois par jour, l'application
  interroge le dépôt public des publications. Si une version plus récente
  existe, elle la propose avec la liste de ce qui change ; accepter suffit — le
  paquet est téléchargé, vérifié, et l'application se remplace puis se rouvre.

  Elle ne se met **jamais à jour toute seule** : la proposition attend une
  réponse. *Ignorer cette version* ne porte que sur celle-là, et *À propos*
  permet de rechercher à tout moment.

  Rien n'est vérifié quand Sage déclenche l'application : l'utilisateur attend
  l'export d'un document ouvert, et une fenêtre surgissant par-dessus Sage au
  milieu d'une saisie serait déplacée.

  Le paquet reçu est du **même mode** que celui installé, autonome ou dépendant
  du framework — jamais l'un pour l'autre. Il est contrôlé avant d'être posé :
  empreinte SHA-256, présence de l'interop, et en-tête PE 32 bits.

  En cas d'échec, **la version précédente est remise en place** et l'application
  reste utilisable.

- **Cycle de publication** : `tools/Version.ps1` ouvre un cycle en alpha, le fait
  avancer et le clôt, en tenant la version, le journal des modifications et
  l'étiquette. L'étiquette déclenche la création des *Releases*.
- **Dépôt public de distribution** : les versions stables y sont publiées avec
  leurs paquets par `tools/Publier-Public.ps1`. Les prereleases restent sur le
  dépôt privé, et GitHub les exclut de `/releases/latest` — le canal d'essai est
  donc invisible d'une future mise à jour automatique.

  Le script **ne recompile pas** : il télécharge les paquets de la publication
  privée, ceux que l'intégration continue a vérifiés. Il part du poste du
  développeur, avec son authentification `gh` : le jeton d'un workflow étant
  scellé au dépôt où il tourne, le faire depuis l'intégration continue aurait
  exigé un jeton personnel dont l'expiration aurait arrêté les publications en
  silence.
- Deux **README** illustrés, l'un pour le développeur, l'autre pour l'utilisateur.

### Modifié

- `tools/Version.ps1` accepte `-Livrer major|minor|patch`, qui passe directement
  à la version stable suivante. Pour une évolution achevée : enchaîner `-Ouvrir`
  puis `-Clore` produirait deux sections de journal dont la seconde serait vide,
  et la Release stable sortirait sans description.

- L'emplacement d'installation **recommandé** devient
  `%LOCALAPPDATA%\Programs\LinkCsvSage`, au lieu de `C:\Program Files (x86)` :
  l'utilisateur peut y écrire, donc les mises à jour s'y installent sans
  autorisation d'administrateur. Les deux emplacements restent valables.

### Corrigé

- L'aide en ligne donnait la syntaxe `%DocEntete.NumPiece%` pour le paramétrage
  dans Sage. La bonne est `$(DocEntete.NumPiece)` : un `%…%` n'est pas substitué
  et arrivait tel quel à l'application, qui cherchait alors une pièce portant ce
  nom. L'exemple mentionne aussi `--gcm`, qui lui manquait.

## 1.0.0 — 2026-07-25

Première version complète, validée de bout en bout sur des sociétés Sage réelles.

### Ajouté

- Export des documents de vente Sage 100 en **CSV, XLSX ou JSON**, au choix du
  profil. Le XLSX écrit des cellules typées — un montant est un nombre, une date
  une date ; le JSON emploie les clés du catalogue et des dates ISO 8601.
- **Rattachement automatique** du fichier à la pièce dans Sage, avec l'option
  *Transmettre* : vérifié, le fichier accompagne bien les e-mails envoyés depuis
  Sage, et un document sans l'option ne les accompagne pas.
- **Déclenchement depuis Sage** en programme externe, contexte *Documents des
  ventes*, avec notification de fin et code de retour.
- **Multi-sociétés** : plusieurs sociétés déclarées, une seule ouverte à la fois,
  bascule sans redémarrage. `--gcm` fait suivre la société réellement ouverte
  dans Sage.
- **Profils d'export** composables : colonnes, ordre par glisser-déposer,
  libellés, mise en forme, nommage, avec aperçu en direct.
- Sélection multicritère, trois politiques de doublon, **rapport de lot
  enregistrable** en CSV.
- Thème clair, sombre ou suivi de Windows ; période de recherche et types de
  document mémorisés.
- Journalisation à rotation quotidienne, purge vérifiée.

### Sécurité

- Secrets chiffrés par **DPAPI en portée utilisateur** : recopier
  `settings.json` sur un autre poste ne donne accès à aucun mot de passe.
