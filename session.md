# SESSION

##Help

1. 设置SESSION传统的不一样

    传统的是
    
    ```php
    session_start();
    $_SESSION['views']=1;
    ```
    
    然而在`Laravel`并没什么卵用.
    
    Laravel在页面中设置SESSION
    
    ```php
   session(['views' => '1']);
    ```
2. session用数字作为key的问题

    假如前面没设置过数字key,则第一次运行以下代码
    
    ```php
    session(['1' => '4']);
    ```
    他在session存储的是
    ```php
    0="4"
    ```
    就是说如果用数字作为key,它会忽略掉这个key,直接把`value`push到session数组中.这个value的key根据`base_ref`递增.
3. 
    
