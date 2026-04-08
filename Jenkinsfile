node {
    stage('Checkout') {
        // Mengambil kode dari GitHub
        checkout scm
    }

    // Sesi Docker Pertama: Build & Test
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        
        stage('Build') {
            echo "Mengunduh dependencies React..."
            sh 'npm install'
        }
        
        stage('Test') {
            echo "Menjalankan testing React..."
            sh './jenkins/scripts/test.sh'
        }
    } 

    // Kriteria 4: Menambahkan Manual Approval (Aman di luar Docker)
    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?'
    }

    // Sesi Docker Kedua: Deploy
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        
        stage('Deploy') {
            echo "Melakukan Deployment Lokal..."
            sh './jenkins/scripts/deliver.sh'
            
            echo "Aplikasi berjalan. Menjeda pipeline selama 1 menit..."
            sleep time: 1, unit: 'MINUTES'
            
            echo "Waktu habis! Menghentikan aplikasi..."
            sh './jenkins/scripts/kill.sh'
        }
    } 
}