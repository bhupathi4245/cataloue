pipeline {
    agent {
        label 'AGENT-1'
    }
    environment {
        // Environment variables
        appVersion = ''
        REGION = 'us-east-1'
        ACC_ID = '123456789012'
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
    }
    options {
        // Options
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }
/*     parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    } */
    // Build section (Out of 3 sections: Pre-Build, Build, Post-Build)
    stages {
        stage('Read package.json') {
            steps {
                script {
                    // Read and parse the JSON file from the workspace
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Project version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    withAWS(credentials: 'aws-creds', region: ${REGION}) {
                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                        
                    }
                }
            }
        }
    }

    // post section
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir() // Clean up our workspace
        }
        success { 
            echo 'Hello again! Success'
        }
        failure { 
            echo 'Hello again! Failure'
        }
    }
}