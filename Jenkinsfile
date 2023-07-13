node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
        stage('Build'){
            checkout scm
            sh 'mvn -B -DskipTests clean package'
        }
        try {
            stage('Test'){
                sh 'mvn test'
                input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
            }
        } finally {
            junit 'target/surefire-reports/*.xml'
            }
        stage('Deliver'){
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
        }
    }
}