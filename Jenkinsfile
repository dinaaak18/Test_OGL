pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh './gradlew build'
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }
        stage('Archive Test Results') {
                    steps {
                        echo 'Archiving test results..'
                        archiveArtifacts artifacts: 'build/test-results/**/*.xml', allowEmptyArchive: true
                    }
                }
        stage('Generate Cucember Reports') {
                     steps {
                     echo 'Generating reports..'
                     sh './gradlew generateCucumberReports'                                    }
                 }

        stage('Build') {
                     steps {
                     echo 'Building Project..'
                     sh './gradlew jar'
                     sh './gradlew javadoc'

                    }
                 }

        /*stage('Unit Tests') {
            steps {
                echo 'Testing..'
                bat './gradlew sonar'
            }
        }*/
    }
}
