#!/usr/bin/env groovy

pipeline {

        agent {label "kubectl"}

        environment {
        docker      = "docker -H tcp://127.0.0.1:2375"
        kube        = "/usr/bin/kubectl"
        commfile    = "/tmp/commit-file"
        }

        stages {
            
            stage ("Get Commit Number...")
            {
                steps {
                    script {
                        sh "git rev-parse HEAD > ${env.commitfile}"
                    }
                }   
            }
            stage ("Get objects...")
            {
                steps {
                    script {
                        def COMM = ((String)(readFile("${env.commitfile}"))).toString().trim()
                        sh "mkdir -p /tmp/${env.commitfile}"
                    }
                }
            }
            stage ("Desplegando ...")
            {
                steps {
                    script {
                        sh "${kube} apply -f postgres-configmap.yaml"
                        sh "${kube} apply -f postgres-storage.yaml"
                        sh "${kube} apply -f postgres-deployment.yaml"
                    }
                }
            }
        }
}
