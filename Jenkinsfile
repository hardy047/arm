#!/usr/bin/env groovy

def label = "k8sagent-e2e"
def home = "/home/jenkins"
def workspace = "${home}/workspace/build-jenkins-operator"
def workdir = "${workspace}/src/github.com/jenkinsci/kubernetes-operator/"

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: alpine
    image: alpine:3.10.2
    command: ['cat']
    tty: true
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) {
    node(label) {
        stage('Checkout Workspace') {
            container('alpine') {
                sh 'echo "Checkout Workspace"'
            }
        }
        stage('Sanity Checks') {
             parallel (
               copyright: {
                     container('alpine') {
                         sh 'echo "copyright checks"'
                     }            
              },
               swagger_validation: {
                     container('alpine') {
                         sh 'echo "swagger checks"'
                     }            
              },
                go_vendor: {
                     container('alpine') {
                         sh 'echo "go vendor"'
                     }            
              })
        }
        stage('Tests') {
             parallel (
               lint: {
                     container('alpine') {
                         sh 'echo "lint test"'
                     }            
              },
               unit_test: {
                     container('alpine') {
                         sh 'echo "unit tests"'
                     }            
              })
        }
        stage('Build') {
             parallel (
               build: {
                     container('alpine') {
                         sh 'echo "build"'
                     }            
              },
               code_quality: {
                     container('alpine') {
                         sh 'echo "code quality"'
                     }            
              })
        }
        stage('Functional Tests') {
            container('alpine') {
                sh 'echo "Functional Tests"'
            }
        }
        if (env.BRANCH_NAME == "master") {
        stage('Deploy') {
            container('alpine') {
                sh 'echo "Deploy"'
            }
          }
        }
    }
}
