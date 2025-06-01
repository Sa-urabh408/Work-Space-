WE HAVE CREATED CLIENT SERVER CHAT APPLICATION BY USING JAVA 
```java
SERVER SIDE :
package king;

import java.io.*;   //package for input output operations 
import java.net.*;  //package for doing socket connections

public class Server {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(20074)) {
            System.out.println("Server is running and waiting for a client...");
            Socket socket = serverSocket.accept();
            System.out.println("Client connected!");
            
            BufferedReader input = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter output = new PrintWriter(socket.getOutputStream(), true);
            
            // Receive two numbers from the client
            int num1 = Integer.parseInt(input.readLine());
            int num2 = Integer.parseInt(input.readLine());
            
            // Send choices to the client
            output.println("Choose an operation:");
            
            int choice = Integer.parseInt(input.readLine());
            String result = "";
            
            
            switch (choice) {
                case 1:
                    result = "first number is: " + (num1 % 2 == 0 ? "Even" : "Odd") + ", second number is: " + (num2 % 2 == 0 ? "Even" : "Odd");
                    break;
                case 2:
                    result = "first number is: " + (num1 >= 0 ? "Positive" : "Negative") + ", second number is: " + (num2 >= 0 ? "Positive" : "Negative");
                    break;
                case 3:
                    result = "Square of first number: " + (num1 * num1) + ",square of second number is:" +(num2*num2);
                    break;
                case 4:
                	socket.close();
                	result="network disconnected";
                	break;
                	default:
                    result = "Invalid choice!!!!";
            }
            
            // Send result to client
            output.println(result);
            
            System.out.println("Result sent to client: " + result);
            
            // Connections will be automatically closed by try-with-resources
        } catch (IOException e) {
           System.out.println(e);
        }
    }
}


CLEINT SIDE:
package king;

import java.io.*;
import java.net.*;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 20074)) {
            System.out.println("Connected to server!");
            
            BufferedReader input = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter output = new PrintWriter(socket.getOutputStream(), true);
            Scanner scanner = new Scanner(System.in);
            
        
            // Enter two numbers
            System.out.print("Enter first number: ");
            int num1 = scanner.nextInt();
            System.out.print("Enter second number: ");
            int num2 = scanner.nextInt();
            
            // Send numbers to the server
            output.println(num1);
            output.println(num2);
            
            // Receive and display choices
            String choices = input.readLine();
            System.out.println(choices);
            
            // Choose an operation
            System.out.print("Enter your choice: enter 1 for odd and even check ,"
            		+ " \n 2 for positive and negative and "
            		+ "\n 3  or square of two numbers "
            		+ "\n  enter 4 for disconnection of client and server ");
            int choice = scanner.nextInt();
            output.println(choice);
            
            
            // Receive and display result
            String result = input.readLine();
            System.out.println("server result: " + result);
            socket.close();
            scanner.close();
            // Connections will be automatically closed by try-with-resources
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

