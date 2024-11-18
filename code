// URL de votre Moodle et votre clé API
define('MOODLE_URL', 'https://www.xxxxxxxxxxxx');
define('MOODLE_API_KEY', 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'); // Nouveau jeton

// Fonction pour créer un utilisateur dans Moodle
function create_user_in_moodle($user_data) {
    $url = MOODLE_URL . '/webservice/rest/server.php';

    // Log des données envoyées avant de les envoyer à Moodle
    error_log('Données utilisateur avant envoi à Moodle : ' . print_r($user_data, true));

    $params = array(
        'wstoken' => MOODLE_API_KEY,  // Ton jeton d'authentification
        'wsfunction' => 'core_user_create_users',  // Fonction Moodle pour créer des utilisateurs
        'moodlewsrestformat' => 'json',
        'users' => array($user_data) // Envoie l'utilisateur dans le format Moodle
    );

    // Effectuer la requête API avec wp_remote_post
    $response = wp_remote_post($url, array(
        'body' => $params
    ));

    // Récupérer et décoder la réponse
    $response_body = wp_remote_retrieve_body($response);
    $result = json_decode($response_body, true);

    // Log de la réponse de Moodle pour le débogage
    error_log('Réponse de Moodle : ' . print_r($result, true));

    // Gérer la réponse (si une exception est renvoyée, loggez l'erreur)
    if (isset($result['exception'])) {
        error_log('Erreur API Moodle lors de la création de l\'utilisateur: ' . $result['message']);
        return $result;
    }

    // Retourne le résultat de l'API
    return $result;
}

// Hook WordPress pour créer un utilisateur
add_action('user_register', 'sync_user_with_moodle', 10, 1);
function sync_user_with_moodle($user_id) {
    $user = get_userdata($user_id);  // Récupérer les données de l'utilisateur WordPress

    // Générer un mot de passe aléatoire (longueur de 12 caractères ici)
    $password = wp_generate_password(12, true);

    // Préparer les données utilisateur dans le format Moodle
    $user_data = array(
        'username' => $user->user_login,       // Nom d'utilisateur unique
        'password' => $password,               // Mot de passe généré aléatoirement
        'firstname' => $user->first_name,      // Prénom de l'utilisateur
        'lastname' => $user->last_name,        // Nom de famille de l'utilisateur
        'email' => $user->user_email,          // Email de l'utilisateur
        'auth' => 'manual',                    // Authentification manuelle
        'lang' => 'fr',                        // Langue de l'utilisateur dans Moodle
        'idnumber' => $user->ID,               // ID unique de l'utilisateur (peut être utilisé comme identifiant dans Moodle)
        'description' => $user->description,   // Description de l'utilisateur (si définie dans WordPress)
        'city' => $user->city,                 // Ville de l'utilisateur (si définie dans WordPress)
        'country' => $user->country,           // Pays de l'utilisateur (si défini dans WordPress)
        // Ajoutez ici d'autres champs personnalisés si nécessaire
    );

    // Crée l'utilisateur dans Moodle
    $result = create_user_in_moodle($user_data);

    // Vérifier si l'utilisateur a été créé avec succès
    if (isset($result['exception'])) {
        // Erreur lors de la création de l'utilisateur
        error_log('Erreur lors de la création de l\'utilisateur Moodle: ' . $result['message']);
    } else {
        // L'utilisateur a été créé avec succès dans Moodle
        error_log('Utilisateur créé avec succès dans Moodle: ' . $user->user_login);
    }

    // Optionnel : Mettre à jour le mot de passe dans WordPress (si nécessaire)
    wp_set_password($password, $user_id); // Met à jour le mot de passe de l'utilisateur dans WordPress
}
?>
