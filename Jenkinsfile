pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/naresh9919/Shopping-Cart.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile -DskipTests=true"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test -DskipTests=true"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=shopping-cart \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=shopping-cart '''
    
                }
            }
        }

        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
         stage("Build"){
            steps{
                sh " mvn clean install -DskipTests=true"
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shopping-cart .'
                }
            }
        }
        
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'dockerhub-cred', toolName: 'docker') {
                        
                        sh "docker tag shopping-cart nareshbabu1991/shopping-cart:latest "
                        sh "docker push nareshbabu1991/shopping-cart:latest "
                    }
                }
            }
        }
    }
}
