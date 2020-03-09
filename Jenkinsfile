pipeline {
    agent { docker { image 'python:3.8.1' } }
    stages {
        stage('build') {
            steps {
                withEnv(["HOME=$env.WORKSPACE"]){
                    sh 'pip install --user -r requirements.txt'
                }
            }
        }
        stage('test') {
            steps {
                withEnv(["HOME=$env.WORKSPACE"]){
                    sh 'python -m pylint -f parseable *.py > pylint.out'
                    sh 'python -m pytest --cov=./ --verbose --junit-xml test-results/results.xml ./tests/'
                    sh 'python -m coverage xml'
                }
            }
        }
    }
    post {
            always {
                junit 'test-results/results.xml'
                step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
                sh 'rm .local -rf'
            }
    }
}
