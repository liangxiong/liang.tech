# ant

ant -f build.xml targetName

http://chenjia66804610.iteye.com/blog/421370


<?xml version="1.0"?>
<project name="helloWorld">
       <target name="sayHelloWorld">
              <echo message="Hello,Amigo"/>
       </target>
</project>

1.     1.   project元素
   1）name属性
用于指定project元素的名称。

2）default属性

用于指定project默认执行时所执行的target的名称。

      3）basedir属性

2.       target元素
1）name属性

指定target元素的名称，这个属性在一个project元素中是唯一的。我们可以通过指定target元素的名称来指定某个target。

2）depends属性

用于描述target之间的依赖关系，若与多个target存在依赖关系时，需要以“,”间隔。Ant会依照depends属性中target出现的顺序依次执行每个target。被依赖的target会先执行。

3）if属性

用于验证指定的属性是否存在，若不存在，所在target将不会被执行。

4）unless属性

该属性的功能与if属性的功能正好相反，它也用于验证指定的属性是否存在，若不存在，所在target将会被执行。

5）description属性

该属性是关于target功能的简短描述和说明。


3. Ant的常用任务
     1）. copy任务

Eg1.复制单个文件：<copy file="file.txt" tofile="copy.txt"/>

Eg2.对文件目录进行复制：

   <copy todir="../newdir/dest_dir">

            <fileset dir="src_dir"/>

 </copy>

Eg3. 将文件复制到另外的目录：

        <copy file="file.txt" todir="../other/dir"/>

<fileset dir="${build.src}">  
                <include name="**/*.properties"/>  
            </fileset>  
     2）.delete任务

对文件或目录进行删除，举例如下：

Eg1. 删除某个文件：<delete file="photo/amigo.jpg"/>

Eg2. 删除某个目录：<delete dir="photo"/>

Eg3. 删除所有的备份目录或空目录：
        <delete includeEmptyDirs="true">
               <fileset dir="." includes="**/*.bak"/>
        </delete>

     3）.mkdir任务

创建目录。eg：<mkdir dir="build"/>

     4）.move任务

移动文件或目录，举例如下：

Eg1. 移动单个文件：<move file="fromfile" tofile="tofile"/>

Eg2. 移动单个文件到另一个目录：<move file="fromfile" todir=”movedir”/>

Eg3. 移动某个目录到另一个目录：

        <move todir="newdir">

               <fileset dir="olddir"/>

        </move>

     5）. echo任务

该任务的作用是根据日志或监控器的级别输出信息。它包括message、file、append和level四个属性，举例如下：

<echo message="Hello,Amigo" file="logs/system.log" append="true">



Ant还提供了一些它自己的内置属性
basedir：project基目录的绝对路径，该属性在讲解project元素时有详细说明，不再赘述；

ant.file：buildfile的绝对路径，如上面的各例子中，ant.file的值为E:"build.xml；

ant.version：Ant的版本，在本文中，值为1.7.0；

ant.project.name：当前指定的project的名字，即前文说到的project的name属性的值；

ant.java.version：Ant检测到的JDK的版本，在上例运行结果中可看到为1.5。

高级功能：
http://blog.csdn.net/hsliwei/article/details/7773881

来源： <https://app.yinxiang.com/Home.action>
