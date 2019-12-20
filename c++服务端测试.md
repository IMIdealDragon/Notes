在c++的世界里，程序设计的优雅让位于程序的稳定性、健壮性。“好程序是测出来的”这句话在C++领域里得到了充分体现。下面是我在开发中使用的测试方法，抛砖引玉，和大家交流下。
测试期间，关闭对core文件的限制，使用命令：ulimit -c unlimited
**（1）开发阶段，使用cppunit维护测试用例。**我一般是用于测试解析类、算法类。
从http://sourceforge.net/projects/cppunit/下载最新版本，解压，看安装文档，一般是./configure & make & make install。
下面举例说明我使用cppunit的方法。假设自己的源码位于src目录下，里面有class1.h/class1.cpp/class2.h/class2.cpp。相对src建立平级目录test存放测试工程，为class1/class2分别建立测试类文件testClass1.h/testClass2.h,建立main函数所在文件test.cpp、makefile。
testClass1.h代码如下,testClass2.h类似。



![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "class1.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include <iostream>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestRunner.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestResult.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestResultCollector.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/extensions/HelperMacros.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/BriefTestProgressListener.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/extensions/TestFactoryRegistry.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TextOutputter.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/CompilerOutputter.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestCaller.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)class testClass1:public CPPUNIT_NS::TestFixture
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedBlockStart.gif)![img](http://www.blogjava.net/Images/OutliningIndicators/ContractedBlock.gif){
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_TEST_SUITE(testClass1);
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_TEST(testCase1);
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_TEST(testCase2);
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_TEST_SUITE_END();
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  public:
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![img](http://www.blogjava.net/Images/OutliningIndicators/ContractedSubBlock.gif)    virtual void setUp(){}
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![img](http://www.blogjava.net/Images/OutliningIndicators/ContractedSubBlock.gif)    virtual void tearDown(){}
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)    void testCase1()
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![img](http://www.blogjava.net/Images/OutliningIndicators/ContractedSubBlock.gif)  {
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)      testClass1 a;
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)      a.oper..;
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)      CPPUNIT_ASSERT_EQAL(a.get..,![img](http://www.blogjava.net/Images/dot.gif));
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)    }
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)    void testCase2()
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![img](http://www.blogjava.net/Images/OutliningIndicators/ContractedSubBlock.gif)  {
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)      CPPUNIT_ASSERT(![img](http://www.blogjava.net/Images/dot.gif)==![img](http://www.blogjava.net/Images/dot.gif));
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)    }
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedBlockEnd.gif)};



test.cpp代码如下：

![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "testClass1.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "testClass2.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include <iostream>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestRunner.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestResult.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestResultCollector.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/extensions/HelperMacros.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/BriefTestProgressListener.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/extensions/TestFactoryRegistry.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TextOutputter.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/CompilerOutputter.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)#include "cppunit/TestCaller.h"
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif) CPPUNIT_TEST_SUITE_REGISTRATION(testClass1);
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif) CPPUNIT_TEST_SUITE_REGISTRATION(testClass1);
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)int main()
 {
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_NS::TestResult controller;
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_NS::TestResultCollector result;
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  controller.addListener( &result );    
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_NS::TestRunner runner;
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  runner.addTest( CPPUNIT_NS::TestFactoryRegistry::getRegistry().makeTest() );
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  runner.run( controller );
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  CPPUNIT_NS::CompilerOutputter out( &result, std::cout );
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  out.write();
![img](http://www.blogjava.net/Images/OutliningIndicators/InBlock.gif)  return 0;
![img](http://www.blogjava.net/Images/OutliningIndicators/ExpandedBlockEnd.gif)}


makefile文件如下:

![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)EXE=test
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)SRC=test.cpp
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)INC_PATH=-I ../src -I (cppunit头文件的目录) -I(依赖的其他头文件路径)
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)LIB_PATH=-L (cppunit动态库所在的目录) -L (依赖的其他库所在目录)
  LIB=-lcppunit -ldl ![img](http://www.blogjava.net/Images/dot.gif)
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)all:
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)  g++ $(SRC) $(LIB_PATH) $(LIB) $(INC_PATH) -o $(EXE)

再有新的需要测试类，增加相应的测试类，稍微修改下test.cpp即可(增加一句＃include,一句CPPUNIT_TEST_SUITE_REGISTRATION)。
保证开发结束后，解析类、算法类等不会有错误。
**（2）白盒测试阶段。
**这个基本是功能逻辑性测试，检测所有数据结构按要求变化以及保证各线程之间变化的一致性。这是最基本也是最全面的一次测试，保证测试的功能覆盖率100％。白盒测试期间可以在代码里加一些宏编译选项或者增加程序交互功能用于观察所有数据结构的变化。
保证测试完毕没有功能性、逻辑性的错误。
**（3）内存测试阶段。**使用valgrind检测显式内存泄漏、内存读写错误。
从http://www.valgrind.org/下载最新版本，解压，看安装文档，一般是./configure & make & make install。
检测内存一般使用命令valgrind --tool=memcheck -v --leak-check=full ./待测程序错误的地方会用==×××==（×××表示数字）标出。
使用一路模拟客户端做陪测。
保证测试完毕，单路客户端陪测的情况下没有任何的显式内存泄漏，没有任何的内存读写错误。
**（4）写批量客户端模拟程序。**建议熟悉一门方便socket编程的脚本语言，推荐perl。脚本语言简单，实现快速，特适合做陪测。
首先写一个能读取配置文件信息，按配置文件的要求向相应的server，按配置文件的流程发送信令的perl程序。
下面是我rtsp相关的一个server陪测的配置文件：

![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)ip=127.0.0.1
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)port=9115
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)url=rtsp://172.24.202.190:554/asset/service?USERID=320101312345670001&ChanelNo-PUID=0-320101000200000001&PlayMethod=0
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)<s,2>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)<p,2>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)<u,2>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)<p,2>
![img](http://www.blogjava.net/Images/OutliningIndicators/None.gif)<t,2>



其中ip是server IP,port是rtsp端口，url是发送信令带的url。<>表示按顺序发送的信令,这个配置文件表示先发送一个setup,然后sleep 2秒，再发送一个play，然后sleep 2秒，继续......这个程序可作为（3）中的陪测程序

在上面程序的基础上修改，读取配置文件后，死循环按顺序发送信令，假设该程序称做B。
写一个新的perl文件，完成如下功能，起几十路使用某配置文件的B程序，sleep几秒后，再起几十路使用其它配置文件的B程序.....,或者一起起也可以，自行设计,最后killall所有，从头循环运行。
总之尽可能的模拟客户端的所有行为，包括突然断联等，并且保证一定的压力。
**（5）压力下测试内存。**继续在valgrind下测试，使用（4）中的测试脚本做配测。
保证压力下无异常状态、无数据不一致状态、无显式内存泄漏、无内存读写异常
**（6）稳定性以及内存泄漏测试。
**陪测脚本起几百路客户端，保证主程序的cpu占用率在70％以上，持续运行20多小时。
测试期间，关注进程对内存的占用率，是保持在恒定水平还是随运行时间的增长而增长。
测试完毕，保证主程序负荷运行长时间而不宕机、没有内存泄漏发生。
**（7）代码覆盖率测试**。gcov
gcov是随gcc安装的，可以检查陪测程序对目标程序的代码覆盖情况。
不断修改测试脚本，保证测试尽量全面。代码被执行的次数也可以做为以后性能测试的一个参考。
**（8）性能测试。**gprof
同gcov一样，gprof也是随gcc安装的，它可以检测目标程序中所有函数的调用时间，并根据消耗时间排序，方便找出性能瓶颈。
找出系统的主要性能瓶颈，经过性能测试后，一般会发现影响系统的主要因素还是数据结构和算法。

测试期间，任何的coredump/任何的内存读写异常，务必处理掉。墨菲法则说，一个事情如果有可能变糟，事实则是会变的更糟。任何一个微小的、出现几率极小的bug，如果不在研发测试阶段解决，都可能造成以后更大代价的返工，甚至给客户的运营带来灾难。希望在每个人身上生效的都是马太效应，而不是墨菲法则。

以上都是我个人摸索的结果，没有参与过测试培训，也没有和其他同事交流过，因此可能有闭门造车的嫌疑，还请看这篇文章的高手们不吝赐教。