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
            echo "1. Menembakkan perintah Deploy ke Vercel..."
            // Jenkins menekan remot Vercel
            sh "curl -X POST https://api.vercel.com/v1/integrations/deploy/prj_K1MDqlQAXMONQc1rr4k3kGZH6ULD/KxH064jBXh"
            
            echo "2. Melakukan Deployment Lokal (Syarat Kriteria 3)..."
            sh './jenkins/scripts/deliver.sh'
            
            echo "Aplikasi berjalan. Menjeda pipeline selama 1 menit..."
            sleep time: 1, unit: 'MINUTES'
            
            echo "Waktu habis! Menghentikan aplikasi lokal..."
            sh './jenkins/scripts/kill.sh'
        }
    } 
}