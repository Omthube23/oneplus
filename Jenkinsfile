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
            steps{
                echo "Deploy to Nexus"
                 sh 'curl -v -u admin:admin --upload-file target/oneplus-0.0.1-SNAPSHOT.jar http://172.30.32.1:8081/repository/Oneplus_app/com/mobile/oneplus/0.0.1/oneplus-0.0.1-SNAPSHOT.jar'                
            }
        }
        // (Optional) Verify the deployment by checking if the artifact exists in Nexus( HTTP request Plugins required )
        stage('05.Verify Deployment') {
            steps {
                echo "Verifying Deployment"
                 script {
                     def nexusURL = "http://172.30.32.1:8081/repository/Oneplus_app/com/mobile/oneplus/0.0.1/oneplus-0.0.1-SNAPSHOT.jar"
                     def response = httpRequest(url: nexusURL, validResponseCodes: '200')
                     echo "Deployment verification response: ${response.content}"
                 }
            }
        }
        stage("06.SCP to Docker Server")
        {
            steps{
                echo "SCP To Docker Server"
                 script {
                     sshagent(credentials: ['id_rsa']) {  // ssh-agent plugin required to install
                         try {
                             def result = sh(script: 'scp -v -i /home/ubuntu/.ssh/id_rsa target msexcel-0.0.1-SNAPSHOT.jar pwd1:/root/Java-CBCICD/target/', returnStdout: true)
                             echo "SCP command output: ${result}"
                         }
                         catch (Exception e) {
                             echo "SCP failed: ${e.getMessage()}"
                         }
                     }
                 }
            }    
        }
    }
}

