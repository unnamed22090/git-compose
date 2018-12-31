node("${deploySite}") {
    
    def gitUrl = ''
    def branch = ''
    def buildEnv = ''
    def tag = ''

    stage('Preparing') {
        gitUrl = env.gitUrl
        branch = env.gitBranch
        buildEnv = env.buildEnv
    }

    stage('Checkout Code') {
        dir('src') {
            ansiColor('xterm') {
                git "${gitUrl}"
            }
        }
    }
    
    stage('Build Docker Image and Start Server') {
        dir('src') {
            ansiColor('xterm') {
                sh "docker-compose build"
            }
        }
    }
    
    stage('Publish Unit Test Report') {
        // dir('test-result') {
        //     sh 'docker cp $(docker-compose ps -q timesheetapi):/test-result/report .'
        // }
        // publishReports()
    }

    stage('Clean Docker Images') {
        ansiColor('xterm') {
            sh 'docker rmi $(docker images -f \"dangling=true\" -q) || echo \"ignored\"'
        }
    }

}

def publishReports() {
    publishHTML([
        allowMissing: false, 
        alwaysLinkToLastBuild: false, 
        keepAll: false, 
        reportDir: "${env.WORKSPACE}/test-result/report", 
        reportFiles: 'index.htm', 
        reportName: 'Unit Test Result', reportTitles: ''])
}