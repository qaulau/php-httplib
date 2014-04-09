php-httplib
===========

> **php-httplib是一个基于php curl库封装的http类库，简单易用的同时兼顾性能及功能，可完美解决您的web请求需求！**

此类本来只是一开始用于个人私下把玩使用的（采集数据），后来也用于自己参加的一些项目中，在实际应用中表现不错，当然不可避免的有些问题需要大家进行修正及提出！


**简单的使用示例**

```php
include 'httplib.class.php';
//初始化
$http = new httplib();
//设置超时时间为60秒
$http->set_timeout(60);
//设置浏览器信息
$http->set_useragent($_SERVER['HTTP_USER_AGENT']);
//设置请求的方式为XMLHttpRequest，模拟ajax请求信息，可用于某些api的限制，此处仅为演示说明
$http->set_header('X-Requested-With','XMLHttpRequest');
//设置代理服务器信息
$http->set_proxy('59.108.116.175','3128');
//请求资源
$http->request('http://www.baidu.com/s?wd=ip');
//获取网页数据
$data = $http->get_data();
//获取cookie（默认返回字符串，参数为true时返回数组）
$cookie = $http->get_cookie();
//获取网页响应状态码
$statcode = $http->get_statcode();
//获取原始未处理的响应信息
$response = $http->get_response();
//获取连接资源的信息（返回数组），如平均上传速度 、平均下载速度 、请求的URL等
$response_info = $http->get_info();
```

**GET请求**

```php
include 'httplib.class.php';
$http = new httplib();
$http->request('http://www.baidu.com/s?wd=ip');
echo $http->get_data();
```


**POST请求**

```php
include 'httplib.class.php';
$http = new httplib();
$http->set_useragent($_SERVER['HTTP_USER_AGENT']);
$postdata = array(
	'from' 	=> 'auto',
	'to'	=> 'zh',
	'query' => 'Hello.'
);
$http->request('http://fanyi.baidu.com/v2transapi',$postdata,'http://fanyi.baidu.com');
$json = $http->get_data();
$arr = json_decode($json,true);
echo '<pre>';
print_r($arr);
echo '</pre>';
```



**上传文件**

```php
include 'httplib.class.php';
$http = new httplib();
$http->set_useragent($_SERVER['HTTP_USER_AGENT']);
$postdata = array(
	'file'=>'@c://1.mp3'
);
$refere = 'http://pan.baidu.com';
$duss = 'ce5d70d0f96459b07d6c6c9d9b39db96';
$path = '/qaulau/1.mp3';
$http->request('https://pcs.baidu.com/rest/2.0/pcs/file?method=upload&path='.$path.'&duss='.$duss,$postdata,$refere);
$json = $http->get_data();
$arr = json_decode($json,true);
echo '<pre>';
print_r($arr);
echo '</pre>';
```

**后话**

说到采集在此需要提出的是另一个比较知名的**Snoopy.class.php**的采集库，虽然封装提供了一些便利功能（如对抓取的数据html处理方面去除HTML标签，匹配链接等），但同时这也是他的弊病以至于整个类库过于臃肿，去除html标签和匹配链接在我看来完全可以交给用户后期处理（并且去除html也很简单使用strip_tags或者正则替换就可以了），而且另一个弊病就是效率不高，Snoopy使用的是fopensock进行封装（linux如果有执行命令行权限则使用的linux的curl命令），性能方面如果是fopensock的话其相对curl会差一些，这个大家可以具体测试一下请求所需时间及内存占用量！

当然上面说了snoopy的很多缺点，如臃肿、效率不高，他还是有相当多的优点比php-httplib出色，如兼容性很好对于没有安装或禁用curl扩展的虚拟主机和服务器上仍然可以运行，而php-httplib需要基于curl的支持才能使用，否则会是500内部错误！
