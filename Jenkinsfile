node {
    stage('Checkout') {
        // Mengambil kode dari GitHub
        checkout scm
    }

    // Menggunakan container Node.js dan mengekspos port 3000
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
}