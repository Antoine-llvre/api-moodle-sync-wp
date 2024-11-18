
Pour synchroniser les utilisateurs entre WordPress et Moodle, vous aurez besoin d'un mécanisme qui permet de créer, mettre à jour ou supprimer des utilisateurs des deux systèmes (WordPress et Moodle) lorsque des actions sont effectuées dans l'un ou l'autre. Voici une méthode pour y parvenir.

### Prérequis :
1. **Moodle Web Services** : Moodle offre des API via des Web Services qui permettent de créer, mettre à jour et supprimer des utilisateurs via des requêtes HTTP. Vous devez activer les services web dans Moodle et obtenir une clé API pour effectuer ces actions depuis WordPress.
2. **Plugin WordPress** : Vous aurez besoin d'un plugin ou d'un script personnalisé dans WordPress pour interagir avec les API de Moodle.

### Étapes :

#### 1. Créer une API dans Moodle
Assurez-vous que les services Web sont activés dans Moodle, puis créez un service web pour accéder aux fonctionnalités utilisateurs.

1. Connectez-vous à Moodle en tant qu'administrateur.
2. Allez dans **Administration du site > Web services > Gestion des services** et activez "Web services" si ce n'est pas déjà fait.
3. Créez un nouveau service web dans **Administration du site > Web services > Services externes**.
4. Ajoutez les fonctions suivantes au service :
   - `core_user_create_users`
   - `core_user_delete_users`
   - `core_user_update_users`
5. Créez un utilisateur qui aura accès aux services Web, et assignez-lui des permissions pour utiliser ces fonctions.
6. Générer une clé API pour ce service.

#### 2. Script PHP pour WordPress
Vous allez maintenant ajouter un script dans WordPress qui interagit avec l'API Moodle à chaque création, mise à jour ou suppression d'un utilisateur. Voici un exemple de code `code.php` à placer dans le fichier `functions.php` de votre thème WordPress ou dans un plugin personnalisé.
