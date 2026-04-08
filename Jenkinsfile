node {
    stage('Checkout') {
        // Mengambil kode dari GitHub
        checkout scm
    }

    // Menggunakan container Node.js
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        
        stage('Build') {
            echo "Mengunduh dependencies React..."
            sh 'npm install'
        }
        
        stage('Test') {
            echo "Menjalankan testing React..."
            sh './jenkins/scripts/test.sh'
        }
        
        // Kriteria 4: Menambahkan Manual Approval sebelum Deploy
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
            // Tombol 'Proceed' dan 'Abort' akan otomatis muncul di Jenkins
        }
        
        // Kriteria 2 & 3: Deploy Stage & Jeda 1 Menit
        stage('Deploy') {
            echo "Melakukan Deployment Lokal..."
            // Menjalankan React App di background
            sh './jenkins/scripts/deliver.sh'
            
            echo "Aplikasi berjalan. Menjeda pipeline selama 1 menit..."
            // Menjeda eksekusi tepat 1 menit
            sleep time: 1, unit: 'MINUTES'
            
            echo "Waktu habis! Menghentikan aplikasi..."
            // Mematikan aplikasi yang berjalan di background
            sh './jenkins/scripts/kill.sh'
        }
    }
}