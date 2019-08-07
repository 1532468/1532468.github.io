
대량적인 폼을 만들었다.

css 파일을 분리하여 관리하려 하였으나, Flask에서 희한하게 관리하여 PASS

~~~HTML
<!DOCTYPE HTML>

<html>
  <head>
    <title>B:CODE</title>
  </head>
  
  <style>
      h1{
          color:red;
      }
      
      div.content{
          margin: 0px auto;
          width:80%;

      }
      div.left{
          float: left;
          background-color:yellow;
          width:50%;
          height:500px;
      }
      #right {
          float: right;
          background-color:blue;
          width:50%;
          height:500px;
      }
  </style>
    
  <body>
    <h1>Hello World!!!</h1>
    <div class="content">
        <div class="left">left</div>
        <div id="right">right!!!</div>
      </div>
  </body>
</html>
~~~


<!DOCTYPE HTML>

<html>
  <head>
    <title>B:CODE</title>
  </head>
  
  <style>
      h1{
          color:red;
      }
      
      div.content{
          margin: 0px auto;
          width:80%;

      }
      div.left{
          float: left;
          background-color:yellow;
          width:50%;
          height:500px;
      }
      #right {
          float: right;
          background-color:blue;
          width:50%;
          height:500px;
      }
  </style>
    
  <body>
    <h1>Hello World!!!</h1>
    <div class="content">
        <div class="left">left</div>
        <div id="right">right!!!
            <p><input type="text", name="code"/></p>
            <p><input type="submit", value="submit"/></p>
        </div>
      </div>
  </body>
</html>




참고한 사이트

- form
    
    <https://roksf0130.tistory.com/97>

- flask socket

   <https://flask-socketio.readthedocs.io/en/latest/>