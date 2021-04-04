---
title: 利用Socket传输摄像头视频流数据
date: 2018-07-03 11:05:11
categories: 编码
tags: Socket
---


### 环境依赖
- python 3.6.1
- cv2
- numpy
- socket
- threading
- struct

### TCP通讯了解
##### 通讯流程图
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" id="processonSvg1000" viewBox="63.0 75.4605734767025 607.0 455.5394265232975" width="607.0" height="455.5394265232975"><defs id="ProcessOnDefs1001"><marker id="ProcessOnMarker1011" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1012" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1019" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1020" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1027" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1028" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1035" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1036" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1043" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1044" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1055" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1056" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1063" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1064" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1071" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1072" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker><marker id="ProcessOnMarker1107" markerUnits="userSpaceOnUse" orient="auto" markerWidth="16.23606797749979" markerHeight="10.550836550532098" viewBox="-1.0 -1.3763819204711736 16.23606797749979 10.550836550532098" refX="-1.0" refY="3.8990363547948754"><path id="ProcessOnPath1108" d="M12.0 3.8990363547948754L0.0 7.798072709589751V0.0Z" stroke="#323232" stroke-width="2.0" fill="#323232" transform="matrix(1.0,0.0,0.0,1.0,0.0,0.0)"/></marker></defs><g id="ProcessOnG1002"><path id="ProcessOnPath1003" d="M63.0 75.4605734767025H670.0V531.0H63.0V75.4605734767025Z" fill="none"/><g id="ProcessOnG1004"><g id="ProcessOnG1005" transform="matrix(1.0,0.0,0.0,1.0,202.0,99.0)" opacity="1.0"><path id="ProcessOnPath1006" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1007" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1008" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">socket()</text></g></g><g id="ProcessOnG1009"><path id="ProcessOnPath1010" d="M252.0 124.84229390681003L252.0 150.536917562724L252.00000000000003 150.536917562724L252.00000000000003 160.99547324113817" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1011)"/></g><g id="ProcessOnG1013" transform="matrix(1.0,0.0,0.0,1.0,202.0,176.23154121863797)" opacity="1.0"><path id="ProcessOnPath1014" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1015" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1016" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">bind()</text></g></g><g id="ProcessOnG1017"><path id="ProcessOnPath1018" d="M252.00000000000003 202.073835125448L252.00000000000003 227.76845878136197L252.00000000000003 227.76845878136197L252.00000000000003 238.22701445977617" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1019)"/></g><g id="ProcessOnG1021" transform="matrix(1.0,0.0,0.0,1.0,202.0,253.46308243727597)" opacity="1.0"><path id="ProcessOnPath1022" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1023" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1024" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">listen()</text></g></g><g id="ProcessOnG1025"><path id="ProcessOnPath1026" d="M252.00000000000003 279.305376344086L252.00000000000003 305.0L252.00000000000003 305.0L252.00000000000003 315.45855567841414" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1027)"/></g><g id="ProcessOnG1029" transform="matrix(1.0,0.0,0.0,1.0,202.0,330.6946236559139)" opacity="1.0"><path id="ProcessOnPath1030" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1031" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1032" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">accept()</text></g></g><g id="ProcessOnG1033"><path id="ProcessOnPath1034" d="M252.00000000000003 356.53691756272394L252.00000000000003 382.231541218638L252.00000000000003 382.231541218638L252.00000000000003 392.6900968970522" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1035)"/></g><g id="ProcessOnG1037" transform="matrix(1.0,0.0,0.0,1.0,202.0,407.92616487455194)" opacity="1.0"><path id="ProcessOnPath1038" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1039" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1040" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">recv()/send()</text></g></g><g id="ProcessOnG1041"><path id="ProcessOnPath1042" d="M252.00000000000003 433.76845878136197L252.00000000000003 459.46308243727594L252.00000000000003 459.46308243727594L252.00000000000003 469.9216381156902" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1043)"/></g><g id="ProcessOnG1045" transform="matrix(1.0,0.0,0.0,1.0,202.0,485.15770609319)" opacity="1.0"><path id="ProcessOnPath1046" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1047" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1048" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">close()</text></g></g><g id="ProcessOnG1049" transform="matrix(1.0,0.0,0.0,1.0,427.0,253.46308243727606)" opacity="1.0"><path id="ProcessOnPath1050" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1051" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1052" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">socket()</text></g></g><g id="ProcessOnG1053"><path id="ProcessOnPath1054" d="M477.0 279.3053763440861L477.0 305.00000000000006L477.0 305.00000000000006L477.0 315.45855567841426" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1055)"/></g><g id="ProcessOnG1057" transform="matrix(1.0,0.0,0.0,1.0,427.0,330.69462365591403)" opacity="1.0"><path id="ProcessOnPath1058" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1059" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1060" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">connect()</text></g></g><g id="ProcessOnG1061"><path id="ProcessOnPath1062" d="M477.0 356.53691756272406L477.0 382.23154121863803L477.0 382.23154121863803L477.0 392.69009689705223" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1063)"/></g><g id="ProcessOnG1065" transform="matrix(1.0,0.0,0.0,1.0,427.0,407.926164874552)" opacity="1.0"><path id="ProcessOnPath1066" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1067" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1068" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">send()/recv()</text></g></g><g id="ProcessOnG1069"><path id="ProcessOnPath1070" d="M477.0 433.7684587813621L477.0 459.46308243727606L477.0 459.46308243727606L477.0 469.9216381156902" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1071)"/></g><g id="ProcessOnG1073" transform="matrix(1.0,0.0,0.0,1.0,427.0,485.15770609319)" opacity="1.0"><path id="ProcessOnPath1074" d="M0.0 0.0L100.0 0.0L100.0 25.842293906810035L0.0 25.842293906810035Z" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" opacity="1.0" fill="#ffffff"/><g id="ProcessOnG1075" transform="matrix(1.0,0.0,0.0,1.0,10.0,4.796146953405017)"><text id="ProcessOnText1076" fill="#000000" font-size="13" x="39.0" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">close()</text></g></g><g id="ProcessOnG1077" transform="matrix(1.0,0.0,0.0,1.0,96.0,95.4605734767025)" opacity="1.0"><path id="ProcessOnPath1078" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1079" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1080" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">创建socket</text></g></g><g id="ProcessOnG1081" transform="matrix(1.0,0.0,0.0,1.0,96.0,172.69211469534054)" opacity="1.0"><path id="ProcessOnPath1082" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1083" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1084" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">绑定IP,端口</text></g></g><g id="ProcessOnG1085" transform="matrix(1.0,0.0,0.0,1.0,96.0,249.9236559139785)" opacity="1.0"><path id="ProcessOnPath1086" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1087" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1088" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">监听IP,端口</text></g></g><g id="ProcessOnG1089" transform="matrix(1.0,0.0,0.0,1.0,83.0,327.1551971326165)" opacity="1.0"><path id="ProcessOnPath1090" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1091" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1092" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">接受客户端连接</text></g></g><g id="ProcessOnG1093" transform="matrix(1.0,0.0,0.0,1.0,83.0,404.38673835125445)" opacity="1.0"><path id="ProcessOnPath1094" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1095" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1096" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">发送或接受消息</text></g></g><g id="ProcessOnG1097" transform="matrix(1.0,0.0,0.0,1.0,96.0,478.078853046595)" opacity="1.0"><path id="ProcessOnPath1098" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1099" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1100" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">关闭socket</text></g></g><g id="ProcessOnG1101" transform="matrix(1.0,0.0,0.0,1.0,531.0,327.15519713261654)" opacity="1.0"><path id="ProcessOnPath1102" d="M0.0 0.0L119.0 0.0L119.0 32.921146953405014L0.0 32.921146953405014Z" stroke="none" stroke-width="0.0" stroke-dasharray="none" opacity="1.0" fill="none"/><g id="ProcessOnG1103" transform="matrix(1.0,0.0,0.0,1.0,0.0,8.335573476702507)"><text id="ProcessOnText1104" fill="#000000" font-size="13" x="58.5" y="13.325" font-family="微软雅黑" font-weight="normal" font-style="normal" text-decoration="none" family="微软雅黑" text-anchor="middle" size="13">连接服务端</text></g></g><g id="ProcessOnG1105"><path id="ProcessOnPath1106" d="M427.0 343.61577060931904L364.5 343.61577060931904L364.5 343.61577060931893L317.2360679774998 343.61577060931893" stroke="#323232" stroke-width="2.0" stroke-dasharray="none" fill="none" marker-end="url(#ProcessOnMarker1107)"/></g></g></g></svg>

- 创建socket:

    ```python
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    ```
    其中参数: <br/>
    **AF_INET:** IPv4 网络协议的套接字类型, **AF_INET6** 则是 IPv6 <br/>
    **SOCK_STREAM:** 有保障的(即能保证数据正确传送到对方)面向连接的SOCKET。TCP则是利用这种方式进行传输。
- 绑定IP,端口

    ```python
    sock.bind("127.0.0.1", 1234)
    ```
- 监听
    
    ```python
    # 参数 指定监听个数
    sock.listen(5)
    ```

- 接受客户端连接

    ```python
    # 返回客户端socket 及IP地址信息
    client,addr = self.sock.accept() 
    ```

- 发送/接收

    ```python
    # 返回发送字节数
    client.send("hello world！")

    # 接收8个字节数据
    client.recv(8)
    ```

- 关闭socket

    ```python
    client.close()
    ```

### 具体实现
#### 服务端
```python
class CameraServer: 
    def __init__(self, host = ("127.0.0.1", 1234)):
        """
            初始化
            @param host: ip及端口
        """
        self.host = host      
        self.setSocket(self.host)    
        self.mutex = threading.Lock()

    def setSocket(self, host):      
        """
            初始化socket，设置端口复用
            绑定host，开始监听
            @param host: ip及端口
        """
        self.socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)   
        self.socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR,1) self.socket.bind(self.host)      
        self.socket.listen(5)      
        print("Server running on port:%d" % host[1])

    def _processConnection(self, client, addr):     
        """
            客户端连接线程执行方法
            @param client 客户端socket
            @param addr 客户端ip信息
        """
        while(1):        
            info = struct.unpack("lhh",client.recv(8))        
            bufSize = info[0]        
            if bufSize:           
                 try:                  
                    self.mutex.acquire()                
                    self.buf=b''                
                    tempBuf=self.buf                
                    while(bufSize):                 
                        #循环读取到一张图片的长度
                        tempBuf = client.recv(bufSize)                    
                        bufSize -= len(tempBuf)                    
                        self.buf += tempBuf                
                        data = numpy.fromstring(self.buf,dtype='uint8')
                        self.image=cv2.imdecode(data,1)     
                        cv2.waitKey(10)    
                        cv2.imshow("from %s:%d"% (addr[0], addr[1]),self.image)            
                 except:                
                     print("接收失败")                
                     pass              
                 finally:                
                     self.mutex.release()               
                     if cv2.waitKey(10) == 27:                    
                         client.close()                    
                         cv2.destroyAllWindows()                    
                         print("放弃连接")                    
                         break

    def run(self):      
        """
            执行方法
        """
        while(1):        
            client,addr = self.socket.accept()        
            clientThread = threading.Thread(target = self._processConnection, 
                args = (client, addr, ))  
            #有客户端连接时产生新的线程进行处理                      
            clientThread.start()
```
#### 客户端
```python

class CameraClient:      
    def __init__(self, resolution = [640,480], remoteAddress = ("127.0.0.1", 1234)):     
        """
            初始化CameraClient
            @param resolution 传送分辨率
            @remoteAddress 服务端host
        """
        self.remoteAddress = remoteAddress        
        self.resolution = resolution                  
        self.path=os.getcwd()        
        self.img_quality = 15

    def _setSocket(self):      
        """
            初始化socket
        """
        self.client=socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
        self.client.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    def connect(self):      
        """
            连接server
        """
        self._setSocket()      
        self.client.connect(self.remoteAddress)  

    def _processConnection(self):  
        """
            视频发送线程方法
        """
        camera = cv2.VideoCapture(0)
        encode_param=[int(cv2.IMWRITE_JPEG_QUALITY), self.img_quality]       
        print("打开摄像头成功",file=f)    
        print("连接开始时间:%s"%time.strftime("%Y-%m-%d %H:%M:%S", 
                time.localtime(time.time())),file=f)    
        while(1):           
            (grabbed, self.img) = camera.read()  
            result, imgencode = cv2.imencode('.jpg',self.img,encode_param)        
            img_code = numpy.array(imgencode)        
            self.imgdata  = img_code.tostring()                   
            try:                    
                self.client.send(struct.pack("lhh",len(self.imgdata),
                        self.resolution[0],self.resolution[1])+self.imgdata) 
                        #发送图片信息(图片 长度,分辨率,图片内容)                
            except:            
                camera.release()                    
                return 

    def run(self): 
        clientThread = threading.Thread(target = self._processConnection)  
        clientThread.start()

```

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>