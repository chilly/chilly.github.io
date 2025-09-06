---
title: jad decompile error with shift operators.
date: 2024-06-05 21:17:26
tags:
categories:
- [code, java]
---

I found sometime jad has bug in decompile the code.  Like the following code:
# Code frame 1: long2Bytes
```java
// this code is for convert long to bytes.
// long2Bytes(0x12345670, 1) => 0x1
// long2Bytes(0x12345670, 2) => 0x12
public static final byte[] long2Bytes(long l, int len) {
    if (len < 0) {
        return null;
    } else {
        if (len > 8) {
            len = 8;
        }

        byte[] temp = new byte[len];
        int i = len - 1;

        for(int j = 0; i >= 0; ++j) {
            temp[j] = (byte)((int)(l >>> (i << 3)));
            --i;
        }

        return temp;
    }
}
```
## Decompile this code will be:
```java

public static final byte[] long2Bytes(long l, int len) {
    if (len < 0) {
        return null;
    } else {
        if (len > 8) {
            len = 8;
        }

        byte[] temp = new byte[len];
        int i = len - 1;

        for(int j = 0; i >= 0; ++j) {
            temp[j] = (byte)((int)(l >>> i << 3));
            --i;
        }

        return temp;
    }
}


```

You will see the byte shift operator`>>> <<` are wrong.

# Code frame 2: bytes2Long
I find another mistake in Jad is:
```java
  public static final long bytes2Long(byte[] b) {
    if (b.length <= 0 || b.length > 8) {
        throw new IllegalArgumentException("byte length should be 1~8bytes.");
    }
       
    long l = 0L;
    for (int i = 0, j = b.length - 1; i <= j; i++) {
        l |= (b[i] & 0xFF) << ((j - i) << 3);
    }
      
    return l;
  }
```
## Decompile this code will be:
```java

public static final long bytes2Long(byte[] b) {
    if (b.length <= 0 || b.length > 8)
      throw new IllegalArgumentException("byte length should be 1~8bytes."); 
    long l = 0L;
    for (int i = 0, j = b.length - 1; i <= j; i++)
      l |= (b[i] & 0xFF) << j - i << 3;
    return l;
  }


```

# Conclusion
You will see the byte shift operator`>>> <<` are wrong again.
So if you want decompile some code, be careful with shift operators.