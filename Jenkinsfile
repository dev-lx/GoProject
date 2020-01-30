pipeline{
    //agent { label 'slav01' }
     agent any
     parameters {
         gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH'
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
         stage('clone'){
             steps{
                checkout ([$class: 'GitSCM', branches: [[name: '${params.BRANCH}']], userRemoteConfigs: [[url: 'https://github.com/dev-lx/GoProject.git']]])
}
}
         stage ('build'){
             steps{
                script{
                   sh 'go version'
                   sh 'go build && ./main'
                  
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



