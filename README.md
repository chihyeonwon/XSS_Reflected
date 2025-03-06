## Reflected 기반 Cross Site Scripting

XSS(Cross-site Scripting)는 웹 상에서 가장 기초적인 취약점 공격 방법 입니다. 
크로스 사이트 스크립팅(XSS)은 애플리케이션에서 브라우저로 전송하는 페이지에서 사용자가 입력하는 데이터를 검증하기 않거나,
출력 시 위험 데이터를 무효화 시키지 않을 때 발생하게 됩니다. 
공격자는 의도적으로 브라우저에서 실행될 수 있는 악성 스크립트를 웹 서버에 입력 또는 출력 될수 있게 공격을 할수 있습니다.
XSS 취약점을 이용한 공격 방법은 크게 3가지로 분류됩니다.

1. XSS(Stored) - 저장 XSS공격- 접속자가 많은 웹 사이트를 대상으로 공격자가 XSS 취약점이 있는 웹 서버에 공격용 스크립트를 입력시켜 놓으면, 방문자가 악성 스크립트가 삽입되어 있는 페이지를 읽는 순간 방문자의 브라우저를 공격하는 방식    
   
2. XSS(Reflected) - 반사 XSS공격- 악성 스크립트가 포함된 URL을 사용자가 클릭하도록 유도하여 URL을 클릭하면 클라이언트를 공격하는 공격 방식     
 
3. XSS(DOM) - DOM 기반 XSS 공격- DOM 환경에서 악성 URL을 통해 사용자의 브라우저를 공격하는 방식

![image](https://github.com/user-attachments/assets/97164236-27cb-4db7-b250-f488ece71a6f)

DVWA 항목중 하나인 Reflected XSS 취약점 페이지 화면 입니다.Reflected XSS(반사 크로스 사이트 스크립팅)   
취약점은 악성 스크립트 코드를 서버에 저장하지 않고 사용자의 요청 웹 서버의 반환 데이터를 이용합니다.    

#### 공격방법

1. 공격자는 웹 게시판 서비스를 이용하거나, 쪽지 서비시를 이용하여 스크립트 구문을 노출시킵니다.      
      
2. 데이터베이스에 저장되는 Stored XSS와 달리, 사용자의 클릭을 유도하는 형식입니다.     
사용자가 게시판에 링크된 주소나 쪽지로 링크된 주소를 클릭할경우 공격자가 심어 놓은 악성코드가 실행됩니다.

![image](https://github.com/user-attachments/assets/6aaf6ad8-c523-4e05-89c2-b085d555981a)

Low레벨의 소스코드 입니다. 소스코드를 보시면 이름을 입력한 후 해당 스크립트는 입력 값을 돌려 줍니다.     
여기서 문제가 되는 부분은 사용자가 입력한 값이 비정상적일 경우에도 값을 돌려준다는 것 입니다. 그럼 해당 화면에 값을 입력해 보도록 하겠습니다.      

![image](https://github.com/user-attachments/assets/f05d0c6f-ee57-4fba-a780-241ddfcd4570)

HTML의 h 태그를 이용하여 값을 입력해 보았습니다. h 태그는 HTML에서 제목 및 헤드라인을 표시할때 주로 사용되는 태그 입니다.    

![image](https://github.com/user-attachments/assets/a6fe419b-8b4f-4887-a578-f73cfd4d8a2c)

스크립트 구문을 입력하여 alert 창을 띄운 모습입니다.공격구문을 몇가지 예시로 들어 보겠습니다.

<script>alert('XSS TEST')</script><SCRIPT>alert('XSS TEST')</SCRIPT><img src="" onerror="alert('XSS TEST')"><script>alert(document.cookie)</script><img src=javascript:alert(“XSS alert!”)>
더 자세한 사항은 아래 링크를 참조 바랍니다.       
(owsap XSS Filter Evasion Chat Sheet)[https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet]     
해당 페이지의 URL 주소를 확인하시면 GET 메소드를 이용하기 때문에 매개 변수를 확인할수 있습니다.      
http://자신의 IP/vulnerabilities/xss_r/?name=값을 입력해본후 name= 부분의 변경되는 모습을 확인하시고 해당부분에 공격 스크립트를 한번씩 넣어 보시기 바랍니다.      
이미지 태그를 이용하여 Reflected XSS 테스트 페이지에서 CSRF 공격도 가능합니다.<img src="http://자신의 IP/vulnerabilities/csrf/?password_new=admin&password_conf=admin&Change=Change">     
[Submit] 버튼을 클릭하면 'Hello' 메시지가 뜨면서 공격이 성공하고, 무차별 대입 공격 테스트 페이지로 이동하여 공격이 성공하였는지 확인해 보시기 바랍니다.    

![image](https://github.com/user-attachments/assets/d4be8604-ef98-484c-b51a-69b7af0cdfb5)

해당 페이지는 테스트페이지라 상관이 없지만 Reflected XSS 취약점의 경우는 서버에 저장되지 않고 웹 브라우저를 통하여 메시지를 반환하기 때문에             
웹 브라우저로 반환되는 메시지 매개변수 값을 악의적인 데이터로 변경하여 보낼경우 서비스에 악영향을 미칠수 있으니 유의하시기 바랍니다.       
  
## More Information
https://owasp.org/www-community/attacks/xss/
https://owasp.org/www-community/xss-filter-evasion-cheatsheet
https://en.wikipedia.org/wiki/Cross-site_scripting
http://www.cgisecurity.com/xss-faq.html
http://www.scriptalert1.com/

