---
title : "[Algorithm Site 만들기]-2 Flask Socket"
categories:
  - bucket
---

통신의 기본 채팅서버 만들기

<https://codeburst.io/building-your-first-chat-application-using-flask-in-7-minutes-f98de4adfa5d>

위 사이트의 내용을 99.9999% 가지고 왔다.

Thank your post.

## 1. Flask Socket 설치

~~~
conda install -y -c conda-forge flask-socketio
~~~
## 2. Server

main.py

~~~python
from flask import Flask , render_template
from flask_socketio import SocketIO, send

app = Flask(__name__)
app.config['SECRET_KEY'] = 'BCODE_Flask'
socketio = SocketIO(app)

@app.route("/")
def main():
     return render_template("index.html")

def messageReceived(methods=['GET', 'POST']):
    print('message was received!!!')

@socketio.on('my event')
def handle_my_custom_event(json, methods=['GET', 'POST']):
    print('received my event: ' + str(json))
    socketio.emit('my response', json, callback=messageReceived)
    
if __name__ == '__main__':
    socketio.run(app, host = "0.0.0.0", port=5000)
~~~

## 3. Client


client.html

~~~html
<!DOCTYPE html>
<html lang="ko">
<head>
  <title>Flask_Chat_App</title>
</head>
<body>

  <h3 style='color: #ccc;font-size: 30px;'>No message yet..</h3>
  <div class="message_holder"></div>

  <form action="" method="POST">
    <input type="text" class="username" placeholder="User Name"/>
    <input type="text" class="message" placeholder="Messages"/>
    <input type="submit"/>
  </form>

  <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
  <!-- 공식 홈페이지 내용 적용 -->
  <!-- <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.7.3/socket.io.min.js"></script> -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js" 
        integrity="sha256-yr4fRk/GU1ehYJPAs8P4JlTgu0Hdsp4ZKrx8bDEDC3I=" 
        crossorigin="anonymous"></script>

  <script type="text/javascript">

    var socket = io.connect('http://0.0.0.0:5000');

    // 서버 접속
    socket.on( 'connect', function() {
      socket.emit( 'my event', {
        data: 'User Connected'
      } )
      var form = $( 'form' ).on( 'submit', function( e ) {
        e.preventDefault()
        let user_name = $( 'input.username' ).val()
        let user_input = $( 'input.message' ).val()
        socket.emit( 'my event', {
          user_name : user_name,
          message : user_input
        } )
        $( 'input.message' ).val( '' ).focus()
      } )
    } )

    // 받은 메시지
    socket.on( 'my response', function( msg ) {
      console.log( msg )
      if( typeof msg.user_name !== 'undefined' ) {
        $( 'h3' ).remove()
        $( 'div.message_holder' ).append( '<div><b style="color: #000">'+msg.user_name+'</b> '+msg.message+'</div>' )
      }
    }) 
  </script>

</body>
</html>
~~~
<figure>
  <img src="/assets/images/2019-07-29-Flask_2_Socket/chat.png">
  <figcaption>채팅화면</figcaption>
</figure>


