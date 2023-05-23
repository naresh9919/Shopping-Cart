pipeline {
    agent any
    tools {
        jdk  'jdk11'
        maven  'maven3'
    }
    environment{
        SCANNER_HOME= tool 'sonar scanner'
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
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://15.206.187.179:9000/ -Dsonar.login=sqp_cd0d48f9ad65e407a99bf5971f0115fe7102d6e0 -Dsonar.projectName=shopping-cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }
    }
