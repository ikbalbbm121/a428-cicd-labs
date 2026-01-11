pipeline {
    agent any

    stages {
        // Stage 1 & 2: Sesuai proyek pertama kamu (contoh: React)
        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test -- --watchAll=false'
            }
        }

        // Kriteria 4: Manual Approval
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }

        // Kriteria 2 & 3: Deploy Stage & Auto-Termination
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    // Menjalankan aplikasi di background (port 3000 untuk React)
                    sh 'npm start &'
                    
                    echo 'Aplikasi berhasil di-deploy. Menunggu 1 menit...'
                    
                    // Jeda selama 60 detik (1 menit)
                    sh 'sleep 60'
                    
                    echo 'Waktu habis. Menghentikan aplikasi otomatis...'
                    
                    // Mematikan process yang berjalan di port 3000
                    // Gunakan || true agar pipeline tidak fail jika process sudah mati
                    sh 'fuser -k 3000/tcp || true'
                }
            }
        }
    }
}