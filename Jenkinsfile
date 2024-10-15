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
		stage('Code coverage') {
            steps {
                withMaven(maven:'M3') {
                    sh "mvn clean cobertura:cobertura install test -Dcobertura.report.format=xml"                    
                }
            }
            post {
                always {
                    junit '**/test-reports/*.xml'
                    step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 2, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])

                }
            }
        }

    }
}