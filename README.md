# Jenkins_home
Jenkins_home
Lien vers la doc officiel Azure :  https://learn.microsoft.com/en-us/azure/devops/?view=azure-devops 

node {
    checkout scm

    def imageName='registry.gitlab.com/xavki/presentations-jenkins'

    docker.withRegistry('https://registry.gitlab.com', 'credentials-id') {

    def customImage = docker.build("$imageName:${env.BUILD_ID}")

    customImage.push()
    }
}


node {

   def registryProjet='registry.gitlab.com/xavki/presentations-jenkins'
   def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

    stage('Clone') {
          checkout scm
    }

    def img = stage('Build') {
          docker.build("$IMAGE",  '.')
    }

    stage('Run') {
          img.withRun("--name run-$BUILD_ID -p 80:80") { c ->
            sh 'curl localhost'
          }
    }

    stage('Push') {
          docker.withRegistry('https://registry.gitlab.com', 'reg1') {
              img.push 'latest'
              img.push()
          }
    }

}

