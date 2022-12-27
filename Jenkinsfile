
pipeline {
   agent any
    tools {
  maven 'M2_HOME'
}
environment {
    registry = '231402041009.dkr.ecr.us-east-1.amazonaws.com/jenkins-images'
    registryCredential = 'jenkins-ecr'
    dockerimage = ''
    SONAR_TOKEN = 'cdbbf9ebf833e93c819f110c8dd3702dfbf752ac'
}

    stages {

        stage("build & SonarQube analysis") {
            agent {
        docker { image 'maven:3.8.6-openjdk-11-slim' }
   }
            
            
            steps {
              withSonarQubeEnv('sonarQube') {
                  sh 'mvn sonar:sonar -Dsonar.projectKey=jawidsaboori_Geolocation1 -Dsonar.java.binaries=.'
              }
            }
          }
        
         
        stage('maven package') {
            steps {
                sh 'mvn clean'
                sh 'mvn install -DskipTests'
                sh 'mvn package -DskipTests'
            }
        }
        stage('Build Image') {
            
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
           
            
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }    
         
         
    }
}
