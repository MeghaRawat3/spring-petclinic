  
pipeline {
    agent {
        docker {
            image 'maven:9-alpine'
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
  
}
    }
}
