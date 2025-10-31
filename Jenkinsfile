pipeline {
    agent {
        docker {
             args '-u root'
        }
    }

    stages {
        stage('Dependances') {
            steps {
                echo "🔧 Installation d'Apache2..."
                sh '''
                    apt update -y
                    apt install -y apache2 curl
                '''
            }
        }

        stage('Checkout') {
            steps {
                echo "📦 Récupération du code..."
                checkout scm
            }
        }

        stage('Backup') {
            steps {
                echo "💾 Sauvegarde du répertoire..."
                sh '''
                    if [ -d /var/www/html ]; then
                        cp -r /var/www/html /var/www/html.backup
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Déploiement du site..."
                sh '''
                    rm -rf /var/www/html/*
                    cp -r * /var/www/html/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "🔍 Vérification..."
                sh '''
                    service apache2 start || true
                    sleep 3
                    curl -f http://localhost/ || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Déploiement réussi !"
        }
        failure {
            echo "❌ Le déploiement a échoué."
        }
        always {
            echo "🧹 Nettoyage..."
            sh '''
                rm -rf /var/www/html/*
                apt remove -y apache2 || true
                apt autoremove -y || true
            '''
        }
    }
}
