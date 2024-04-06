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
            when {
                expression {params.RUN_SONAR}
            }
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
        stage('Quality Gate Check'){
            when {
                expression {params.RUN_SONAR}
            }
            steps{
                waitForQualityGate abortPipeline: true
            }
        }
        stage('Artifact Upload')
        {
            steps{
                script{
                    rtUpload (
                        serverId: 'artifactory-server',
                        spec: '''{
                                    "files": [
                                        {
                                            "pattern": "target/*.jar",
                                            "target": "kloudlab-generic-local/"
                                        }
                                    ]
                        }'''
                    )
                }
            }
        }
        stage('Deploy'){
            steps{
                script{
                    if(params.Env == 'dev')
                    {
                        echo "You have selected dev environment"
                    }
                    if(params.Env == 'test')
                    {
                        echo "You have selected test environment"
                    }
                    if(params.Env == 'prod')
                    {
                        echo "You have selected prod environment"
                    }
                    print(params.Name)
                }
                
            }
        }
    }
}
