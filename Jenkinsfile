pipeline {
    agent any

    stages {


        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew test'
                echo 'Archiving test results..'
                archiveArtifacts artifacts: 'build/test-results/**/*.xml', allowEmptyArchive: true
                echo 'Generating reports..'
                sh './gradlew generateCucumberReports'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code quality with SonarQube..'
                withSonarQubeEnv('sonar') {
                   sh './gradlew sonarqube'  // Execute SonarQube analysis
                }
            }
            }

        stage('Quality Gate') {
        steps {
            echo 'Waiting for SonarQube quality gate...'
            // Wait for the SonarQube analysis to complete and check the quality gate status
            waitForQualityGate abortPipeline: true  // If the gate fails, the pipeline will be aborted
            }
           }
        stage('Build') {
                     steps {
                     echo 'Building Project..'
                     sh './gradlew jar'
                     sh './gradlew javadoc'
                    echo 'Archiving Artifacts...'
                    archiveArtifacts artifacts: '**/build/libs/TP5-1.0-SNAPSHOT.jar, **/build/tmp/javadoc/**/*', fingerprint: true
                    }
                 }
        stage('Deploy') {
                  steps {
                      echo 'Deploying Project..'
                      sh './gradlew publish'
                      }
                }
        stage('Notification') {
               steps {
                    echo 'Sending Notification..'
                    mail bcc: '', body: 'Project succefully deployed', cc: '', from: 'kd_keddour@esi.dz', replyTo: '', subject: 'Deploy success', to: 'keddourdina@gmail.com'                    sh './gradlew postBuiltSucceedToSlack'
                    }
               }
    }
}
