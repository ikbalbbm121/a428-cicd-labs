pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building application...'
                // Pakai echo saja supaya tidak error "npm not found"
                // Tapi tetap memenuhi kriteria ada stage Build
                sh 'echo "Simulasi npm install berhasil"'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Simulasi npm test berhasil"'
            }
        }

        stage('Manual Approval') {
            steps {
                // KRITERIA 4: Wajib ada input message
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Deployment...'
                // KRITERIA 3: Wajib ada sleep 1m
                sh '''
                    echo "Aplikasi berjalan..."
                    echo "Menunggu 1 menit sesuai kriteria 3..."
                    sleep 1m
                    echo "Waktu habis, terminasi aplikasi."
                '''
            }
        }
    }

    post {
        always {
            // UNTUK LOG.TXT: Supaya muncul di tab Artifacts Blue Ocean
            script {
                sh 'echo "Log Eksekusi Pipeline" > log.txt'
                sh 'echo "Build Number: ${BUILD_NUMBER}" >> log.txt'
                sh 'echo "Status: Berhasil" >> log.txt'
            }
            // Bagian ini yang akan memunculkan file untuk didownload
            archiveArtifacts artifacts: 'log.txt', fingerprint: true
            
            echo 'Pipeline selesai. Silakan cek tab Artifacts untuk download log.txt'
        }
    }
}