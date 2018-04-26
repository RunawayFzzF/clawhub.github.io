### unzip遇到的一些事儿

#### 背景

3个Tb的数据，每个压缩包一个Gb,SSD4T,现要求全部解压。

#### 策略

解压后立即删除原有zip包，以防止空间不足。

#### 尝试一  linux shell脚本实现 

##### 遇到的问题  

unzip命令会另外fork出线程去执行解压缩，以至于我捕捉不到解压是否完成，会出现还没等解压的时候，rm命令先执行，从而删除zip包，程序报错。

##### 解决办法 

查资料，问大神，折腾半天，最后转变思路，用JAVA实现。

#### 尝试二 java实现

commons-compress

##### 问题一 文件名中文乱码。

发现默认的是UTF8编码，尝试了多个编码方式，我一直以为GBK与GB2312是一样的，所以只试了GBK，后来同事帮助下，GB2312完美解决。

##### 问题二 windows与linux对于文件名称限制问题。

因为我是在window系统上跑程序，报错：java.nio.file.InvalidPathException: Illegal char <*>

###### Windows 中文件命名规则

> - 文件名或文件夹名可以由1～256个西文字符或128个汉字（包括空格）组成，不能多于256个字符。 
> - 文件名可以有扩展名，也可以没有。有些情况下系统会为文件自动添加扩展名。一般情况下，文件名与扩展名中间用符号“.”分隔。 
> - 文件名和文件夹名可以由字母、数字、汉字或~、!、@、#、$、%、^、&、( )、_、-、{}、’等组合而成。 
> - 可以有空格，可以有多于一个的圆点。 
> - 文件名或文件夹名中不能出现以下字符：\、/、:、*、?、"、<、>、| 。 
> - 不区分英文字母大小写。 

###### Linux 中文件命名规则

> - 除了/之外，所有字符都合法；
> - 特殊字符如@、#、￥、&、()、-、空格等最好不要使用，当使用空格作为文件名时，执行命令会出错；
> - 避免使用”.”作为文件名的第一个字符，因为在Linux系统中以”.”为开头的文件代表隐藏，系统将自动隐藏以”.”为开头的文件；
> - Linux系统区分大小写，因此文件命名也区分大小写；
> - Linux文件后缀名无意义，但是为方便识别应定义后缀(.txt、.php等)，定义后缀在大多数情况亦能将文件与目录区分；
> - 文件位置最好设置在Linux专用目录下，如配置文件大多时候放置于/etc目录下
> - 文件夹及文件的命名尽量聚有其特定的含义。
> - (8) 三个特殊目录，”.”：代表当前目录，”..”：代表上一级目录，”/”：代表根目录。

##### 问题三 zip解压顺序问题

zip包解压时，路径上的文件夹必须存在。

#### 核心代码

```java
public static List<String> unZip(File zipFile) throws Exception {
        ZipArchiveInputStream is = null;
        List<String> fileNames = new ArrayList<>();
        OutputStream os = null;
        Path path;
        try {
            is = new ZipArchiveInputStream(new BufferedInputStream(new FileInputStream(zipFile), BUFFER_SIZE), "GB2312");
            ZipArchiveEntry entry;
            while ((entry = is.getNextZipEntry()) != null) {
                path = Paths.get(zipFile.getParent() + entry.getName());
                path.getParent().toFile().mkdirs();
                fileNames.add(path.toString());
                try {
                    os = new BufferedOutputStream(new FileOutputStream(path.toFile()), BUFFER_SIZE);
                    IOUtils.copy(is, os, BUFFER_SIZE);
                } finally {
                    IOUtils.closeQuietly(os);
                }
            }
        } catch (Exception e) {
            throw e;
        } finally {
            IOUtils.closeQuietly(is);
        }

        return fileNames;
    }
```

