## 1 什么是buffer
- 读取的内容是 2 进制的， 展现形式是 16 进制的
- 位(2进制)- *8-> 字节(16进制) -*3-> 汉字(utf-8)
- 2进制--> 10进制 (当前位的最大值*2^(当前位-1)进行累加)


## 2 创建 buffer
### 长度创建
```
let buffer = new Buffer(6);
console.log(buffer);//<Buffer 01 00 00 00 01 00>
buffer.fill(1);//手动填充buffer
console.log(buffer);//<Buffer 01 01 01 01 01 01>

```

### 数组创建
- 不能正确的转换 -> 0
- 负数 -> +256
- 大于255的数字 -> num%256

```
let buffer = new Buffer([-1, 22, 344]);
console.log(buffer);//<Buffer ff 16 58>

```

### 字符串创建
- 通过汉字创建的buffer内容和汉字是对应的
- 将buffer装换成字符,方法是toString()
- 如果取buffer中的某一个，则是16进制转化成为的10进制
```
let buffer = new Buffer('珠峰培训');
console.log(buffer);//<Buffer e7 8f a0 e5 b3 b0 e5 9f b9 e8 ae ad>
console.log(buffer.toString());//珠峰培训
console.log(buffer[0]);//231
```

## 3 Buffer的copy

### slice 浅拷贝 [statIndex, endIndex) 截取
```
var arr = [1, 2, 3, 4];
var ary = [arr, 1, 2, 3];
var newAry = ary.slice(0); //浅拷贝
arr[0] = 100;
console.log(newAry);//[ [ 100, 2, 3, 4 ], 1, 2, 3 ]--> 二维数组
```

> buffer 存的是内存地址

```
var buffer = new Buffer([1, 2, 3]);
var newBuffer = buffer.slice(0, 1);
console.log(newBuffer);//<Buffer 01>
newBuffer[0] = 100;
console.log(buffer);//<Buffer 64 02 03>
```


### copy
- copy(targetBuffer,targetStart,sourceStart,sourceEnd);
```
let buffer = new Buffer(12);
let buf1 = new Buffer('珠峰');
let buf2 = new Buffer('培训');
buf2.copy(buffer,0);
buf1.copy(buffer,buf1.length );
console.log(buffer.toString());//培训珠峰
```

### Buffer.concat([buf1, buf2])

```
let buf1 = new Buffer('珠峰');
let buf2 = new Buffer('培训')

let buf = Buffer.concat([buf1, buf2]);
console.log(buf.toString());//珠峰培训
```

- Buffer.myConcat()
```
var buf1 = new Buffer('珠峰培');
var buf2 = new Buffer('训');
var buf3 = new Buffer('训');
//1.如果不写长度 默认拼接后的长度，如果写的过长多余的截取掉，长度过小则拷贝不进去
//push slice filter map forEach find reduce (for of/for in/forEach) some
Buffer.myConcat = function (list, totalLength) {
    //1.不传递长度，计算出一个总长度，根据计算出的长度构建一个大buffer
    if (typeof totalLength === 'undefined') {
        totalLength = list.reduce((prev, next) => {  //[buf1,buf2,buf3]
            return prev + next.length
        }, 0);//计算总长度,手动设置prev
    }
    //2.如果传递长度，按照传的长度来构建buffer new Buffer
    let buffer = new Buffer(totalLength);
    //3.将list中每一个buffer拷贝到大buffer上 copy
    let index = 0;
    list.forEach(item => {
        item.copy(buffer, index);
        index += item.length;
    });
    //4.最后截取掉多余的长度 slice
    return buffer.slice(0, index);
};
console.log(Buffer.myConcat([buf1, buf2, buf3]).toString());
```

### 对象的拷贝
- JSON.parse(JSON.stringify(obj))-- 不能拷贝函数
- Object.assign(newObj, oldObj);--es6
```
let obj = {
    name: 1,
    age: 2,
    c: function () {}
};
let copy = JSON.parse(JSON.stringify(obj));
console.log(copy);//{ name: 1, age: 2 }

let obj1 = {};
Object.assign(obj1, obj, {name: 'lucy'});
console.log(obj1);//{ name: 'lucy', age: 2, c: [Function: c] }
```

## 4 进制的转换

### 任意进制 -> 十进制  parseInt()
```
console.log(parseInt("00111111",2));//63
console.log(parseInt('077', 8));//63
console.log(parseInt('0xff', 16));//255
```

### 任意进制 -> 任意进制
```
console.log(0xff.toString(10));//255
console.log(0xff.toString(2));//11111111
console.log(066.toString(10));//54
```

### base64 
```
//base64 将汉字转换成base64,没有加密功能
//md5加密 1.不可逆 2.加密的结果的长度都是一样的 3.不同的内容 加密后的结果不一致
//00111111 = 63
//一个汉字 3*8 => 6*4 ,每一个字节不能超过63这么大
var buffer = new Buffer('珠');
console.log(buffer);// e7 8f a0
console.log(0xe7.toString(2));
console.log(0x8f.toString(2));
console.log(0xa0.toString(2));
// 111001 111000 111110 100000 需要转化成10进制去对应的编码中取值
console.log(parseInt("111001",2));
console.log(parseInt("111000",2));
console.log(parseInt("111110",2));
console.log(parseInt("100000",2));
// 57 56 62 32
var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
str+=str.toLowerCase();
str+= "0123456789";
str+= "+/";
console.log(str[57]+str[56]+str[62]+str[32]);
```





