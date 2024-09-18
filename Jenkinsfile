pipeline {
    agent any
    environment {
        TOMCAT_HOST ="172.31.65.187"
        TOMCAT_USER ="ec2-user"
    }

    stages {
        stage('code checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/vinaylekkalapudii/ai-leads'
            }
        }
        
        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('tomcat deploy') {
            steps {
                  sshagent(['tomcat-dev']) {
                    sh " scp -o StrictHostKeyChecking=no target/ai-leads.war ${env.TOMCAT_USER}@${env.TOMCAT_HOST}:/opt/tomcat10/webapps"
                    sh " ssh ${env.TOMCAT_USER}@${env.TOMCAT_HOST} /opt/tomcat10/bin/shutdown.sh"
                    sh " ssh ${env.TOMCAT_USER}@${env.TOMCAT_HOST} /opt/tomcat10/bin/startup.sh"
                 }
            }
        }
    }
}
