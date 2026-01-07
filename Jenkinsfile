node {
    stage('Checkout') {
        // Mengambil kode dari repositori
        checkout scm
    }

    stage('Build') {
        echo 'Melakukan Instalasi Dependensi...'
        // Perintah asli membangun aplikasi
        sh 'npm install'
    }

    stage('Test') {
        echo 'Menjalankan Unit Testing...'
        // CI=true agar test tidak hang/berhenti menunggu input
        sh 'CI=true npm test'
    }

    stage('Archive') {
        echo 'Mengarsipkan Log...'
        // Perintah untuk menyimpan log ke tab artifacts
        archiveArtifacts artifacts: 'log.txt', followSymlinks: false
    }
}