# Stock-Management-System

import java.util.ArrayList;
import java.util.Scanner;

// Manageable interface to define stock management operations
interface Manageable {
    void add();
    void updateQuantity();
    void delete();
    void viewAll();
}

// Base Product class
abstract class Product {
    private int id;
    private String name;
    private int quantity;

    public Product(int id, String name, int quantity) {
        this.id = id;
        this.name = name;
        this.quantity = quantity;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public abstract String getCategory(); // To be implemented by subclasses

    @Override
    public String toString() {
        return String.format("ID: %d | Name: %s | Category: %s | Quantity: %d", id, name, getCategory(), quantity);
    }
}

// Subclass for Perishable Products
class PerishableProduct extends Product {
    public PerishableProduct(int id, String name, int quantity) {
        super(id, name, quantity);
    }

    @Override
    public String getCategory() {
        return "Perishable";
    }
}

// Subclass for Electronic Products
class ElectronicProduct extends Product {
    public ElectronicProduct(int id, String name, int quantity) {
        super(id, name, quantity);
    }

    @Override
    public String getCategory() {
        return "Electronic";
    }
}

// StockManagement class implementing Manageable interface
class StockManagement implements Manageable {
    private final ArrayList<Product> productList = new ArrayList<>();
    private int nextId = 1; // Auto-incrementing ID
    private final Scanner scanner = new Scanner(System.in);

    @Override
    public void add() {
        scanner.nextLine(); // Consume leftover newline

        System.out.print("Enter product name: ");
        String name = scanner.nextLine();

        System.out.print("Enter product quantity: ");
        int quantity = getValidatedInteger("quantity");

        System.out.println("Select product category:");
        System.out.println("1. Perishable");
        System.out.println("2. Electronic");
        System.out.print("Enter your choice: ");
        int categoryChoice = getValidatedInteger("category");

        Product product;
        if (categoryChoice == 1) {
            product = new PerishableProduct(nextId++, name, quantity);
        } else if (categoryChoice == 2) {
            product = new ElectronicProduct(nextId++, name, quantity);
        } else {
            System.out.println("Invalid category! Product not added.");
            return;
        }

        productList.add(product);
        System.out.println("Product added successfully!");
    }

    @Override
    public void viewAll() {
        if (productList.isEmpty()) {
            System.out.println("No products found.");
        } else {
            for (Product product : productList) {
                System.out.println(product);
            }
        }
    }

    @Override
    public void updateQuantity() {
        System.out.print("Enter product ID to update: ");
        int id = getValidatedInteger("ID");

        Product product = findProductById(id);
        if (product == null) {
            System.out.println("Product not found! Please try again.");
            return;
        }

        System.out.print("Enter new quantity: ");
        int quantity = getValidatedInteger("quantity");

        product.setQuantity(quantity);
        System.out.println("Product quantity updated successfully!");
    }

    @Override
    public void delete() {
        System.out.print("Enter product ID to delete: ");
        int id = getValidatedInteger("ID");

        Product product = findProductById(id);
        if (product == null) {
            System.out.println("Product not found! Please try again.");
            return;
        }

        productList.remove(product);
        System.out.println("Product deleted successfully!");
    }

    private Product findProductById(int id) {
        for (Product product : productList) {
            if (product.getId() == id) {
                return product;
            }
        }
        return null;
    }

    private int getValidatedInteger(String fieldName) {
        while (!scanner.hasNextInt()) {
            System.out.printf("Invalid input! Please enter a valid %s (integer):\n", fieldName);
            scanner.next(); // Clear invalid input
        }
        return scanner.nextInt();
    }
}

// Main Class
public class InteractiveStockManagement {
    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        StockManagement stockManagement = new StockManagement();

        System.out.println("Welcome to the Interactive Stock Management System!");

        while (true) {
            displayMenu();
            int choice = getValidatedInteger("choice");

            switch (choice) {
                case 1 -> stockManagement.add();
                case 2 -> stockManagement.viewAll();
                case 3 -> stockManagement.updateQuantity();
                case 4 -> stockManagement.delete();
                case 5 -> {
                    System.out.println("Thank you for using the system. Goodbye!");
                    return;
                }
                default -> System.out.println("Invalid choice. Please try again!");
            }
        }
    }

    private static void displayMenu() {
        System.out.println("=== Main Menu ===");
        System.out.println("1. Add Product");
        System.out.println("2. View All Products");
        System.out.println("3. Update Product Quantity");
        System.out.println("4. Delete Product");
        System.out.println("5. Exit");
        System.out.print("Enter your choice: ");
    }

    private static int getValidatedInteger(String fieldName) {
        while (!scanner.hasNextInt()) {
            System.out.printf("Invalid input! Please enter a valid %s (integer):\n", fieldName);
            scanner.next(); // Clear invalid input
        }
        return scanner.nextInt();
    }
}
