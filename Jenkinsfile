pipeline {
    agent any

    stages {
        stage ("Checkout code") {
            steps {
                git url: "https://github.com/drsahook/pulumi-jenkins-app-deploy-new.git",
                    // Set your credentials id value here.
                    // See https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials
                    credentialsId: "f51c21d3-65ec-46f5-b034-190160ab09d7",
                    // You could define a new stage that specifically runs for, say, feature/* branches
                    // and run only "pulumi preview" for those.
                    branch: "main"
            }
        }

        stage ("Install dependencies") {
            steps {
                sh "npm install"
                sh "curl -fsSL https://get.pulumi.com | sh"
                sh "$HOME/.pulumi/bin/pulumi version"
            }
        }

        stage ("Pulumi up") {
            steps {
                // The value "node 8.9.4" is the configuration name in our Global Tool Configuration setup for node.
                // You should use the name that you used when you added the installation on that page.
                nodejs(nodeJSInstallationName: "node 14.10.1") {
                    withEnv(["PATH+PULUMI=$HOME/.pulumi/bin"]) {
                        sh "pulumi stack select ${PULUMI_STACK}"
                        sh "pulumi up --yes"
                    }
                }
            }
        }
    }
}
