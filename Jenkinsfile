pipeline {
    agent{ label 'packer_node' }
        parameters {
        string(name: 'slave_ami', defaultValue: ' ', description: 'slave_ami is the folder in git which should contains packer template')
        }
            stages {
                stage('git clone') {
                    steps {
                        git url: 'https://github.com/Acheremisincicd/try1.git', branch: 'master',
                        credentialsId: 'github'
                        }
                stage('Building Packer AMI') {
                    steps {
                        withEnv(["ENV=DEV","STAGE=STABLE"])
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
