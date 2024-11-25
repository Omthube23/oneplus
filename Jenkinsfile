pipeline {
    agent any 

    stages {
        stage('01.Clone Repo') {
            steps {
                echo "Git Cloning"
                git branch: 'main', credentialsId: 'Omthube23', url: 'git@github.com:Omthube23/oneplus.git'
            }
        }
        stage('02.Clean') {
            steps {
                echo "Cleaning"
                sh 'mvn clean'
            }
        }
        stage('03.Package') {
            steps {
                echo "Packaging"
                sh 'mvn package'
            }
        }
        stage('04.Deploy to Nexus'){
            steps {
                echo "Deploy to Nexus"
                sh 'curl -v -u admin:admin --upload-file target/oneplus-0.0.1-SNAPSHOT.jar http://172.30.32.1:8081/repository/Oneplus_app/com/mobile/oneplus/0.0.1/oneplus-0.0.1-SNAPSHOT.jar'
            }
        }
    }
}
