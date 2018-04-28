### FTPS碰到的事儿

#### 背景

传输数据方案设计时，把SFTP定为了传输工具，但是昨天突然说SFTP不安全，要变成FTPS。

#### SFTP为什么不安全

**SFTP**全称叫Secure File Transfer Protocol，客户端和服务器端都通过端口22传输和接收数据。SFTP 为 SSH的一部分，而**SSH**专为远程登录会话和其他网络服务提供安全性的协议。

![](/img/in-post/2018-04-28-sftp-ftps-java-linux/sftp.jpg)

又因为服务器是集群的，内部互联，所以不能对外开放这个SFTP。

#### FTPS是啥

就是FTP加一层外壳SSL/TSL。

**FTP**全称：File Transfer Protocol 是用于在网络上进行文件传输的一套标准协议。有主动和被动两种传输方式，服务器端和客户端个通过不同的端口传输命令以及数据。

##### FTP主动模式的传输过程：

![](/img/in-post/2018-04-28-sftp-ftps-java-linux/active_ftp.png)

##### FTP被动模式的传输过程：

![](/img/in-post/2018-04-28-sftp-ftps-java-linux/passive_ftp.png)

**SSL(**Secure Sockets Layer 安全套接层),及其继任者传输层安全（Transport Layer Security，TLS）是为网络通信提供安全及数据完整性的一种安全协议。TLS与SSL在传输层对网络连接进行加密。

SSL/TLS协议在传输层（TCP/IP）之上、但是在应用层之下工作的。因此，它可以很容易在诸如HTTP，Telnet，POP3，IMAP4，SMTP和FTP等应用层协议上实现。SSL安全扩展至少有两种不同的初始化方法：显式安全和隐式安全。 
显示安全:为了建立SSL连接，显式安全要求FTP客户端在和FTP服务器建立连接后发送一个特定的命令给FTP服务器。客户端使用服务器的缺省端口。 
隐式安全: 当FTP客户端连接到FTP服务器时，隐式安全将会自动和SSL连接一起开始运行。在隐式安全中服务器定义了一个特定的端口（**TCP**端口**990**）让客户端来和其建立安全连接。

**FTPS**是在安全套接层使用标准的FTP协议和指令的一种增强型FTP协议，为FTP协议和数据通道增加了SSL安全功能。FTPS也称作“FTP-SSL”和“FTP-over-SSL”。

![](/img/in-post/2018-04-28-sftp-ftps-java-linux/ftps.jpg)

#### 核心代码

##### 建立连接

```java
   public static boolean connectFTP(FtpConfig ftpConfig) {
        boolean flag = false;

        try {
            if (null != ftpConfig) {
                ftpClient = new FTPClient();
                ftpClient.connect(ftpConfig.getServerName(), ftpConfig.getProt());
                ftpClient.login(ftpConfig.getUserName(), ftpConfig.getPassword());
                ftpClient.changeWorkingDirectory("/");
                ftpClient.setRemoteVerificationEnabled(false);
                if (ftpConfig.getMode().equalsIgnoreCase("active")) {
                    ftpClient.enterLocalActiveMode();
                } else {
                    ftpClient.enterLocalPassiveMode();
                }
                flag = true;
            } else {
                flag = false;
            }
        } catch (SocketException var3) {
            flag = false;
            logger.error(var3.getMessage());
        } catch (IOException var4) {
            flag = false;
            logger.error(var4.getMessage());
        }

        return flag;
    }
```

##### 发送数据

```java
public static String uploadFile(String localPath, String remotePath, String targetFileName) {
        String statusCode = null;
        InputStream fis = null;

        try {
            File srcFile = new File(localPath);
            fis = new BufferedInputStream(new FileInputStream(srcFile), bufferSize);
            ftpClient.changeWorkingDirectory(remotePath);
            ftpClient.setBufferSize(bufferSize);
            ftpClient.setControlEncoding("UTF-8");
            ftpClient.setFileType(2);
            ftpClient.storeFile(targetFileName, fis);
            statusCode = ftpClient.getStatus();
        } catch (IOException var9) {
            throw new RuntimeException("FTP客户端出错！", var9);
        } finally {
            if (null != fis) {
                ftpClose(fis);
            }

        }

        return statusCode;
    }
```

##### 关闭连接

```java
private static void ftpClose(InputStream fis) {
        try {
            fis.close();
            if (null != ftpClient && ftpClient.isConnected()) {
                ftpClient.logout();
                ftpClient.disconnect();
            }
        } catch (IOException var2) {
            logger.error("close ftpclient error");
        }

    }
```

