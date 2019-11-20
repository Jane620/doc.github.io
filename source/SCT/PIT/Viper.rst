


endpoint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
所有的注册入口都在固件System中register_endpoints方法
:
def system(bts, vbts):
    test_system = TestSystem('root')
    bts.register_endpoints(test_system)

    with test_system:
        yield test_system

jsonrpc register过程
1. 通过bts固件可以看到register_endpoints方法 system.register(obj.port)
2. 其中register最终来源obj.port ,此处应该为 JsonRpcEndpoint()
3. 检查JsonRpcEndpoint的基类为BaseEndpoint, 调用的为该基类的register方法
4. 检查register方法，发现最终实现为 self.open()
5. 检查自身的open()方法，发现为 self._open()
6. 发现自身没有 _open()，从子类JsonRpcEndpoint中寻找实现，发现为 self._gateway.open()
7. 检查 self._gateway, 发现为实例化了 JsonRpcGateway()
8. 检查 JsonRpcGateway().open() , 发现为 self._transport.open()
9. 检查 self._transport , 发现为实例化 HttpTransport()
10.检查 HttpTransport().open(), 发现为 self._server.open()
11.检查self._server , 发现实例化 http.HttpServer()
12.检查 http.HttpServer().open(),  最终发现为
  self._create_socket()
  self._socket.bind(self._local_address)
  self._socket.listen()

sic register过程
1. 通过bts固件可以看到register_endpoints方法 system.register(obj.port)
2. 其中register最终来源obj.port ,此处应该为 SicEndpoint()
3. 检查SicEndpoint的基类为BaseEndpoint, 调用的为该基类的register方法
4. 检查register方法，发现最终实现为 self.open()
5. 检查自身的open()方法，发现为 self._open()
6. 发现自身没有 _open()，从子类SicEndpoint中寻找实现，发现为 self._transport.open() 和 self._gateway.register()
  6.1 self._transport.open()
    6.1.1 self._transport , 发现实例了 sic.ConnectionlessTransport()
    6.1.2 检查sic.ConnectionlessTransport无open(), 继续找基类 , 找到BaseTransport().open()
    6.1.3 检查open(), 发现为 self._server.open()
    6.1.4 检查 self._server 发现为子类传入的参数，最终在ConnectionlessTransport中找到为 sctp.Server
    6.1.5 检查 sctp.Server, 未找到open，继续寻找基类，最终在 BaseServer().open() 中找到 self._socket.create() 和 self._socket.open()
      6.1.5.1 self._socket.create()
        6.1.5.1.1 检查self._socket, 发现为子类传入的类, 最终为 sctp.Server 中的 SocketWrapper.create()
        6.1.5.1.2 检查SocketWrapper.create(), 最终发现为 self._socket = native_socket.socket()
      6.1.5.2 self._socket.open()
        6.1.5.2.1 发现open()同理，最终也是SocketWrapper.open()
  6.2 self._gateway.register()
    6.2.1 检查 self._gateway ,发现实例了 syscom.Gateway()
    6.2.2 检查 syscom.Gateway().register(), 发现发送注册消息以及添加路由过程
      request = sic.Envelope()
      response = self._testport.execute(request)
      message = sack.AaSysComGwRegReply()
      ...
      route = Route()
      self._routes.append(route)
