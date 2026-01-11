pipeline {
    agent any

    tools {
        // Nama ini HARUS SAMA dengan kolom 'Name' yang kamu isi di Jenkins UI tadi
        nodejs 'node-terbaru'
    }

    environment {
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
                // CI=true agar tidak stuck saat running test di Jenkins
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
                    
                    // KRITERIA 3: Jalankan aplikasi di background
                    // JENKINS_NODE_COOKIE=dontKillMe supaya Jenkins tidak mematikan app setelah stage selesai
                    sh 'JENKINS_NODE_COOKIE=dontKillMe npm start &'
                    
                    echo "Aplikasi berhasil di-deploy di port ${APP_PORT}."
                    echo "Menunggu 1 menit (sesuai kriteria 3) sebelum terminasi otomatis..."
                    
                    // KRITERIA 3: Jeda 1 menit
                    sleep time: 1, unit: 'MINUTES'
                    
                    echo 'Waktu 1 menit habis. Menghentikan aplikasi otomatis...'
                    
                    // KRITERIA 3: Otomatis berakhir (Terminasi proses)
                    // Menggunakan fuser untuk mematikan process di port 3000
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
            echo 'Pipeline gagal. Membersihkan port jika aplikasi masih hidup...'
            sh "fuser -k ${APP_PORT}/tcp || true"
        }
    }
}