void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/sintopek/hula-codeo.git"],
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
                sh "rm -rf hula"
                sh "mkdir hula"
                sh "cd hula && git clone https://github.com/ninjapiraatti/poc-rust-vue.git"
                sh "cp -r scss hula/poc-rust-vue/app/scss/custom"
                sh "cd hula/poc-rust-vue && mkdir -p public"
                sh "cd hula/poc-rust-vue && cd app && npm install"
                sh "cd hula/poc-rust-vue && cd app && npm i @smartweb/vue-flash-message@1.0.0-alpha.12"
                sh "cd hula/poc-rust-vue && cd app && npm i @popperjs/core"
                sh "cd hula/poc-rust-vue && cd app && npm run dev & sleep 60s"
                sh "cd hula/poc-rust-vue && cargo build --release"
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
