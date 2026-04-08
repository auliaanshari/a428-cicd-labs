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

    // Kriteria 2 & Saran 3: Deploy
    stage('Deploy') {
        echo "1. Menembakkan perintah Deploy ke Vercel (dari mesin Jenkins)..."
        // Jenkins menekan remot Vercel menggunakan curl bawaannya
        sh "curl -X POST https://api.vercel.com/v1/integrations/deploy/prj_K1MDqlQAXMONQc1rr4k3kGZH6ULD/KxH064jBXh"
        
        // Sesi Docker Kedua: Hanya untuk Deploy Lokal (Kriteria 3)
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            echo "2. Melakukan Deployment Lokal..."
            sh './jenkins/scripts/deliver.sh'
            
            echo "Aplikasi berjalan. Menjeda pipeline selama 1 menit..."
            sleep time: 1, unit: 'MINUTES'
            
            echo "Waktu habis! Menghentikan aplikasi lokal..."
            sh './jenkins/scripts/kill.sh'
        }
    } 
}