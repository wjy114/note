## Jiffies

  记录自系统启动以来产生的节拍的总数。启动时，内核将该变量初始化为0，此后，每次时钟中断处理程序都会增加该变量的值。一秒内时钟中断的次数等于Hz，所以jiffies一秒内增加的值也就是Hz。
  
  无符号长整型(unsigned long)
  系统运行时间以秒为单位，等于jiffies/Hz   seconds * Hz


  `time_after(a,b) `returns true if the time a is after time b.
 ` #define time_before_eq(a,b) time_after_eq(b,a)`
                
                

        #include <asm/div64.h>


        unsigned long long x,y,result;

        unsigned long mod;

        mod = do_div(x,y);

        result = x; 

    64 bit division 结果保存在x中；余数保存在返回结果中。
    
 schedule_timeout()可以使当前任务睡眠指定的jiffies 之后重新被调度执行
