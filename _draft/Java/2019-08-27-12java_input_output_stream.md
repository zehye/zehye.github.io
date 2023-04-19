---
layout: post
title: Java - 입력과 출력(Inputstream, Outputstream)
category: Java
tags: [Java]
comments: true
---

> 개인적인 연습 내용을 정리한 글입니다.      
> 더 좋은 방법이 있거나, 잘못된 부분이 있으면 편하게 의견 주세요. :)

<hr>

## 입/출력(I/O)란?

- 입력: 다른곳의 데이터를 가져오는 것
- 출력: 다른 곳으로 데이터를 내보내는 것

일반적으로 우리가 컴퓨터를 할때, 키보드에 데이터를 치는 것은 입력이라하고 입력된 데이터들이 모니터를 통해 보여지는것을 출력이라고 생각하면 의미가 조금 쉽게 느껴진다.

이러한 데이터들이 오고가는 길을 `stream`이라고 한다. 즉, 프로그램 작성에서 데이터를 입/출력하기 위해서는 입/출력하려는 대상의 stream 연결만 해주면 된다.


### 입/출력 기본 클래스

입/출력에 사용되는 기본 클래스는 1바이트 단위로 데이터를 전송하는 `InputStream, Outputstream`이 있다. 프로그램으로부터 어떤 대상을 입력하고 출력할때는 스트림을 만들어놓는데 입력을 할때에는 `InputStream`, 출력을 할때는 `Outputstream`을 놓는다. 이들 모두 추상메서드를 갖는다.


### FileInputStream / FileOutputStream

파일에 데이터를 읽고 쓰기 위한 클래스로 read(), write()메서드를 이용한다.  

- FileInputStream > read() : 1바이트 단위로 데이터를 읽어옴
- FileInputStream > read(byte[]) : 매개변수로 바이트 배열을 줌으로써 배열의 크기만큼 읽어옴
  - 배열의 크기가 10이라면 10바이트씩 한번에 묶음을 해서 읽어옴
  - 속도면에서 조금 더 효율적임

- FileOutputStream > write(byte[] b) : 쓰고자 하는 데이터 한번에 쓰기 가능
- FileOutputStream > write(byte[], int off, len) : 글을 출력시 범위를 설정 가능(시작점, 길이)

```java
// read()
import java.io.FileInputStream;
public class MainClass {
  public static void main(String[] args){

    InputStream inputStream = null;
    try {
      inputStream = new FileOutputStream("파일의 경로");
      int data = 0;

      while(true) {
        try {
          data = inputStream.read();
        } catch (IOException e) {
          e.printStackTrace();
        }
        if(data == -1) break;
        System.out.println(data);
      }
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if(inputStream != null) inputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

```java
// read(byte[])
import java.io.FileInputStream;
public class MainClass {
  public static void main(String[] args){

    InputStream inputStream = null;
    try {
      inputStream = new InputStream("파일의 경로");
      int data = 0;
      byte[] bs = new byte[3]; //3바이트 만큼

      while(true) {
        try {
          data = inputStream.read(bs);
        } catch (IOException e) {
          e.printStackTrace();
        }
        if(data == -1) break;
        System.out.println(data);
        for (int i = 0; i <bs.length; i++) {
          System.out.println(bs[i]);
        }
      }
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if(inputStream != null) inputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

```java
// write(byte[] b)
public class MainClass {
  public static void main(String[] args) {
    Outputstream outputStream = null;

    try {
      outputStream = new Outputstream("파일을 쓸 경로");
      String data = "Hello java world!!";
      byte[] arr = data.getBytes(); // 전체 바이트 다 가지고 온다.

      try {
        outputStream.write(arr);
      } catch (IOException e) {
        e.printStackTrace();
      }
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if(outputStream != null) outputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

```java
// write(byte[], int off, len)
public class MainClass {
  public static void main(String[] args) {
    Outputstream outputStream = null;

    try {
      outputStream = new Outputstream("파일을 쓸 경로");
      String data = "Hello java world";
      byte[] arr = data.getBytes();

      try {
        outputStream.write(arr, 0, 5); // arr의 0에서 5까지만 쓰기
      } catch (IOException e) {
        e.printStackTrace();
      }
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if(outputStream != null) outputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

### 파일 복사

파일 입/출력 클래스를 이용해 파일을 복사할 수 있다.

```java
public class MainClass {
  public static void main(String[] args) {
    InputStream inputStream = null;
    Outputstream outputStream = null;

    try {
      inputStream = new InputStream("파일 경로");
      outputStream = new Outputstream("파일 쓸 경로");

      byte[] arr = new byte[3];

      while(true) {
        int len = inputStream.read(arr); // 읽고
        if(len == -1) break;
        outputStream.write(arr, 0, len); // 쓰고
      }
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      if (inputStream != null) {
        inputStream.close();
      } catch (Exception e) {
        e.printStackTrace();
      }
    } if (outputStream != null) {
      outputStream.close();
    } catch (Exception e) { e.printStackTrace(); }
  }
}
```

### DataInputStream / DataOutputStream

바이트 단위의 입출력을 개선해 문자열을 좀 더 편리하게 다룰 수 있다.

기존 FileInputStream, FileOutputStream은 바이트 단위로 데이터를 읽고 쓰기 때문에 아스키코드의 바이트값을 뽑아냈다. 이는 인간이 쓰는 문자가 아니기 때문에 이러한 것을 해결하기 위해 문자열/문자로 읽고 쓰는 방법을 만들어 낸 것.


```java
public class MainClass {

	public static void main(String[] args) {

		String str = "Hello Java World!!";
		OutputStream outputStream = null;
		DataOutputStream dataOutputStream = null;

		try {
			outputStream = new FileOutputStream("파일을 쓸 경로"); // FileOutputStream 먼저 출력
			dataOutputStream = new DataOutputStream(outputStream); // outputStream을 매개변수로 받음

			dataOutputStream.writeUTF(str); // 문자 인코딩 > writeUTF

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(dataOutputStream != null) dataOutputStream.close();
			} catch (Exception e) {
				e.printStackTrace();
			}

			try {
				if(outputStream != null) outputStream.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}
}
```

```java
public class MainClass {

	public static void main(String[] args) {

		InputStream inputStream = null;
		DataInputStream dataInputStream = null;
		OutputStream outputStream = null;
		DataOutputStream dataOutputStream = null;

		try {

			inputStream = new FileInputStream("파일의 경로");
			dataInputStream = new DataInputStream(inputStream);

			String str = dataInputStream.readUTF();

			outputStream = new FileOutputStream("파일을 쓸 경로");
			dataOutputStream = new DataOutputStream(outputStream);

			dataOutputStream.writeUTF(str);

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(dataOutputStream != null) dataOutputStream.close();
			} catch (Exception e) {
				e.printStackTrace();
			}

			try {
				if(outputStream != null) outputStream.close();
			} catch (Exception e) {
				e.printStackTrace();
			}			
		}		
	}
}
```

### BufferedReader / BufferedWriter

바이트 단위의 입출력을 개선해 문자열을 좀 더 편리하게 다룰 수 있다.


```java
public class MainClass {

	public static void main(String[] args) {

		String fileName = "파일의 경로";

		BufferedReader br = null;
		FileReader fr = null;

		try {

			fr = new FileReader(fileName);
			br = new BufferedReader(fr);

			String strLine;

			while ((strLine = br.readLine()) != null) { //한 라인씩 읽어오는것 
				System.out.println(strLine);
			}

		} catch (IOException e) {
			e.printStackTrace();
		} finally {

			try {
				if (br != null) br.close();
				if (fr != null) fr.close();
			} catch (IOException ex) {
				ex.printStackTrace();
			}
		}
	}
}
```
