pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {
        stage("build and deploy") {
            when {
                expression {
                    env.BRANCH_NAME == 'deploy-on-k8s'
                }
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
                            withCredentials([
                                usernamePassword(
                                    credentialsId: 'docker-hub-repo',
                                    passwordVariable: 'PASS',
                                    usernameVariable: 'USER'
                                )
                            ]) {
                                sh 'docker build -t analystrusso/twn-bootcamp-repo:jma-2.0 .'
                                sh 'echo $PASS | docker login -u $USER --password-stdin'
                                sh 'docker push analystrusso/twn-bootcamp-repo:jma-2.0'
                            }
                        }
                    }
                }
                stage("deploy") {
                    environment {
                        AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                        AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
                    }
                    steps {
                        script {
                            echo "deploying app..."
                            sh 'kubectl create deployment nginx-deployment --image=nginx'
                        }
                    }
                }

            }
        }
    }
}