pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
        }
		}
        stage('Test') {
            steps {
              sh 'mvn test'
            }
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
                }
                script {
                    if (currentBuild.currentResult == 'FAILURE') { // Other values: SUCCESS, UNSTABLE
                        // Send an email only if the build status has changed from green/unstable to red
                        emailext subject: '$DEFAULT_SUBJECT',
                            body: '$DEFAULT_CONTENT',
                            recipientProviders: [
                                [$class: 'CulpritsRecipientProvider'],
                                [$class: 'DevelopersRecipientProvider'],
                                [$class: 'RequesterRecipientProvider']
                            ],
                            replyTo: '$DEFAULT_REPLYTO',
                            to: '$DEFAULT_RECIPIENTS'
                    }
                }

            }
        }
    }
}
