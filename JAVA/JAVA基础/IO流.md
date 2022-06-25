# IO流

## File类

### file类的用法

可以使用file类实例化出一个文件对象或者目录对象。

实例：

虽然实例化file类对象已经指定了文件或者目录的路径，但是此时硬盘上还没有这些file文件，这些file只是内存中的file对象。

```java
public class testFile {
    @Test
    public void test(){
        //构造器一：创建文件对象
        //相对路径，idea的@Test的相对路径在Module路径下
        File file1 = new File("hello1.txt");
        //绝对路径，包括盘符直到Module路径。
        File file2 = new File("D:\\Java\\IdeaProjects\\java基础2\\src\\测试\\hello2.txt");
        System.out.println(file1);
        System.out.println(file2);
        //构造器二：创建目录对象
        File file3 = new File("D:\\Java\\IdeaProjects","java基础2");
        System.out.println(file3);
        //构造器三：在在指定目录对象下创建文件对象
        File file4 = new File(file3,"hello3.txt");
        System.out.println(file4);
    }

}
```

### File的一些方法使用

```java
   @Test
    public void test02(){
        File file1 = new File("hello.txt");
        File file2 = new File("D:\\IOTest\\hi.txt");
        System.out.println(file1.getAbsoluteFile());
        System.out.println(file1.getPath());
        System.out.println(file1.getName());
        System.out.println(file1.getParent());
        System.out.println(file1.length());
        System.out.println(new Date(file1.lastModified()));

        System.out.println();

        System.out.println(file2.getAbsoluteFile());
        System.out.println(file2.getPath());
        System.out.println(file2.getName());
        System.out.println(file2.getParent());
        System.out.println(file2.length());
        System.out.println(new Date(file2.lastModified()));
    }
	//文件重命名
    @Test
    public void renameTest(){
        File file1 = new File("hello.txt");
        File file2 = new File("D:\\IOTest\\hi.txt");
        System.out.println(file1.renameTo(file2));
    }

}
```

hello.txt

![image-20220414220805028](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220414220805028.png)

**输出两个文件的信息结果：**

因为hello.txt这个文件存在于硬盘中，所以可以通过file1获取hello.txt的信息

而h1.txt这个文件不存在，所以只能通过file2对象获取对象路径信息

![image-20220414220712053](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220414220712053.png)

**文件重命名测试结果：**

结果返回true

hello.txt文件被成功移动到D:\\IOTest\\hi.txt目录下，并将文件名改为hi.txt。

重命名成功条件：即将要重命名的文件必须存在，要改的成的新名的文件不存在

![image-20220414221048490](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220414221048490.png)

![image-20220414221113346](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220414221113346.png)

#### 创建文件或者目录

以下使用file对象在硬盘上创建目录或文件

```java
@Test
public void test03() throws IOException {
    File file1 = new File("hello.txt");
    //当hello.txt文件不存在时createNewFile()将创建这个文件，存在则不创建
    if(!file1.exists()){
        file1.createNewFile();
        System.out.println("文件创建成功");
        //当hello.txt文件存在时delete()将删除文件，不存在则不删除
    }else {
        file1.delete();
        System.out.println("文件删除成功");
    }
    //当IOTest\\IO01目录存在时，mkdir()方法才能创建01目录
    File file2 = new File("D:\\IOTest\\IO01\\01");
    boolean mkdir1 = file2.mkdir();
    if(mkdir1){
        System.out.println("01目录创建成功");
    }
    //即使IOTest\\IO02不存在，mkdirs()方法会将\\IOTest\\IO02\\02三层目录都创建出来。
    File file3 = new File("D:\\IOTest\\IO02\\02");
    boolean mkdir2 = file3.mkdirs();
    if(mkdir2){
        System.out.println("02目录创建成功");
    }
}
```

## Java IO

IO有输入和输出操作，这两个操作的参考点都是内存。

**输入input：将外部数据读取到内存中。**

**输出output：将内存里的数据写到磁盘等存储设备存储起来。**

#### 流的分类

**按照数据单位来分：**

分为字节流和字符流

**字节流：InputStream、OutputStream**

**字符流：Reader、Writer**

上面四个API都是抽象类。

字节流的单位是Byte，字符流的单位是char

字符流适用于传输文本文件，字节流使用于传输图片或者视频等非文本文件。

**按照按流向来分，分为输入流和输出流。**

按照角色来分，分为节点流和处理流。

处理流是对节点流的包装，节点流是最基本的操作流的API。相当于水管和水流的关系。

![image-20220416164105003](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220416164105003.png)

### 流的体系结构

**对应关系：**

**节点流是直接操作文件的API**

抽象基类：				节点流（文件流）				缓冲流（处理流中的一种）

InputStream 			FileInputStream					BufferedInputStream

OutputStream		  FileOutputStream				BufferedputStream	

Reader						FileReader							BuffereReader

Writer						  FileWriter							  BuffereWriter			

### FileReader

FileReader是读出文件数据的节点流。

**实例：**

```java
@Test
public void test1(){
    FileReader fr = null;
    try {
        //实例化文件对象，指明要操作哪个文件
        File file1 = new File("hello.txt");
        //提供具体的节点流实现流操作，确保文件存在，不然会抛出文件不存在的运行时异常。
        fr = new FileReader(file1);
        int data;
        //一个字符一个字符的读出，读到end返回-1，表示读出结束。
        while ((data = fr.read())!=-1){
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {//关闭流
            if(fr != null){
            fr.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**运行结果：**

![image-20220416184020440](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220416184020440.png)

#### read的重载方法

read()空参方法每次只能读取一个字符，很麻烦，可以使用它的重载方法，一次读取一个字符串大小的字符。

**实例：**
	read(buf)方法每次先读取5个字符(没到末尾)，下一次读取在buf数组覆盖原来5个字符，当后面只剩下3个字符字符时，只会将前面三个字符覆盖,所以在写循环体时，不能将固定的字符串数组长度作为遍历字符的长度。

​	**read(buf)方法，会返回每次新读取的字符个数len**。

​	可以将len作为循环遍历的次数。

**hello.txt文件**

![image-20220416205037987](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220416205037987.png)

**测试用例：**

```java
@Test
public void test02(){
    FileReader fr = null;
    try {
        File file = new File("hello.txt");
        fr = new FileReader(file);
        char [] buf = new char[5];
        int len;//每次读入数组的字符个数
        while ((len = fr.read(buf))!=-1){
            //方式一：
            //错误写法i<buf.length
            for(int i=0;i<len;i++){
                System.out.print(buf[i]);
            }
            //方式二:
            //错误写法：
            String s = new String(buf);
            System.out.print(s);
            //正确写法
            String s = new String(buf,0,len);
            System.out.print(s);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (fr !=null){
                fr.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### FileWriter

FileWriter是写文件的字节流

如果要写数据的文件不存在，write方法会创建一个文件，并写上数据。

如果如果要写数据的文件存在，默认会将新写文件覆盖原文件。

FileWriter(文件名,是否进行写追加)，该构造器方法是否进行写追加默认为false，即不进行写追加，将文件覆盖。

如要追加则改为true。

```java
 @Test
    public void test04() {
        FileWriter fw = null;
        try {
            File file = new File("hello1.txt");
            //创建字节流，设置写追加。
            fw = new FileWriter(file,true);
            //调用写方法
            fw.write("I am jack\n");
            fw.write("I am a student");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(fw!=null){
                fw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

#### 使用FileWriter和FileRead实现文件复制

```java
@Test
public void test05() {
    FileReader fr = null;
    FileWriter fw = null;
    try {
        File file1 = new File("hello.txt");
        File file2 = new File("hello2.txt");
        fr = new FileReader(file1);
        fw = new FileWriter(file2);
        char[] buf = new char[5];
        int len;
        while ((len = fr.read(buf)) != -1){
            fw.write(buf,0,len);//将字符以数组的形式读入hello2.txt文件，每次写入文件的字符长度就是读字符的长度
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if(fw!=null){
                fw.close();             
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (fr!=null){
            fr.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 字节流和字符流的使用情景

​		字节流也可以操作文本文件，一个字节（8位二进制）可以存一个字母或者数组。但是在UTF-8中，一个汉字需要三个字节来保存，如果使用字节流读入字符的数组的大小不是3的倍数，在控制台输出文本内容会出现中文乱码问题，**但是只是对文本文件进行复制操作(读写)，不进行控制台打印，使用字节流还是可以的**。所以字符流处理文本文件更合适。

​		结论：

对于文本文件(.txt,.java,.c,.cpp)这些文件使用字符流处理

对应非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt等)这些文件使用字节流读取

### 使用字符流处理非文本文件

**如果使用流复制较大的文件，则存放字节的数组大小一般设置为1024**

测试实例：

```java
@Test
public void test06()  {
    FileInputStream fi = null;
    FileOutputStream fo = null;
    try {
        File file1 = new File("荷花.jpg");
        File file2 = new File("荷花2.jpg");
        fi = new FileInputStream(file1);
        fo = new FileOutputStream(file2);
        byte[] buff = new byte[5];
        int len;
        while ((len =fi.read(buff)) != -1){
            fo.write(buff,0,len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (fi!=null)
            fi.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (fo!=null)
            fo.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

测试结果：

成功将复制图片

![image-20220416221434320](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220416221434320.png)

## 缓冲流

#### 缓冲流的作用：
**加快流的读取，写入的速度**

**原理：缓冲流内部使用了缓冲区，先将文件读入缓冲区如果数据超过8kb自动调用flush方法清理缓冲区，将缓冲区的数据一次性写入文件。**

**flush方法是在输出流的方法**

普通字节流是读取几个字节的数据然后写入几个字节的数据。

**测试加上缓冲流的字节流和普通字节流复制文件的快慢**

```java
//加上缓冲流
public void BufferedTest1(String f1,String f2){
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            File file1 = new File(f1);
            File file2 = new File(f2);
            FileInputStream fis = new FileInputStream(file1);
            FileOutputStream fos = new FileOutputStream(file2);
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);
            byte[] buff = new byte[1024];
            int len;
            while ((len = bis.read(buff))!=-1){
                bos.write(buff,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(bos!=null)
                    bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(bis!=null)
                    bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
	//普通的字节流
    public void FileIOTest(String f1,String f2){
        FileInputStream fi = null;
        FileOutputStream fo = null;
        try {
            File file1 = new File(f1);
            File file2 = new File(f2);
            fi = new FileInputStream(file1);
            fo = new FileOutputStream(file2);
            byte[] buff = new byte[1024];
            int len;
            while ((len =fi.read(buff)) != -1){
                fo.write(buff,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fi!=null)
                    fi.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (fo!=null)
                    fo.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    @Test
    public void test1(){
        //加上缓冲流的字节流复制视频（182M），只需要0.6秒
//        long start = System.currentTimeMillis();
//        String f1 = "C:\\Users\\鹤\\Desktop\\01-视频.avi";
//        String f2 = "C:\\Users\\鹤\\Desktop\\02-视频.avi";
//        BufferedTest bs = new BufferedTest();
//        bs.BufferedTest1(f1,f2);
//        long end = System.currentTimeMillis();
//        System.out.println(end-start);//664
		//普通字节流复制视频（182M）,需要3秒多
        long start = System.currentTimeMillis();
        String f1 = "C:\\Users\\鹤\\Desktop\\01-视频.avi";
        String f2 = "C:\\Users\\鹤\\Desktop\\02-视频.avi";
        BufferedTest bs = new BufferedTest();//3354
        bs.FileIOTest(f1,f2);
        long end = System.currentTimeMillis();
        System.out.println(end-start);
    }
```

### 缓冲流读写文本

使用缓冲流读取文本也能提高读写速度

缓冲流读取文本可以一行一行的读取（不包括换行符），所以在写的时候在每一行数据（字符串）后面加上换行符，或者调用newLine()方法换行。

```java
    @Test
    public void test2(){
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            File file1 = new File("test.txt");
            File file2 = new File("test2.txt");
            FileReader fr = new FileReader(file1);
            FileWriter fw = new FileWriter(file2);
            br = new BufferedReader(fr);
            bw = new BufferedWriter(fw);
            //方式一：旧方法
//            int len;
//            char[] buff = new char[5];
//            while((len = br.read(buff))!=-1){
//                bw.write(buff,0,len);
//            }
            //方式二：
            String data;
            while ((data =br.readLine())!=null){
//                bw.write(data+'\n');
                //或者
                bw.write(data);
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(br!=null)
                br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(bw!=null)
                bw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

## InputStreamReader转换流的使用

InputStreamReader是一个转换流，转换流是处理流的一种。

​	InputStreamReader是将字节流读取文件转换成字符流读取文件，字节流边读取文件边打印到控制台会出现乱码情况，使用InputStreamReader将转换成字符流，就不会有这种情况了。

**InputStreamReader的作用是将字节解码成字符。**

### 代码实例

```java
@Test
public void test() throws IOException {
    File file = new File("test.txt");
    FileInputStream fis = new FileInputStream(file);
    InputStreamReader isr = new InputStreamReader(fis);//构造方法不传字符格式默认使用UTF-8
    char[] buf = new char[10];
    int len;
    while ((len = isr.read(buf))!=-1){
        String s = new String(buf,0,len);
        System.out.println(s);
    }
    fis.close();
    isr.close();
}
```

测试结果：

![image-20220419213155931](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220419213155931.png)

### 转换流实现文件的读写

代码示例：

```java
@Test
public void test() throws IOException {
    File file1 = new File("test.txt");
    File file2 = new File("test2.txt");
    FileInputStream fis = new FileInputStream(file1);
    FileOutputStream fos = new FileOutputStream(file2);
    InputStreamReader isr = new InputStreamReader(fis,"UTF-8");
    OutputStreamWriter osw = new OutputStreamWriter(fos,"GBK");
    char[] buf = new char[10];
    int len;
    while ((len = isr.read(buf))!=-1){
        osw.write(buf,0,len);
    }
    isr.close();
    osw.close();
}
```

测试结果：成功将文件1读出，在以不同的编码方式写入文件2

![image-20220419221729014](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220419221729014.png)

![image-20220419221703238](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220419221703238.png)

### 字符编码集

![image-20220419221139476](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220419221139476.png)