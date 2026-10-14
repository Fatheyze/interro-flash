# Interro Flash 3e

Interrogations d'automatismes de 3e (épreuve du brevet), tirées au sort, projetées au tableau et corrigées automatiquement sur les tablettes des élèves.

## Le principe

1. **Le professeur crée un sujet** : nombre de questions, thèmes, QCM ou réponses libres. Il obtient un **code de sujet** de 10 caractères (et un QR code).
2. **Il projette l'interro** en plein écran, une question à la fois, avec ou sans minuteur (durée totale, temps par question). Les flèches du clavier et les télécommandes de présentation fonctionnent.
3. **Les élèves répondent** :
   - sur la **feuille réponse imprimée** (préparée pour le sujet, avec les cases A, B, C, D des QCM, deux feuilles par page A4) ;
   - ou sur la **tablette** : ils ouvrent la page, touchent « J'ai un code », tapent le code (ou scannent le QR code) et leur nom. La tablette affiche les cases réponse numérotées, ou les énoncés complets si le professeur l'a choisi.
4. **La tablette corrige** dès que l'élève rend sa copie : note, détail juste/faux, correction expliquée (si le professeur l'autorise) et un **code résultat** de 7 caractères.
5. **Les notes arrivent toutes seules chez le professeur** : dès qu'un élève rend sa copie, le résultat est envoyé à l'écran du professeur et s'inscrit en face de son nom. La page calcule la note sur 20, la moyenne de la classe et le **taux de réussite par question**. Un bouton copie la colonne des notes dans l'ordre de la liste, prête à coller dans Pronote (« Abs » pour les absents). Export CSV possible.

### Listes de classe

Le professeur colle une fois la liste de chaque classe (copiée depuis Pronote ou un tableur, un élève par ligne). Quand un sujet est lié à une classe, les élèves **choisissent leur nom dans la liste** sur la tablette : plus de faute de frappe. Si un élève tape son nom lui-même (« Prénom Nom » ou « Nom Prénom »), la page le retrouve quand même ; sinon, sa copie apparaît dans « Copies à attribuer ».

### Si l'envoi automatique ne passe pas

Si le réseau du collège bloque l'envoi, la tablette affiche le code résultat. Le professeur tape seulement ce code (7 caractères), dans n'importe quel ordre : la page retrouve à quel élève de la liste il appartient.

Le professeur peut aussi imprimer le sujet (pour les élèves sans tablette), le corrigé, ou projeter la correction question par question.

## Comment ça marche sans serveur

Le code du sujet contient tous les réglages et une graine de tirage. Chaque appareil qui reçoit ce code retire exactement les mêmes questions : le tableau et les tablettes affichent le même sujet sans connexion entre eux.

Le code résultat contient les réussites de l'élève question par question, avec une clé de contrôle liée à son nom et au sujet : un code ne peut pas être réutilisé sous un autre nom.

L'envoi automatique passe par un relais public, [ntfy.sh](https://ntfy.sh) (logiciel libre). Les données sont **chiffrées de bout en bout** (AES-GCM) avec une clé tirée du code du sujet : le relais ne voit que des données illisibles, et les messages y expirent au bout de quelques heures. Seuls les appareils qui connaissent le code du sujet peuvent lire les résultats et la liste de classe. Le professeur doit avoir la page du sujet ouverte pendant l'interro, ou l'ouvrir le jour même, pour recevoir les copies.

Les listes de classe, l'historique des sujets, les copies en cours et les relevés restent dans le navigateur (stockage local).

## Points d'attention

- **Tous les appareils doivent utiliser la même version de la page.** Si vous modifiez la banque de questions, incrémentez `VERSION_BANQUE` (0 à 3) dans le moteur : les anciens codes seront alors refusés proprement au lieu de donner des sujets différents.
- La correction automatique accepte la virgule ou le point, les espaces, une unité après le nombre (« 12 cm »), « x = 12 » et les fractions équivalentes (« 3/4 » pour 0,75). Une réponse rédigée autrement peut être comptée fausse : la correction détaillée permet à l'élève et au professeur de le voir.
- Les points de vigilance d'une interro (élève qui recommence sous un autre nom, tablette prêtée…) restent ceux d'une interro classique : la page ne surveille pas les élèves.

## Utilisation

Ouvrir `index.html` dans un navigateur. Pour que les élèves y accèdent depuis leur tablette, publier le dossier en ligne, par exemple avec GitHub Pages ou la Forge des communs numériques éducatifs. Le QR code et le lien élève n'apparaissent que sur la version en ligne.

## Origine et licence

Ce projet est une **version modifiée d'[AutoDNB3ème](https://auriaphilippe.forge.apps.education.fr/Automatismes_DNB)**, de Philippe Auria (Collège Fernand Donatien, Fort-de-France), distribué sous licence **GNU GPL v3**. Il est distribué sous la même licence (voir [LICENSE](LICENSE)).

Modifications apportées en octobre 2026 : la banque de questions (générateurs et figures) est reprise sans changement ; l'interface est entièrement refaite et complétée par le tirage reproductible par code, le mode projection, les copies sur tablette avec correction automatique et code résultat, l'impression (feuille réponse, feuille vierge, sujet, corrigé), les listes de classe, l'envoi automatique et chiffré des résultats, et le relevé des notes avec statistiques par question et copie vers Pronote.

Le générateur de QR code provient du projet « Chatbot pix » de la Drane d'Orléans-Tours (licence MIT, © 2026 Drane d'Orléans-Tours).
