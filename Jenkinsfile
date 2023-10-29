pipeline {
    agent any
    tools {
        maven 'mvn'
    }
    stages {
        stage('Declarative CI Pipeline') {
            steps {
                echo 'This is Declarative Pipeline'
            }
        }
        stage('Code Checkout') {
            steps {
                echo 'Started Cloning'
                git 'https://github.com/samirkesare/DevOpsCodeDemo.git'
            }
        }
        stage('Parallel Phase') {
            parallel {
                stage('Code Stability') {
                    steps {
                        echo "Testing code stability"
                        sh 'mvn test'
                    }
                }
                stage('Code Quality Analysis') {
                    steps {
                        echo "Check code quality using PMD"
                        sh 'mvn pmd:pmd'
                        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/pmd-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    }
                }
                stage('Code Coverage Analysis') {
                    steps {
                        echo "Check code coverage analysis using Jacoco"
                        sh 'mvn clean verify'
                        jacoco()
                    }
                }
            }
        }
        stage('Build Artifacts') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Publish Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
        stage('Deploy Artifacts') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://13.114.143.171:8080/')], contextPath: '/var/lib/jenkins/workspace/assignment04/target/addressbook', war: '**/*.war'
            }
        }
    }
    }
