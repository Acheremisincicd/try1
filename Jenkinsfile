pipeline {
    agent{ label 'packer-executor' }
        parameters {
        string(name: 'slave_ami', defaultValue: '', description: 'slave_ami is the folder in git which should contains packer template')
        string(name: 'ENV', defaultValue: '', description: '')
        string(name: 'STAGE', defaultValue: '', description: '')
        string(name: 'PIPELINE_GIT', defaultValue: '', description: 'Branch')
        }
            stages {
                stage('Checkout') {
                    steps {
                        git(
                        url: 'https://github.com/Acheremisincicd/try1.git',
                        branch: "${PIPELINE_GIT}",
                        credentialsId: 'github'
                        )
                        }
                }
                stage('Building Packer AMI') {
                    steps {
                        withEnv(["ENV=${ENV}","STAGE=${STAGE}"])
                         {
                            withCredentials([[
                                $class: 'AmazonWebServicesCredentialsBinding', 
                                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                                credentialsId: 'packer-executor', 
                                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
                                {
                                    sh "packer build jenkins-slave-factory/${slave_ami}/template.json"
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
} 
