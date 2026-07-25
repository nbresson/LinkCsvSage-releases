# Déploiement de LinkCsvSage

Procédure d'installation sur un poste utilisateur Sage 100.

---

## Prérequis

| Prérequis | Obligatoire ? | Vérification |
|---|---|---|
| **Runtime Sage 100 Objets Métiers** | **Oui, toujours** | `C:\Program Files (x86)\Common Files\Sage\Objets métiers\objets100c.dll` existe |
| Sage 100 Gestion commerciale | Oui | La société s'ouvre normalement |
| Accès en lecture à la base SQL | Oui | Authentification Windows, ou compte SQL dédié |
| .NET 10 Desktop Runtime **x86** | Selon le paquet | Voir ci-dessous |

Le Runtime Objets Métiers est requis **quel que soit le paquet** : il fournit
l'ActiveX que l'application pilote. Il ne s'agit pas du même composant que .NET.

### Quel paquet choisir

| Paquet | Taille | .NET requis sur le poste |
|---|---|---|
| `…-autonome.zip` | ~140 Mo | **Non** — le runtime est embarqué |
| `…-framework.zip` | ~10 Mo | **Oui**, .NET 10 Desktop Runtime **x86** |

**Prenez l'autonome sauf raison contraire.** Le .NET Desktop Runtime *x86* est un
composant peu répandu — un poste qui possède déjà .NET a presque toujours la
version x64, qui ne convient pas ici. L'autonome supprime cette dépendance et son
besoin de droits d'administration.

Le paquet dépendant du framework se justifie sur un parc important déployé via un
partage lent, ou lorsqu'on préfère que les correctifs de sécurité .NET arrivent
par Windows Update plutôt que par une redistribution de l'application.

---

## Pourquoi 32 bits

L'ActiveX Objets Métiers n'est publié qu'en **win32** : sa clé de registre
TypeLib ne comporte pas d'entrée 64 bits. L'application est donc compilée en
x86 et le processus s'exécute en 32 bits.

C'est invisible à l'usage, mais explique deux choses : le paquet embarque un
runtime .NET x86, et un poste 64 bits n'y change rien — Windows exécute
parfaitement un processus 32 bits.

---

## Installation

1. **Décompresser** l'archive dans un dossier local, par exemple
   `C:\Program Files (x86)\LinkCsvSage`.

   Un dossier local est préférable à un partage réseau : l'application est
   lancée depuis Sage, parfois plusieurs fois par minute, et un démarrage
   depuis un partage est sensiblement plus lent.

2. **Lancer `LinkCsvSage.exe`** une première fois.

3. **Déclarer la société** : bouton *Déclarer une société*, puis désigner les
   fichiers `.gcm` et `.mae`. Ils se trouvent dans le répertoire de la société,
   là où Sage tient son dossier `Multimedia`.

4. **Vérifier la connexion** en cliquant *Se connecter*, puis lancer une
   recherche. Si des documents apparaissent, la chaîne complète fonctionne.

5. **Créer un raccourci** vers `LinkCsvSage.exe` si les utilisateurs doivent
   pouvoir lancer l'application directement.

Les paramètres — sociétés déclarées, profils d'export — sont enregistrés dans
`%APPDATA%\LinkCsvSage\settings.json`, **par utilisateur**. Rien n'est à
déployer : chaque utilisateur déclare sa configuration au premier lancement.

---

## Déclencher depuis Sage

*Menu Fenêtre / Personnaliser l'interface / onglet Programmes externes*,
contexte **Documents des ventes** :

```
Type          : Exécutable
Programme     : C:\Program Files (x86)\LinkCsvSage\LinkCsvSage.exe
Arguments     : --piece "$(DocEntete.NumPiece)" --type "$(DocEntete.Type)"
                --gcm "$(Dossier.LocalisationCommercial)"
```

**`--gcm` est vivement recommandé en multi-sociétés.** Sans lui, l'application
ouvre la société de *son* paramétrage et ignore celle réellement ouverte dans
Sage. Deux sociétés issues d'une même copie portant les mêmes numéros de pièce,
l'export aboutirait alors sur le **mauvais document** — avec un code 0 et une
notification de succès. Avec `--gcm`, le même paramétrage vaut pour toutes les
sociétés, et une société non déclarée fait échouer proprement au lieu de se
rabattre.

N'ajoutez pas `--societe` à côté : les deux se contrediraient tôt ou tard, et la
commande serait alors refusée.

La syntaxe de substitution est **`$(...)`**, et non `%...%` : Sage remplace les
arguments « précédés du sigle Dollar $ et entre parenthèses ». Un `%…%` n'est pas
reconnu, il arrive tel quel à l'application, qui refuse alors la commande. Le
bouton de sélection des arguments, à côté de la zone, évite la faute de frappe.

Dans cette zone, un `\` ou un `$` voulus **littéralement** doivent être précédés
d'un `\`. Nos arguments n'en contiennent pas — mais un chemin ajouté à la main en
contiendrait.

L'argument `--type` est **obligatoire** : un numéro de pièce seul n'identifie pas
un document, la clé étant (domaine, type, pièce). Sont acceptés indifféremment
`6` (la valeur `DO_Type`), `60` (celle de l'énumération des Objets Métiers), `FA`
et `Facture` — la forme réellement transmise par `$(DocEntete.Type)` n'étant pas
documentée.

Options utiles :

| Argument | Effet |
|---|---|
| `--gcm <chemin>` | Chemin du `.gcm` de la société **ouverte dans Sage**. À renseigner avec `$(Dossier.LocalisationCommercial)`. |
| `--societe <nom>` | Force la société par son nom. Sinon, celle marquée par défaut. Inutile avec `--gcm`. |
| `--profil <nom>` | Force le profil d'export. |
| `--doublon Ignorer\|Ajouter\|Remplacer` | Conduite si un export existe déjà. |
| `--sans-transmettre` | Désactive l'option Transmettre. |
| `--sans-notification` | Aucune fenêtre de fin. Pour un traitement par lot. |

### Deux cases à cocher, dont une qui compte

| Case | Réglage | Pourquoi |
|---|---|---|
| *Attendre la fin de l'exécution de la commande* | **décochée** | Cochée, Sage se fige en attendant la fin du programme. Or celui-ci attend justement que vous fermiez le document — que Sage figé vous empêche de fermer. |
| *Fermer la société en cours avant exécution* | **décochée** | Elle ferme la société entière, pas le document : vous perdriez tout votre contexte de travail pour attacher un fichier. |

### Retrouver et ouvrir le fichier rattaché

Depuis le document, bouton **Fonctions / Documents liés**. Deux façons de faire,
au choix :

- sélectionner la ligne du fichier CSV, puis cliquer sur **Ouvrir** ;
- ou **double-cliquer** sur la ligne : l'enregistrement passe en modification et
  l'emplacement du fichier y apparaît sous forme de **lien hypertexte**, qu'un
  simple clic ouvre.

### Le document ouvert : ce que vous verrez

Le programme n'est appelable que depuis un document **ouvert**, que Sage
verrouille pendant ce temps. Le rattachement ne peut donc pas se poser tout de
suite. Ce n'est pas une anomalie, et il n'y a rien à faire de particulier :

1. une notification signale *« Document ouvert dans Sage »* ;
2. vous fermez votre facture quand vous en avez fini avec elle ;
3. le rattachement se pose **seul**, dans la seconde qui suit, et la notification
   de succès prend le relais.

L'attente est bornée à deux minutes. Passé ce délai, le message vous invite à
fermer le document et à relancer ; le fichier CSV, lui, a bien été produit.

### La notification de fin

Déclenchée depuis Sage, l'application signale son résultat par une petite fenêtre
en bas à droite. Elle **s'efface toujours d'elle-même** : 4,5 s pour un succès,
20 s pour une erreur. Le liseré coloré décroît pendant ce temps et indique donc ce
qu'il reste à courir ; le survol à la souris suspend le décompte, le temps de lire
ou de recopier un message. Le bouton *Fermer* abrège, il n'est jamais obligatoire.

Aucune fenêtre n'apparaît dans deux cas : avec `--sans-notification`, et lorsque
l'application est appelée depuis une console — le message y est déjà écrit.

Codes de retour, exploitables par un script :

| Code | Signification |
|---|---|
| `0` | Export réalisé |
| `2` | Arguments invalides |
| `3` | Connexion impossible |
| `4` | Échec sur le document |
| `5` | Document ignoré (export déjà présent) |
| `6` | Pièce restée ouverte dans Sage — rien n'est cassé, à relancer plus tard |

Pour lire ce code depuis PowerShell, **`Start-Process -Wait` est obligatoire** :

```powershell
$p = Start-Process $exe -ArgumentList '--piece','FA00001','--type','6',
                                      '--sans-notification' -PassThru -Wait
$p.ExitCode
```

L'appel direct `& $exe …` rend la main immédiatement et `$LASTEXITCODE` ne veut
alors rien dire : l'exécutable est de sous-système *Windows*, non *console*, et
n'est donc pas attendu par l'interpréteur.

---

## Mise à jour

1. Fermer l'application sur le poste.
2. Remplacer le contenu du dossier d'installation par celui du nouveau paquet.
3. Relancer.

Les paramètres vivant dans `%APPDATA%`, ils survivent à la mise à jour. Le
dossier d'installation ne contient rien de personnel : il peut être écrasé
intégralement.

---

## Désinstallation

Supprimer le dossier d'installation. Pour retirer aussi les paramètres et les
journaux d'un utilisateur : `%APPDATA%\LinkCsvSage`.

Aucune entrée de registre n'est créée, aucun composant n'est enregistré au
niveau système.

---

## En cas de problème

**« Les Objets Métiers Sage sont introuvables sur ce poste »**
Le Runtime Sage 100 Objets Métiers n'est pas installé, ou l'application ne
s'exécute pas en 32 bits. Vérifier la présence de
`C:\Program Files (x86)\Common Files\Sage\Objets métiers\objets100c.dll`.

**« La session Sage est ouverte, mais la lecture SQL est impossible »**
L'application lit les documents directement en SQL. Autoriser l'authentification
Windows de l'utilisateur sur l'instance, ou renseigner un compte SQL en lecture
seule dans le profil de connexion.

**Le fichier rattaché ne s'ouvre pas depuis Sage**
Vérifier que le fichier est bien présent dans le dossier `Multimedia` de la
société. Sage y recopie lui-même les exports et enregistre un lien relatif ;
si le dossier a été déplacé, le lien devient invalide.

**Rien ne se passe au déclenchement depuis Sage**
Consulter `%APPDATA%\LinkCsvSage\logs\linkcsvsage-AAAAMMJJ.log`. Chaque
lancement y est tracé, y compris les échecs d'arguments.

---

## Produire un paquet

Depuis un poste de développement, à la racine du dépôt :

```powershell
powershell -File tools\Publish.ps1                       # autonome (recommandé)
powershell -File tools\Publish.ps1 -DependantDuFramework # léger
```

Le script vérifie que l'exécutable produit est bien 32 bits et que l'assembly
d'interop l'accompagne, puis dépose l'archive versionnée dans `artefacts\`.

L'intégration continue produit également ces deux paquets à la demande
(*Actions / Intégration continue / Run workflow*).
