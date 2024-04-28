pipeline {
    agent any
    stages {
        stage ('Build Backend') {
           steps {
               bat 'mvn clean package -DskipTests=true'
           }
        }
        stage ('Unit Tests') {
           steps {
               bat 'mvn test'
           }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
           steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                     bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.projectName='DeployBack' -Dsonar.host.url=http://localhost:9000 -Dsonar.token=sqp_ef8d4d1a306b2bdc36302b5ced4c7baa1d9546a -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                }  
           }
        }
    }
}


