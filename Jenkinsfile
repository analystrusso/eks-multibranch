def gv

pipeline {   
    agent any
    tools {
        maven 'maven-3.9'
    }  
    stages {
        stage("debug") {
            steps {
                echo "BRANCH_NAME is: [${env.BRANCH_NAME}]"
            }
        }
        
        stage("build jar") {
            steps {
                script {
                    echo 'building the application...'
                    sh 'mvn package'
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker build -t analystrusso/twn-bootcamp-repo:jma-2.0 .'
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push analystrusso/twn-bootcamp-repo:jma-2.0'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo 'deploying the application...'
                }
            }
        }               
    }
}
} 
