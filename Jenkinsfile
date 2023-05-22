pipeline {
    agent any
    tools {
        jdk  'jdk11'
        maven  'maven3'
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/naresh9919/Shopping-Cart.git'
            }
        }
        stage("Build Application"){
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        stage("Test Application"){
            steps {
                sh "mvn test -DskipTests=true"
            }
        }
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://65.0.125.172:9000/ -Dsonar.login=squ_09f8f857e5293e2f265be0eaadc2953b0a527080 -Dsonar.projectName=shopping-cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }
    }
