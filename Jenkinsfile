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
                     bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.projectName='DeployBack' -Dsonar.host.url=http://localhost:9000 -Dsonar.token=squ_f73d00e64d3b2ea8bd4d30e442a771ab728fb5f5 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                }  
           }
        }
        stage ('Deply Backend') {
           steps {
               deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
           }
        }
    }
}


