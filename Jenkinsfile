pipeline {
    agent {
        docker {
            image 'node:latest' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
    }
}
stage('Install Dependencies') {
    steps {
        // Tambahkan flag --no-audit agar Jenkins tidak berhenti karena masalah keamanan paket
        sh 'npm install --no-audit'
    }
}