#Algorithm  #MultiCore 
[멀코 > lab1_prob2.pptx](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2023%201학기%20멀코/PPT.one#lab1_prob2.pptx&section-id={627E5AF0-ABBC-4CD0-BB92-F6ACBFF97C34}&page-id={91094A59-A619-428D-9860-76A50C5F3E87}&object-id={9C3361EC-F828-09A6-06FF-5C17027F5C49}&34)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21307&id=documents&wd=target%28PPT.one%7C627E5AF0-ABBC-4CD0-BB92-F6ACBFF97C34%2Flab1_prob2.pptx%7C91094A59-A619-428D-9860-76A50C5F3E87%2F%29))
![[Merge Sort in Multicore.png]]
- parallel 이후, 각 thread의 결과를 reduce할 때 [[Divide & Conquer]] 활용 가능

## 성능 개선
### Sequential Cutoff
📌 [[재귀]] 대신 iteration으로 구현하는 게 더 효율적인 data size의 기준
- 🔎 500~1000, but 응용에 의존적
- 실제로는 너무 많은 thread가 생기면 오히려 부담 (자원 낭비, overhead)
- ↔️ 이론적, T.C는 동일
### Parent thread 활용
parent thread에서 sub-prob를 처리해주는 child threads를 생성 및 시작시킨 뒤,
- parent는 children 전부 끝날 때까지 **대기**
- ↔️ **child처럼 활용** ➡️ 생성되는 thread 개수 -1씩 가능