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
	    stage('Compile-Package'){
	      // Get maven home path
	      def mvnHome =  tool name: 'maven-3', type: 'maven'
	      sh "${mvnHome}/bin/mvn package"
	   }

	   stage('SonarQube Analysis') {
		def mvnHome =  tool name: 'maven-3', type: 'maven'
		withSonarQubeEnv('SonarQube') {
		  sh "${mvnHome}/bin/mvn sonar:sonar"
		}
	   }
    }
}
