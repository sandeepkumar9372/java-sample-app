pipeline{
    agent any
    tools {maven 'Maven-3.9.6'}
    stages {
        stage('Git Clone'){
            steps {
                git branch: 'main', credentialsId: 'github-PAT', url: 'https://github.com/DevopsFarmer/java-sample-app.git'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }

        stage('Test Stage'){
            steps{
                echo "This is a test stage"
            }
        } 
    }
}