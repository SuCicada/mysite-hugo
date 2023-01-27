精简版
```js
    var date = new Date();
    datetime = date.toLocaleDateString().split('/').slice(2,3).concat(
        date.toLocaleDateString().split('/').splice(0,2)).join('-') + " " + (
            date.toLocaleTimeString().split(' ')[1]=='AM' ?
                date.toLocaleTimeString().split(' ')[0] :
                [parseInt(date.toLocaleTimeString().split(' ')[0].split(':')[0]) + 12].concat(
                    date.toLocaleTimeString().split(' ')[0].split(':').splice(1,3)).join(':')
        );
```

人看的版
```js
    var date = new Date();
    var dd = date.toLocaleDateString().split('/');
    var dd = dd.slice(2,3).concat(
        dd.splice(0,2)).join('-');
    var time = date.toLocaleTimeString().split(' ');
    var time = time[1]=='AM' ? time[0] : [parseInt(time[0].split(':')[0]) + 12].concat(time[0].split(':').splice(1,3)).join(':');
    var datetime = dd + " " + time;
```



