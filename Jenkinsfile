pipeline {
    agent any

    environment {
        PROJECT_NAME = 'spring-petclinic'
        REPO_URL = 'https://github.com/spring-projects/spring-petclinic'
    }    

    stages {

        stage('git clone') {
            steps {
                sh 'rm -rf $PROJECT_NAME'
                sh 'git clone --depth 1 --branch main $REPO_URL'
            }
        }

        stage('Build') {
            steps {
                echo 'Building..'
                sh '''
                    cd $PROJECT_NAME && pwd && ls -al
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
                sh '''
                    cd $PROJECT_NAME
                    mvn test
                    ls -al target/surefire-reports
                '''
                junit skipMarkingBuildUnstable: true, testResults: '**/surefire-reports/*.xml'                

            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'

                sh '''
                    cd $PROJECT_NAME
                    scp target/*.jar spring-petclinic@192.168.1.28:/opt/spring-petclinic/spring-petclinic.jar
                '''

            }
        }
    }
}