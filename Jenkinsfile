pipeline { 
    agent { 
        node { 
        label 'dockerhost-build-server' 
        } 
    } 
    tools { 
        maven 'maven-3.9.6' 
    } 
    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }
    stages { 
        stage('Packaging') {
            steps {
                echo 'Packaging..'
                sh '''
                export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
                export PATH=$JAVA_HOME/bin:$PATH
                
                echo "=== VERIFICANDO LA VERSIÓN DE MAVEN Y JAVA ==="
                mvn -version
                
                echo "=== INICIANDO COMPILACIÓN ==="
                mvn clean package
                '''
            }
        }
        stage('Copying jar file') { 
            steps { 
                echo 'Copying war file..' 
                sh 'mv target/*.jar .' 
            } 
        } 
        stage('cleanup') { 
          steps { 
            sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server"' 
          } 
        } 
        stage('build image') { 
          steps { 
            sh 'docker build -t uaxivbru/campaign-demo:v1 --label campaign-demo-server .' 
          } 
        } 
        stage('run container') { 
          steps { 
            sh 'docker run -d --name campaign-demo-server --label campaign-demo-server -p 5000:5000 uaxivbru/campaign-demo:v1' 
          } 
        } 
    } 
  } 
