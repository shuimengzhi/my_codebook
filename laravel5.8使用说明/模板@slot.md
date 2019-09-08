模板代码

test/app.blade.php

```
 11111{{ $title }}
    33{{$bb}}
   2222 {{ $slot }}
```

具体实现代码

child.blade.php

```
@component('test.app')
@slot("bb")
    sadfasdf
    @endslot
    @slot('title')
        Forbidden
    @endslot
    

    You are not allowed to access this resource!
@endcomponent
```

@component('套用的模板')

@slot('与模板对应的参数')

浏览器运行child.blade.php

```
11111Forbidden 33sadfasdf 2222 You are not allowed to access this resource!
```

