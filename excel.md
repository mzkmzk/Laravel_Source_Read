# Excel

使用<https://github.com/Maatwebsite/Laravel-Excel>进行Excel处理

##1. 安装

    composer require maatwebsite/excel ~2.0.0

在`config/app.php`中

    'providers' => [...
        Maatwebsite\Excel\ExcelServiceProvider::class,
    ]
     
    'aliases' => [
        'Excel' => Maatwebsite\Excel\Facades\Excel::class,
    ]
    
##2. 简单使用

1. 定义好`Controller`和`route`
2. `Controller`代码
        use App\Http\Controllers\Base_Controller;
        use Excel;
        
        class Text extends Base_Controller{
            function Text(){
                //Excel文件导出功能 By Laravel学院
                    $cellData = [
                        ['学号','姓名','成绩'],
                        ['10001','AAAAA','99'],
                        ['10002','BBBBB','92'],
                        ['10003','CCCCC','95'],
                        ['10004','DDDDD','89'],
                        ['10005','EEEEE','96'],
                    ];
                    Excel::create('学生成绩',function($excel) use ($cellData){
                        $excel->sheet('score', function($sheet) use ($cellData){
                            $sheet->rows($cellData);
                        });
                    })->export('xls');
        
            }
        }
        ?>

##参考链接

<http://laravelacademy.org/post/2024.html>