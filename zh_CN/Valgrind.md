# 使用Valgrind对项目进行内存泄漏检查

**1.首先安装Valgrind工具：**  
    sudo apt install valgrind  

**2.使用Valgrind检查对应的程序：**  
    valgrind --leak-check=yes --trace-children=yes --show-reachable=yes --log-file=log program args  
   以文件管理器为例：  
    valgrind --leak-check=yes --trace-children=yes --show-reachable=yes --log-file=peony.log peony  
   该命令会启动文件管理器，并将检查的结果输出到文件：peony.log  

**3.程序测试：**  
   在启动的程序上尽量把每个模块都用鼠标点击一遍，程序运行过程中valgrind会不断对程序进行检查，并将检查结果输出到日志。  

**4.对日志文件进行分析**  
   先翻到日志底部看总结：  
    ==5009== LEAK SUMMARY:  
    ==5009==    definitely lost: 23,660 bytes in 97 blocks  
    ==5009==    indirectly lost: 89,779 bytes in 943 blocks  
    ==5009==    possibly lost: 29,171 bytes in 463 blocks  
    ==5009==    still reachable: 48,951,994 bytes in 442,445 blocks  
    ==5009==    of which reachable via heuristic:  
    ==5009==    length64 : 3,232 bytes in 55 blocks  
    ==5009==    newarray : 2,088 bytes in 46 blocks  
    ==5009==    multipleinheritance: 73,624 bytes in 425 blocks  
    ==5009==    suppressed: 0 bytes in 0 blocks  
    ==5009==    Reachable blocks (those to which a pointer was found) are not shown.  
    ==5009== To see them, rerun with: --leak-check=full --show-leak-kinds=all  
    ==5009== Use --track-origins=yes to see where uninitialised values come from  
    ==5009== For lists of detected and suppressed errors, rerun with: -s  
    ==5009== ERROR SUMMARY: 6313747 errors from 750 contexts (suppressed: 2 from 2)  

   definitely lost 的部分是可以肯定的内存泄漏，可以在日志中查找对应的地方，会提示到项目中具体哪个类的哪个函数。  
   indirectly lost 是间接泄漏的地方，也是需要关注的.  
   possibly lost只是可能存在的泄漏，提示的地方比较多，有时间可以关注一下.  

**5. 查看valgrind详细用法**  
     valgrind -h  

