#OS

## Call Stack
[소프트웨어프로젝트 > 05장 Method](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2020%201학기%20소프/PPT.one#05장%20Method&section-id={4589C03E-6588-45C3-B6FF-8907E0947D09}&page-id={F073ECCA-5058-40FE-B6B9-954465BB4502}&object-id={0E778EF4-962B-0E06-39C2-555CC661DA53}&12)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21246&id=documents&wd=target%28PPT.one%7C4589C03E-6588-45C3-B6FF-8907E0947D09%2F05%EC%9E%A5%20Method%7CF073ECCA-5058-40FE-B6B9-954465BB4502%2F%29))
![[Call Stack 공용 공간.png]]
1. Caller에서 함수 호출문에 도달 
2. Callee의 **리턴값, 파라미터 크기만큼 공용 공간 할당**
	- 함수 Signature ➡ 공용 공간의 크기 잡을 수 있음 
	- **공용 공간**: Caller와 Callee가 공유하는 공간, 다른 위치는 
3. 공용 공간의 파라미터 메모리 공간에 Caller에서 넘겨준 인자값 저장
4. **Callee 실행을 위한 메모리** 할당
	- 한 줄씩 실행하며, 선언문이 있을 경우 메모리 할당 
5. Callee의 리턴문에 도달하면, 공용 공간의 리턴값 메모리 공간에 결과 저장
6. Caller의 호출문에서 리턴값을 다른 변수에 할당할 경우, 공용 공간으로부터 결과를 읽어와 할당
7. Callee 호출을 위해 할당했던 공간들 메모리 해제 