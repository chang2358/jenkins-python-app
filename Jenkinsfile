pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/python:2-alpine'
                }
            }
            dir(path: '/home/shiyanlou/Code') {
                git(credentialsId: '7fe344af-6134-4b88-899c-fd2adad914f9', url: 'git@github.com:chang2358/jenkins-python-app.git', branch: 'main')
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/qnib-pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            agent {
                docker {
                    image 'registry.cn-hangzhou.aliyuncs.com/louplus-linux/cdrx-pyinstaller-linux:python2' 
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py' 
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}