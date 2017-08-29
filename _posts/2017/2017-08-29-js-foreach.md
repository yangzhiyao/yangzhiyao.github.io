---
title: js foreach 的几种形式
---

## 集合数据准备
```js
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js">
</script>
<script>
  // Setup collection
  var collection = [];
  for (var i = 0; i < 200; i++) {
    collection.push({
      index: i,
      data: 'Index element ' + i
    });
  }
</script>
```

## jQuery (fastest)
```js
$.each(collection, function(index, element) {
     console.log( index,"SSSSSSSSSSSSSSSSSSSSSSSSSS");
});
```

## js forEach()
```js
collection.forEach(function(element, index) {
    console.log(index,"SSSSSSSSSSSSSSSSSSSSSSSSSS");
});
```

## js for(var x in collection) (fastest)
```js
for (var index in collection) {
    if (collection.hasOwnProperty(index)) 
        console.log(index,"SSSSSSSSSSSSSSSSSSSSSSSSSS");
}
```

## js for
```js
for (var i=0,len=collection.length; i<len ; i++) {
    console.log(i,"SSSSSSSSSSSSSSSSSSSSSSSSSS");
}
```