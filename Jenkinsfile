stage('Manual Approval') {
    steps {
        input message: 'Lanjutkan ke tahap Deploy?'
    }
}

stage('Deploy') {
    steps {
        script {
            // Jalankan aplikasi di background
            // Jika React:
            sh 'npm start &' 
            
            echo "Aplikasi berjalan... Menunggu 1 menit sebelum terminasi otomatis."
            
            // Kriteria 3: Jeda 1 menit
            sh 'sleep 60'
            
            // Kriteria 3: Otomatis berakhir (Kill process agar pipeline selesai)
            // Mencari PID yang berjalan di port (misal 3000) dan mematikannya
            sh 'fuser -k 3000/tcp || true' 
        }
    }
}