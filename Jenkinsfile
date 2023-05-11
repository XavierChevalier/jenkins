pipeline {
    agent {
        docker {
            image 'gplane/pnpm'
        }
    }

    environment {
        PROJECT_GIT_URL = credentials('PROJECT_GIT_URL')
    }

    stages {
        stage('Install dependencies') {
            steps {
                //git "$PROJECT_GIT_URL"
                
                cache(caches: [
                    arbitraryFileCache(path: '**/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml')
                ]) {
                    sh 'pnpm install --no-frozen-lockfile'
                }
            }
        }

        stage('Run tests') {
            steps {
                sh 'pnpm test:coverage'
            }

            post {
                always {
                    step([$class: 'CoberturaPublisher', coberturaReportFile: 'packages/identifier/coverage/cobertura-coverage.xml'])
                }
            }
        }

        stage('Build') {
            steps {
                cache(caches: [
                    arbitraryFileCache(path: 'node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml')
                ]) {
                    sh 'pnpm build'
                }
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
