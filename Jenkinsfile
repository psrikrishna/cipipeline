pipeline {
  agent any
   stages {
       stage("aws jenkins packer") {
         steps {
                  withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                                 ]]) {
                    sh "aws s3 ls"
                }
               }
       }
         stage ('Packer init') {
                  steps {
                   script {
                    echo "initializing packer"
                    sh "/usr/bin/packer version"
                    sh  "/usr/bin/packer init ami.pkr.hcl"
                      }  
                }
        }
          stage ('Packer validate') {
                   steps {
                    echo 'validating packer code'
                    sh '/usr/bin/packer validate ami.pkr.hcl'
                }
             }
          stage ('packer build ami') {
                    steps {
                        withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                                 ]]) {
                    echo 'building ami'
                    sh '/usr/bin/packer build ami.pkr.hcl'
              }
        }    
  }
   }
}
