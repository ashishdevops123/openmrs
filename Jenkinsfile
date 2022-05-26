pipeline{
    agent{ label'JDK11'}
    options{
        timeout(time: 1, unit: 'HOURS')
        retry(1)
        }
    triggers{
        cron('0 * * * *')   
    
    }
    parameters{
        choice(name: 'GOAL', choices: ['compile','package', 'clean package'] )
    }

    stages{
        stage('sourcecode'){
            steps{
                git url: 'https://github.com/ashishdevops123/openmrs.git',
                    branch: 'prod'
                
            }
        }
        stage("build & SonarQube analysis") {
            steps {
                withSonarQubeEnv('sonar_latest') {
                    sh script: "mvn ${params.GOAL} sonar:sonar"
                }
          }
        }
        stage('reporting'){
            steps{
                junit testResults: '**/target/surefire-reports/*.xml'
               
            }
        }
    }
    // post{
    //     success{
    //         echo "success"
    //         mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS ${RUN_TESTS_DISPLAY_URL} ", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
    //             subject: "${JOB_BASE_NAME}: Build ${BUILD_ID} Succeded", to: 'gannapuramashish1996@gmail.com'

    //     }
    //     failure{
    //         echo "failure"
    //         mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS ${RUN_TESTS_DISPLAY_URL} ", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
    //             subject: "${JOB_BASE_NAME}: Build ${BUILD_ID} failed", to: 'gannapuramashish1996@gmail.com'
    //     }
    //     }
    }


