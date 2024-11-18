#!/usr/bin/env groovy
pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage ('increment version') {
            steps {
                script {
                    echo 'incrementing version....'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher =readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version =matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
         stage('build app') {
            steps {
                script {
                    echo "building the application..."
                    sh 'mvn clean package'

                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t rajibmardi/my-repo:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push rajibmardi/my-repo:${IMAGE_NAME}"
                    }
                }
            }
        }


        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
                stage('commit version update') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'gitlab-token', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                sh 'git config --global user.email "jenkins@example.com"'
                                sh 'git config --global user.name "jenkins"'

                                sh 'git status'
                                sh 'git branch'
                                sh 'git config --list'
                                sh "git remote set-url origin https://${USER}:${PASS}@gitlab.com/Rajib-Mardi/java-maven-app.git"
                                sh 'git add .'
                                sh 'git commit -m "ci:jenkins-jobs"'
                                sh 'git push origin HEAD:jenkins-jobs'
                            }
                        }
                    }
                }
    }
}




