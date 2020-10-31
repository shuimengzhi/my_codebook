[TOC]
# version_compare
version_compare($version1, $version2, $operator = null)
```
 * When using the optional operator argument, the
 * function will return true if the relationship is the one specified
 * by the operator, false otherwise.
```
example :
```
if (version_compare(PHP_VERSION, '5.3.0', '<')) {
    die('require PHP > 5.3.0 !');
}
```
# microtime
```
/**
 * Return current Unix timestamp with microseconds
 * @link https://php.net/manual/en/function.microtime.php
 * @param bool $get_as_float [optional] <p>
 * When called without the optional argument, this function returns the string
 * "msec sec" where sec is the current time measured in the number of
 * seconds since the Unix Epoch (0:00:00 January 1, 1970 GMT), and
 * msec is the microseconds part.
 * Both portions of the string are returned in units of seconds.
 * </p>
 * <p>
 * If the optional get_as_float is set to
 * true then a float (in seconds) is returned.
 * </p>
 * @return string|float
 */
function microtime ($get_as_float = null) {}
```
example :
```
$GLOBALS['_beginTime'] = microtime(true);
```
# function_exists
example:
```
define('MEMORY_LIMIT_ON', function_exists('memory_get_usage'));
```
# memory_get_usage
```
$GLOBALS['_startUseMems'] = memory_get_usage();
```
# str_replace
```
function str_replace ($search, $replace, $subject, &$count = null) {}
```
```
Replace all occurrences of the search string with the replacement string
```
example:
```
$_temp = explode('.php', $_SERVER['PHP_SELF']);
define('_PHP_FILE_', rtrim(str_replace($_SERVER['HTTP_HOST'], '', $_temp[0] . '.php'), '/'));
```
# trim
Strip whitespace (or other characters) from the beginning and end of a string
```
string(32) "        These are a few words :) ...  "
string(16) "    Example string
"
string(11) "Hello World"

string(28) "These are a few words :) ..."
string(24) "These are a few words :)"
string(5) "o Wor"
string(9) "ello Worl"
string(14) "Example string"
```
# rtrim
去除末尾的空格，如果有第二个参数则删除最后一个指定的参数
example:
```
$_temp = explode('.php', $_SERVER['PHP_SELF']);
define('_PHP_FILE_', rtrim(str_replace($_SERVER['HTTP_HOST'], '', $_temp[0] . '.php'), '/'));
```
# ucwords
```
Uppercase the first character of each word in a string
```
example:
```
if (!self::$storage) {
            $type          = $type ?: C('LOG_TYPE');
            $class         = 'Think\\Log\\Driver\\' . ucwords($type);
            self::$storage = new $class();
}
```

# error_get_last
```
Get the last occurred error
```
example:
```
 if ($e = error_get_last()) {
            switch ($e['type']) {
                case E_ERROR:
                case E_PARSE:
                case E_CORE_ERROR:
                case E_COMPILE_ERROR:
                case E_USER_ERROR:
                    ob_end_clean();
                    self::halt($e);
                    break;
            }
        }
```
# ob_end_clean
```
Clean (erase) the output buffer and turn off output buffering
```
example:
```
if ($e = error_get_last()) {
            switch ($e['type']) {
                case E_ERROR:
                case E_PARSE:
                case E_CORE_ERROR:
                case E_COMPILE_ERROR:
                case E_USER_ERROR:
                    ob_end_clean();
                    self::halt($e);
                    break;
            }
        }
```
# debug_backtrace
# debug_print_backtrace
```
Prints a backtrace
```
# set_exception_handler
```
Sets a user-defined exception handler function
```
# strip_tags
```
Strip HTML and PHP tags from a string
```
example
```
<?php
echo strip_tags("Hello <b>world!</b>");
?>
```
# $_SERVER['SCRIPT_NAME']
`Contains the current script's path`

# date_default_timezone_set
`Sets the default timezone used by all date/time functions in a script`

# list
```
$info = array('coffee', 'brown', 'caffeine');

list($a[0], $a[1], $a[2]) = $info;

var_dump($a);
```
```
array(3) {
  [0]=>
  string(6) "coffee"
  [1]=>
  string(5) "brown"
  [2]=>
  string(8) "caffeine"
}
```

# array_shift
Shift an element off the beginning of array
```
<?php
$stack = array("orange", "banana", "apple", "raspberry");
$fruit = array_shift($stack);
print_r($stack);
?>
```
```
Array
(
    [0] => banana
    [1] => apple
    [2] => raspberry
)
```
# array_slice
array_slice() returns the sequence of elements from the array array as specified by the offset and length parameters.
```
<?php
$input = array("a", "b", "c", "d", "e");

$output = array_slice($input, 2);      // returns "c", "d", and "e"
$output = array_slice($input, -2, 1);  // returns "d"
$output = array_slice($input, 0, 3);   // returns "a", "b", and "c"
```

# parse_str
Parses the string into variables
```
<?php
parse_str("My Value=Something");
echo $My_Value; // Something

parse_str("My Value=Something", $output);
echo $output['My_Value']; // Something
?>
```

# strpos
`Find the position of the first occurrence of a substring in a string`
https://php.net/manual/en/function.strpos.php

# strip_tags
Strip HTML and PHP tags from a string
```
<?php
$text = '<p>Test paragraph.</p><!-- Comment --> <a href="#fragment">Other text</a>';
echo strip_tags($text);
echo "\n";

// Allow <p> and <a>
echo strip_tags($text, '<p><a>');

// as of PHP 7.4.0 the line above can be written as:
// echo strip_tags($text, ['p', 'a']);
?>
```
The above example will output:

```
Test paragraph. Other text
<p>Test paragraph.</p> <a href="#fragment">Other text</a>
```

# array_walk_recursive
Apply a user function recursively to every member of an array
```
<?php
$sweet = array('a' => 'apple', 'b' => 'banana');
$fruits = array('sweet' => $sweet, 'sour' => 'lemon');

function test_print($item, $key)
{
    echo "$key holds $item\n";
}

array_walk_recursive($fruits, 'test_print');
?>
```
```
a holds apple
b holds banana
sour holds lemon
```
# array_map
数组每个值都执行一次回调函数
```
<?php
function cube($n)
{
    return ($n * $n * $n);
}

$a = [1, 2, 3, 4, 5];
$b = array_map('cube', $a);
print_r($b);
?>

```
```
Array
(
    [0] => 1
    [1] => 8
    [2] => 27
    [3] => 64
    [4] => 125
)
```

# PDOStatement::bindParam() different with PDOStatement::bindValue()

```
$value = 'foo';
$s = $dbh->prepare('SELECT name FROM bar WHERE baz = :baz');
$s->bindParam(':baz', $value); // use bindParam to bind the variable
$value = 'foobarbaz';
$s->execute(); // executed with WHERE baz = 'foobarbaz'
```
or
```
$value = 'foo';
$s = $dbh->prepare('SELECT name FROM bar WHERE baz = :baz');
$s->bindValue(':baz', $value); // use bindValue to bind the variable's value
$value = 'foobarbaz';
$s->execute(); // executed with WHERE baz = 'foo'
```

# __call()
在对象中调用一个不可访问方法时，__call() 会被调用。
```
<?php
class MethodTest 
{
    public function __call($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling object method '$name' "
             . implode(', ', $arguments). "\n";
    }

    /**  PHP 5.3.0之后版本  */
    public static function __callStatic($name, $arguments) 
    {
        // 注意: $name 的值区分大小写
        echo "Calling static method '$name' "
             . implode(', ', $arguments). "\n";
    }
}

$obj = new MethodTest;
$obj->runTest('in object context');

MethodTest::runTest('in static context');  // PHP 5.3.0之后版本
?>
```
output:
```
Calling object method 'runTest' in object context
```

# is_scalar
检测变量是否是一个标量

# func_get_args
Returns an array comprising a function's argument list
```
<?php
function foo() {
    $args = func_get_args();
    var_export($args);
}

foo('First arg', 'Second arg');
?>
```
```
array (
  0 => 'First arg',
  1 => 'Second arg',
)
```
# addslashes
Quote string with slashes
```
<?php
$str = "Is your name O'Reilly?";

// Outputs: Is your name O\'Reilly?
echo addslashes($str);
?>
```

# vsprintf
格式化字符
```
<?php
print vsprintf("%4d-%02d-%02d", explode('-', '1988-8-1'));
?>
```
output:
```
1988-08-01
```
# get_object_vars
获得这个类里面的属性,example:$x,$y,$label
```
class Point2D {
    var $x, $y;
    var $label;

    function Point2D($x, $y)
    {
        $this->x = $x;
        $this->y = $y;
    }

    function setLabel($label)
    {
        $this->label = $label;
    }

    function getPoint()
    {
        return array("x" => $this->x,
                     "y" => $this->y,
                     "label" => $this->label);
    }
}

// "$label" is declared but not defined
$p1 = new Point2D(1.233, 3.445);
print_r(get_object_vars($p1));

$p1->setLabel("point #1");
print_r(get_object_vars($p1));
```
output:

```
 Array
 (
     [x] => 1.233
     [y] => 3.445
     [label] =>
 )

 Array
 (
     [x] => 1.233
     [y] => 3.445
     [label] => point #1
 )
```

# array_diff
Computes the difference of arrays
```
<?php
$array1 = array("a" => "green", "red", "blue", "red");
$array2 = array("b" => "green", "yellow", "red");
$result = array_diff($array1, $array2);

print_r($result);
?>
```

```
Array
(
    [1] => blue
)
```

# preg_replace_callback
执行一个正则表达式搜索并且使用一个回调进行替换

# strtr
转换指定字符
```
<?php
$trans = array("hello" => "hi", "hi" => "hello");
echo strtr("hi all, I said hello", $trans);
?>
```
output:
```
hello all, I said hi
```

# is_numeric
 Finds whether a variable is a number or a numeric string
```
<?php
$tests = array(
    "42",
    1337,
    0x539,
    02471,
    0b10100111001,
    1337e0,
    "0x539",
    "02471",
    "0b10100111001",
    "1337e0",
    "not numeric",
    array(),
    9.1,
    null
);

foreach ($tests as $element) {
    if (is_numeric($element)) {
        echo var_export($element, true) . " is numeric", PHP_EOL;
    } else {
        echo var_export($element, true) . " is NOT numeric", PHP_EOL;
    }
}
?>
```
```
'42' is numeric
1337 is numeric
1337 is numeric
1337 is numeric
1337 is numeric
1337.0 is numeric
'0x539' is NOT numeric
'02471' is numeric
'0b10100111001' is NOT numeric
'1337e0' is numeric
'not numeric' is NOT numeric
array (
) is NOT numeric
9.1 is numeric
NULL is NOT numeric
```

# mt_rand
Generate a random value via the Mersenne Twister Random Number Generator

```
<?php
echo mt_rand() . "\n";
echo mt_rand() . "\n";

echo mt_rand(5, 15);
?>
```

```
1604716014
1478613278
6
```
# stripslashes
去除斜杠
```
<?php
$str = "Is your name O\'reilly?";

// 输出: Is your name O'reilly?
echo stripslashes($str);
?>
```
