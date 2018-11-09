#!/usr/bin/groovy

podTemplate(label: 'Jenkins', containers: [
   containerTemplate(name: 'jnlp', image: 'lachlanevenson/jnlp-slave:3.10-1-alpine', args: '${computer.jnlpmac} ${computer.name}', workingDir: '/home/jenkins', resourceRequestCpu: '200m', resourceLimitCpu: '300m', resourceRequestMemory: '256Mi', resourceLimitMemory: '512Mi'),
        //containerTemplate(name: 'maven', image: 'jenkinsxio/builder-maven:0.1.22', command: 'cat', ttyEnabled: true),
    //containerTemplate(name: 'gradle', image: 'gradle-4.10.2-jdk7', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker:1.12.0', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'kubectl', image: 'pahud/eks-kubectl-docker', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:v2.8.2', command: 'cat', ttyEnabled: true)
],volumes:[
        hostPathVolume(mountPath: '/home/gradle/.gradle', hostPath: '/tmp/jenkins/.gradle'),
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
]) {
  node('Jenkins') {


    stage('Deploy api-orders') {
      container('helm') {


                checkout([$class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'CleanCheckout']],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: 'githubcredentials', url: 'https://github.com/KubeBitPoc/api-orders.git']]
                ])

                sh "helm list"

                          try{
                            sh "helm del --purge api-orders-helm"
                          }
                          catch(error) {
                              echo "No previous helm deployments found"
                          }


                          echo "Removing the current helm chart package"

                          try{
                                sh "rm -f helm/api-orders-helm-0.1.0.tgz"
                          }catch(error) {
                              echo "No previous helm package found"
                          }


                          echo "Creating the new helm package"
                          try {
                            sh "helm package helm/api-orders-helm"
                          } catch(error) {
                              echo "created the package"
                          }
                          sh "cp api-orders-helm-0.1.0.tgz helm/"
                          echo "Installing the new helm package"

                        sh "helm install --name api-orders-helm helm/api-orders-helm-0.1.0.tgz"

                        echo "Application anilbb/api-orders successfully deployed. Use helm status anilbb/api-orders to check"


       }
    }

}

}
