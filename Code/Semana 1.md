
# Identificación y conectividad

```java
//7 de Febrero de 2025
package org.example;

import java.io.IOException;
import java.net.InetAddress;
import java.net.UnknownHostException;

public class Main {
    public static void main(String[] args) throws IOException {
        InetAddress polanco = InetAddress.getByName("192.168.130.59");
        InetAddress pacheco = InetAddress.getByName("192.168.130.69");
        InetAddress icesi = InetAddress.getByName("www.icesi.edu.co");
        InetAddress google = InetAddress.getByName("www.google.com");

        boolean isPolancoReachable = polanco.isReachable(10);
        boolean isPachecoReachable = pacheco.isReachable(10);

        System.out.println(isPolancoReachable);
        System.out.println(isPachecoReachable);

        System.out.println(icesi.getHostAddress());
        System.out.println(google.getHostAddress());
    }
}
```
