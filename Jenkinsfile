node('ubuntu-Appserver')
{
    def app
    stage('Cloning Git')
    {
        checkout scm
    }
    
    stage('SCA-SAST-SNYK-TEST')
    {
    agent
        {
            label 'ubuntu-Appserver'
        }
        
    snykSecurity(
        snykInstallation: 'Snyk',
        snykTokenId: 'Snykid',
        severity: 'critical'
        )
    }
    
    stage('Build-and-Tag')
    {
        app = docker.build("kevenmang/snake")
    }

    stage('Post-to-Dockerhub')
    {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
        {
            app.push("latest")
        }
    }

    stage('Pull-image-server')
    {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }
}