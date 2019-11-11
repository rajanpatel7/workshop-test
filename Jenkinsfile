pipeline {

agent {
    label 'mule-builder'
  }
 
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('rajan-deploy-anypoint-user')
    MULE_VERSION = '4.1.4'
    BG = "Rajan Patel ALC Workshop trial"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
          withMaven(mavenSettingsConfig: 'mvn-settings') {
            sh 'mvn -B -U -e -V clean -DskipTests package'
          }
      }
    }

    // stage('Test') {
    //   steps {
    //     withMaven(mavenSettingsConfig: 'mvn-settings') {  
    //       sh "mvn test"
    //     }
    //   }
    // }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'sandbox-omni-channel-api-RP'
      }
      steps {
            sh 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="$MULE_VERSION" -Danypoint.username="$DEPLOY_CREDS_USR" -Danypoint.password="$DEPLOY_CREDS_PSW" -Dcloudhub.app="$APP_NAME" -Dcloudhub.environment="$ENVIRONMENT" -Dcloudhub.bg="$BG" -Dcloudhub.worker="$WORKER"'
      }
    }
    stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Production'
        APP_NAME = 'prod-omni-channel-api-RP'
      }
      steps {
            sh 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="$MULE_VERSION" -Danypoint.username="$DEPLOY_CREDS_USR" -Danypoint.password="$DEPLOY_CREDS_PSW" -Dcloudhub.app="$APP_NAME" -Dcloudhub.environment="$ENVIRONMENT" -Dcloudhub.bg="$BG" -Dcloudhub.worker="$WORKER"'
      }
    }
  }

  tools {
    maven 'M3'
  }
}