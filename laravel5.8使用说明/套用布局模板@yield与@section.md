公用的内容
```
<!-- 保存在  resources/views/layouts/app.blade.php 文件中 -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```
具体内容
```
<!-- 保存在 resources/views/child.blade.php 中 -->
{{--引用上方的文件，真正加载的时候只需要加载这里就好--}}
@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```
运行结果
```
This is the master sidebar.
This is appended to the master sidebar.

This is my body content.
```
公用文件用@section则必须使用@show才能显示

@yield不具体显示内容，具体显示的内容由@section来定义,括号内的字符来联系@yield与@section。

@section有两种书写方式：
1.@section('@yield括号内的字符','具体内容')，
2.
```
@section('@yield括号内的字符')
  具体内容
@endsection
```

公用文件使用了@section，具体文件也用了同一个@section，用@parent 不覆盖内容

#一般@yield写在公用文件内
#@section写在具体文件内，是@yield的实例化