node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
        try {
            stage('Build'){ 
                checkout scm
                sh 'mvn -B -DskipTests clean package'
            }
        } finally {
            junit 'target/surefire-reports/*.xml'
        }
        stage('Test'){ 
            sh 'mvn test'
        }
        stage('Deliver'){
            sh './jenkins/scripts/deliver.sh'
        }
    }
}