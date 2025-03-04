def gv

pipeline {
    agent any
    parameters {
        choice (name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0' ], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description:'')
    }
      stages {
        stage("init") {
            steps {
                script{
                    gv = load "script.groovy"
                }
            }def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("build jar") {
            steps {
                script{
                    echo "building the application..."
                    sh 'mvn package'
                }
            }
        }
        stage("build image") {
            steps {
                script{
                    echo "building the building the docker image..."
                    withCredentials([usernamePassword(credentialsID: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t j30brooks/demo-app:jma2.0 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push j30brooks/demo-app:jma-2.0'
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                 script{
                    echo "deploying the application"
                 }
            }
        }
    }
}

        }
        stage("build") {
            steps {
                script{
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script{
                    gv.testApp()
                }
            }
        }
        stage("deploy") {
            steps {
                 script{
                    env.ENV = input message: "Select the environment to deploy to", ok: "Done", parameters: [choice (name: 'ONE', choices: ['dev', 'stageing', 'production'], description:'')]
                    gv.deployApp()
                    echo "Deploying to ${ENV}"
                 }
            }
        }
    }
}
