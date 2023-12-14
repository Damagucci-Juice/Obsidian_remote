[vimeo](https://vimeo.com/870890508?share=copy)
Miro board link: [https://miro.com/app/board/uXjVMhDE2io=/](https://miro.com/app/board/uXjVMhDE2io=/ "https://miro.com/app/board/uxjvmhde2io=/") Password: 12345678
![[Task, DispatchQueue By Team.png]]![[Task, DispatchQueue by Gucci.png]]

serial은 한번에 하나씩 하는 것 처럼 보이는데, sync도 하나의 하나처럼 실행이 되는 것 처럼 보인다
이게 둘이 어떻게 다릅니까?
serial, concurrent는 큐에서 이야기하는 내용이고 
sync, async는 작업에서 이야기하는 내용이다. 


같은 serial 큐라도 async 작업을 넣는지, sync 작업을 넣는지 다르다. 
마찬 가지로 concurrent 큐라도 async 작업을 넣는지 sync 작업을 넣는지 다르다. 


main Thread: UI와 관련된 모든 작업을 해요. 
일반적인 Dispatchqueue는 시리얼큐로 동작을 합니다. 
