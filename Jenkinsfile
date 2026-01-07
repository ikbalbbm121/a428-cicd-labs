pipeline {
    agent {
        docker {
            image 'node:18-alpine' // Disarankan menggunakan versi spesifik & ringan (alpine)
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Install & Build') { 
            steps {
                // Gunakan --no-audit agar tidak berhenti karena peringatan keamanan
                // Gunakan --quiet agar log tidak terlalu kotor
                sh 'npm install --no-audit --quiet'
                
                // Biasanya setelah install, perlu menjalankan build
                // sh 'npm run build' 
            }
        }
    }
}