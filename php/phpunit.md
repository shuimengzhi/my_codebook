[TOC]
# 标记未完成
$this->markTestIncomplete()
```
<?php
use PHPUnit\Framework\TestCase;

class SampleTest extends TestCase
{
    public function testSomething()
    {
        // Optional: Test anything here, if you want.
        $this->assertTrue(true, 'This should already work.');

        // Stop here and mark this test as incomplete.
        $this->markTestIncomplete(
          'This test has not been implemented yet.'
        );
    }
}
?>
```

# 依赖
testProducerFirst,testProducerSecond执行完再执行testConsumer
```
<?php
use PHPUnit\Framework\TestCase;

class MultipleDependenciesTest extends TestCase
{
    public function testProducerFirst()
    {
        $this->assertTrue(true);
        return 'first';
    }

    public function testProducerSecond()
    {
        $this->assertTrue(true);
        return 'second';
    }

    /**
     * @depends testProducerFirst
     * @depends testProducerSecond
     */
    public function testConsumer($a, $b)
    {
        $this->assertSame('first', $a);
        $this->assertSame('second', $b);
    }
}
```

# 数据提供
```
<?php
use PHPUnit\Framework\TestCase;

class DataTest extends TestCase
{
    /**
     * @dataProvider additionProvider
     */
    public function testAdd($a, $b, $expected)
    {
        $this->assertSame($expected, $a + $b);
    }

    public function additionProvider()
    {
        return [
            [0, 0, 0],
            [0, 1, 1],
            [1, 0, 1],
            [1, 1, 3]
        ];
    }
}
```

# 严格比较
$this->assertSame()
这样会报错
```
$this->assertSame(
            [1, 2, 3, 4, 5, 6],
            ['1', 2, 3, 4, 5, 6]
        );
```
# 宽松的比较
这样不报错
```
$this->assertEquals(
            [1, 2, 3, 4, 5, 6],
            ['1', 2, 3, 4, 5, 6]
        );
```
# 跳过测试
```
$this->markTestSkipped(
              'The MySQLi extension is not available.'
            );
```
# Test double(测试替身)
## 生成虚拟的返回
```
class SomeClass
{
    public function doSomething()
    {
    }
}
```
```
public function testSub(){
        $sub=$this->createStub(SomeClass::class);
        $sub->method('doSomething')->willReturn('foo');
        var_dump($sub->doSomething());
        $this->assertSame('foo',$sub->doSomething());
    }
```

## 返回第几个输入的参数,从0开始计数
```
public function testSub(){
        // Create a stub for the SomeClass class.
        $stub = $this->createStub(SomeClass::class);

        // Configure the stub. 返回数组key=1的参数
        $stub->method('doSomething')
            ->will($this->returnArgument(1));

        // $stub->doSomething('foo') returns 'aa'
        $this->assertSame('foo', $stub->doSomething('foo','aa'));

        // $stub->doSomething('bar') returns 'ns'
        $this->assertSame('bar', $stub->doSomething('bar','ns'));
    }
```
## 返回最后一个值

returnValueMap方法到$map里面查找匹配的一组并返回该组的最后一个值
```
public function testReturnValueMapStub()
    {
        // Create a stub for the SomeClass class.
        $stub = $this->createStub(SomeClass::class);

        // Create a map of arguments to return values.
        $map = [
            ['a', 'b', 'c', 'd'],
            ['e', 'f', 'g', 'h']
        ];

        // Configure the stub.
        $stub->method('doSomething')
             ->will($this->returnValueMap($map));

        // $stub->doSomething() returns different values depending on
        // the provided arguments.
        $this->assertSame('d', $stub->doSomething('a', 'b', 'c'));
        $this->assertSame('h', $stub->doSomething('e', 'f', 'g'));
    }
```

## 按顺序返回,每次读取数组头部信息并将它移除
```
public function testOnConsecutiveCallsStub()
    {
        // Create a stub for the SomeClass class.
        $stub = $this->createStub(SomeClass::class);

        // Configure the stub.
        $stub->method('doSomething')
             ->will($this->onConsecutiveCalls(2, 3, 5, 7));

        // $stub->doSomething() returns a different value each time
        $this->assertSame(2, $stub->doSomething());
        $this->assertSame(3, $stub->doSomething());
        $this->assertSame(5, $stub->doSomething());
    }
```
## 模拟抛出异常
```
public function testThrowExceptionStub()
    {
        // Create a stub for the SomeClass class.
        $stub = $this->createStub(SomeClass::class);

        // Configure the stub.
        $stub->method('doSomething')
             ->will($this->throwException(new Exception));

        // $stub->doSomething() throws Exception
        $stub->doSomething();
    }
```

# Mock

```
class Subject
{
    protected $observers = [];
    protected $name;

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function doSomething()
    {
        // Do something.
        // ...

        // Notify observers that we did something.
        $this->notify('something');
    }

    public function doSomethingBad()
    {
        foreach ($this->observers as $observer) {
            $observer->reportError(42, 'Something bad happened', $this);
        }
    }

    protected function notify($argument)
    {
        foreach ($this->observers as $observer) {
            $observer->update($argument);
        }
    }

}
```
```
class Observer
{
    public function update($argument)
    {
        // Do something.
    }

    public function reportError($errorCode, $errorMessage, Subject $subject)
    {
        // Do something
    }
}
```

## with
```
<?php
use PHPUnit\Framework\TestCase;

class SubjectTest extends TestCase
{
    public function testObserversAreUpdated()
    {
        // Create a mock for the Observer class, mocking the
        // reportError() method
        $observer = $this->createMock(Observer::class);

        $observer->expects($this->once())
            ->method('update')
            ->with(
                $this->equalTo('aa')
            );
        $observer->update('aaa');
    }
}
```
```
$observer->expects($this->once())
                 ->method('update')
                 ->with($this->equalTo('aa'));
```
设置期待生成的数是'aa',如果$observer->update('aaa')这里模拟生成的值和期待的不一样，则报错


```
$observer->expects($this->once())
                 ->method('reportError')
                 ->with(
                       $this->greaterThan(0),
                       $this->stringContains('Something'),
                       $this->anything()
                   );
```
```
$observer->reportError(42, 'Something bad happened', $this);
```
这里代表模拟的reportError,有三个参数输入，期待第一个参数大于0，第二个参数包含Something中的一个值,第三个参数任何参数都可以

## Mock is not exist method
```
$mock = $this->getMockBuilder(SomeClass::class)
            ->addMethods(['test'])
            ->getMock();
        $mock->expects($this->once())
            ->method('test')
            ->with($this->equalTo('aa'))
            ->will($this->returnValue(1));
        $mock->test('aa');
```
test方法不存在与SomeClass类中，用getMock创建，设置期待的值，然后执行.
$mock->test('aa')返回值为1