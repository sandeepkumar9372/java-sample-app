pipeline{
    agent any
    
    tools {
        maven 'Maven-3.9.5'
    }
    
    parameters{
        string(name: 'Enter_Name', defaultValue: 'Mr Jenkins', description: 'Enter your name')
        booleanParam(name: 'Run_Sonar', defaultValue: true, description: 'Check this box to run sonar')
        choice(name: 'Environment', choices: ['dev', 'prod'], description: 'Select the environment')
    }
    
    stages{
        stage('Git Clone'){
            steps{
                echo "Cloning repository....."
                git branch: 'main', credentialsId: 'GITHUB-CREDENTIALS', url: 'https://github.com/DevopsFarmer/java-sample-app.git'
            }
        }
        
        stage('Build'){
            
            steps{
                echo '\033[34mHello\033[0m \033[33mcolorful\033[0m \033[35mworld!\033[0m'
                ansiColor('xterm'){
                //Building with Maven
                echo '\033[34mHello\033[0m \033[33mcolorful\033[0m \033[35mworld!\033[0m'
                sh 'mvn clean install'
            }
            }
        }
        
        stage('Sonar Scan'){
            when {
                expression {params.Run_Sonar}
            }
            
            environment {
                scannerHome = tool 'Sonar'
            }
            steps{
                withSonarQubeEnv('SonarQubeServer'){
                    echo "Starting sonar scanning......"
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Lur_IT_Java -Dsonar.sources=. -Dsonar.java.binaries=target/classes/com/mycompany/app/"
                }
            }
        }
        
        stage('Quality Gate'){
            when {
                expression {params.Run_Sonar}
            }
            steps{
                waitForQualityGate abortPipeline: true
            }
        }
        
        stage('Artifactory Deploy'){
            steps{
                script{
                    if(params.Environment == 'dev'){
                    rtUpload (
                        serverId: 'lur-it-artifactory',
                        spec: '''{
                            "files": [
                                {
                                    "pattern": "target/*.jar",
                                    "target": "lurit-generic-local/"
                                }
                            ]
                        }'''
                        
                    )
                    }
                    else{
                        rtUpload (
                        serverId: 'lur-it-artifactory',
                        spec: '''{
                            "files": [
                                {
                                    "pattern": "target/*.jar",
                                    "target": "prod-lur-it/"
                                }
                            ]
                        }'''
                        
                        )
                    }
                }
            }
        }
        
    }
}
