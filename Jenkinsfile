pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
        STAGING_SERVER = 'dockeruser@maven-ssh'
        ARTIFACT_NAME = 'demo-0.0.1-SNAPSHOT.jar'
        REMOTE_PATH = '/home/dockeruser/projects'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/nahuelXZV/spring-docker'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Code Quality') {
            steps {
                sh 'mvn checkstyle:check || true'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
        }
        stage('Deploy to Staging') {
            steps {
                sh 'scp target/${ARTIFACT_NAME} $STAGING_SERVER:${REMOTE_PATH}/'
                sh 'ssh $STAGING_SERVER "nohup java -jar ${REMOTE_PATH}/${ARTIFACT_NAME} > ${REMOTE_PATH}/app.log 2>&1 &"'
            }
        }
        stage('Validate Deployment') {
            steps {
                sh 'sleep 10'
                sh 'curl --fail http://maven-ssh:8080/health'
            }
        }
    }
}
