---
layout: post
title:  "python thread lock"
comments: true
description: "Python"
author: SeungHyeon Tak
date:   2020-01-31 12:07:00 +0700
categories: [python]
tags: [python]
keywords: "python thread lock"
---
## python thread lock


### lock

다수의 스레드 실행으로 공유 자원에 접근할 때 필요한 메커니즘이다.

예를 들어 1개의 공용 컴퓨터를 쓰는 방과 여러명의 사람을 생각할 수 있다.

한명이 컴퓨터를 사용하려면 방안에 들어가서 다른 사람이 볼 수 없게 문을 닫아야 한다.(잠궈야함)

python의 lock은 문을 잠그는 동기화 프리미티브다.

즉, 락 / 언락 상태가 있고 언락 상태에서만 락을 요청 할 수 있다.

예제 코드로 살 펴 보자

```python
import threading
import time

count = 1

def worker_a():
	global count
	while count < 10:
		count += 1
		print(f'worker_a : {count}')
		time.sleep(1)
		
def worker_b():
	global count
	while count > -10:
		count -= 1
		print(f'worker_b : {count}')
		time.sleeo(1)
		
def main():
	t = time.time()
	thread1 = threading.Tread(target=worker_a)
	thread2 = threading.Tread(target=worker_b)
	thread1.start()
	thread2.start()
	thread1.join()
	thread2.join()
	
	t_time = time.time()
	print(f"total_time{t_time - t}")
	
if __name__=="__main__":
	main()
```

위의 코드는 락 없이 아무렇게나 count변수에 접근할 수 있는 코드이다.

이렇게 진행되면 두 스레드에서 서로 감소, 증가가 동시에 되기 때문에

스레드가 종료 되지 않고 두개의 스레드는 무한정 돌아가게 된다.

락을 추가해서 안전한 상태로 접근 하게 해야한다.

```python
import threading
import time

count = 1
lock = threading.Lock()

def worker_a():
	global count
	lock.acquire()
	try:
    while count < 10:
      count += 1
      print(f'worker_a : {count}')
      time.sleep(1)
	finally:
		lock.release()
		
def worker_b():
	global count
	lock.acquire()
	try:
    while count > -10:
      count -= 1
      print(f'worker_b : {count}')
      time.sleeo(1)
	finally:
		lock.release()
		
def main():
	t = time.time()
	thread1 = threading.Tread(target=worker_a)
	thread2 = threading.Tread(target=worker_b)
	thread1.start()
	thread2.start()
	thread1.join()
	thread2.join()
	
	t_time = time.time()
	print(f"total_time{t_time - t}")
	
if __name__=="__main__":
	main()
```

위의 코드를 보면 이제 서로 락을 제한해서 실행하면

먼저 리소스에 접근한 스레드가 while 조건을 마칠때 까지 다른 스레드는 count에 접근 하지 못한다.

즉, 한개의 스레드가 종료하고 그 다음에 다른 스레드가 동작하고 종료하게 된다.

*****

* 프리미티브(Primitive)
  * 공학적인 용어로는 어떤 동작을 실행하거나, 수행될 동작에 대한 통보의 의미를 가짐

