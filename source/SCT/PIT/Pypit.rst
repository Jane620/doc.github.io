


Runs On container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
container deplyment
10.131.150.90 root/nokia123
/root/helm/aic-oamfh
1. modify value.yaml and value-override.yaml
2. helm list (if exist vdu1 ,then delete)
  helm delete --purge vdu1
3. install : helm install ./ -f values-override.yaml --name vdu1 --namespace oamfh-pit
4. check status : kubectl get pods --all-namespaces
5. 查看详细pods日志: kubectl describe pods -A (check details , if pulling)

打knife
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 查看pod在哪个node上
  kubectl get pods --all-namespaces -o wide
2. 获取所在node的地址
  kubectl get nodes --all-namespaces -o wide
3. ssh 到这个node上(一般root)
4. 获取对应container的id  (b3af1e4d5af2)
  docker ps |grep oamfh
5. 拷贝需要替换的文件到node上，通过scp username@ip
6. 在node中进行拷贝到container: docker cp 文件 container_id:path
6. 重启container ： docker restart container_id
7. 检查日志是否有错和启动状态
    kubectl logs vdu1-aic-vdu-oamcp-deployment-0 -c oamfh -n oamfh-pit
    kubectl exec -it vdu1-aic-vdu-oamcp-deployment-0 -c oamfh -n oamfh-pit -- /bin/bash
    ps -ef (see oamfh.json， it's ok)
8. 宿主机拷贝到container的方式 ：  kubectl cp  /root/knife/libprotobuf.so.17.0.0 oamfh-pit/vdu1-aic-vdu-oamcp-deployment-0:/root/ -c oamfh


更新image，在register失效的情况下
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 更新本地的image: sudo docker pull vran-aic-docker-local.hzisoj70.china.nsn-net.net/vdu_release/oamfh:0.0.5
2. 保存本地的image: sudo docker save vran-aic-docker-local.hzisoj70.china.nsn-net.net/vdu_release/oamfh:0.0.5 -o oamfhrmt.0.0.5.tar
3. 拷贝到对应的node: scp oamfhrmt.0.0.5.tar root@192.168.0.12:/root
4. 重新load image: docker load -i oamfhrmt.0.0.5.tar


pypit deployment
1. enter the pypit : kubectl exec -it vdu1-aic-vdu-oamcp-deployment-0 -c cprt -n oamfh-pit -- /bin/bash   (can change this name to pypit in value.yaml)
2. copy file inside of container: kubectl cp  /root/helm/vdu/vdupit oamfh-pit/vdu1-aic-vdu-oamcp-deployment-0:/root -c cprt
3. enter case location : cd /root/vdupit
4. source pypit.env
5. run case :  pypit-test src/testcases/startup/test_oamfh_static_l3_call.py -- -s
6. if this passed ,then can switch to CI
