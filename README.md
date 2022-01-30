# jenkins-tutorials

Youtube : https://www.youtube.com/c/xtremeexcel

1. Jenkins installation



2. Github account, github plugin, github token

    If you don't have a github account, then visit https://github.com/signup and create an account.

    If you are not having Github plugins installed on Jenkins, then follow these steps

        a. Go to Manage Jenkins and then click on Manage Plugins.

            ![manage_jenkins](../images/manage_jenkins.png)

        b. Install git and github plugins and restart jenkins.

            ![git_plugin](../images/git_plugin.png)

            ![github_plugin](../images/github_plugin.png)



3. Pipeline using Pipeline script

    Dashboard > New Item > Pipeline
    Definition = Pipeline script
    Enter script
    
    Windows
    ```
    pipeline {
       agent any
        stages {
            stage('build') {
                steps {
                    bat 'python -V'
                }
            }
        }
    }
    ```

    Linux
    ```
    pipeline {
       agent any
        stages {
            stage('build') {
                steps {
                    sh 'python -V'
                }
            }
        }
    }
    ```


4. JENKINSFILE using SCM

    Dashboard > New Item > Pipeline
    Pipeline script from SCM
    Enter github repo
    Credentials
    Specify branch
    Script Path = JENKINSFILE1

    Windows
    ```
    pipeline {
       agent any
        stages {
            stage('build') {
                steps {
                    bat 'python -V'
                }
            }
        }
    }
    ```

    Linux
    ```
    pipeline {
       agent any
        stages {
            stage('build') {
                steps {
                    sh 'python -V'
                }
            }
        }
    }
    ```

5. Pipeline with post actions

JENKINSFILE2

Windows
[Note :use sh instead of bat for Linux]
```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                bat 'python -V'
            }
        }
    }
 post {
        always {
            echo 'Always'
        }
        success {
            echo 'Only on SUCCESS'
        }
        failure {
            echo 'Only on Failure'
        }
        unstable {
            echo 'Only if run is unstable'
        }
        changed {
            echo 'Only if status changed from Success to Failure or vice versa w.r.t. last run.'
        }
    }
}
```

7. Multiple Stages

JENKINSFILE3
```
pipeline {
    agent any
    stages {
        stage('pre -build') {
            steps {
                bat 'echo Pre-build'
            }
        }
        stage('build') {
            steps {
                bat 'echo Build in progress.'
            }
        }
        stage('Unit tests') {
            steps {
                bat 'echo Running unit tests'
            }
        }
        stage('deploy') {
            steps {
                bat 'echo Deploying build'
            }
        }
        stage('Regression tests') {
            steps {
                bat 'echo Running E2E tests'
            }
        }
        stage('Release to prod') {
            steps {
                bat 'echo Releasing to prod'
            }
        }
    }
 post {
        always {
            echo 'Cleanup after everything!'
        }
    }
}
```

7. Blue ocean

8. Parallel steps in a stage

JENKINSFILE 4
```
pipeline {
    agent any
    stages {
        stage('pre -build') {
            steps {
                bat 'echo Pre-build'
            }
        }
        stage('build') {
            steps {
                bat 'echo Build in progress.'
            }
        }
        stage('Unit tests') {
            steps {
                bat 'echo Running unit tests'
            }
        }
        stage('deploy') {
            steps {
                bat 'echo Deploying build'
            }
        }
        stage('Regression tests') {
            steps {
                parallel(
                    chrome : {
                        bat 'echo Running E2E tests on chrome'
                    },
                    firefox : {
                        bat 'echo Running E2E tests on chrome'
                    },
                    safari : {
                        bat 'echo Running E2E tests on chrome'
                    }
                )
                
            }
        }
        stage('Release to prod') {
            steps {
                bat 'echo Releasing to prod'
            }
        }
    }
 post {
        always {
            echo 'Cleanup after everything!'
        }
    }
}
```