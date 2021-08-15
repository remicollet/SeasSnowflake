
English | [中文](./README-CH.md)

# Introductions
SeasSnowflake is a distributed unique ID generator inspired by Twitter's Snowflake.

Parts of a snowflake ID:

sign bit: will always be 0

this is because some languages don't support unsigned integers properly (Java and PHP are notable cases)

next 41 bits: milliseconds since custom epoch

next 4 bits: datacenter id

next 10 bits: worker id

lowest 12 bits: sequence number
![avatar](https://img-blog.csdnimg.cn/20191009093154467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZseTkxMDkwNQ==,size_16,color_FFFFFF,t_70)


### Benchmark test：
```php
--TEST--
SeasSnowflake testBenchmark
--FILE--
<?php
//test code
$config=['worker_id'=>10,'datacenter_id'=>1];
$client = new SeasSnowflake($config);
$start = microtime(true);
for($i=0;$i<100000;$i++){
    $id= $client->generate();
}
$end = (float)microtime(true);
echo 'cost:'.round($end-$start,6).' second ';

echo ' and memory usage: ' . memory_get_usage() . 'byte';

?>

---- ACTUAL OUTPUT
cost:0.731212 second  and memory usage: 387520byte
---- FAILED
```
```php
---- ACTUAL OUTPUT
make 100000 id=================
cost:0.712473 second  and memory usage: 395816byte
make 200000 id=================
cost:1.401492 second  and memory usage: 395816byte
make 300000 id=================
cost:1.415514 second  and memory usage: 395816byte
make 500000 id=================
cost:1.422977 second  and memory usage: 395816byte
---- FAILED
```
3、Benchmark code：[More examples](./tests/benchmark.phpt)


### How to use it：

#### Building

    phpize
    ./configure
    make
    sudo make install

Once built and installed, be sure to add 'seassnowflake.so' to the php.ini extensions configuration.

    extension=seassnowflake.so


```php  
$config=["worker_id"=>1,"datacenter_id"=>2];
$client = new SeasSnowflake($config);
$id= $client->generate();
var_dump("id is ====",$id);
$data=$client->degenerate($id);
var_dump("data is ====",$data);
```
### echo:
```php
string(10) "id is ===="
int(273387429541355520)
string(12) "data is ===="
array(6) {
  ["worker_id"]=>
  int(10)
  ["datacenter_id"]=>
  int(15)
  ["sequence"]=>
  int(0)
  ["timestamp"]=>
  int(1626140642495)
  ["begin_timestamp"]=>
  int(1560960000000)
  ["interval"]=>
  int(65180642495)
}
```

#### [More examples](./tests/003.phpt)

## Support
SeasX Group 





