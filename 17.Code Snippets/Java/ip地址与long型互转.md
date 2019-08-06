```java
public static long ip2Long(String ip) {
    String[] segs = ip.split("\\.");

    long x = 0;
    for (int i = 0; i < segs.length; i++) {
        x = x << 8 | Integer.valueOf(segs[i]);
    }

    return x;
}

public static String ip2Str(long ip) {
    long ip1 = (ip & 0xff000000) % 0xffffff;
    long ip2 = (ip & 0x00ff0000) % 0xffff;
    long ip3 = (ip & 0x0000ff00) % 0xff;
    long ip4 = ip & 0x000000ff;

    return String.format("%d.%d.%d.%d", ip1, ip2, ip3, ip4);
}
```
