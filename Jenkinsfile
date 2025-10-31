pipeline {
    agent any

    stages {
        stage('Dependances') {
            steps {
                echo "🔧 Installation d'Apache2..."
                sh '''
                    sudo apt update -y
                    sudo apt install -y apache2
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
                sh 'cp -r /var/www/html /var/www/html.backup  true'
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Déploiement du site..."
                sh 'cp -r * /var/www/html/'
            }
        }

        stage('Test') {
            steps {
                echo "🔍 Vérification..."
                sh '''
                    service apache2 start  true
                    sleep 3
                    curl -f http://localhost/  exit 1
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
                sudo rm -rf /var/www/html/*
                sudo apt remove -y apache2  true
                sudo apt autoremove -y || true
            '''
        }
    }
}