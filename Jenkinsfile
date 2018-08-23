podTemplate(name: 'jnlp', label: 'jnlp', cloud: 'kubernetes',
  containers: [
        containerTemplate(
            name: 'jnlp',
            image: 'hub.easystack.io/3dc70621b8504c98/jenkins-slave-maven:v1',
            command: '',
            args: '${computer.jnlpmac} ${computer.name}',
            privileged: true,
            alwaysPullImage: false,
            ttyEnabled: true, 
        ),
  ],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
            hostPathVolume(hostPath: '/usr/bin/docker', mountPath: '/usr/bin/docker'),
            hostPathVolume(hostPath: '/usr/bin/docker-current', mountPath: '/usr/bin/docker-current'),
            hostPathVolume(hostPath: '/etc/sysconfig/docker', mountPath: '/etc/sysconfig/docker'),
            hostPathVolume(hostPath: '/usr/bin/kubectl', mountPath: '/usr/bin/kubectl')]
  ) {

  node('jnlp') {
    stage('CICD for Dubbo Demo') {
        container('jnlp') {
            stage("Clone source code of Dubbo Demo") {
                //请按需修改Git源代码库地址
                //如果是Private项目，参考示例如下（需使用GitLab Access Token）
                sh """
                    git clone http://oauth2:QK6YdGhk3muxTPzAQC1B@172.16.6.28:30080/easystack/dubbo-demo.git
                """
                //如果是Public项目，参考示例如下
                //git 'http://172.16.6.28:30080/easystack/dubbo-demo.git'
            }
            
            stage('Compile') {
                echo 'Hello, Maven!'
                sh 'java -version'
                dir('./dubbo-demo/dubbo-demo')
                {
                    sh '/opt/rh/rh-maven33/root/usr/bin/mvn clean install'
                }
            }
            
            stage('Build & push docker image') {
                //请按需修改镜像仓库的账号和密码，并注意docker build命令中Dockerfile所在路径
                sh """
                    docker login -u 3dc70621b8504c98 -p Tcdf4f05247d79dd7 hub.easystack.io
                    docker build -t hub.easystack.io/3dc70621b8504c98/dubbo-consumer:v${BUILD_NUMBER} ./dubbo-demo/dubbo-demo/dubbo-demo-consumer
                    docker push hub.easystack.io/3dc70621b8504c98/dubbo-consumer:v${BUILD_NUMBER}
                    docker build -t hub.easystack.io/3dc70621b8504c98/dubbo-provider:v${BUILD_NUMBER} ./dubbo-demo/dubbo-demo/dubbo-demo-provider
                    docker push hub.easystack.io/3dc70621b8504c98/dubbo-provider:v${BUILD_NUMBER}
                """
            }
            
            //stage('Deploy app to EKS') {
                //请按需修改Deployment名称和dubbo-demo-provider、dubbo-demo-consumer的镜像名称
                //sh """kubectl set image deployment/dubbo-demo-provider dubbo-demo-jar=hub.easystack.io/3dc70621b8504c98/dubbo-provider:v${BUILD_NUMBER}"""
                //sh """kubectl set image deployment/dubbo-demo-consumer dubbo-demo-jar=hub.easystack.io/3dc70621b8504c98/dubbo-consumer:v${BUILD_NUMBER}"""
            //}
        }
    }
 }
}
