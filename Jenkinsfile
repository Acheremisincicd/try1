pipeline {
    agent{ label 'packer_node' }
            stages {
                stage('git clone') {
                    steps {
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: '*/develop']], 
                            doGenerateSubmoduleConfigurations: false, 
                            userRemoteConfigs:[[
                                    credentialsId: "github",
                                    url: "https://github.com/Acheremisincicd/try1.git"]]
                                ])
                            }
                        }
                    
                stage('Building Packer AMI') {
                    steps {
                        withEnv(["ENV=DEV", "STAGE=STABLE"]){
                            withCredentials([[
                                $class: 'AmazonWebServicesCredentialsBinding', 
                                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                                credentialsId: 'packer-executor', 
                                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                                    // $slave_ami is folder in git which should contain packer template 
                                    sh "packer build jenkins-slave-factory/${slave_ami}/template.json"
                    }
                }
            }
        }
    }
}
