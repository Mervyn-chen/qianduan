## JavaScript(V8) 牛客输入输出

```

while(line=readline()){
    var lines = line.split(' ');
    var a = parseInt(lines[0]);
    var b = parseInt(lines[1]);
    print(a+b);
}
```



## JavaScript(Node)

```
var readline = require('readline');
const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout
});
rl.on('line', function(line){
   var tokens = line.split(' ');
    console.log(parseInt(tokens[0]) + parseInt(tokens[1]));
});
```

## 用sort给json对象数组排序

如下例子，我们按学生的
总分排序，如果总分相等，我们再按照语文成绩排序。

```
var jsonStudents = [
    {name:"Dawson", totalScore:"197", Chinese:"100",math:"97"},
    {name:"HanMeiMei", totalScore:"196",Chinese:"99", math:"97"},
    {name:"LiLei", totalScore:"185", Chinese:"88", math:"97"},
    {name:"XiaoMing", totalScore:"196", Chinese:"96",math:"100"},
    {name:"Jim", totalScore:"196", Chinese:"98",math:"98"},
    {name:"Joy", totalScore:"198", Chinese:"99",math:"99"}];
jsonStudents.sort(function(a,b){
    var value1 = a.totalScore,
        value2 = b.totalScore;
    if(value1 === value2){
        return b.Chinese - a.Chinese;
    }
    return value2 - value1;
});
```

输出：

```
0:{name: "Joy", totalScore: "198", Chinese: "99", math: "99"}
1:{name: "Dawson", totalScore: "197", Chinese: "100", math: "97"}
2:{name: "HanMeiMei", totalScore: "196", Chinese: "99", math: "97"}
3:{name: "Jim", totalScore: "196", Chinese: "98", math: "98"}
4:{name: "XiaoMing", totalScore: "196", Chinese: "96", math: "100"}
5:{name: "LiLei", totalScore: "185", Chinese: "88", math: "97"}
```


