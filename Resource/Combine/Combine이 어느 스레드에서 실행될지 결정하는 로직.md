**피기**

- 개발 -> 기획 -> 디자인 -> 개발

  

**어떤 스레드에서 이 스트림을 실행하게 할건지 설정하는 방법**

Subject

.receive(on:_)   // 1?   실행 … 

.subscribe(on:_) // 2?   구독 …

  

  

**Principle of dviding upstream and downstream**

둘의 차이를 이해하는게 중요하다. 

  

둘을 생략할 수도 있는데, 현재 실행되고 있는 스레드를 따라가는데, 

샘플 코드에서는 UI Event에서 실행이 되었으니까, Main Thread 에서 실행이 되는 거였음 

카메라 권한이 바꿨을 때 UI에 영향을 줘야하는데, 

.onChange() 는 무적권 main thread 에서 돌아가야함 

  

Runloop.main 과 DispatchQueue.main의 차이

  

**Case of Just**

초기값이 있는애들은 Subscribe(on:_)을 따라간다. 

  

초기값이 없는 애들은 receive(on:_)을 따라간다. 

  

  

**.onChnage 는 Main Thread에서 실행되는게 아니면 아예 안호출된다.**

  

  

  

참조

[https://velog.io/@ictechgy/Combine-subscribeon-VS.-receiveon](https://velog.io/@ictechgy/Combine-subscribeon-VS.-receiveon)

[https://sujinnaljin.medium.com/combine-subscriber-409023bc6d89](https://sujinnaljin.medium.com/combine-subscriber-409023bc6d89)