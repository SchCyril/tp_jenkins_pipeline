pipeline {
agent any
stages {
stage("Dependances") {
steps {
// Installer apache2
}
}
stage('Checkout') {
steps {
// Récupération du code
}
}
stage('Deploy') {
steps {
// Copie des fichiers vers le serveur web (/var/www/html/)
}
}
stage('Test') {
steps {
// Vérification du déploiement
}
}
}
post {
success {
// Action en cas de succès (echo, ou autre)
}
failure {
// Action en cas d'échec
}
always {
// Nettoyage
// Supprimer les fichiers copiés dans /var/www/html
// Desinstaller apache2
}
}
}