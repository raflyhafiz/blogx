pipeline {
 agent any

 environment {
  //  def GIT_COMMIT_SHORT = sh(returnStdout: true, script: '''echo $GIT_COMMIT | head -c 7''')
   def GIT_COMMIT_SHORT = sh(returnStdout: true, script: 'git log -1 --pretty=format:"%h"').trim()
 }

 stages {
   stage('Prepare .env') {
     steps {
       sh 'echo GIT_COMMIT_SHORT=$(echo $GIT_COMMIT_SHORT) > .env.compose'
     }
   }

   stage('Build laravel') {
     steps {
         sh 'docker build -t rafly21/laravel:$GIT_COMMIT_SHORT .'
         sh 'docker tag rafly21/laravel:$GIT_COMMIT_SHORT rafly21/laravel:$GIT_COMMIT_SHORT'
         sh 'docker push rafly21/laravel:$GIT_COMMIT_SHORT'
       
     }
   }

   stage('Deploy to remote server') {
     steps {
        // sshPublisher(publishers: [sshPublisherDesc(configName: 'Remote Server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''mv .env.compose .env

        // docker-compose up -d''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '.env.compose,docker-compose.yaml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

        sshPublisher(publishers: [sshPublisherDesc(configName: 'Remote Server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''mv .env.compose .env

        docker-compose up -d

        docker exec -it rafly21/laravel:${GIT_COMMIT_SHORT} php artisan key:generate

        docker exec -it rafly21/laravel:${GIT_COMMIT_SHORT} php artisan migrate''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '.env.compose,docker-compose.yaml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

     }
   }
 }
}