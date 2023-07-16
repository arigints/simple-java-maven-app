node {
    stage('Build'){
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            checkout scm
            sh 'mvn -B -DskipTests clean package'
        }
    }
    try {
        stage('Test'){
            docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
                sh 'mvn test'
            }    
        }
    } finally {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
            junit 'target/surefire-reports/*.xml'
        }    
    }
    stage('Manual Approval'){
        input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
    }
    stage('Deploy'){
        if ( env.SERVER_ROLE == 'PRODUCTION') {
            docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
                sh './jenkins/scripts/deliver.sh'
                sleep(60)
            }
        } else {
            docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2'){
                sh './jenkins/scripts/deliver.sh'
                sleep(60)
            }
            docker.image('curlimages/curl').inside{
                sh "curl -u ${env.JENKINS_USERNAME}:${env.JENKINS_PASSWORD} ${env.JENKINS_PROD_URL}/job/java-maven/build?token=${env.JAVA_TOKEN}" //trigger build production via API
            }
        }
    }
}