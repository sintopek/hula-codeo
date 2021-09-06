void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/ninjapiraatti/poc-rust-vue.git"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "mkdir hula"
                sh "cd hula && git clone https://github.com/ninjapiraatti/poc-rust-vue.git"
                sh "cd hula && mkdir -p public"
                sh "cd hula && cd app && npm install"
                sh "cd hula && cd app && npm i @smartweb/vue-flash-message@1.0.0-alpha.12"
                sh "cd hula && cd app && npm i @popperjs/core"
                sh "cd hula && cd app && npm run dev & sleep 60s"
                sh "cd hula && cargo build --release"
            }
        }
    }
    post {
        success {
            setBuildStatus("Build succeeded", "SUCCESS");
        }
        failure {
            setBuildStatus("Build failed", "FAILURE");
        }
    }
}
