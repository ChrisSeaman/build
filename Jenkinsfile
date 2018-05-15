pipeline {
      agent none
    
      environment {
          NODE_VER = '8.1.0'
      }

        // Post block
        post {
            // Don't have access to the workspace...  really just for notifications
            // can also do success
            success {
                echo "Build is complete"
            }
        }

        // Options block
        // options {
            // have to tell Jenkins when to checkout code
            //skipDefaultCheckout()
            //timeout(time: 1, unit: 'DAYS')
        //}


        // Tools block
        // tools {
            // Causes tool to be installed on the agent
            //nodejs "${env.NODE_VER"
        //}

      stages {
        stage('Beginning') { agent any
            environment {
                DEPLOY_VERSION = 'stage'
            }
            steps {
                echo 'Hello world'
                sh 'echo $NODE_VER'
                echo "${env.DEPLOY_VERSION}"
            }
        }

        stage('Who am I?') { agent any
            environment {
                DEPLOY_VERSION = 'prod'
            }
            steps {
                echo "${env.DEPLOY_VERSION}"
                sh 'host -t TXT pgp.michaelholley.us | awk -F \'"\' \'{print $2}\''
            }
        }

        stage('Deploy to stage?') { agent none
            // agent none is important if you want UI interaction

            when {
                branch 'stage'
                // branch 'PR-*'
                // environment nmae: 'NODE_VER", value: '8.1.0'
                // build tag, env variables, expressions, etc
                // anyOf {
                    //expressions...
                //}
            }
            steps {
                input 'Deploy to staging?'
            }
        }

        stage('Parallel') {
            // If any one parallel instance fails, fail all
            failFast true

            parallel {
                stage('Build 1') { agent any
                    steps {
                        echo "It's me!"
                    }
                }
                stage('Build 2') { agent any
                    steps {
                        echo "Build #2"
                    }
                }
            }
        }
      }
}
