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
                     bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.projectName='DeployBack' -Dsonar.host.url=http://localhost:9000 -Dsonar.token=sqp_5279dd06ae451c6c43be3313fc9eaf70f77b3adb -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                }  
           }
        }
        stage ('Deploy Backend') {
           steps {
               deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
           }
        }
        stage ('API Test') {
           steps {
                dir('api-test'){
                    git credentialsId: 'GitHub', url: 'https://github.com/AlexandreFreitasCampos/tasks-api-test.git'
                    bat 'mvn test'
                }
           }
        }
        stage ('Deploy Frontend') {
           steps {
                dir('frontend'){
                    git credentialsId: 'GitHub', url: 'https://github.com/AlexandreFreitasCampos/tasks-frontend.git'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
           }
        }
        stage ('Functional Test') {
           steps {
                dir('functional-test'){
                    git credentialsId: 'GitHub', url: 'https://github.com/AlexandreFreitasCampos/tasks-functional-tests.git'
                    bat 'mvn test'
                }
           }
        }
    }
}


