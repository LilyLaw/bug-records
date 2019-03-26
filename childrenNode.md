## JavaScript获取DOM节点的子节点，各种踩坑(；′⌒`)

上代码：
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <ul id="pul">
        <li class="eli">item 1i</li>
        <li class="eli">item 2i</li>
        <li class="eli">item 3i</li>
        <li class="eli">item 4i</li>
        <li class="eli">item 5i</li>
    </ul>

    <div id="pdiv">
        <span>item 1</span>
        <p>item 2</p>
        <a href="">item 3</a>
    </div>

    <script>
        var pul = document.getElementById("pul");
        // 看控制台
        console.log(pul.childNodes);
        console.log(pul.children);

        var pdiv = document.getElementById("pdiv");
        // 看控制台
        console.log(pdiv.childNodes);
        console.log(pdiv.children);
    </script>
</body>
</html>
```

### Node.childNodes

只读属性，返回给定元素的子节点的实时节点列表，第一个子节点索引为0.

语法：`var nodeList = elementNodeReference.childNodes; `

childNodes 列表包括所有的子节点，包括非元素节点，如文本和注释节点。若要获取仅元素的集合，请改用**ParentNode.children**

----------

### ParentNode.children

只读属性，返回实时的给定元素的所有子元素的html集合

语法：`var children = node.children;`

可以通过item() 方法或者JavaScript的数组方式来访问所需子节点。

item() 方式： ```var element = HTMLCollection.item(index)```

建议用数组方式：```var element = HTMLCollection[index]```
