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
                   echo "app version: ${appversion}"
                }    
            }
        }
        stage('Test') {
            steps {
                echo "Testing"
            }
        }
        stage('Deploy') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello too?')
            //     }
            // }    
            when {
                expression { "$params.DEPLOY" == "true" }
            }
            steps {
                echo "Deploying"
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

