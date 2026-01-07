node {
    stage('Checkout') {
        // Mengambil kode dari GitHub
        checkout scm
    }

    stage('Build') {
        echo 'Menjalankan Tahap Build...'
        // Simulasi build, atau jika ada npm: sh 'npm install'
        sh 'echo "Proses Build Selesai"'
    }

    stage('Test') {
        echo 'Menjalankan Tahap Test...'
        // Simulasi test
        sh 'echo "Semua Test Pass"'
    }

    stage('Archive') {
        // Membuat file log sederhana agar muncul di tab Artifacts Blue Ocean
        sh 'echo "Build Log untuk Submission" > build.log'
        archiveArtifacts artifacts: 'build.log', followSymlinks: false
    }
}