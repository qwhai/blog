```java
/**
 * 判断一个字符串是否是一个合法ip地址
 * @param ip    ip地址字符串
 * @return      true: 是合法ip / false：非法ip
 */
public static boolean isIpStr(String ip) {
    if(ip.length() < 7 || ip.length() > 15) return false;

    String rexp =
            "^(1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|[1-9])\\." +
            "(1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)\\." +
            "(1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)\\." +
            "(1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)$";

    Pattern pattern = Pattern.compile(rexp);
    return pattern.matcher(ip).find();
}
```
