pipeline {
    agent any

    tools {
        // PASTIKAN nama ini SAMA dengan yang ada di Manage Jenkins > Tools
        nodejs 'node20' 
    }

    environment {
        // Mendefinisikan port aplikasi agar mudah dimatikan nanti
        APP_PORT = '3000'
    }

    stages {
        stage('Build') {
            steps {
                echo '--- Stage: Build ---'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo '--- Stage: Test ---'
                // CI=true agar test berhenti otomatis setelah selesai (tidak nunggu input)
                sh 'CI=true npm test'
            }
        }

        // KRITERIA 4: Manual Approval
        stage('Manual Approval') {
            steps {
                echo '--- Stage: Manual Approval ---'
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }

        // KRITERIA 2: Deploy Stage
        stage('Deploy') {
            steps {
                script {
                    echo '--- Stage: Deploy ---'
                    
                    // KRITERIA 3: Menjalankan aplikasi & Jeda 1 menit
                    // JENKINS_NODE_COOKIE memastikan Jenkins tidak membunuh proses background terlalu cepat
                    sh "JENKINS_NODE_COOKIE=dontKillMe npm start &"
                    
                    echo "Aplikasi berjalan di port ${APP_PORT}. Menunggu 1 menit sebelum otomatis berakhir..."
                    
                    // Tunggu 1 menit
                    sleep time: 1, unit: 'MINUTES'
                    
                    echo 'Waktu habis! Menghentikan aplikasi...'
                    
                    // KRITERIA 3: Otomatis berakhir menggunakan fuser
                    // || true digunakan agar pipeline tetap sukses meskipun proses sudah berhenti duluan
                    sh "fuser -k ${APP_PORT}/tcp || true"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline selesai dikerjakan.'
        }
        failure {
            echo 'Ada masalah pada pipeline. Membersihkan port...'
            sh "fuser -k ${APP_PORT}/tcp || true"
        }
    }
}