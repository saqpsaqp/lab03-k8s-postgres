#!/usr/bin/env groovy

pipeline {

        agent {label "kubectl"}

        environment {
        docker      = "docker -H tcp://127.0.0.1:2375"
        kube        = "/usr/bin/kubectl"
        image       = "saqpsaqp/pythontest"
        commfile    = "/tmp/commit-file"
        }

        stages {
            
            stage ("Compilanding imagen de docker...")
            {
                steps {
                    script {
                    sh "git rev-parse HEAD > ${env.commitfile}"
                    def COMM = ((String)(readFile("${env.commitfile}"))).toString().trim()
                    sh "${env.docker} build -t ${env.image}:${COMM} ."
                    }
                }   
            }
            stage ("Subiending imagen...")
            {
                steps {
                    script {
                    def COMM = ((String)(readFile("${env.commitfile}"))).toString().trim()
                    sh "${env.docker} push ${env.image}:${COMM}"
                    }
                }
            }
            stage ("Reemplazanding imagen de docker...")
            {
                steps {
                    script {
                        def COMM = ((String)(readFile("${env.commitfile}"))).toString().trim()
                        sh "DOCKERIMAGE=${env.image}:${COMM} envsubst < pythontest.tpl.yaml > /tmp/pythontest.yaml"
                    }
                }
            }
            stage ("Despleganding contenedor...")
            {
                steps {
                    sh "${kube} apply -f /tmp/pythontest.yaml"
                }
            }
        }
}
