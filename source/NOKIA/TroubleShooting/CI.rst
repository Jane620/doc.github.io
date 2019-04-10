



HWSCT vtk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. 检查oam参数targetDNtype=VM(VM则生效oamtls参数,否则则默认走证书)
2. 检查oam的参数 oamtls=off(off走http,否则走https，需要安装证书)
3. 检查CU oam虚拟机的端口是否开启 , netstat -ntulp |grep 8080
4. 检查robot pc机是否开启vtk的8088监听端口，通同上
5. 可以采用telnet target_ip port的方式测试两则的连通性
6. 遇到问题可以抓起对应的wireshark, tcpdump -i net_port host ip -w file.pcap
