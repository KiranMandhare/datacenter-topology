pipeline {
    agent any
    options {
        // clean before build
        skipDefaultCheckout(true)
    }
    environment {
        ENVIRONMENT = 'TEST'
        RELEASE     = '15.0'
    }
    stages{
        stage('Deploy Kubernetes Network Emulation'){
            steps{
                sh 'kind get clusters'
                sh 'kubectl get deployments -n srlinux-controller'
            }
        }
        stage('Git Pull - Topology1') {
            steps{
            cleanWs()
            script {
                   VERSION_NUMBER = "${RELEASE}." +currentBuild.number
                   currentBuild.displayName = "${VERSION_NUMBER}"
            }
            checkout([$class: 'GitSCM',
                    branches: [[name: '*/test']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'CleanCheckout']],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: '830e2b5c-4676-42b7-8aff-3551e02073e1', url: 'https://github.com/KiranMandhare/datacenter-topology.git']]
            ])
            }
        }
        stage('Deploy Toplogy1') {
            steps{
                    sh  'kne create datacenter-toplogy'
            }
        }
    }
    post {
        success {
            mail(body: 'Jenkins build : SUCCESS', subject: 'Jenkins Pipeline Status', to: 'kima4508@colorado.edu', from: 'jenkins@wtdc.com')  
            echo "Mail sent to kima4508@colorado.edu regarding pipeline sucess"    
        }
        failure {
            mail(body: 'Jenkins build : FAILURE', subject: 'Jenkins Pipeline Status', to: 'kima4508@colorado.edu',from: 'jenkins@wtdc.com') 
            echo "Mail sent to kima4508@colorado.edu regarding pipeline failure"       
        }
    }

}


