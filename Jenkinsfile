pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'python -m py_compile add2vals.py calc.py'
                stash(name: 'compiled-results', includes: '*.py*')
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh "pyinstaller --onefile add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
