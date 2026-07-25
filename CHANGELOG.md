# Journal des modifications

Format inspiré de [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/),
versionnage [SemVer](https://semver.org/lang/fr/).

**Ce fichier est le texte des publications.** `tools/Version.ps1` déplace la
section *Non publié* sous le numéro de version au moment de livrer, et le contenu
devient la description de la *Release* GitHub. Un cycle sans note ne se clôt pas :
le script refuse une section vide, parce qu'une publication que personne ne sait
décrire n'aide personne à décider s'il doit mettre à jour.

## Non publié

### Ajouté

- **Cycle de publication** : `tools/Version.ps1` ouvre un cycle en alpha, le fait
  avancer et le clôt, en tenant la version, le journal des modifications et
  l'étiquette. L'étiquette déclenche la création des *Releases*.
- **Dépôt public de distribution** : les versions stables y sont publiées avec
  leurs paquets. Les prereleases restent sur le dépôt privé, et GitHub les exclut
  de `/releases/latest` — le canal d'essai est donc invisible d'une future mise à
  jour automatique.
- Deux **README** illustrés, l'un pour le développeur, l'autre pour l'utilisateur.

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
