pipeline {   
    agent any
    
    tools {
        maven 'maven-3.9'
        }
    
    stages {
        stage("conditional") {
            when {
                expression {
                    env.BRANCH_NAME == 'feature-1'
                }
            }

            stage("debug") {
                steps {
                    echo "BRANCH_NAME is: [${env.BRANCH_NAME}]"
                }
            }

            stage("build jar") {
                steps {
                    script {
                        gv.buildJar()

                    }
                }
            }

            stage("build image") {
                steps {
                    script {
                        gv.buildImage()
                    }
                }
            }

            stage("deploy") {
                steps {
                    script {
                        gv.deployApp()
                    }
                }
            }
        }               
    }
} 
