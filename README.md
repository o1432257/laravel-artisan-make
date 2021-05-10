# Laravel 自定義生成目標類
###### tags: `Laravel 8` `Artisan Console` `php`

## 前言
今天會用Service類來示範如何自定義生成目標類
### make:controller的源碼位置
```
vendor/laravel/framework/src/Illuminate/Routing/Console/ControllerMakeCommand.php
```
### 其他的生成命令的位置
```
vendor/laravel/framework/src/Illuminate/Foundation/Console
```
## 生成命令
```
$ php artisan make:command MakeService 
```

### 修改繼承類
```php=
class MakeService extends GeneratorCommand
```
把繼承類修改成GeneratorCommand

刪除方法 handle

### 設置name屬性
```php=
protected $name = 'make:service';
```
修改$signature屬性為name屬性
### 設置type屬性
```php=
protected $type = 'Service';
```
type 類型是自己去定義的，本身沒有特殊含義，可以不用設置。

type 屬性值僅僅在創建錯誤的時候，給你一個友好的提示

[詳細](https://learnku.com/articles/57058)
### 新增方法
```php=
/**
 * Get the stub file for the generator.
 *
 * @return string
 */
protected function getStub()
{
    // Implement getStub() method.
    return $this->laravel->basePath('/stubs/service.stub');
}

/**
 * Get the default namespace for the class.
 *
 * @param  string  $rootNamespace
 * @return string
 */
protected function getDefaultNamespace($rootNamespace)
{
    return $rootNamespace.'\Services';
}
```
## Stub 定制
Artisan的make命令可用於創建多種多樣的類，如：控制器、任務、遷移和測試。這些類是使用stub文件生成的，它將根據您的輸入來填充值。

您可以使用stub:publish命令來發布最常見的定制stub來實現之：
```
$ php artisan stub:publish 
```
新增目標類的模板
```
# stubs/service.stub

<?php

namespace DummyNamespace;

class DummyClass
{
    //
}
```
DummyClass 和 DummyNamespace 在繼承的GeneratorCommand類內部會被自動替換成自動生成的類名和設置的命名空間。

## 完成
![](https://i.imgur.com/Yb71J8M.png)


## 參考來源
[Laravel自定义Make命令生成Service类](https://houxin.blog.csdn.net/article/details/115480230)

[Laravel自定义Make命令生成目标类](https://learnku.com/articles/57058)