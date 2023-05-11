pipeline {
    agent {
        docker {
            image 'gplane/pnpm'
        }
    }

    stages {
        stage('Install dependencies') {
            steps {
                git 'https://github.com/XavierChevalier/labeilleviennoise-pipeline.git'

                cache(maxCacheSize: 500, caches: [
                    arbitraryFileCache(path: '**/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml')
                ]) {
                    sh 'pnpm install --no-frozen-lockfile'
                }
            }
        }

        stage('Run tests') {
            steps {
                sh 'pnpm test:coverage'

                post {
                    always {
                        step([$class: 'CoberturaPublisher', coberturaReportFile: 'output/coverage/jest/cobertura-coverage.xml'])
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'pnpm build'
            }
        }

        stage('Deliver') {
            steps {
                input message: 'Do you want to deliver this new version? (Click "Proceed" to continue)'
                echo 'Deploy...'
                echo 'Successfully deployed'
            }
        }
    }
}
