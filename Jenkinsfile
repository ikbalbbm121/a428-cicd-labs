pipeline {
    agent {
        docker {
            image 'node:latest' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Install & Build') { 
            steps {
                // Tahap instalasi
                sh 'npm install --no-audit --quiet'
                
                // Tambahkan perintah build agar ada proses selanjutnya
                // sh 'npm run build' 
            }
        }
        stage('Test') {
            steps {
                // Contoh jika ingin menjalankan test (pastikan ada script test di package.json)
                sh 'echo "Running tests..." '
            }
        }
    }
}