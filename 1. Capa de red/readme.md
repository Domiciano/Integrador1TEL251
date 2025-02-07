<img width="256" src="https://www.icesi.edu.co/launiversidad/images/La_universidad/logo_icesi.png">

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

## Interfaces

```java
import java.io.IOException;
import java.net.InterfaceAddress;
import java.net.NetworkInterface;
import java.util.Enumeration;
import java.util.List;

public class Main {
    public static void main(String[] args) throws IOException {
        Enumeration<NetworkInterface> networks =  NetworkInterface.getNetworkInterfaces();
        while(networks.hasMoreElements()){
            NetworkInterface network = networks.nextElement();
            if(network.isUp()){
                System.out.println(network.getDisplayName());
                List<InterfaceAddress> addresses = network.getInterfaceAddresses();
                for(InterfaceAddress address : addresses){
                    System.out.println("  >>>"+address);
                }
            }
        }
    }
}

```
