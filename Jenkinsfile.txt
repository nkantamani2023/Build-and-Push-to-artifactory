pipeline {
    agent any
    tools{
        maven 'maven-3.9.9'
    }

    stages {
        stage('Clone the repository') {
            steps {
                git credentialsId: 'Github_username_password', url: 'https://github.com/nkantamani2023/Build-and-Push-to-artifactory.git'
            }
        }
        stage('Build thecode') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Deploy to tomcat server'){
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat_ID', path: '', url: 'http://43.204.141.27:8080')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
