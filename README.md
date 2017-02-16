# GitTest-Repository
20일차

Test70

이벤트와 리스너  : 

각각의 태그 객체는 고유의 이벤트가 있다.
이벤트가 발생했을 때 호출될 함수의 이름을 정해놓았다.

Test71

이벤트와 리스너 : 주요 이벤트 확인

마우스 커서가 태그 영역에 집입할 때 
     자식 태그 영역에 있다가 나올 때는 무시한다.

var tag = document.querySelector("#header");

tag.onmouseenter = function(event) {
     console.log("onmouseenter");
}

onmouseenter 마우스 커서가 태그 영역에 진입할 때 
     =>자식 태그영역에 있다가 나올때는 무시한다.

tag.onmouseover = function(event) {
     console.log("onmouseover");
}

마우스 커서가 태그 영역에 진입할때
     자식 태그 영역이든 바깥이든 현재 태그의 표면에 진입하면 발생한다.

tag.onmouseleave = function(event) {
     console.log("onmouseleave")
}

마우스 커서가 태그 영역에서 나올 때
     자식 태그 영역에 있다가 나올 때는 무시한다.


tag.onmouseout = function(event) {
console.log("onmouseout")
}

/마우스 커서가 태그 영역에서 나올 때
//=> 자식 태그 영역이든 바깥이든 현재 태그의 표면에서 벗어날 때 발생한다.



Test72

onclick 이벤트를 사용한다면


#header > button.onclick
div#header.onclick
div#main.onclick
body.onclick

body 안에 메인안에 header안에 버튼을 클릭하면
안에서 부터 호출된다.

Test73

이벤트와 리스너 : 버블 단계로 이벤트를 전달하는 것을 막기
     event.stopPropagation() : 버블 단계로의 이벤트 전달을 막는다.
     event.stopImmediatePropagation() : 즉시 이벤트 전달을 멈춘다.
                              같은 객체에 등록된 다른 리스너의 호출까지도 막는다.

Test74

이벤트와 리스너 : 여러 개의 리스너 등록하기
     onxxxx 프로퍼티에는 오직 한 개의 리스너만 등록할 수 있다.
     여러 개의 리스너를 등록하려면 다음의 함수를 사용해야 한다.

     위에 전달되지않게하기 위해서
     event.stopPropagation()

     addEventListener(이벤트명, 리스너)

var btn = document.querySelector("#header > button")
btn.addEventListener("click" , function(event) {
     console.log("button.onclick")
     event.stopPropagation();
}

Test75

span.addEventListener("click", function(event) {
     console.log("span누름");
     event.stopPropagation();
}
위에 전달되지 하고 독립적실행

span.addEventListener("click", function(event) {
  console.log("span누름");
  event.stopImmediatePropagation();
}
위에 전달되지않고 같은 tag에 여러개의 이벤트가 있다면 
다음 이벤트는 실행하지않는다.

Test76

임의로 이벤트 발생시키기 : event.dispatchEvent()
     애플리케이션에서 임의로 특정 태그 객체에 대해 이벤트를 발생시킬 수 있다.

var h1 = document.querySelector("#contents >h1");

h1.addEventListener("click2" , function(event) {
     h1.style['background-color'] = "red";
     or
      h1.style.backgroundColor = "white";    
});

document.querySelector("#loginBtn").onclick = function(event) {
     var event3 = new Event("click2");
     
     h1.dispatchEvent(event3);
}





document.querySelector("#main").onclick = function(event) {
  console.log("div#main.onclick")
}



Test77

비동기 요청(AJAX : Asynchronous JavaScript and XML)

XMLHttpRequest() 생성자를 통해 만든 객체를 사용한다.
HTTP 요청을 수행할 수 있다.
동기 또는 비동기로 요청 할 수 있다.
GET 또는 POST, DELETE 등 요청 가능.


	1. 비동기 요청을 수행할 객체를 준비한다.
		1. var xmlHttpRequest = new XMLHttpRequest();

	2. 서버에 연결한다.
		1. open (요청, URL, 비동기여부)
		2. xmlHttpRequest.open("GET","test77.jsp",false)

	3. 서버에 요청을 보낸다.
		1. 동기(synchronous) 방식으로 요청할 때는 서버 응답이 완료되어야만 리턴한다.
		2. xmlHttpRequest.send()

	4. 서버가 보낸 값을 출력해보자!
		1. console.log(xmlHttpRequest.responseText)





Test79

비동기 요청 - 동기 요청의 문제점
     서버에서 응답이 올 때까지 다른 작업을 수행할 수 없다.

Test80

비동기 요청 - 동기 요청의 문제점 해결하기  = > 비동기 요청하기
     서버에 요청한 후 응답을 기다리지 않고 즉시 리턴한다.
     서버에서 응답이 오면 리스너를 통해 보고 받는다.

document.querySelector("#loginBtn").onclick =function(event) {
     var xhr = new XMLHttpRequest();
     
     xhr.onreadystatechange = function(event) {
          console.log(xhr.responseText);
     }
     xhr.open("GET" , "test79.jsp", true)
     
     xhr.send() // 비동기요청이기 때문에 서버에 요청을 한후 바로 리턴
     console.log("서버에 요청했다.");
}

Test81

비동기 요청 - onreadystatechange 이벤트
     이 이벤트는 요청 상태가 바뀔 때 마다 호출된다.

     상태
     0 : 객체 생성
     1 : open() 호출되었다.
     2 : send() 호출되었다.
     3 : 서버로부터 데이터를 받는 중이다.
     4 : 서버 응답이 완료되었다. 모든 데이터를 받음

서버로부터 응답이 완료된 후 어떤 작업을 하고 싶다면, 4번 상태일때 수행하라

document.querySelector("#loginBtn").onclick = function(event) {
     var xhr = new XMLHttpRequest();

     xhr.onreadystatechange = function(result) {
               console.log(xhr.readyState)
     if(xhr.readyState==4) {
          console.log(xhr.responseText);
     }
     }
     xhr.open("GET","...jsp",true);
     xhr.send();
     console.log("서버연결됨");
}


Test82

비동기 요청  - 서버에 잘못된 요청을 했을 때를 검사해야 한다.
     응답의 상태 값을 확인하라
     상태 코드가 "200"일 경우에만 작업하도록 하라!

xhr.onreadystatechange = function(result) {
     ....

     서버에 상태값을 계속 바꿔준다. 그러므로 
     if(xhr.readyState==4) {
          
     }
}

Test83

비동기 요청 - 페이지의 내용을 동적으로 바꾸기

Test83.jsp에 
<li>자바 프로그래밍</li>
<li>C/C++ 프로그래밍</li>
<li>윈도우 프로그래밍</li>
<li>리눅스 프로그래밍</li>
<li>IoT(Internet of Things) 프로그래밍</li>
이 있다.

document.querySelector("#contents >ul").innerHTML = xhr.responseText;
Test84

비동기 요청 - 서버에서 JSON 형식의 문자열을 받아서 자바스크립트로 바꾼 후 작업 수행

document.querySelect("#loginBtn").onclick = function(event) {
     var xhr = new XMLHttpRequest();
     
     xhr.onreadystatechange = function(result) {
          if(xhr.readyState != 4)
               return;

          if(xhr.status != 200){
               alert("서버에 잘못 요쳥했습니다")
               return;
          }
          
 
           var arr = JSON.parse(xhr.responseText)

  console.log(arr)
  var ulTag = document.querySelector("#contents > ul")

  var contents = ""

  for (var i in arr) {
  contents += "<li>" + arr[i] + "</li>\n"
  }

  //console.log(contents)
  ulTag.innerHTML = contents
     }
xhr.open("GET", "test84.jsp", true)
  xhr.send()

}

Test85

비동기 요청 
     서버에서 JSON 형식의 문자열을 받아서 DOM API를 사용하여 태그 객체를 만든 ㅎ 
     기존 태그에 붙인다.


document.querySelector("#loginBtn").onclick = function(event) {
  var xhr = new XMLHttpRequest();
  xhr.onreadystatechange = function(result) {
  if (xhr.readyState != 4)
  return;

  if (xhr.status != 200) {
  alert("서버에 잘못 요청했습니다.")
  return;
  }

  var arr = JSON.parse(xhr.responseText)

  var ulTag = document.querySelector("#contents > ul")
  var li;
  for (var i in arr) {
  li = document.createElement("li") // 태그 객체 생성
  li.innerHTML = arr[i] // 태그 객체에 콘텐츠 저장
  ulTag.appendChild(li) // li 태그 객체를 ul의 자식 태그로 붙인다.
  }

  }
  xhr.open("GET", "test84.jsp", true)
  xhr.send()
}


