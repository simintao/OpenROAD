pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/test.sh'
      }
    }
    stage('Build-Flow') {
      environment {
        OPENROAD_FLOW_NO_GIT_INIT = 1
      }
      steps {
        sh 'cd flow && ./build_openroad.sh'
      }
    }
    stage('Test-Flow') {
      failFast true
      parallel {
        stage('nangate45_gcd') {
          steps {
            catchError {
              sh label: 'nangate45_gcd', script: '''
              docker run -u $(id -u ${USER}):$(id -g ${USER}) openroad/flow bash -c "cd flow && source setup_env.sh && cd flow && test/test_helper.sh gcd nangate45"'''
            }
            echo currentBuild.result
          }
        }
        /* stage('nangate45_aes') {
          steps {
            catchError {
              sh label: 'nangate45_aes', script: '''
              docker run -u $(id -u ${USER}):$(id -g ${USER}) openroad/flow bash -c "cd flow && source setup_env.sh && cd flow && test/test_helper.sh aes nangate45"'''
            }
            echo currentBuild.result
          }
        }
        stage('nangate45_tinyRocket') {
          steps {
            catchError {
              sh label: 'nangate45_tinyRocket', script: '''
              docker run -u $(id -u ${USER}):$(id -g ${USER}) openroad/flow bash -c "cd flow && source setup_env.sh && cd flow && test/test_helper.sh tinyRocket nangate45"'''
            }
            echo currentBuild.result
          }
        }
        stage('nangate45_bp_fe_top') {
          steps {
            catchError {
              sh label: 'nangate45_bp_fe_top', script: '''
              docker run -u $(id -u ${USER}):$(id -g ${USER}) openroad/flow bash -c "cd flow && source setup_env.sh && cd flow && test/test_helper.sh bp_fe_top nangate45"'''
            }
            echo currentBuild.result
          }
        } */
      }
    }
  }
}
