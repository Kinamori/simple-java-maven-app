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
    }
}

node{
   stage('SCM Checkout'){
     git 'https://github.com/Kinamori/simple-java-maven-app'
   }
	
   stage('Compile-Package'){
      sh "mvn clean package"
   }

   stage('SonarQube Analysis') {
        withSonarQubeEnv('SonarQube') {
          sh "mvn sonar:sonar"
        }
   }
}
