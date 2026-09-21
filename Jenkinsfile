pipeline {
    agent any

    stages {
        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('SonarCloud Scan') {
            steps {
                sh '''
                    mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                      -Dsonar.projectKey=Divyaprabha2007_java-maven-app \
                      -Dsonar.organization=divyaprabha2007 \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.token=203dfda01ddb4039db6dee1c260da2fc300eb0db
                '''
            }
        }

        stage('Package & Run') {
            steps {
                sh '''
                    mvn package
                    CLASS_NAME=$(find target/classes -name "*.class" ! -name "*Test*" | head -n 1 | sed 's|target/classes/||; s|\\.class$||; s|/|\\.|g')
                    java -cp target/classes "$CLASS_NAME"
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: false
        }
    }
}
