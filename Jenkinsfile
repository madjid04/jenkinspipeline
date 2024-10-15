pipeline {
    
    agent any

    tools {
        maven "M3"
    }

    stages {
        stage('Checking out the repo') {
            steps {
                checkout changelog: true, poll: true, scm: [$class: 'GitSCM', branches: [[name: '*/main'], [name: '*/dev']], 
                extensions: scm.extensions, userRemoteConfigs: [[credentialsId: '086d9d12-b52a-44b4-9907-e3ea5487b4e1', url: 'https://github.com/madjid04/jenkinspipeline.git']]]
                sh "ls -lart ./"    
            }
        }
        stage('Compling ...') {
            steps {
                withMaven(maven:'M3') {
                    sh "mvn compile"
                }
            }
        }
		stage('Testing ...') {
            steps {
                sh "mvn test"
            }
        }        
        stage('Building ...') {
            steps {
                sh "mvn -B -DskipTests clean package"
            }
        }        

    }
}