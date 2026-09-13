pipeline {
    agent {
        label 'AGENT-1'
    }
/*     environment {
        // Environment variables
        MY_VAR = 'My First Agent'
    } */
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
        stage('Build') {
            steps {
                sh """ 
                    echo "Hellow Build"
                    sleep 10    // Simulate a long build.. testing timeout option with 10 SECONDS
                    env
                    echo "Hello ${params.PERSON}"
                """
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
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