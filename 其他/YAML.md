`YAML` 是专门用来写配置文件的语言，非常简洁和强大，远比 `JSON` 格式方便。`YAML`语言（发音 /ˈjæməl/）的设计目标，就是方便人类读写。它实质上是一种通用的数据串行化格式。

它的基本语法规则如下：

- 大小写敏感
- 使用缩进表示层级关系
- 缩进时不允许使用`Tab`键，只允许使用空格
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- `#` 表示注释，从这个字符一直到行尾，都会被解析器忽略

在 Kubernetes 中，我们只需要了解两种结构类型就行了：

- Lists（列表）
- Maps（字典）

### Maps

首先我们来看看 `Maps`，我们都知道 `Map` 是字典，就是一个 **key:value** 的键值对，`Maps` 可以让我们更加方便的去书写配置信息，例如：

```yaml
---
apiVersion: v1
kind: Pod
```



第一行的`---`是分隔符，是可选的，在单一文件中，可用连续三个连字号`---`区分多个文件。这里我们可以看到，我们有两个键：kind 和 apiVersion，他们对应的值分别是：v1 和 Pod。

YAML 处理器是根据行缩进来知道内容之间的嗯关联性的。比如我们上面的 YAML 文件，**我用了两个空格作为缩进，空格的数量并不重要，但是你得保持一致，并且至少要求一个空格。但是：YAML中决不允许使用Tab作为缩进。**

### Lists

`Lists`就是列表，说白了就是数组，在 YAML 文件中我们可以这样定义：

```yaml
args
  - Cat
  - Dog
  - Fish
```

你可以有任何数量的项在列表中，每个项的定义以破折号（`-`）开头的，与父元素之间可以缩进也可以不缩进。对应的 JSON 格式如下：

```json
{
    "args": [ 'Cat', 'Dog', 'Fish' ]
}
```



当然，Lists 的子项也可以是 Maps，Maps 的子项也可以是 Lists 如下所示：

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: ydzs-site
  labels:
    app: web
spec:
  containers:
    - name: front-end
      image: nginx
      ports:
        - containerPort: 80
    - name: flaskapp-demo
      image: cnych/flaskapp
      ports:
        - containerPort: 5000
```

### 纯量

纯量是最基本的，不可再分的值，包括：

- 字符串
- 布尔值
- 整数
- 浮点数
- Null
- 时间
- 日期

使用一个例子来快速了解纯量的基本使用：

```yaml
boolean: 
    - TRUE  #true,True都可以
    - FALSE  #false，False都可以
float:
    - 3.14
    - 6.8523015e+5  #可以使用科学计数法
int:
    - 123
    - 0b1010_0111_0100_1010_1110    #二进制表示
null:
    nodeName: 'node'
    parent: ~  #使用~表示null
string:
    - 哈哈
    - 'Hello world'  #可以使用双引号或者单引号包裹特殊字符
    - newline
      newline2    #字符串可以拆成多行，每一行会被转化成一个空格
date:
    - 2018-02-17    #日期必须使用ISO 8601格式，即yyyy-MM-dd
datetime: 
    -  2018-02-17T15:02:31+08:00    #时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
```