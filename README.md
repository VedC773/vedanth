public class BankService { 
 
    // Nested class (can be public or private, doesn't matter for functionality) 
    static class BankAccount { 
        private double balance; 
 
        public BankAccount(double initialBalance) { 
            if (initialBalance < 0) { 
                throw new IllegalArgumentException("Initial balance cannot be negative"); 
            } 
            this.balance = initialBalance; 
        } 
 
        public void deposit(double amount) { 
            if (amount <= 0) { 
                throw new IllegalArgumentException("Deposit must be positive"); 
            } 
            balance += amount; 
        } 
 
        public void withdraw(double amount) { 
            if (amount <= 0 || amount > balance) { 
                throw new IllegalArgumentException("Invalid withdrawal"); 
            } 
            balance -= amount; 
        } 
 
        public double getBalance() { 
            return balance; 
        } 
    } 
 
    // The main entry point 
    public static void main(String[] args) { 
        // Create an instance of the BankAccount class 
        BankAccount acc = new BankAccount(5000); 
         
        System.out.println("Initial Balance: " + acc.getBalance()); 
 
        // Perform operations 
        acc.deposit(700); 
        System.out.println("Balance after deposit of 500: " + acc.getBalance()); 
 
        acc.withdraw(100); 
        System.out.println("Balance after withdrawal of 300: " + acc.getBalance()); 
 
        // Print final result 
        System.out.println("Final Balance: " + acc.getBalance()); 
    } 
} 
---------------------------------------------------------
mvn clean
mvn package
cd target
java -cp sample-app-1.0-SNAPSHOT.jar com.bnmit
----------------------------------------------------------
Calculator.java 
public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public double divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return (double) a / b;
    }

    public int square(int a) {
        return a * a;
    }

    public int max(int a, int b) {
        return (a > b) ? a : b;
    }
}


---------------------------------------------------------------------------------------------------
CalulatorTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

    Calculator calc = new Calculator();

    @Test
    void testAdd() {
        assertEquals(8, calc.add(5, 3));
    }

    @Test
    void testSubtract() {
        assertEquals(2, calc.subtract(5, 3));
    }

    @Test
    void testMultiply() {
        assertEquals(15, calc.multiply(5, 3));
    }

    @Test
    void testDivide() {
        assertEquals(2.0, calc.divide(6, 3));
    }

    @Test
    void testSquare() {
        assertEquals(25, calc.square(5));
    }

    @Test
    void testMax() {
        assertEquals(9, calc.max(9, 4));
    }

    @Test
    void testDivideByZero() {
        Exception exception = assertThrows(
            ArithmeticException.class,
            () -> calc.divide(5, 0)
        );

        assertEquals("Cannot divide by zero", exception.getMessage());
    }
}


---------------------------------------------
1)
cmd -    
javac -cp "lib/junit-platform-console-standalone-1.10.0.jar" -d bin src/*.java  
java -jar lib/junit-platform-console-standalone-1.10.0.jar execute --class-path bin --scan-class-path --details tree 


------------------------------------------------
4)
plugins

<plugins>
  <plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>

    <executions>
      <!-- Attach JaCoCo agent -->
      <execution>
        <goals>
          <goal>prepare-agent</goal>
        </goals>
      </execution>

      <!-- Generate report -->
      <execution>
        <id>report</id>
        <phase>test</phase>
        <goals>
          <goal>report</goal>
        </goals>
      </execution>
    </executions>
  </plugin>
</plugins>
--------------------------------------------------------
5)
terminal
docker run hello-world
docker pull nginx
docker run -d -p 8082:80 ngnix


---------------------------------------------------
6)
dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
CMD ["python", "app.py"]
from http.server import SimpleHTTPRequestHandler, HTTPServer 
PORT = 5020 

class MyHandler(SimpleHTTPRequestHandler): 
    def do_GET(self): 
        self.send_response(200) 
        self.send_header("Content-type", "text/html") 
        self.end_headers() 
        self.wfile.write(b"<h1>Hello from Docker Python Container</h1>") 
    
server = HTTPServer(("0.0.0.0", PORT), MyHandler) 
print(f"Server running on port {PORT}...") 
server.serve_forever()
docker build -t myapp .
docker run myapp
---------------------------------------------------
7)
Docker map and volume 

(VScode)

--For porting

docker run -d nginx

docker run -d -p 8084:80 nginx

docker volume create myvolume
docker volume

docker run -it -v myvolume:/data ubuntu bash

cd /data
echo "Docker Volume Persistence Test" > test.txt
exit

docker run -it -v myvolume:/data ubuntu bash
cat /data/test.txt

--combining both

docker run -d -p 8081:80 -v myvolume:/usr/share/nginx/html nginx
docker run -it -v myvolume:/usr/share/nginx/html ubuntu bash
echo "<h1>Persistent Web Page</h1>" > /usr/share/nginx/html/index.html
exit

http://localhost:8081
----------------------------------------------------------------------------------------
8)
dockerfile
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY . /app
RUN javac Grade.java
CMD ["java", "Grade"]


Grade code

import java.util.Scanner;

public class Grade{

  // Returns a letter grade based on the average
  static char gradeFunction(double avg) {
    if (avg >= 90) return 'A';
    else if (avg >= 80) return 'B';
    else if (avg >= 70) return 'C';
    else if (avg >= 60) return 'D';
    else return 'F';
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);

    System.out.print("How many grades (1 to 5)? ");
    int count = scanner.nextInt();

    // Validate the input count
    if (count < 1 || count > 5) {
      System.out.println("Invalid number. You must enter between 1 and 5 grades.");
      scanner.close();
      return; // Exit
    }

    double sum = 0.0;

    // Read each grade
    for (int i = 1; i <= count; i++) {
      System.out.print("Enter grade " + i + ": ");
      double grade = scanner.nextDouble();
      sum += grade;
    }

    double avg = sum / count;
    System.out.println("Average: " + avg);
    System.out.println("Letter grade: " + gradeFunction(avg));
    scanner.close();
  }
}

docker build -t myapp
docker login 
docker tag myapp user_name/myapp:v1.0
docker push user_name/myapp:v1.0

----------------------------------------------------------------------------------------------

9) pipeline

pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Vani-Khursapur/docker-jenkins-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t my-app .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d my-app'
            }
        }
    }
}

------------------------------------------------------------------------------------------------------
10)
pipeline {
    agent any

    environment {
        IMAGE_NAME = "sample-webapp"
        CONTAINER_NAME = "sample-webapp-container"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Vani-Khursapur/cicd-webapp.git'
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t sample-webapp .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker stop sample-webapp-container || exit 0'
                bat 'docker rm sample-webapp-container || exit 0'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 8087:8087 --name sample-webapp-container sample-webapp'
            }
        }
    }
}

http://localhost:8087/webapp
