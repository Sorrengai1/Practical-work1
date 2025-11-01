``` java 
package ua.opnu;

interface Discountable {
    double applyDiscount(double percent);
}


abstract class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    public abstract String getInfo();
}


class Pizza extends Product implements Discountable {
    private String size;

    public Pizza(String name, double price, String size) {
        super(name, price);
        this.size = size;
    }

    @Override
    public String getInfo() {
        return "Pizza " + getName() + " (" + size + ") - " + getPrice() + " UAH";
    }

    @Override
    public double applyDiscount(double percent) {
        return getPrice() - (getPrice() * percent / 100.0);
    }

    @Override
    public String toString() {
        return getInfo();
    }
}

class Drink extends Product implements Discountable {
    private boolean isCold;

    public Drink(String name, double price, boolean isCold) {
        super(name, price);
        this.isCold = isCold;
    }

    @Override
    public String getInfo() {
        String temp = isCold ? "cold" : "hot";
        return getName() + " [" + temp + "] - " + getPrice() + " UAH";
    }

    @Override
    public double applyDiscount(double percent) {
        return getPrice() - (getPrice() * percent / 100.0);
    }

    @Override
    public String toString() {
        return getInfo();
    }
}

class Cart<T> {
    private T[] items;
    private int count;

    @SuppressWarnings("unchecked")
    public Cart() {
        items = (T[]) new Object[10];
        count = 0;
    }

    public void addItem(T item) {
        if (count < items.length) {
            items[count] = item;
            count++;
        } else {
            System.out.println("Cart is full!");
        }
    }

    public int getCount() {
        return count;
    }


    public String printCart(java.util.function.Function<T, String> formatter) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < count; i++) {
            result.append(formatter.apply(items[i])).append("\n");
        }
        return result.toString();
    }

    public String printSimpleCart() {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < count; i++) {
            result.append(items[i].toString()).append("\n");
        }
        return result.toString();
    }
}


public class Main {
    public static void main(String[] args) {
        Pizza p1 = new Pizza("Margherita", 189.0, "Large");
        Pizza p2 = new Pizza("Pepperoni", 210.0, "Medium");

        Drink d1 = new Drink("Cola", 45.0, true);
        Drink d2 = new Drink("Tea", 30.0, false);

        double newPrice = p1.applyDiscount(10);
        System.out.println("Price after discount: " + newPrice);
        System.out.println();


        Cart<Pizza> pizzaCart = new Cart<>();
        Cart<Drink> drinkCart = new Cart<>();

        pizzaCart.addItem(p1);
        pizzaCart.addItem(p2);

        drinkCart.addItem(d1);
        drinkCart.addItem(d2);

        System.out.println("=== Pizza cart ===");
        System.out.println(pizzaCart.printCart(p -> p.getInfo()));

        System.out.println("=== Drink cart ===");
        System.out.println(drinkCart.printCart(d -> "Drink: " + d.getInfo()));

        System.out.println("Pizza count = " + pizzaCart.getCount());
        System.out.println("Drink count = " + drinkCart.getCount());

        System.out.println("\n=== Discount check ===");
        System.out.println("Pizza original: " + p1.getPrice() + " UAH");
        System.out.println("Pizza with 20% discount: " + p1.applyDiscount(20) + " UAH");
        System.out.println("Drink original: " + d1.getPrice() + " UAH");
        System.out.println("Drink with 15% discount: " + d1.applyDiscount(15) + " UAH");
    }
}
```
