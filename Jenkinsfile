pipeline {
 
  agent {
    kubernetes {
      label 'hugo-agent'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: "jnlp"
      volumeMounts:
      - name: "volume-known-hosts"
        mountPath: /home/jenkins/.ssh
      - name: "volume-0"
        mountPath: "/opt/tools"
        readOnly: false
      resources:
        limits:
          memory: "2Gi"
          cpu: "1"
        requests:
          memory: "2Gi"
          cpu: "1"
    - name: hugo
      image: eclipsemosaic/hugo:0.75
      command:
      - cat
      tty: true
      resources:
        limits:
          memory: "2Gi"
          cpu: "1"
        requests:
          memory: "2Gi"
          cpu: "1"
  volumes:
  - name: volume-known-hosts
    configMap:
      name: known-hosts
  - name: "volume-0"
    persistentVolumeClaim:
      claimName: "tools-claim-jiro-mosaic"
      readOnly: true
"""
    }
  }
  
  parameters {
    text(name: 'JAVADOC_VERSION', defaultValue: 'keep', description: 'Enter the MOSAIC version to generate JAVADOC from.')
  }
 
  environment {
    PROJECT_NAME = "mosaic"
    PROJECT_BOT_NAME = "MOSAIC Bot"
  }
 
  triggers { pollSCM('H/10 * * * *') 
 
 }
 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
    checkoutToSubdirectory('hugo')
    disableConcurrentBuilds()
  }
 
  stages {
      
    stage('Checkout www branch') {
      steps {
        dir('www') {
            sshagent(['github-bot-ssh']) {
                sh '''
                    git clone git@github.com:eclipse/mosaic.website.git .
                    git checkout www
                '''
            }
        }
      }
    }
    
    stage('Generate JavaDoc') {
        when {
            expression { params.JAVADOC_VERSION != 'keep' }
        }
        steps {
            dir('mosaic') {
                checkout scm: [$class: 'GitSCM', 
                  userRemoteConfigs: [[url: 'https://github.com/eclipse-mosaic/mosaic.git']], 
                  branches: [[name: "refs/tags/${params.JAVADOC_VERSION}"]]
                ], poll: false
                sh '/opt/tools/apache-maven/3.6.3/bin/mvn install -DskipTests=true -Dmaven.repo.local=.m2'
                sh '/opt/tools/apache-maven/3.6.3/bin/mvn javadoc:javadoc javadoc:aggregate -DadditionalJOption=-Xdoclint:none -Dmaven.repo.local=.m2'
            }
        }
    }
    
    stage('Backup existing JavaDoc') {
        when {
            expression { params.JAVADOC_VERSION == 'keep' }
        }
        steps {
            dir('mosaic') {
               sh 'mkdir -p target/site/apidocs'
               sh 'cp -Rvf ../www/java_docs/* target/site/apidocs/ || :'
            }
        }
    }
    
    stage('Build website from main with Hugo') {
      when {
        branch 'main'
      }
      steps {
        container('hugo') {
            dir('hugo') {
                sh 'hugo -b https://www.eclipse.dev/${PROJECT_NAME}/'
            }
        }
      }
    }
   
    stage('Push to www branch') {
      when {
        branch 'main'
      }
      steps {
        sh 'rm -rf www/* && cp -Rvf hugo/public/* www/'
        sh 'mkdir -p www/java_docs && cp -Rvf mosaic/target/site/apidocs/* www/java_docs/ || :'
        dir('www') {
            sshagent(['github-bot-ssh']) {
                sh '''
                    git add -A
                    if ! git diff --cached --exit-code; then
                      echo "Changes have been detected, publishing to repo 'www.eclipse.org/${PROJECT_NAME}'"
                      git config user.email "${PROJECT_NAME}-bot@eclipse.org"
                      git config user.name "${PROJECT_BOT_NAME}"
                      git commit -m "Website build ${JOB_NAME}-${BUILD_NUMBER}"
                      git log --graph --abbrev-commit --date=relative -n 5
                      git push origin HEAD:www
                    else
                      echo "No change have been detected since last build, nothing to publish"
                    fi
                '''
            }
        }
      }
    }
  }
}
