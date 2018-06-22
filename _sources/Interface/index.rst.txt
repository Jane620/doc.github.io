接口测试相关
==========================

::

  node{
      stage('get clone'){
          //check CODE
         git credentialsId: 'a680ff2c-ea6a-430a-8aff-9c2f1dcd375c', url: 'git@gitlab.yst.com.cn:firefox/NFSpring_service.git'
      }

      //定义mvn环境
      def mvnHome = tool "M3"
      def project_name = "NFSpring_service"
      env.PATH = "${mvnHome}/bin:${env.PATH}"

      stage('test:unit'){
          //mvn 测试测试测试
          echo "do nothing"
      }

      stage('test：functional'){
          //mvn 测试
          echo "do nothing"
      }

      stage('mvn build'){
          //mvn构建
          sh "mvn org.jacoco:jacoco-maven-plugin:0.7.4.201502262128:prepare-agent clean install -Dmaven.test.failure.ignore=true"
          sh "mvn sonar:sonar -Dsonar.host.url=http://10.213.3.181:9000/"
          sh "python3 /home/jenkins/.python/sonar.py $project_name"
      }

      stage('deploy'){
          //执行部署脚本
          echo "deploy ......"
          sh "/usr/bin/sshpass  -p 'admin' ssh  -oUserKnownHostsFile=/dev/null -oStrictHostKeyChecking=no admin@10.213.2.56 -C '/home/admin/bin/webservices_ctl_tomcat_nginx.sh release NFSpring_service_test_lw/target/NFSpring_service.war `date +'%Y-%m-%d'` `date +'%s'`'"
          sh "/usr/bin/sshpass  -p 'admin' ssh  -oUserKnownHostsFile=/dev/null -oStrictHostKeyChecking=no admin@10.213.3.58 -C '/home/admin/bin/webservices_ctl_tomcat_nginx.sh release NFSpring_service_test_lw/target/NFSpring_service.war `date +'%Y-%m-%d'` `date +'%s'`'"
      }

  }
