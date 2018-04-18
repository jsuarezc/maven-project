pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '52.47.136.216', description: 'Staging Server') //ip public aws - EC2
         //string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server') //ip public aws - EC2
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        //sh "StrictHostKeyChecking=no scp -i /home/job/Escritorio/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                        sh "ssh ec2-user@52.47.136.216 -i /home/job/Escritorio/tomcat-demo.pem"
                    }
                }
            }
        }
    }
    
}
