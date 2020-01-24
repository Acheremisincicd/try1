node (label: 'aws') {
 withEnv(["ENV=DEV", "STAGE=STABLE"]) {
   stage('Git clone') {
      checkout(
         [
            $class: 'GitSCM', 
               branches: [[name: '*/master']], 
               doGenerateSubmoduleConfigurations: false, 
               userRemoteConfigs: [
                  [
                     credentialsId: "git", 
                    url: "git@git.epam.com:Artem_Cheremisin/packer_build_test.git"
                  ]
               ]
         ]
      )
   }
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'packer_executor', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) { 
    sh "packer build jenkins-slave-factory/${source_ami}/template.json"
    sh "packer build jenkins-slave-factory/${slave_ami}/template.json"
        }
    }
}