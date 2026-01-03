# Jenkins Pipeline

### 🧩 Simple Pipeline Echo Hello World

```bash
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello Jenkins Pipeline!'
            }
        }

        stage('Run Shell Command') {
            steps {
                sh 'echo "Pipeline is working successfully"'
            }
        }
    }
}
```

### 🧩 Simple Pipeline with ssh login, build docker image and restart pods

```bash
pipeline {
    agent any

    stages {
        stage('Build & Restart Deployment') {
            steps {
                echo 'Starting deployment restart...'

                sh '''
                    ssh -o StrictHostKeyChecking=no \
                    -i /var/jenkins_home/.ssh/id_rsa \
                    sourov@172.17.186.110 "
                        set -e
                        cd ~/www
                        echo 'Using Minikube Docker...'

                        source ~/.bashrc || true
                        eval \\$(minikube docker-env)

                        echo 'Building Docker image...'
                        docker build -t html-website:latest .

                        echo 'Restarting deployment...'
                        kubectl rollout restart deployment/html-website-deployment

                        echo 'Waiting for rollout...'
                        kubectl rollout status deployment/html-website-deployment

                        echo 'Cluster status:'
                        kubectl get all
                    "
                '''
            }
        }

        stage('Post-Check') {
            steps {
                echo 'Deployment restarted successfully'
            }
        }
    }

     post {
        success {
            echo 'Build successfully ✅'
        }
        failure {
            echo 'Build failed ❌'
        }
    }
}
```

### 🧩 Add Parameters

```bash
 parameters {
        // 1. String parameter
        string(
            name: 'APP_NAME',
            defaultValue: 'my-app',
            description: 'Enter application name'
        )

        // 2. Boolean parameter
        booleanParam(
            name: 'DEPLOY',
            defaultValue: true,
            description: 'Deploy after build?'
        )

        // 3. Choice (single selection)
        choice(
            name: 'GIT_BRANCH',
            choices: ['main', 'dev', 'staging', 'feature'],
            description: 'Select Git branch to build'
        )

        // 4. Multi-line Text
        text(
            name: 'ENV_VARS',
            defaultValue: 'KEY1=VALUE1\nKEY2=VALUE2',
            description: 'Environment variables (multi-line)'
        )

        // 5. Password (hidden)
        password(
            name: 'SECRET_KEY',
            defaultValue: '',
            description: 'Enter secret key'
        )

        // 6. Number parameter
        string(
            name: 'REPLICAS',
            defaultValue: '3',
            description: 'Number of replicas (integer)'
        )

        // 7. Multi-choice (via extended choice plugin, optional)
        // Requires: Extended Choice Parameter Plugin
        // Uncomment if plugin installed
        /*
        extendedChoice(
            name: 'SERVICES',
            type: 'PT_MULTI_SELECT',
            description: 'Select services to deploy',
            multiSelectDelimiter: ',',
            value: 'auth,api,frontend,worker'
        )
        */
    }

    stage('Show Parameters') {
        steps {
            echo "App Name: ${params.APP_NAME}"
            echo "Deploy: ${params.DEPLOY}"
            echo "Git Branch: ${params.GIT_BRANCH}"
            echo "Environment Variables: ${params.ENV_VARS}"
            echo "Secret Key: ${params.SECRET_KEY ? '****' : ''}"
            echo "Replicas: ${params.REPLICAS}"
            // echo "Selected Services: ${params.SERVICES}"  // For extended choice
        }
    }

```

### 🧩 Use Built-in Jenkins Variables

```bash
    pipeline {
        agent any
        stages {
            stage('Show Built-in Vars') {
                steps {
                    echo "Job Name: ${env.JOB_NAME}"
                    echo "Build Number: ${env.BUILD_NUMBER}"
                    echo "Workspace: ${env.WORKSPACE}"
                    echo "Git Branch: ${env.GIT_BRANCH}"
                }
            }
        }
    }
```

### 🧩 Use Environment Variable

```bash
    pipeline {
        agent any
    
        environment {
            APP_NAME = "task-management"
            ENV = "production"
            DOCKER_IMAGE = "task-app:v1"
        }
    
        stages {
            stage('Use Env Var') {
                steps {
                    sh '''
                        echo App: $APP_NAME
                        echo Env: $ENV
                        echo Image: $DOCKER_IMAGE
                    '''
                }
            }
        }
    }
```

### 🧩 Use Custom Variable inside stage

```bash
    pipeline {
        agent any
    
        stages {
            stage('Custom Var') {
                steps {
                    script {
                        def version = "1.0.${env.BUILD_NUMBER}"
                        echo "Version: ${version}"
                    }
                }
            }
        }
    }
```

### 🧩 Use SSH Agent Plugin for SSH Access

```bash
// Plugin: SSH Agent
// SSH Register: http://localhost:8080/manage/credentials/store/system/

stage('Test SSH') {
    steps {
        sshagent(['testkey']) {
            sh ''' ssh -o StrictHostKeyChecking=no sourov@172.17.186.110 "echo Hello from remote" '''
        }
    }
}
```








