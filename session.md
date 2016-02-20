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
    $_SESSION['views']=1;
    ```
