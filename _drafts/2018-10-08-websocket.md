---
title: "Websocket Server!!!"
author_profile: true
image:
  teaser: https://avatars0.githubusercontent.com/u/12326850?s=400&v=4

---


HTML5 dataset
--------------------

`Websocket` data frame 에 맞춰 server 구현

``` java
String unique = headers.get("Sec-WebSocket-Key") + WS_MAGIC_STRING;
MessageDigest md = MessageDigest.getInstance("SHA-1");
md.update(unique.getBytes());

byte byteData[] = md.digest();// SHA-1 hash value

StringBuffer sb = new StringBuffer();
for (int i = 0; i < byteData.length; i++) {
    sb.append(Integer.toString((byteData[i] & 0xff) + 0x100, 16).substring(1));
}

String retVal = sb.toString();

System.out.println(retVal);
String accHd = new String(Base64.getEncoder().encode(byteData));
System.out.println(accHd);
// header
out.write("HTTP/1.1 101 Switching Protocols\r\n");
out.write("Upgrade: websocket\r\n");
out.write("Connection: Upgrade\r\n");
out.write("Sec-WebSocket-Accept: " + accHd + "\r\n");
out.write("\r\n");
out.flush();


// Web socket connection
int payloadLenth = 0;
int[] maskKey = new int[4];
int[] payloadArray = null;
int streamIndex = 0;
int payloadIndex = 0;

while (true) {

    int dataSegment = din.read();
    System.out.println(Integer.toBinaryString(dataSegment));


    if (streamIndex == 0) {

    } else if (streamIndex == 1) {

        payloadLenth = dataSegment - 128; // payload 길이 추출

    } else if (streamIndex >= 2 && streamIndex <= 5) {

        maskKey[streamIndex - 2] = dataSegment;

    } else {

        if (payloadIndex == 0) {
            payloadArray = new int[payloadLenth];
        }
        payloadArray[payloadIndex] = dataSegment;

        if (payloadLenth - 1 == payloadIndex) {

            dout.write(0b10000001);
            dout.write(payloadLenth);

            for (int i = 0; i < payloadArray.length; i++) {
                int mask = maskKey[i % 4];
                int encoded = payloadArray[i];
                dout.write(encoded ^ mask); // XOR MASK
            }
            dout.flush();

            payloadIndex = -1;
            streamIndex = -1;
        }

        payloadIndex++;
    }

    streamIndex++;

}
```


참고 : [HTMLElement.dataset](https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset){:target="_blank"}
