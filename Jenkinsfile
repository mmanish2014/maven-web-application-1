pipeline{
    agent any
    tools{
      maven "maven-3.8.5"
    }

    options{
        timestamps()
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
    }

   // triggers{
        // cron('*  *  *  *  *')
       // pollSCM('*  *  *  *  *')
       // githubPush()
        
   // }

    stages{

        stage('CodeCheckout'){
            steps{
                git 'https://github.com/mmanish2014/maven-web-application-1.git'
            }
        }

        stage('SourceCodeBuild'){
            steps{
                sh "mvn clean package"
            }
        }

        stage('SonarQubeAnalysisReport'){
            steps{
                sh "mvn clean sonar:sonar"
            }
        }

        stage('SendBuildToNexusArtifactory'){
            steps{
                sh "mvn clean deploy"
            }
        }

        stage('DeployingArtifactstoTomcatServer'){
            steps{
                sshagent(['Tomcat-ssh-credential']) {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.82.244.131:/opt/tomcat/webapps/"  
               }
            }
        }

    } // stages closed

    post{
        always{
        emailext body: '''Build is over!
        Manish Madhukar
        DevOps Engineer''', subject: 'Build is Over!', to: 'sweetmaniapps@gmail.com'
        }

        failure{
        emailext body: '''Build is failure!
        Manish Madhukar
        DevOps Engineer''', subject: 'Build is over!', to: 'sweetmaniapps@gmail.com'
        }

        success{
        emailext body: '''Build id success!
        Manish Madhukar
        DevOps Engineer''', subject: 'Build is over!', to: 'sweetmaniapps@gmail.com'
        }
    }
}
