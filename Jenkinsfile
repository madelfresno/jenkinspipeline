pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '18.222.90.69', description: 'Staging Server')         
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/mafresno/Descargas/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }         
                }
            }
        }
    }
}