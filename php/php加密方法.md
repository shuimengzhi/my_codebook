```p
//可逆加密
  /*
*$data :需要加密解密的字符串
*$key :加密的钥匙(密匙);
*/
//加密
function encrypt($data, $key)  
{  
    $key    =   md5($key);  
    $x      =   0;  
    $len    =   strlen($data);  
    $l      =   strlen($key);  
    for ($i = 0; $i < $len; $i++)  
    {  
        if ($x == $l)   
        {  
            $x = 0;  
        }  
        $char .= $key{$x};  
        $x++;  
    }  
    for ($i = 0; $i < $len; $i++)  
    {  
        $str .= chr(ord($data{$i}) + (ord($char{$i})) % 256);  
    }  
    return base64_encode($str);  
}  
//解密
function decrypt($data, $key)  
{  
    $key = md5($key);  
    $x = 0;  
    $data = base64_decode($data);  
    $len = strlen($data);  
    $l = strlen($key);  
    for ($i = 0; $i < $len; $i++)  
    {  
        if ($x == $l)   
        {  
            $x = 0;  
        }  
        $char .= substr($key, $x, 1);  
        $x++;  
    }  
    for ($i = 0; $i < $len; $i++)  
    {  
        if (ord(substr($data, $i, 1)) < ord(substr($char, $i, 1)))  
        {  
            $str .= chr((ord(substr($data, $i, 1)) + 256) - ord(substr($char, $i, 1)));  
        }  
        else  
        {  
            $str .= chr(ord(substr($data, $i, 1)) - ord(substr($char, $i, 1)));  
        }  
    }  
    return $str;  
}  

//实验

$sign = '啊啊啊啊';//一般签名数据最好加上当前时间戳

$signE = encrypt($sign,"asd");

echo $signE;

$signD = decrypt($signE,"asd");

echo "\r\n".$signD;
```