pipeline
{
    agent none

    stages
    {
        stage('CLONE GIT REPOSITORY')
        {
            agent
            {
                label 'ubuntu-Appserver'
            }
            steps
            {
                checkout scm
            }
        }

        stage('SCA-SAST-SNYK-TEST')
        {
            agent
            {
                label 'ubuntu-Appserver'
            }
            steps
            {
                echo "SNYK-TEST"
            }
        }

        stage('BUILD-AND-TAG')
        {
            agent
            {
                label 'ubuntu-Appserver'
            }
            steps
            {
                script 
                {
                    def app = docker.build("kevenmang/snake")
                    app.tag("latest")
                }
            }
        }

        stage('POST-TO-DOCKERHUB')
        {
            agent
            {
                label 'ubuntu-Appserver'
            }
            steps
            {
                script 
                {
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub_credentials")
                    {
                        def app = docker.image("kevenmang/snake")
                        app.push("latest")
                    }
                }
            }
        }

        stage('DEPLOYMENT')
        {
            agent
            {
                label 'ubuntu-Appserver'
            }
            steps
            {
                sh "docker-compose down"
                sh "docker-compose up -d" 
            }
        }
    }
}