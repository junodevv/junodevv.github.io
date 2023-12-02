---
title: "[Docker] 도커 실습 8 - Redis을 활용한 마이크로 서비스의 세션 공유"
category: Cloud
tags: cloud docker 
---

2023-11-08 클라우드 프로그래밍 10주차 강의

-----

목차
- 마이크로 서비스 세션 공유

---

# 마이크로 서비스 세션 공유 

시나리오

- 로그인 화면 생성
- user DB 테이블 생성 및 데이터 삽입
- 로그인 처리 및 redis 세션 생성 파일 생성
- tasks-list.php 세션 공유 내용 추가
- app.js 세션 공유 내용 추가

### 로그인 화면 생성
- /home/master/html/login.html

```html
<!doctype html>
<html>
<head>
<meta charset='utf-8'>
<meta name='viewport' content='width=device-width, initial-scale=1'>
<title>로그인 페이지</title>
<link
    href='https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css'
    rel='stylesheet'>
<link href='#' rel='stylesheet'>
<script type='text/javascript'
    src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js'></script>
<style>
::-webkit-scrollbar {
    width: 8px;
}
/* Track */
::-webkit-scrollbar-track {
    background: #f1f1f1;
}
/* Handle */
::-webkit-scrollbar-thumb {
    background: #888;
}
/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
    background: #555;
}
body {
    background-color: #f9f9fa;
}
.flex {
    -webkit-box-flex: 1;
    -ms-flex: 1 1 auto;
    flex: 1 1 auto
}
@media ( max-width :991.98px) {
    .padding {
        padding: 1.5rem
    }
}
@media ( max-width :767.98px) {
    .padding {
        padding: 1rem
    }
}
.padding {
    padding: 5rem
}
.card {
    background: #fff;
    border-width: 0;
    border-radius: .25rem;
    box-shadow: 0 1px 3px rgba(0, 0, 0, .05);
    margin-bottom: 1.5rem
}
.card-header {
    background-color: transparent;
    border-color: rgba(160, 175, 185, .15);
    background-clip: padding-box
}
.card-body p:last-child {
    margin-bottom: 0
}
.card-hide-body .card-body {
    display: none
}
.form-check-input.is-invalid ~.form-check-label, .was-validated .form-check-input:invalid
    ~.form-check-label {
    color: #f54394
}
</style>
</head>
<body className='snippet-body'>
    <div id="content" class="flex">
        <div class="">
            <div class="page-content page-container" id="page-content">
                <div class="padding">
                    <div class="row">
                        <div class="col-md-6">
                            <div class="card">
                                <div class="card-header">
                                    <strong>Login to your account</strong>
                                </div>
                                <div class="card-body">
                                    <form>
                                        <div class="form-group">
                                            <label class="text-muted" for="myEmail">Email
                                                address</label><input type="email" class="form-control"
                                                id="myEmail" aria-describedby="emailHelp"
                                                placeholder="Enter email"> <small id="emailHelp"
                                                class="form-text text-muted">We don't share email
                                                with anyone</small>
                                        </div>
                                        <div class="form-group">
                                            <label class="text-muted" for="myPassword">Password</label><input
                                                type="password" class="form-control"
                                                id="myPassword" placeholder="Password"> <small
                                                id="passwordHelp" class="form-text text-muted">your
                                                password is saved in encrypted form</small>
                                        </div>
                                        <button type="submit" class="btn btn-primary">Submit</button>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <script type='text/javascript'
        src='https://stackpath.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.bundle.min.js'></script>
    <script>
        $(function(){
        
            $("form").on("submit", function(event) {
                event.preventDefault(); // 기본 동작 중단
        
                // 입력 값을 가져옴
                var email = $("#myEmail").val();
                var password = $("#myPassword").val();
        
                // AJAX로 POST 요청을 보냄
                $.ajax({
                    url: "/user/login.php", // 로그인 처리를 하는 서버의 URL
                    type: "POST",
                    dataType: "json", // 서버에서 받는 데이터 타입
                    data: {
                        email: email,
                        password: password
                    },
                    success: function(data) {
                        // 로그인 성공 시 처리
                        alert(data.msg);
                        if(data.result == "ok"){
                            window.location.href = "/";
                        }
                        console.log("Login successful:", data);
                    },
                    error: function(xhr, status, error) {
                        // 로그인 실패 시 처리
                        console.log("Login failed:", error);
                    }
                });
            });        
            
        });
    </script>
</body>
</html>
```

        - 이메일 정보를 입력하는 input과 password 정보를입력하는 input이 있음
        - form의 submit 이벤트가 발생하면 기존의 동작을 멈추고 ajax 통신으로 로그인 처리를 시작 -> /user/login.php 서버url에 접속해서 POST방식으로 입력된 값을 보내고 json으로 데이터를 받아온다.

### user DB 테이블 생성 및 데이터 삽입

![image](https://github.com/junodevv/junodevv.github.io/assets/126752196/efbdf458-189e-4392-8763-22efd2e0e3a9)


### redis 세션 연동 설정
- /home/master/php/user/database.php 수정
- /home/master/php/task/database.php 수정

```php
//header("Access-Control-Allow-Origin: *"); // Cross origin 
$mysql_host = "mysql"; // 172.26.0.2
$mysql_user = "php-mysql";
$mysql_password = "123456";
$mysql_db = "php-mysql";
   
$connection = mysqli_connect(
    $mysql_host, $mysql_user, $mysql_password, $mysql_db
);
include_once("./redis_session.php");
```

### 로그인 처리 및 redis 세션 생성 파일 생성
- /home/master/php/user/login.php

```php
include_once("./database.php"); //DB에 연결
$ret = array(); // php에서 array는 곧 json이다? json으로 만들어주나?
// 세션 시작
session_start();
if(!isset($_POST['email']) || $_POST['email'] == ""){
    $ret['result'] = "no";
    $ret['msg'] = "이메일 정보가 없습니다.";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}
if(!isset($_POST['password']) || $_POST['password'] == ""){
    $ret['result'] = "no";
    $ret['msg'] = "패스워드 정보가 없습니다.";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}
$query = "SELECT * from user where email='{$_POST['email']}' and pass='{$_POST['password']}'";
$result = mysqli_query($connection, $query);
if (!$result) {
    $ret['result'] = "no";
    $ret['msg'] = "데이터베이스 연결 오류";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}
$row = mysqli_fetch_array($result);
if($_POST['email'] == $row['email'] &&  $_POST['password'] == $row['pass']){
    $_SESSION['username'] = "홍길동";
    $ret['result'] = "ok";
    $ret['msg'] = "정상 로그인이 되었습니다.";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}else{
    $ret['result'] = "no";
    $ret['msg'] = "로그인 정보가 틀립니다.";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}
```
- 로그인 테스트 http://gctask.com/login.html

<img width="600" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d215cdf9-dfbf-4cb8-95f9-95f5cd50d22b">

### tasks-list.php 세션 공유 내용 추가
- /home/master/php/task/tasks-list.php

```php
include_once("./database.php");
$ret = array();
// 세션 시작
session_start();
if (!isset($_SESSION['username']) || $_SESSION['username'] == "") {
    $ret['result'] = "no";
    $ret['msg'] = "로그인을 해주십시오.";
    $jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
    echo $jsonstring;
    exit;
}
$query = "SELECT * from task";
$result = mysqli_query($connection, $query);
if (!$result) {
    die('Query Failed' . mysqli_error($connection));
}
$json = array();
while ($row = mysqli_fetch_array($result)) {
    $json[] = array(
        'name' => $row['name'],
        'description' => $row['description'],
        'id' => $row['id']
    );
}
$ret['result'] = "ok";
$ret['msg'] = "정상적으로 데이터를 가져왔습니다.";
$ret['tasks'] = $json;
$ret['username'] = $_SESSION['username'];
$jsonstring = json_encode($ret, JSON_UNESCAPED_UNICODE);
echo $jsonstring;
```

### app.js 세션 공유 내용 추가

- /home/master/html/app.js 에서 `feachTasks`()함수 수정

```js
    // Fetching Tasks
    function fetchTasks() {
        $.ajax({
            url: hostNameServerUrl+'tasks-list.php',
            type: 'GET',
            dataType: "json", // 서버에서 받는 데이터 타입
            success: function(tasks) {
                //const tasks = JSON.parse(response);
                
                if(tasks.result == "ok"){
                    const list = tasks.tasks;
                    let template = '';
                    list.forEach(task => {
                        template += `
                        <tr taskId="${task.id}">
                            <td>${task.id}</td>
                            <td><a href="#" class="task-item"> ${task.name} </a></td>
                            <td>${task.description}</td>
                            <td>
                                <button class="task-delete btn btn-danger" data-taskId="${task.id}" >Delete</button>
                            </td>
                        </tr>
                        `
                    });
                    $('#tasks').html(template);
                    $('.user-name').remove();
                    $(".table-bordered").before("<p class='user-name'>"+tasks.username+"님의 할일</p>");
                }else{
                    alert(tasks.msg);
                    window.location.href = "/login.html";
                }
            
            }
        });
    }
```

- 최종 로그인 테스트 및 세션 공유 확인
- http://gctask.com/login.html

        이때 이전과 같은 화면이 나온다면 캐시 삭제 후 다시 진행해보면 된다.

<img width="600" alt="Pasted Graphic 4" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e661b76c-b3aa-4dc9-945f-3903233e3428">

성공!

---

# 끝

## reference

[교수님블로그 및 강의, hull.kr](https://hull.kr/cloud/19?page=1)