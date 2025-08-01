#!/usr/bin/env groovy
library identifier: 'jenkins-shared-library@main', retriever: modernSCM([
    $class: 'GitSCMSource',              // use Git source
    id: 'jenkins-shared-library',        // unique ID for tracking
    remote: 'https://github.com/endiesworld/jenkins-shared-library.git',
    credentialsId: 'github-PAT',         // your GitHub token in Jenkins
    // traits: [
    //     [$class: 'jenkins.plugins.git.traits.BranchDiscoveryTrait']  // This is the fix!
    // ]
])

def gv

pipeline {   
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }

        stage("build and push image") {
            steps {
                script {
                    buildImage 'okoro/demo-app:java-maven-3.0'
                    dockerLogin()
                    dockerPush 'okoro/demo-app:java-maven-3.0'
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
