首先参看官网文档的写法 https://photonstorm.github.io/phaser-ce/Phaser.Create.html#texture
```js
var game = new Phaser.Game(800, 600, Phaser.CANVAS, 'game', {
    preload: preload,
    create: create,
});
function preload(){
    var data = [
        ' 333 ',
        ' 777 ',
        'E333E',
        ' 333 ',
        ' 3 3 '];
    game.create.texture('bob', data);
}
function create() {
    game.add.sprite(0, 0, 'bob');
}
```
控制台警告
```
Phaser.Cache.addImage: Image "xx" is not complete 
```
改为
```js
......
    game.create.texture('bob', data,100,10,0,true,function(f){});
......
```
警告
```
Phaser.Cache.getImage: Key "xx" not found in Cache.
```

查找资料无果，实验之后发现
使用
```js
    game.load.imageFromTexture('bob',data,100,1);
```
替换
```js
    game.create.texture('bob', data,100,10,0,true,function(f){});
```

即可。
注意这里```game.load.imageFromTexture```和```game.add.sprite```要放在不同部分，比如一个在preload 一个在create。


测试浏览器为chrome，firefox
测试phaser版本为2.13.2和2.12.0

至于为什么会这样，我现在没有力气琢磨了，希望好心的大神能赐教。


---
2个小时后发现
```js
    game.create.texture('bob', data,100,10,0,true,function(){
	    game.add.sprite(0, 0, 'bob');
    }); 
```
设置回调函数是可以完成的，也没有警告，所以问题就出在太快了。
请求图片时，但是图片没有进入缓存所以没有办法拿到。
之前试过回调函数，但是为什么没有成功。。。

注意这里```game.create.texture```和```game.add.sprite```即便放在不同部分，比如一个在preload 一个在create。只要不是回调，或等```create.texture```执行完毕，都是不行的。
