node {
    stage('Checkout') {
        git branch: 'react-app', url: 'https://github.com/ikbalbbm121/a428-cicd-labs.git'
    }
    stage('Build') {
        // Ubah build.log menjadi log.txt
        sh 'echo "Menjalankan Build..." > log.txt'
    }
    stage('Test') {
        // Tambahkan isi ke log.txt
        sh 'echo "Menjalankan Test..." >> log.txt'
    }
    stage('Archive') {
        // Simpan sebagai log.txt agar muncul di tab Artifacts dengan nama yang benar
        archiveArtifacts artifacts: 'log.txt', followSymlinks: false
    }
}
stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                sh './jenkins/scripts/kill.sh'
            }
        }