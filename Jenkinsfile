pipeline {
    agent any
    tools {
        jdk  'jdk11'
        maven  'maven3'
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
                "mvn sonar:sonar \
                -Dsonar.projectKey=sonar-server \
                -Dsonar.host.url=http://65.0.125.172:9000 \
                -Dsonar.login=sqp_5b2d85a49c90303497e603ac4b68eaacf0a49df3"
               }
            }
        }
    }
