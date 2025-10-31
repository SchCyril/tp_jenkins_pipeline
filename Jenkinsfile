pipeline {
    agent testnode

    stages {

        stage('Dependances') {
            steps {
                echo "📦 Installation d’Apache2..."
                sh '''
                    apt update -y
                    apt install -y apache2
                    systemctl start apache2
                    systemctl enable apache2
                '''
                echo "✅ Apache2 installé et démarré."
            }
        }

        stage('Checkout') {
            steps {
                echo "📥 Récupération du code source depuis le dépôt..."
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Déploiement de l’application dans /var/www/html/..."
                sh '''
                    rm -rf /var/www/html/*
                    cp -r * /var/www/html/
                '''
                echo "✅ Fichiers copiés dans /var/www/html/"
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Vérification du déploiement..."
                sh '''
                    curl -f http://localhost/ || (echo "❌ Le site ne répond pas !" && exit 1)
                '''
                echo "✅ Le site est accessible sur http://localhost/"
            }
        }
    }

    post {
        success {
            echo "🎉 Déploiement réussi !"
            echo "L’application est en ligne et Apache fonctionne correctement."
        }

        failure {
            echo "❌ Une erreur est survenue pendant le pipeline."
            echo "⚠️ Déploiement interrompu."
        }

        always {
            echo "🧹 Nettoyage de l’environnement..."
            sh '''
                echo "Suppression du contenu de /var/www/html..."
                rm -rf /var/www/html/*
                echo "Désinstallation d’Apache2..."
                apt remove -y apache2
                apt autoremove -y
            '''
            echo "✅ Environnement nettoyé."
        }
    }
}
