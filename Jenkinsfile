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
    }
}
