# self different with static

```
class A{
    function selfFactory(){
        return new self();
    }

    function staticFactory(){
        return new static();
    }
}

class B extends A{
}


$b = new B();

$a1 = $b->selfFactory(); // a1 is an instance of A

$a2 = $b->staticFactory(); // a2 is an instance of B
```

new static($a)
相当于给构造函数传值

```
 public function __construct(...$a)
{
            var_dump(...$a);
}
```