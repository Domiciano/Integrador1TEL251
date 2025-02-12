# Server

```java
package org.example;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;

//192.168.130.59
public class Server {

    static ArrayList<Session> sessions = new ArrayList<Session>();


    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8080);

        while(true) {
            System.out.println("Waiting for connection...");
            Socket socket = server.accept(); // Esperando la conexión // Bloqueante
            //socket representa la conexión con el cliente
            System.out.println("Connection accepted");

            //Vamos a pasarle el socket a las sesion, para que esta lo gestione
            Session session = new Session(socket);
            session.deploy();

            sessions.add(session);
        }
    }

}
```

# Sesion

```java
package org.example;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.nio.charset.StandardCharsets;
import java.util.Date;

public class Session {

    private Socket socket;

    private OutputStream outputStream;
    private InputStream inputStream;

    public Session(Socket socket) throws IOException {
        this.socket = socket;
        this.inputStream = socket.getInputStream();
        this.outputStream = socket.getOutputStream();
    }

    private void receive() {
        new Thread(
                () -> {
                    while (true) {
                        try {
                            byte[] buffer = new byte[1024];
                            // X X X X X X X X X X ... X
                            inputStream.read(buffer); // Bloqueante
                            // H O L A X X X X X X ... X
                            String message = new String(buffer).trim();
                            System.out.println(message);
                            //Envio del broadcast

                            if (message.equals("time")) {
                                this.send(new Date().toString());
                            }else {
                                //Para poder hacerlo, necesitamos la lista de conexiones
                                Server.sessions.forEach(session -> {
                                    session.send(message);
                                });
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
        ).start();
    }

    private void send(String message) {
        new Thread(() -> {
            try {
                outputStream.write(message.getBytes());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }).start();
    }

    public void deploy() {
        receive();
    }


}

```

# Client
```java
package org.example;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) throws IOException {
        //Iniciar conexión
        Socket socket = new Socket("127.0.0.1", 5001);
        System.out.println("Connected");

        InputStream is = socket.getInputStream();
        OutputStream out = socket.getOutputStream();
        //Lectura
        startListening(is);

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("Escriba un mensaje");
            String message = scanner.nextLine();
            out.write(message.getBytes());
        }
    }

    //Proceso de lectura
    private static void startListening(InputStream is){
        new Thread(
                ()->{
                    while (true) {
                        try {
                            byte[] buffer = new byte[1024];
                            // X X X X X X X X X X ... X
                            is.read(buffer); // Bloqueante
                            // H O L A X X X X X X ... X
                            String message = new String(buffer).trim();
                            System.out.println(message);
                        }catch (IOException e){
                            e.printStackTrace();
                        }
                    }
                }
        ).start();
    }
}
```
