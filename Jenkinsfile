pipeline {
    agent {
        docker {
            image 'gplane/pnpm'
        }
    }

    stages {
        stage('Install dependencies') {
            steps {
                cache(caches: [
                    arbitraryFileCache(path: 'node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'apps/auth/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'apps/blog/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'apps/shop/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'apps/website/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/auth-client/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/auth-server/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/catch-boundary/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/environment-client/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/environment-server/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/forms-client/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/forms-server/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/icons/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/identifier/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/layouts/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/logo/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/layouts/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/merge-classes/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/monitoring/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/navigation/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/router/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/seo/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/state-hooks/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml'),
                    arbitraryFileCache(path: 'packages/ui/node_modules', cacheValidityDecidingFile: 'pnpm-lock.yaml')
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
                cache(maxCacheSize: 500, caches: [
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
