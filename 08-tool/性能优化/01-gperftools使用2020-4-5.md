https://gperftools.github.io/gperftools/cpuprofile.html



进程内存优化思路：
gperftools工具内存泄露分析实际链接库文件是libtcmalloc. so；
进程运行一段时间，生成 xxx.heap文件，
pprof利用xxx.heap文件生成的xxx.pdf文件。

xxx. pdf提供一下信息，进程占用内存的大小Total MB（top命令也可以获得），进程malloc申请内存过程以及大小。
内存优化，就是利用gperftool工具以及gdb获取