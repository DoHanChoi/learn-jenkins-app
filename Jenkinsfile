pipeline {

    // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent none

    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2'
    }

    // environment {
    //     NETLIFY_SITE_ID = '954982c2-7dee-4008-9849-3d3d3e4ec16f'
    //     NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    // }

    stages {

        stage('Deploy to AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    // aws-cli 이미지는 기본적으로 실행 후 바로 중단되므로 엔트리포인트 무력화
                    args "-u root --entrypoint=''"
                }
            }

            // environment {
            //     AWS_S3_BUCKET = 'learn-jenkins-202611061134'
            // }

            steps {

                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        yum install jq -y
                        LATEST_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                        echo $LATEST_REVISION
                        aws ecs update-service --cluster knowledgeable-crocodile-yobhcy --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:2
                    '''
                }
            }
        }
        

        stage('Build') {
            agent {
                docker { image 'mcr.microsoft.com/playwright:v1.39.0-jammy' 
                reuseNode true
                }
            }
            steps {
                sh '''
                    echo '빌드 시작 ...'
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        

        // stage('Test') {
        //     agent {
        //         docker { image 'mcr.microsoft.com/playwright:v1.39.0-jammy' }
        //     }
        //     steps {
        //         sh 'npm test'
        //     }
        // }

        // stage('E2E') {
        //     agent {
        //         docker { image 'mcr.microsoft.com/playwright:v1.39.0-jammy' }
        //     }
        //     steps {
        //         sh '''
        //             # serve를 로컬에 설치하여 실행
        //             npm install serve
        //             node_modules/.bin/serve -s build & sleep 10
        //             npx playwright test --reporter=html
        //         '''
        //     }
        // }

        // stage('Deploy staging') {
        //     agent {
        //         docker { image 'node:18-bullseye' } 
        //     }
        //     steps {
        //         sh '''
        //             npm install netlify-cli@20.1.1
        //             node_modules/.bin/netlify deploy --dir=build
        //         '''
        //     }
        // }

        // stage('Approval') {
        //     agent none
        //     steps {
        //         timeout(time: 15, unit: 'MINUTES') {
        //             input message: '운영 환경에 배포할까요?',
        //               ok: '네, 배포합니다.'
        //         }
        //     }
        // }

        // stage('Deploy prod') {
        //     agent {
        //         docker { image 'node:18-bullseye' }
        //     }
        //     steps {
        //         sh '''
        //             npm install netlify-cli@20.1.1
        //             node_modules/.bin/netlify deploy --dir=build --prod
        //         '''
        //     }
        // }

        // stage('Prod E2E') {
        //     agent {
        //         docker { image 'mcr.microsoft.com/playwright:v1.39.0-jammy' }
        //     }
        //     environment {
        //         CI_ENVIRONMENT_URL = 'https://spiffy-baklava-487682.netlify.app'
        //     }
        //     steps {
        //         sh '''
        //             npx playwright test --reporter=html
        //         '''
        //     }
        // }
    }


}