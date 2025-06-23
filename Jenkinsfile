pipeline {
    agent any

    environment {
        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_BASELINE_REF = "origin/master"
    }


    tools {
        maven "Maven-3"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/scott-sg-org/test3.git'

                sh "mvn -DskipTests=true clean package"
            }
        }
        stage('Semgrep code analysis') {
            steps {
                sh '''docker run \
                -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
                -e SEMGREP_BASELINE_REF=$SEMGREP_BASELINE_REF \
                -v "$(pwd):$(pwd)" --workdir $(pwd) \
                semgrep/semgrep semgrep ci'''
            }
        }
    }
}
