pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/virtualmail-rgb/js-e2e-express-server.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "JFROG-01-02",
                    url: "https://vardevops123.jfrog.io/",
                    credentialsId: 'JFROG_CRED'
                )
                rtNpmDeployer (
                    id: "NPM_DEPLOYER",
                    serverId: "JFROG-01-02",
                    repo: "nodedevops-npm-local"
                )
            }
        }

        stage ('Exec npm install') {
            steps {
                rtNpmInstall (
                    path: "package.json"
                )
            }
        }

        stage ('Exec npm publish') {
            steps {
                rtNpmPublish (
                    path: "package.json",
                    deployerId: "NPM_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG-01-02"
                )
            }
        }
    }
}
