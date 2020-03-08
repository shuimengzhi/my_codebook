with方法的使用

场景：一张操作记录表和一张用户表，操作记录表和用户表共有user_id,操作记录表有很多相同的user_id,用户表只有一个user_id。两张表的主键都是id。操作记录表作为主表去连接用户表

控制器

```php
$test = \App\Models\ZpRechargeLog::where('user_id',218)->with('userzhenpin')->get()->toArray();
```

操作记录表 数据模型

```php
public function userzhenpin(){
		return $this->hasOne('App\Models\UserZhenpin','user_id','user_id');
	}
```

with填写需要连表的那个方法