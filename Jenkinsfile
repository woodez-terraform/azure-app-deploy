pipeline {

  agent { label 'azure' }

  parameters{
      string(defaultValue: 'eastus2', name: 'Location', description: 'what region to deploy in azure', trim: true)
      string(defaultValue: 'woodez-app-rg', name: 'Resourcegroup', description: 'Azure resourcegroup', trim: true)
      string(defaultValue: 'woodez-appserviceplan', name: 'Appserviceplan', description: 'App Service Plan', trim: true)
      string(defaultValue: 'woodez-app-service', name: 'Appservice', description: 'App Service Name', trim: true)
      string(defaultValue: 'https://github.com/woodez-terraform/python-docs-hello-django.git', name: 'appurl', description: 'git url for app code')
      choice(choices: ['Free'], name: 'tier', description: 'app service plan tier')
      choice(choices: ['F1'], name: 'size', description: 'app service plan size')
  }

  stages {

    stage('Checkout') {
      steps {
          checkout scm
      }  
    }

    stage('Checkout app code to deploy') {
        steps {
            script {
                    sh """
                       [[ -d "app-deploy" ]] && rm -rf app-deploy
                       git clone ${params.appurl} app-deploy
                    """    
             
            }
        }
    }

    stage('Deploy app code to azure app service'){
        steps {
               sh """
                  cd app-deploy
                  echo "Deploying code to ${params.Appservice}"
                  ls -la
                  az webapp up -l ${params.Location} -g ${params.Resourcegroup} -p ${params.Appserviceplan} -n ${params.Appservice} --sku FREE
               """
        }
    }
  
  } 
}