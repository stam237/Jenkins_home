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
