pipeline{
    //agent { label 'slav01' }
     agent any
     parameters {
        choice(choices: [ 'master', 'dev', 'stage' ], description: 'Select deployment branch', name: 'BRANCH')
        // gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH'
}
     environment{
        dockerImage = ''
        registryurl = 'alwaysavail/gitops'
        dockercred = 'hub' 

}
     stages{
         stage("Buildstarted"){
            steps{
                slackSend channel: '#jenkins_build',
                    color: 'good',
                    message: "* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL} is started"
            }
        }
         stage('cleanWorkspace'){
            steps{
                step([$class: 'WsCleanup'])
}
}
         stage('EchoChoice'){
             steps{
                 echo "${params.BRANCH}"
             }
}
         stage('clone'){
             steps{
                checkout ([$class: 'GitSCM', branches: [[name: "*/${params.BRANCH}" ]], userRemoteConfigs: [[url: 'https://github.com/dev-lx/GoProject.git']]])
}
}
         stage('prebuild'){
             steps{
                script{
                    withEnv(["PATH=PATH=$PATH:/usr/local/go/bin", "GOPATH=${JENKINS_HOME}/workspace/${JOB_NAME}"]) {
                        sh 'go version'
                        sh 'pwd'
                        sh 'ls'
                        sh 'go build'
}
}
}
}
         stage ('build'){
             steps{
                script{
                   echo "Prebuild is done"
                  
                }
             }
         }
        stage("imagepush"){
            steps{
                script{
                     echo "Generating artifacts"
                    }
                }
            }
        stage("deploy"){
            steps{
                script{
                     echo "Deploying"
                    }
                }
            }
        stage("buildStatus"){
            steps{
                slackSend channel: '#jenkins_build',
                    color: 'good',
                    message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }
        }
}
}        



