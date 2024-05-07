// pipeline {
//     agent any
//     parameters {
//         choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
//         booleanParam(name: 'executeTests', defaultValue: true, description: '')
//     }

//     stages {
//         stage("build") {
//             steps {
//                 echo 'Building the application...'
//             }
//         }
//         stage("test") {
//             when {
//                 expression {
//                     params.executeTests
//                 }
//             }
//             steps {
//                 echo 'Testing the application...'
//             }
//         }
//         stage("deploy") {
//             steps {
//                 echo 'Deploying the application...'
//                 echo "Deploying version ${params.VERSION}"
//             }
//         }
//     }
// }

def gv

pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }

    stages {
        stage("init") {
            steps {
                script{
                    gv = load "script.groovy"
                }
            }
        }
        stage("build") {
            steps {
                script{
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script{
                    gv.testApp()
                }
            }
        }
        stage("deploy") {
            input{
    message "Select the Environment to deploy to"
    ok "Done"
    parameters{
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: '')
    }
}

            steps {
                script{
                    gv.deployApp()
                    echo "Deploying to ${ENV}"
                }
            }
        }
    }
}
