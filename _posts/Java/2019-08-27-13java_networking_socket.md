---
layout: post
title: Java - 네트워킹(Socket)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>


## 네트워크 데이터 입력 및 출력

네트워크 대상(객체) 사이에 입/출력(InputStream/OutputStream)을 이용해 데이터를 입력하고 출력한다.

결국 네트워크를 이용한 데이터 전송은 로컬에서 이루어지든 아니든 InputStream/OutputStream에 네트워크만 연결하면 되는 것이다. 즉, 네트워크 상에서 이루어지는 네트워크 데이터의 입력과 출력은 `socket`을 통해 이루어지는데 이 socket의 사용법만 알면 쉽게 데이터의 전송을 할 수 있다는 것이다.


## Socket

네트워크 상에서 데이터를 주고받기 위한 장치이다.

- 두개의 객체(대상)이 있다고 하자
- 네트워크로 연결된 상태에서 InputStream/OutputStream 하는것을 데이터 전송이라고 한다.
- 서로 다른 객체들간에 데이터를 입출력 하는 장치가 바로 `socket`
  - 소켓은 전화기라고 생각하면 이해하기가 쉽다
  - 친구와 통화를 하는데 멀리 있음에도 불구하고 내 음성을 전화기는 입력도 하고 출력도 하고, 이 전화기가 socket의 역할을 한다고 생각하면 된다!


### Socket 클래스

서버는 클라이언트를 맞을 준비를 하고 있다가 클라이언트 요청에 반응한다.

- 클라이언트: 데이터를 요청하는 곳
  - 예) 검색을 하기 위해 라우저를 연다. (naver.com을 치는것)
- 서버: 데이터를 제공해주는 곳
  - 예) 네이버 그 자체가 서버

이렇듯 클라이언트는 네트워크 상에서 데이터 전송을 위한 장치(소켓)을 만들어줘야 하고 서버에서는 서버 소켓을 만들어 요청이 들어왔을때 서버소켓으로부터 데이터를 넘겨줄 수 있어야 한다.


#### ClientSocket

```java
public class MainClassSocket {
  public static void main(String[] args) {

    Socket socket = null;

    try {
      socket = new Socket("localhost", 9000);
      // localhost: pc의 ip주소 , 9000번 포트번호
      System.out.println("서버 연결");
      System.out.println(socket);
    }catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if(socket != null) socket.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

#### ServerSocket

```java
public class MainClassServerSocket {
  public static void main(String[] args){

    ServerSocket serverSocket = null; //서버 소켓 생성 > 클라이언트를 맞이할 준비가 된것
    Socket socket null;

    try {
      serverSocket = new ServerSocket(9000);
      System.out.println("클라이언트 맞을 준비 완료");

      socket = serverSocket.accept(); //클라이언트가 요청하면 accept() 자동실행 > 서버에서 소켓 반환
      System.out.println("클라이언트 연결");
      System.out.println(socket); // 그래서 서버에서 소켓의 정보를 출력할 수 있음
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (socket != null) socket.close();
        if (serverSocket != null) serverSocket.close();
    } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

### 양방향 통신

클라이언트와 서버는 InputStream, OutputStream을 통해 양방향 통신을 할 수 있다.


#### ClientClass

```java
public class ClientClass {

	public static void main(String[] args) {

		Socket socket = null;

		OutputStream outputStream = null;
		DataOutputStream dataOutputStream = null;

		InputStream inputStream = null;
		DataInputStream dataInputStream = null;

		Scanner scanner = null;

		try {

			socket = new Socket("localhost", 9000);
			System.out.println("서버 연결 완료");

			outputStream = socket.getOutputStream();
			dataOutputStream = new DataOutputStream(outputStream);

			inputStream = socket.getInputStream();
			dataInputStream = new DataInputStream(inputStream);

			scanner = new Scanner(System.in);

			while (true) {
				System.out.println("메시지 입력");
				String outMessage = scanner.nextLine();
				dataOutputStream.writeUTF(outMessage);
				dataOutputStream.flush();

				String inMessage = dataInputStream.readUTF();
				System.out.println("inMessage : " + inMessage);

				if(outMessage.equals("STOP")) break;

			}

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {

				if(dataOutputStream != null) dataOutputStream.close();
				if(outputStream != null) outputStream.close();
				if(dataInputStream != null) dataInputStream.close();
				if(inputStream != null) inputStream.close();

				if(socket != null) socket.close();

			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}
}
```

#### ServerClass

```java
public class ServerClass {

	public static void main(String[] args) {

		ServerSocket serverSocket = null;
		Socket socket = null;

		InputStream inputStream = null;
		DataInputStream dataInputStream = null;

		OutputStream outputStream = null;
		DataOutputStream dataOutputStream = null;

		try {

			serverSocket = new ServerSocket(9000);
			System.out.println("클라이언트 맞을 준비 완료");

			socket = serverSocket.accept();
			System.out.println("클라이언트 연결");
			System.out.println("socket: " + socket);

			inputStream = socket.getInputStream();
			dataInputStream = new DataInputStream(inputStream);

			outputStream = socket.getOutputStream();
			dataOutputStream = new DataOutputStream(outputStream);

			while (true) {
				String clientMessage = dataInputStream.readUTF();
				System.out.println("clientMessage : " + clientMessage);

				dataOutputStream.writeUTF("메시지 전송 완료");
				dataOutputStream.flush();

				if(clientMessage.equals("STOP")) break;
			}


		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {

				if(dataInputStream != null) dataInputStream.close();
				if(inputStream != null) inputStream.close();
				if(dataOutputStream != null) dataOutputStream.close();
				if(outputStream != null) outputStream.close();

				if(socket != null) socket.close();

			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}
}
```
