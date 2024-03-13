@Library('Jenkins_shared_library') _
  

    pipeline{
    agent any
        parameters {
        choice(name: 'action', choices: 'create\ndelete', description: 'Select create or destroy.')
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    }
    stages{
        stage('clean workspace'){
            steps{
                cleanWorkspace()
            }
        }
        stage('checkout from Git'){
            steps{
                checkoutGit(
                    branch: "main"
                    url: "https://github.com/udayveer-singh/youtube-clone.git"
                )
            }
        }
     }
     post {
         always {
             echo 'Slack Notifications'
             slackSend (
                 channel: '#channel name',   #change your channel name
                 color: COLOR_MAP[currentBuild.currentResult],
                 message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} \n build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
               )
           }
         
       }
        stage('sonarqube Analysis'){
        when { expression { params.action == 'create'}}    
            steps{
                sonarqubeAnalysis()
            }
        }
        stage('sonarqube QualitGate'){
        when { expression { params.action == 'create'}}    
            steps{
                script{
                    def credentialsId = 'Sonar-token'
                    qualityGate(credentialsId)
                }
            }
        }
        stage('Npm'){
        when { expression { params.action == 'create'}}    
            steps{
                npmInstall()
            }
        }

   }
