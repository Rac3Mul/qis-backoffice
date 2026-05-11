# Migration PHP 7 → 8.2 — journal des modifications



| Fichier | Résumé des changements |
|---------|----------------------------------------|
| `inc_co/connect.php` | On détecte correctement si la page est en **http** ou **https**, et on compare les adresses de même façon. Ça évite des messages d’erreur PHP 8 et des redirections ou blocages inexpliqués. |
| `envoi_mail/PHPMailerAutoload.php` | Ancienne méthode de chargement automatique retirée : **PHP 8 ne l’autorise plus**. Remplacée par la méthode moderne recommandée. |
| `design/script_mail.php` | Si l’image de l’intervenant n’est pas renseignée (ex. nouveau client), le programme met une valeur par défaut au lieu de planter avec « variable inexistante ». |
| `design/script_mail_tache.php` | Même principe que `script_mail.php` pour l’image de l’intervenant. |
| `accueil.php` | **Fiabilisation** : moins de plantages PHP 8 (listes, données base, variables manquantes), HTML plus propre (boutons, styles). **Chat** : quand on fait défiler la discussion, le reste de la page ne bouge plus. **Affichage** : certains blocs « Quick Info Bordeaux » sont masqués (commentés). **Graphique** : corrections pour que la colonne tâches et les fenêtres s’affichent correctement. |
| `design/nb_pc.php` | Bouton « Enregistrer un appel » et structure des formulaires **corrigés** (HTML valide, plus de formulaire dans un formulaire). |
| `design/footer.php` | Sur l’accueil, le script du **graphique** est inclus au bon endroit : le graphique refonctionne comme prévu. |
| `design/graph_intervention.php` | Graphique **plus fiable** (données, dates, cas où il n’y a rien à afficher). **Correction bloquante** : une erreur PHP 8 empêchait le script de finir → graphique vide. |
| `list_contrat.php` | Liens « Voir les clients Quick Info Bordeaux » **masqués** (interface). |
| `list_fiche.php` | **Sous Windows**, le tri et l’affichage des dates ne marchaient pas bien ; c’est corrigé. Le texte des fiches est **nettoyé / sécurisé** pour que le tableau ne casse pas si le contenu contient du HTML. |
| `list_contrat_histo.php` | **Exports** (PDF, Excel, CSV, copier) : colonne « Action » retirée des exports, dates plus lisibles, PDF avec titres plus clairs et colonne matériel **sans mise en page bizarre** (moins de « carrés » ou de sauts). |
| `pret_retour.php` | Formulaire **retour de prêt** : tout est initialisé avant affichage, marque/modèle pris au bon endroit, champs optionnels gérés. **Moins d’erreurs** PHP 8 et affichage plus cohérent. |
| `pret_materiel.php` | Caution et date de retour : le formulaire **ne suppose plus** que les champs existent toujours ; affichage sécurisé. |
| `pret_modif.php` | Si le type de matériel n’existe plus en base, les libellés restent **vides** au lieu de provoquer des alertes PHP. |
| `etat_atelier.php` | Quand une ligne base est **manquante** (intervenant, client, etc.), on affiche des valeurs par défaut au lieu de planter. Liens « PC Quick Info Bordeaux » **masqués** dans l’en-tête. |
| `stats_antivirus.php` | Page **beaucoup plus rapide** : au lieu de refaire énormément de petites requêtes, le calcul se fait en **une fois** avec regroupement en base (même résultat, chargement allégé). |
| `ajout_hebergement.php` | Ouverture **sans client** dans l’URL : plus de messages parasites. Formulaire **ajout** : après une erreur de saisie, les choix (tarifs, serveur, etc.) **restent visibles** comme avant envoi. |
| `ajout_domaine.php` | Même logique : **PHP 8** plus calme, formulaire qui **garde les valeurs** après erreur, contrôles renforcés (intervenant, doublon de nom de domaine, etc.). |
| `hebergement_modif.php` | Au **premier chargement** de la page, plus de message PHP bizarre : la variable d’affichage des messages est toujours définie. |
| `ndd_modif.php` | Après enregistrement, rechargement du nom de domaine **par son identifiant** (plus fiable). Si la validation échoue, le formulaire **conserve** ce que vous avez saisi. |
| `list_hebergement.php` | **Suppression** d’un hébergement : vérifications si la ligne existe, message si introuvable, compte des domaines dissociés ; **bug** sur une variable inexistante corrigé. |
| `inc_co/fonctions.php` | Anciennes fonctions PHP remplacées par des équivalents **compatibles PHP 8.2**. **Chiffrement** : réécrit avec OpenSSL mais **lecture identique** des données déjà en base. **Dates en français** : petites fonctions maison (jour, mois, date) pour remplacer l’ancienne méthode dépréciée. |
| `lib/PHPMailer/` (ajout) + `envoi_mail/` (suppression) | **Envoi d’e-mails** : passage à une version **récente et maintenue** de PHPMailer dans `lib/PHPMailer/`. L’ancien gros dossier `envoi_mail/` a été **supprimé** (remplacé par cette structure). |
| `action/mail_tache.php`, `action/gestion_heb.php`, `action/gestion_dom.php`, `action/gestion_antivirus.php` | Ces scripts **pointent** vers le nouveau fichier d’autoload de PHPMailer dans `lib/`. |
| `design/script_mail.php`, `script_mail_tache.php`, `script_mail_ndd.php`, `script_mail_ndd_gest.php`, `script_mail_antivirus.php` | Même chose : **liens** vers PHPMailer 7 et **instructions** d’import adaptées au nouveau paquet. |
| `test_phpmailer_smtp.php` | Fichier de **test SMTP** mis à jour (chemins + gestion des exceptions comme dans PHPMailer 7). |
| `action/action_recup_mail.php` | Ancienne boucle supprimée en PHP 8 **remplacée** par une boucle moderne équivalente. |
| `envoi_mail/extras/htmlfilter.php` | Syntaxe des tableaux **modernisée** pour PHP 8 ; mêmes boucles remplacées. *(Fichier historique si encore présent dans une copie du projet.)* |
| `envoi_mail/extras/ntlm_sasl_client.php` | Anciennes fonctions de **hachage / cryptage** retirées de PHP remplacées par celles d’aujourd’hui. |
| `envoi_mail/class.phpmailer.php` | Bloc de code **obsolète** (magic quotes) retiré — sans effet utile et **interdit** en PHP 8. |
| `recup.php` | Connexion base : passage de l’**ancienne API MySQL** à **PDO** (méthode actuelle). L’identifiant dans l’URL est **contrôlé** (sécurité). Affichage des noms **échappé** pour le HTML. |
| `design/script_mail.php`, `script_mail_tache.php`, `script_mail_ndd.php`, `script_mail_ndd_gest.php`, `script_mail_antivirus.php` | Fonction d’encodage **dépréciée** en PHP 8.2 remplacée par une conversion **recommandée**. |
| `accueil.php`, `fiche_modif.php` | Affichage de l’**heure** : ancienne fonction retirée en PHP 8.1 remplacée par `date()`. |
| `ajout_materiel.php`, `ajout_materiel2.php`, `testajout.php` | **Jours et dates en français** sans dépendre de la langue du serveur (`strftime` retiré). Titre de page mieux **protégé** pour le HTML. |
| `ajout_cat.php` | Liste **couleur de fond** : plus de variable « oubliée » ni d’avertissements PHP dans le menu déroulant ; la couleur choisie **reste** après une erreur sur le nom. |
| `list_liaisons_clients.php` | À chaque ligne du tableau, les données sont **réinitialisées** ; si une ligne liée manque, on affiche des **valeurs de repli** au lieu de planter. **Suppression** : on efface seulement si l’enregistrement existe vraiment. |

---

## Historique

| Date | Fichier | Note |
|------|---------|---------------|
| 2026-05-02 | `MODIF.MD` | Création du journal ; première fiche : connexion / URLs. |
| 2026-05-02 | `footer.php`, `graph_intervention.php`, `accueil.php` | Graphique accueil : script remis au bon endroit, données fiabilisées. |
| 2026-05-02 | `graph_intervention.php`, `MODIF.MD` | Erreur fatale PHP sur le graphique corrigée (fonction date). |
| 2026-05-02 | `etat_atelier.php`, `MODIF.MD` | Atelier : gestion des lignes base « vides » pour PHP 8. |
| 2026-05-02 | `accueil.php`, `MODIF.MD` | Chat : défilement limité à la zone de discussion. |
| 2026-05-02 | `accueil.php`, `MODIF.MD` | Formulaires « Quick Info Bordeaux » masqués (gestion des appels). |
| 2026-05-02 | `etat_atelier.php`, `MODIF.MD` | Liens « PC Bordeaux » masqués en haut de page. |
| 2026-05-02 | `list_contrat.php`, `MODIF.MD` | Liens « clients Bordeaux » masqués. |
| 2026-05-02 | `list_contrat_histo.php`, `MODIF.MD` | Exports plus propres : sans colonne Action inutile ; PDF plus lisible. |
| 2026-05-02 | `accueil.php`, `MODIF.MD` | Lien direct vers un client depuis les contrats : variable d’affichage corrigée. |
| 2026-05-02 | `list_fiche.php`, `MODIF.MD` | Liste fiches : dates OK sous Windows ; texte travaux sans HTML qui casse le tableau. |
| 2026-05-02 | `pret_materiel.php`, `MODIF.MD` | Prêt matériel : champs optionnels sans erreur PHP 8. |
| 2026-05-02 | `pret_retour.php`, `MODIF.MD` | Retour prêt : initialisations et libellés cohérents. |
| 2026-05-07 | `pret_modif.php`, `MODIF.MD` | Modif prêt : si type matériel introuvable, affichage sans alerte. |
| 2026-05-08 | `ajout_hebergement.php`, `MODIF.MD` | Ajout hébergement : page sans client, moins de bruit PHP ; formulaire après erreur. |
| 2026-05-08 | `ajout_hebergement.php`, `MODIF.MD` | Formulaire création : valeurs réaffichées si validation en échec. |
| 2026-05-08 | `ajout_domaine.php`, `MODIF.MD` | Ajout domaine : robustesse POST / erreurs / doublons. |
| 2026-05-08 | `list_hebergement.php`, `MODIF.MD` | Suppression hébergement : correctifs + message si absent. |
| 2026-05-08 | `ndd_modif.php`, `MODIF.MD` | Modif NDD : rechargement fiable après sauvegarde ; formulaire après erreur. |
| 2026-05-08 | `hebergement_modif.php`, `MODIF.MD` | Modif hébergement : affichage sans avertissement au chargement. |
| 2026-05-10 | `envoi_mail/extras/htmlfilter.php` | Tableaux et boucles adaptés PHP 8. |
| 2026-05-10 | `inc_co/fonctions.php` | Regex, chiffrement OpenSSL (données anciennes lisibles), dates FR. |
| 2026-05-10 | `action/action_recup_mail.php` | Boucle moderne à la place de l’ancienne. |
| 2026-05-10 | `envoi_mail/extras/ntlm_sasl_client.php` | Cryptage / hachage mis à niveau. |
| 2026-05-10 | `envoi_mail/class.phpmailer.php` | Code obsolète retiré dans l’encodage des pièces jointes. |
| 2026-05-10 | `recup.php` | PDO + sécurisation de l’identifiant dans l’URL. |
| 2026-05-10 | `design/script_mail*.php` | Encodage caractères sans fonction dépréciée. |
| 2026-05-10 | `accueil.php`, `fiche_modif.php` | Heure affichée avec `date()`. |
| 2026-05-10 | `ajout_materiel*.php`, `testajout.php` | Dates en français via helpers internes. |
| 2026-05-10 | `scripts/php_scan_log.php`, `scripts/verif_chiffrement.php`, `MIGRATION_PHP82.md` | Petit outillage pour contrôler la migration 7 → 8.2. |
| 2026-05-10 | `inc_co/fonctions.php` | Durées type « il y a X » : correction avertissement PHP 8.1 (nombre entier propre). |
| 2026-05-10 | `lib/PHPMailer/`, `envoi_mail/` | Grosse mise à jour envoi mails ; ancien dossier volumineux supprimé. |
| 2026-05-10 | `action/{mail_tache,gestion_heb,gestion_dom,gestion_antivirus}.php`, `design/script_mail*.php`, `test_phpmailer_smtp.php` | Tous ces fichiers branchés sur PHPMailer 7 ; contrôle global OK. |
| 2026-05-10 | `ajout_cat.php`, `MODIF.MD` | Liste couleurs sans avertissements. |
| 2026-05-10 | `list_liaisons_clients.php`, `MODIF.MD` | Liste liaisons : lignes manquantes gérées ; suppression plus sûre. |
