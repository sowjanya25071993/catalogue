pipeline{
    agent{
        node{
            label 'Agent-1'
        }
    }
    environment{
        PackageVersion=''
    }
    options{
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    stages{
        stage('get the version'){
            steps{
                script{
                    def packagejson = readJSON file: 'package.json'
                    PackageVersion = packagejson.version
                    echo "application version:$PackageVersion"
                }
            }
        }
        stage('install dependancies'){
            steps{
                sh """
                 npm install
                """
            }
        }
    }
    post{
        always{
            echo "i will always say hello again"
        }
        success{
            echo "i will always say hello when pipeline is success"
        }
        failure{
            echo "this is when pipeline failed"
        }
    }
}