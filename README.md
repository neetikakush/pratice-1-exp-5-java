import java.io.*;
import java.util.*;

// ------------------- Part (a) -------------------
class SumAutoboxing {
    public void run() {
        Integer num1 = 10, num2 = 20, num3 = 30;
        int sum = num1 + num2 + num3; // auto unboxing during addition
        System.out.println("Sum of integers = " + sum);
    }
}

// ------------------- Part (b) -------------------
class Student implements Serializable {
    int id;
    String name;
    transient double gpa;

    Student(int id, String name, double gpa) {
        this.id = id;
        this.name = name;
        this.gpa = gpa;
    }

    public void display() {
        System.out.println("ID: " + id + ", Name: " + name + ", GPA: " + gpa);
    }
}

class StudentSerialization {
    String filename = "student.ser";

    public void run() {
        // Serialization
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename))) {
            Student s1 = new Student(101, "Alice", 8.7);
            oos.writeObject(s1);
            System.out.println("Student object serialized.");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Deserialization
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename))) {
            Student s2 = (Student) ois.readObject();
            System.out.println("Student object deserialized.");
            s2.display();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// ------------------- Part (c) -------------------
class Employee implements Serializable {
    int id;
    String name;
    double salary;

    Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public void display() {
        System.out.println("ID: " + id + ", Name: " + name + ", Salary: " + salary);
    }
}

class EmployeeManagementSystem {
    static final String FILE_NAME = "employees.dat";

    public void addEmployee(Employee emp) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(FILE_NAME, true)) {
            @Override
            protected void writeStreamHeader() throws IOException {
                reset(); // prevent header corruption
            }
        }) {
            oos.writeObject(emp);
            System.out.println("Employee added successfully!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void viewEmployees() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            while (true) {
                Employee emp = (Employee) ois.readObject();
                emp.display();
            }
        } catch (EOFException e) {
            System.out.println("End of employee list.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void run() {
        Scanner sc = new Scanner(System.in);
        int choice;
        do {
            System.out.println("\n--- Employee Management Menu ---");
            System.out.println("1. Add Employee");
            System.out.println("2. View Employees");
            System.out.println("3. Exit");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter ID: ");
                    int id = sc.nextInt();
                    sc.nextLine();
                    System.out.print("Enter Name: ");
                    String name = sc.nextLine();
                    System.out.print("Enter Salary: ");
                    double salary = sc.nextDouble();
                    addEmployee(new Employee(id, name, salary));
                    break;

                case 2:
                    viewEmployees();
                    break;

                case 3:
                    System.out.println("Exiting Employee Management...");
                    break;

                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 3);
    }
}

// ------------------- Main Program -------------------
public class AssignmentMain {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n=== Assignment Menu ===");
            System.out.println("1. Part (a) Sum with Autoboxing/Unboxing");
            System.out.println("2. Part (b) Student Serialization");
            System.out.println("3. Part (c) Employee Management System");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    new SumAutoboxing().run();
                    break;
                case 2:
                    new StudentSerialization().run();
                    break;
                case 3:
                    new EmployeeManagementSystem().run();
                    break;
                case 4:
                    System.out.println("Exiting Program...");
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 4);

        sc.close();
    }
}
