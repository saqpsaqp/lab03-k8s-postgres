#!/usr/bin/env groovy

pipeline {

        agent {label "kubectl"}

        environment {
        docker      = "docker -H tcp://127.0.0.1:2375"
        kube        = "/usr/bin/kubectl"
        }

        stages {
            
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
