pipeline {
    agent any

    tools {
        // Pastikan nama 'node20' sesuai dengan yang kamu buat di Global Tool Configuration
        nodejs 'node20'
    }

    environment {
        // Mendefinisikan port agar mudah diatur dan dimatikan nanti
        APP_PORT = '3000'
    }

    stages {
        stage('Build') {
            steps {
                echo '--- Tahap Build Dimulai ---'
                sh 'npm install'
                // sh 'npm run build' // Aktifkan jika aplikasi butuh build production
            }
        }

        stage('Test') {
            steps {
                echo '--- Tahap Testing Dimulai ---'
                // CI=true memastikan test tidak berjalan selamanya (non-interactive)
                sh 'CI=true npm test'
            }
        }

        // Kriteria 4: Manual Approval
        stage('Manual Approval') {
            steps {
                echo '--- Menunggu Persetujuan Manual ---'
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }

        // Kriteria 2 & 3: Deploy Stage & Jeda Otomatis
        stage('Deploy') {
            steps {
                script {
                    echo '--- Tahap Deploy Dimulai ---'
                    
                    // Menjalankan aplikasi di background
                    // Penggunaan 'BUILD_ID=dontKillMe' agar Jenkins tidak mematikan process setelah stage selesai
                    sh 'BUILD_ID=dontKillMe npm start &'
                    
                    echo "Aplikasi berjalan di port ${APP_PORT}. Menunggu 1 menit..."
                    
                    // Kriteria 3: Jeda 1 menit (60 detik)
                    sleep time: 1, unit: 'MINUTES'
                    
                    echo 'Waktu 1 menit berselang. Menghentikan aplikasi otomatis...'
                    
                    // Mematikan aplikasi berdasarkan port
                    // 'fuser -k' akan mencari process di port tersebut dan mematikannya
                    sh "fuser -k ${APP_PORT}/tcp || true"
                }
            }
        }
    }

    post {
        always {
            echo '--- Pipeline Selesai ---'
        }
        success {
            echo 'Pipeline berhasil dijalankan sepenuhnya!'
        }
        failure {
            echo 'Pipeline gagal. Silakan cek log di atas.'
            // Opsional: Memastikan port bersih jika terjadi error di tengah jalan
            sh "fuser -k ${APP_PORT}/tcp || true"
        }
    }
}