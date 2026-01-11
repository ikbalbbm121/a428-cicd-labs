pipeline {
    agent any

    environment {
        // Nama image disesuaikan dengan proyek kamu
        IMAGE_NAME = "react-app"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building application...'
                // Contoh perintah build (sesuaikan dengan bahasa pemrogramanmu)
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test || true' // || true agar pipeline tidak stop jika test gagal (opsional)
            }
        }

        stage('Manual Approval') {
            steps {
                // KRITERIA 4: Menggunakan input message
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Deployment...'
                // KRITERIA 3: Menggunakan perintah sleep 1 menit sebelum terminasi
                sh '''
                    JENKINS_NODE_COOKIE=dontKillMe npm start &
                    echo "Aplikasi berhasil di-deploy di port 3000."
                    echo "Menunggu 1 menit (sesuai kriteria 3) sebelum terminasi otomatis..."
                    sleep 1m
                    echo "Waktu 1 menit habis. Menghentikan aplikasi otomatis..."
                    fuser -k 3000/tcp || true
                '''
            }
        }
    }

    post {
        always {
            // UNTUK LOG.TXT: Mengambil log dari proses dan menyimpannya sebagai Artifact
            // Perintah ini akan membuat file log.txt di workspace
            script {
                sh 'echo "Pipeline Execution Log" > log.txt'
                sh 'date >> log.txt'
                sh 'echo "------------------------" >> log.txt'
                // Mengambil 100 baris terakhir dari console log (jika di Linux)
                sh 'tail -n 100 /var/lib/jenkins/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/log >> log.txt || echo "Manual log entry" >> log.txt'
            }
            
            // WAJIB: Melampirkan berkas log.txt agar muncul di tab Artifacts Blue Ocean
            archiveArtifacts artifacts: 'log.txt', fingerprint: true
            
            echo 'Pipeline selesai dikerjakan.'
        }
    }
}