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
            }
        }
        stage('Cleanup'){
			steps {
				echo 'prune and cleanup'
				

				mail body: 'project build successful',
                   from: 'sprasad.tech812@gmail.com',
                   replyTo: 'sprasad.tech812@gmail.com',
                   subject: 'project build successful',
                   to: 'sprasad@gmail.com'
        }
		}
    }
}
