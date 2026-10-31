pipeline {
    // Pre build section
    agent {
        node {
            label 'AGENT-1'
    
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "574826928727"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
      
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }    
    // This is build section
    stages {
        stage('Read Version') {
            steps {
                script{
                   def packageJSON = readJSON file: 'package.json'
                   appVersion = packageJSON.version 
                   echo "app version: ${appVersion}"
                }    
            }
        }
        stage('Install Dependencies') {
            steps {
                sh """
                   npm install
                """
            }
        }
        stage('Build Image') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
    
                    }
                }
                

            }
        }    
        
       
    }
    post {
        always {
            echo 'I will always say Hello!'
            cleanWs() 
        }
        success {
            echo 'I will run if success'
        }
        failure {
            echo 'I will run if failure'
        }
        aborted {
            echo 'Pipeline is aborted'
        }
    }
}

