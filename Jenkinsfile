pipeline {
    agent any

    stages {
        stage('Docker Hub Login') {
            steps {
                script {
                    def count = bat(script: """
                        curl -s -u admin:admin -X GET https://7tiuxysa.c1.gra9.container-registry.ovh.net/harbor/projects/2/repositories | jq ".items[].name" | find /c /v ""
                    """, returnStatus: true).trim()

                    if (count.toInteger() > 0) {
                        withCredentials([usernamePassword(credentialsId: 'harbor-token', usernameVariable: 'HARBORTOKEN_USERNAME', passwordVariable: 'HARBORTOKEN_PASSWORD')]) {
                            bat("docker login -u %HARBORTOKEN_USERNAME% -p %HARBORTOKEN_PASSWORD%")
                        }
                    } else {
                        error('Docker Hub repository not found or login failed.')
                    }
                }
            }
        }

        stage('Check Repository') {
            steps {
                script {
                    def count = bat(script: """
                        curl -s -u admin:admin -X GET https://7tiuxysa.c1.gra9.container-registry.ovh.net/harbor/projects/2/repositories | jq ".items[].name" | find /c /v ""
                    """, returnStatus: true).trim()

                    if (count.toInteger() == 0) {
                        echo "No images in the repository"
                    }
                }
            }
        }

        
    }
}
