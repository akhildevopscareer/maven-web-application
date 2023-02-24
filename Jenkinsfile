pipeline{
    agent any
    tools {maven 'maven-3.8.6'}
    triggers{
        pollSCM('* * * * *')
    }
    options{
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
        timestamps()
    }
    stages{
        stage('CheckoutCode'){
            steps{
                git branch: 'development', credentialsId: '286eff0c-0154-45d6-8dc7-7d7a4e021e40', url: 'https://github.com/akhildevopscareer/maven-web-application.git'
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean package"
            }
        }
        stage('SonarReport'){
            steps{
                sh "mvn clean sonar:sonar"
            }
        }
        stage('UploadArtifactToNexus'){
            steps{
                sh "mvn clean deploy"
            }
        }
        stage('DeployToTomcat'){
            steps{
                sshagent(['6e2f8d48-52dc-4d2c-a9cc-9d65a8a1725b']) {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.69.87.167://opt/apache-tomcat-9.0.71/webapps/"
            }
        }
        }
    }
    post{
        success{
            emailext body: '''build triggered and is success

            Thanks,
            Akhil Reddy''', subject: 'build triggered', to: 'akhilreddymatta@gmail.com, akhilreddy135@gmail.com'
        }
        failure{
            emailext body: '''build triggered and is failure

            Thanks,
            Akhil Reddy''', subject: 'build triggered', to: 'akhilreddymatta@gmail.com, akhilreddy135@gmail.com'
            
        }
        
    }
}
