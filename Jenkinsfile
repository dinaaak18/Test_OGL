pipeline {
    agent any

    stages {
        //stages

        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew build'
                sh './gradlew test'
                echo 'Archiving test results..'
                archiveArtifacts artifacts: 'build/test-results/**/*.xml', allowEmptyArchive: true
                echo 'Generating reports..'
                sh './gradlew generateCucumberReports'
            }
            post {
                                    failure {
                                    mail to: 'keddourdina@gmail.com',
                                         from: 'kd_keddour@esi.dz',
                                         subject: 'Build failed',
                                         body: 'Test failed.'

                                    }
                                }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code quality with SonarQube..'
                withSonarQubeEnv('sonar') {
                   sh './gradlew sonarqube'  // Execute SonarQube analysis
                }
            }
            post {
                                    failure {
                                    mail to: 'keddourdina@gmail.com',
                                         from: 'kd_keddour@esi.dz',
                                         subject: 'Analyse failed',
                                         body: 'Analyse failed.'

                                    }
                                }
            }

        stage('Quality Gate') {
        steps {
            echo 'Waiting for SonarQube quality gate...'
            // Wait for the SonarQube analysis to complete and check the quality gate status
            waitForQualityGate abortPipeline: true  // If the gate fails, the pipeline will be aborted
            }
            post {
                                    failure {
                                    mail to: 'keddourdina@gmail.com',
                                         from: 'kd_keddour@esi.dz',
                                         subject: 'Gate failed',
                                         body: 'Gate failed.'

                                    }
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
                    post {
                        failure {
                        mail to: 'keddourdina@gmail.com',
                             from: 'kd_keddour@esi.dz',
                             subject: 'Build failed',
                             body: 'Build failed.'

                        }
                    }
                 }

        stage('Deploy') {
                  steps {
                      echo 'Deploying Project..'
                      sh './gradlew publish'
                      }
                      post {
                                              failure {
                                              mail to: 'keddourdina@gmail.com',
                                                   from: 'kd_keddour@esi.dz',
                                                   subject: 'Deploy failed',
                                                   body: 'Deploy failed.'

                                              }
                                          }
                }
        stage('Notification') {
               steps {
                    echo 'Sending Notification..'
                    mail  to: 'keddourdina@gmail.com',
                    from: 'kd_keddour@esi.dz',
                    subject: 'Deploy success',
                    body: 'Project succefully deployed'
                    slackSend channel: '#all-ogl',message: 'Project succefully deployed'




                    }
               }
    }
}
