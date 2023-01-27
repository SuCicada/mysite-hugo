https://stackoverflow.com/questions/23840756/how-to-disable-perspective-in-mplot3d

简单的解决方法是

```
ax = fig.add_subplot(111, projection='3d', proj_type='ortho')
```
注意111 和 proj_type='ortho'
辛亏在打算转用Mayavi 前找到了解决方法 
