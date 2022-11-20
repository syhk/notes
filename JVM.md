
JVM 和 java 没有关系！

如果其他语言生成 .class 文件，也可以让 jvm 跑

eg: c   lua   php  python  ruby  javaScript  Lisp 等 100 多种语言

![image_zUqyqlN7Qx.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663899687201-780e20c0-5d1d-4e85-870b-631a26fd8d8c.png#clientId=u5199040b-4137-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=ude1eea37&name=image_zUqyqlN7Qx.png&originHeight=491&originWidth=851&originalType=binary&ratio=1&rotation=0&showTitle=false&size=451400&status=done&style=none&taskId=u4c1c6ca8-9900-48e1-9950-9bf3f60a161&title=)

![image_LOB0882lEn.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663899695144-3bb86e23-10dd-4b9e-9371-56f50f1903ff.png#clientId=u5199040b-4137-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=ub81be52b&name=image_LOB0882lEn.png&originHeight=441&originWidth=566&originalType=binary&ratio=1&rotation=0&showTitle=false&size=281300&status=done&style=none&taskId=u6c7e2b17-4415-4131-a7b5-dd3d8067049&title=)

### java 的类加载过程
5个阶段：载入，验证，准备，解析，初始化
整个生命周期为：加载，验证，准备，解析，初始化，使用，卸载
`xxd`命令可以查看 .class 的文件内容
每个 Class 文件的头 4 个字节称为 魔数 （它的作用是：确定这个文件是否是一个能被虚拟机接受的 class 文件）
验证阶段：文件格式验证，元数据验证，字节码验证，符号引用验证

`元空间`: 原来的方法区（永久代）被元空间取代。





常见的 JVM 实现：

Hotspot

快，非常快：最新垃圾回收的业界标杆


openJDK： 是 hotpot 的开源版本

![image_qMghgBBDhO.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663899705603-a1782959-a62a-4c25-9233-5414019e0049.png#clientId=u5199040b-4137-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=ub4fedf0f&name=image_qMghgBBDhO.png&originHeight=341&originWidth=529&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92937&status=done&style=none&taskId=u31361138-7b60-4600-bc87-8c9ace80b60&title=)

![image_jTeaR4uvvL.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663899714160-fa948237-8352-40ae-a063-1c771412a4a5.png#clientId=u5199040b-4137-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u6f586e3c&name=image_jTeaR4uvvL.png&originHeight=142&originWidth=883&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32921&status=done&style=none&taskId=u93a43c63-d80b-439f-b398-b2cb423e5cb&title=)

```java
public class FinalizeEscapeGC{

    public static FinalizeEscapeGC SAVE_HOOK = null;

    public void isAlive(){
        System.out.println("yes, i am still alive");
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("finalize method executed!");
        FinalizeEscapeGC.SAVE_HOOK = this;
    }

    public static void main(String[] ages) throws Throwable{

        SAVE_HOOK  =  new FinalizeEscapeGC();

        //对象第一次成功拯救自己
        SAVE_HOOK = null;
        System.gc();//回收
        //finalize 方法优先级很低， 暂停一下等待它
        Thread.sleep(500);
        if (SAVE_HOOK  != null )
            SAVE_HOOK.isAlive();
        else
            System.out.println("no , i am dead:");
        //这代码和上面的一样但是却自救失败了
        SAVE_HOOK = null;
        System.gc();

        Thread.sleep(500);
        if (SAVE_HOOK != null)
            SAVE_HOOK.isAlive();
        else
            System.out.println("no, i am dead:");
        //原因： 任何一个对象的 finalize() 方法都只会被系统自动调用一次，所 以第二次自救失败了

    }
}
```
finalize() 已经被官方声明为不推荐的语法。

#### 判断无用的类三个条件：

- 该类的所有实例都已经被回收（java 堆中不存在该类及其任何派生子类的实例）
- 加载该为的类加载器 ClassLoader 已经被回收
- 该类的对应的 java.lang.Class 对象没有要任何地方被引用，无法在任何地方通过反射访问该类的方法

垃圾收集器中的并发和并行：
并行：指多条垃圾收集线程并行工作，但此时用户线和仍然处于等待状态
并发：指用户线程与垃圾收集线各同时执行（但不一定是并行的，可以会交替执行），用户程序 在继续运行，而垃圾收集程序 运行于另一个 CPU 上。

java 可以把对象分配栈上。


#### 对象的内存布局
在 HotSpot 虚拟机中，对象在内存中存储的布局可以分为 3 块区域： 对象头，实例数据，对齐填充
对象头包含两部分信息：

- 第一部分用于存储对象自身的运行时数据，eg: HashCode , GC 分代年龄，锁状态标志，线程持有的锁，偏向线程 ID，偏向时间戳
- 第二部分是类型指针，对象指向它的类元数据指针，虚拟机通过这个指针来确定这个对象是哪个类的实例

HotSpot VM 的自动内存管理系统要求对象起始地址必须是 8 字节的整数倍（也就是对象的大小必须 8 字节的整数倍）如果没有对齐，就会对齐填充来补全
对象的访问定位 ：对象访问方式取决一虚拟机实现的：主流的有**句柄**和**直接指针**两种：

#### GC 相关：
STW : 世界停止 ()  stop-the-world
清理时：单线程清理，多线程清理
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663999755537-2fd99347-c654-41c3-8fdc-5d7c85df6a42.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=278&id=u557b8a37&name=image.png&originHeight=347&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=282969&status=done&style=none&taskId=u56155b98-0a60-49da-9872-1a051ec8027&title=&width=580)
虚线的清除算法是可以相互配合使用的。
分代算法：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663999891556-763e6e24-a9a1-4a79-b8a8-8f755ac52ca8.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=239&id=udb844071&name=image.png&originHeight=299&originWidth=646&originalType=binary&ratio=1&rotation=0&showTitle=false&size=177016&status=done&style=none&taskId=u432e4faf-b87d-4da7-8e39-08a4b964f48&title=&width=516.8)
新生对象先放到 `eden`区 ，第一次扫描后活下来的对象，复制到  survivor1 区；然后下一次扫描连着 eden 区和 survivor1 区一起扫描， survicor1 区活着的又放到 survivor2 区，（年龄够了就会放到老年代）最后活过一定年龄后放到老年代。
survivor 区放不下直接放到 老年代区 
年龄成长到 16 岁（也就是 15 岁就会被送往）的时候也会送去老年代
大对象直接存放到老年代区
老年代满了就会触发 FULLGC 
进入老年代的条件：

- 大对象
- 长期存活对象
- 动态对象年龄（如果 Survivor 空间中相同年龄的所有对象大小的总合大于 Survivoe 空间的一半，年龄大于等于该年龄对象就可以直接进入老年区）

可达必分析：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664012255022-8d241a6d-64e8-4b43-96c3-9d103b0bd622.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uc638d46d&name=image.png&originHeight=437&originWidth=619&originalType=url&ratio=1&rotation=0&showTitle=false&size=29455&status=done&style=none&taskId=uadc359b4-cb4b-46f4-8e61-051453070ec&title=)
#### Serial （单线程 STW 垃圾回收）年青代和老年代都一样
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664000490930-2c8fb450-ed06-4e23-bc78-8a5378f7c930.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=258&id=ua74a70f7&name=image.png&originHeight=323&originWidth=612&originalType=binary&ratio=1&rotation=0&showTitle=false&size=233423&status=done&style=none&taskId=u7f0ce2ee-a2d3-4527-8803-527867057f8&title=&width=489.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664012592469-599f3d78-90fb-4f43-b055-092554ecd76d.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8162b800&name=image.png&originHeight=267&originWidth=756&originalType=url&ratio=1&rotation=0&showTitle=false&size=23174&status=done&style=none&taskId=ue8af784f-c3a4-4a7d-b7f7-8f0a86e8464&title=)
#### Parallel 多线程回收 
工作在年青代叫： Parallelnew   Scavenge  简称 PS
工作在老年代叫 : Parallel Old   简称 PO

#### ParNew 
它是 Serial 收集器的多线程版本，可以使用多条线程进行垃圾回收：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664012629750-969eee59-9387-486e-9006-601127778bbe.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u63887e85&name=image.png&originHeight=263&originWidth=754&originalType=url&ratio=1&rotation=0&showTitle=false&size=25253&status=done&style=none&taskId=u19b90f8a-41c2-446e-9fe9-0ab7b2cdaaf&title=)


### CMS （并发标记清除）
它工作在老年代，看上面图中：与它配合的在年轻代的有： ParNew  , Serial 
初始标记，并发标记，重新标记，并发清理
并发标记是发生标记错误，两个地方会发生 `STW` ：初始标记（只标记根结点），重新标记
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664001578622-503dbe40-bda1-4791-83e6-41f0ef6c5d89.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=166&id=u942feaa6&name=image.png&originHeight=208&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=117937&status=done&style=none&taskId=u2d484835-49c0-4eaa-8d1b-609e9560ed9&title=&width=405.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664012666428-886e1a24-584a-4b75-9ff3-e11bf50f843a.png#clientId=ub1bc307a-02d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u2faca285&name=image.png&originHeight=276&originWidth=845&originalType=url&ratio=1&rotation=0&showTitle=false&size=24091&status=done&style=none&taskId=u06cc9f96-aa63-4c21-becb-31baeaa1b56&title=)
CMS 漏标 bug ，它采用的解决方法有 BUG（G1 采用的解决方法没有BUG）
CMS 一旦产生 STW 时间非常长，也就是 CMS 的 remark 阶段（也就是重头扫描一次）

#### 三色标记算法



#### Epsilon （不做清理，只做记录）
> 开发 JVM  的人做 DGBUG 用的








标量替换：
标题就是不可被进一步分解的量，基本类型就是标量 eg: int  long 
聚合量：可以被进一步分解的量， java 中的对象
通过逃逸分析确定该对象不会被外部访问，并且对象可以被进一步分解时，JVM 不会创建对象，而会将对象的成员变量分解为若干个变量来代替，代替的成员变量在栈帧或寄存器上分配空间。
> 通过 -XX:+EliminateAllocations 可以开启标量替换，  -XX:+PrintEliminateAllocations 查看标量替换情况




逃逸分析： 是编译语言中的一种优化分析，不是优化手段，通过对象的作用范围分析，为其他优化手段提供分析数据从而进行优化

### java G1 收集器 （Garbage First)
这里讲的非常好：官方
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663944632239-65367318-e667-4be6-8b4e-0c124c6390a1.png#clientId=uf610bbf2-a9e8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uac4762d3&name=image.png&originHeight=720&originWidth=960&originalType=url&ratio=1&rotation=0&showTitle=false&size=34237&status=done&style=none&taskId=u4328fd91-ad9f-4d8e-8d4b-1324a78fac3&title=)
（region）分区算法：
**物理上不分代；逻辑上分代**









### ZGC (orcale)  The Z  Garbage Collector
ZGC 采用标记-复制算法
![](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664021365933-43847aa7-03e6-406d-a381-a87a3c633960.png#clientId=u93dd5fd4-d0c8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u3911abc2&originHeight=560&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud40f5c6b-0bb2-4fc7-a1c6-431b0c2be94&title=)


### Shenandoah （radhat)



`jps`命令： 列出整个系统中 java  的所有进程
`jinfo`:  查看详细信息 跟上进程号 配置信息工具
`jstat` :   -gc 进程号 虚拟机统计信息监视
`jstack` : java 堆栈跟踪工具
`jmap`:  java 内存映像工具2
`jhat`: 虚拟机堆转存储快照分析工具




编译 JVM 出现的问题解决办法：
安装相关依赖库

配置检查没问题后：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663994316334-8d7fad27-a96b-490f-90c0-cc7264024812.png#clientId=u1804bdaf-3f40-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=604&id=ua7ab890a&name=image.png&originHeight=755&originWidth=951&originalType=binary&ratio=1&rotation=0&showTitle=false&size=216504&status=done&style=none&taskId=ued47b88f-9663-4e76-8ebb-532c846fcb9&title=&width=760.8)make images  
时间挺长的，慢慢等！！！
完成：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663994915261-235c3a1b-933a-4a75-8345-19464e685160.png#clientId=u1804bdaf-3f40-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=242&id=u34fdec97&name=image.png&originHeight=302&originWidth=959&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94042&status=done&style=none&taskId=u41cf2bf9-df44-4f7d-bb2f-70f6e6549e3&title=&width=767.2)
测试：` ./build/linux-x86_64-server-release/jdk/bin/java --version`
正常编译测试：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663995070727-c0cddf15-39c8-41d0-a937-9ef5233876a6.png#clientId=u1804bdaf-3f40-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=170&id=uc1b9f574&name=image.png&originHeight=213&originWidth=942&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26833&status=done&style=none&taskId=u9524399c-038d-4ec8-a10c-a46b689eb61&title=&width=753.6)















源码下载：

