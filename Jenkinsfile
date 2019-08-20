pipeline {
    agent { docker { image 'maven:3.3.3' } }
    options {
        skipStagesAfterUnstable()
    }
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
                sh 'echo $DB_ENGINE'
                echo "DISABLE_AUTH"

                timeout(time: 5, unit: 'SECONDS') {
                    retry(2) {
                        sh 'echo "hello"'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
            mail to: 'lianjun.zhao@tr.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }

}
