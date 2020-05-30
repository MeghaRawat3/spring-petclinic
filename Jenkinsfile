  
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
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
	stage('containerize') {
	    steps {
		export BUILD=$BUILD_NUMBER
		sh './containerize.sh'
    }
    post {
        always {
            echo 'Jenkins has finished the job'
            deleteDir() /* clean up our workspace */
        }
        success {
            slackSend channel: '#openshift-on-aws',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
        failure {
            slackSend channel: '#openshift-on-aws',
                  color: 'bad',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."'
        }
}
