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

        stage('Sonar Scanning'){
            environment {
                scanner = tool 'Sonar'
            }
            steps{
                withSonarQubeEnv('SonarQubeServer'){
                    sh '''
                        ${scanner}/bin/sonar-scanner -Dsonar.projectKey=KloudLabJavaApp \
                        -Dsonar.sources=. \
                        -Dsonar.projectName=KloudLab \
                        -Dsonar.java.binaries=target/
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                echo "Deploying..."
            }
        }
    }
}
