#include <stdio.h>

void displayWelcomeMessage();
int topUpAccount();
void displayFoodMenu();
void handleFoodPurchase(char input, int *remaining_pizza, int *remaining_burger, int *remaining_pasta,
                        int *remaining_sandwich, int *remaining_salad, float *total_cost, int *amount);
void handlePizzaPurchase(int *remaining_pizza, float *total_cost, int *amount);
void handleBurgerPurchase(int *remaining_burger, float *total_cost, int *amount);
void handlePastaPurchase(int *remaining_pasta, float *total_cost, int *amount);
void handleSandwichPurchase(int *remaining_sandwich, float *total_cost, int *amount);
void handleSaladPurchase(int *remaining_salad, float *total_cost, int *amount);
void displayDenominationBreakdown(int amount);
void displayUpdatedStock(int remaining_pizza, int remaining_burger, int remaining_pasta,
                         int remaining_sandwich, int remaining_salad);
void applyDiscountAndFinalize(float total_cost, int *amount);
void handleItemRemoval(float *total_cost, int *amount, int *remaining_pizza, int *remaining_burger,
                       int *remaining_pasta, int *remaining_sandwich, int *remaining_salad);

int main() {
    displayWelcomeMessage();
    int amount = topUpAccount();
    char input;
    float total_cost = 0.0;

    int remaining_pizza = 10, remaining_burger = 15, remaining_pasta = 8;
    int remaining_sandwich = 12, remaining_salad = 20;

    while (1) {
        displayFoodMenu();
        scanf(" %c", &input);

        if (input != 'c' && input != 'C' && input != 'r' && input != 'R') {
            handleFoodPurchase(input, &remaining_pizza, &remaining_burger, &remaining_pasta,
                               &remaining_sandwich, &remaining_salad, &total_cost, &amount);
        }

        if (input == 'c' || input == 'C') {
            break;
        }

        else if (input == 'r' || input == 'R') {
            handleItemRemoval(&total_cost, &amount, &remaining_pizza, &remaining_burger,
                              &remaining_pasta, &remaining_sandwich, &remaining_salad);
        }

        char moreOrder;
        printf("Would you like to add more orders? (y/n): ");
        scanf(" %c", &moreOrder);
        if (moreOrder == 'n' || moreOrder == 'N') {
            break;
        }
    }
    displayDenominationBreakdown(amount);
    displayUpdatedStock(remaining_pizza, remaining_burger, remaining_pasta,
                        remaining_sandwich, remaining_salad);
    applyDiscountAndFinalize(total_cost, &amount);

    return 0;
}

void displayWelcomeMessage() {
    printf("Welcome to the Food Store!\n");
}

int topUpAccount() {
    int amount;
    printf("Please top up your account balance, please enter amount to add: \n");
    scanf("%d", &amount);
    return amount;
}

void displayFoodMenu() {
    printf("\nEnter the food item you want to buy (or 'c' to close) (or 'r' to remove):\n");
    printf("p - Pizza\nb - Burger\nP - Pasta\ns - Sandwich\nS - Salad\n");
    printf("Enter 'c' to close (or 'r' to remove).\n");
}

void handleFoodPurchase(char input, int *remaining_pizza, int *remaining_burger, int *remaining_pasta,
                        int *remaining_sandwich, int *remaining_salad, float *total_cost, int *amount) {
    switch (input) {
        case 'p':
            handlePizzaPurchase(remaining_pizza, total_cost, amount);
            break;
        case 'b':
            handleBurgerPurchase(remaining_burger, total_cost, amount);
            break;
        case 'P':
            handlePastaPurchase(remaining_pasta, total_cost, amount);
            break;
        case 's':
            handleSandwichPurchase(remaining_sandwich, total_cost, amount);
            break;
        case 'S':
            handleSaladPurchase(remaining_salad, total_cost, amount);
            break;
        default:
            printf("Product not available.\n");
            break;
    }
}

void handlePizzaPurchase(int *remaining_pizza, float *total_cost, int *amount) {
    int choice, quantity;
    float price = 0.0;

    printf("Select a pizza:\n");
    printf("1 - Margherita Pizza (Price: 200 rupees)\n");
    printf("2 - Pepperoni Pizza (Price: 250 rupees)\n");
    printf("3 - BBQ Chicken Pizza (Price: 300 rupees)\n");
    printf("4 - Veggie Pizza (Price: 220 rupees)\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: price = 200.0; break;
        case 2: price = 250.0; break;
        case 3: price = 300.0; break;
        case 4: price = 220.0; break;
        default:
            printf("Invalid choice!\n");
            return;
    }

    printf("Please enter the quantity of pizza: ");
    scanf("%d", &quantity);

    if (quantity > *remaining_pizza) {
        printf("Sorry, we don't have enough pizza in stock. You can buy up to %d pizza.\n", *remaining_pizza);
        return;
    }

    *remaining_pizza -= quantity;
    *total_cost += price * quantity;
    *amount -= price * quantity;

    printf("You bought %d pizza(s) for %.2f rupees. Remaining balance: %d rupees.\n", quantity, price * quantity, *amount);
}

void handleBurgerPurchase(int *remaining_burger, float *total_cost, int *amount) {
    int choice, quantity;
    float price = 0.0;

    printf("Select a burger:\n");
    printf("1 - Cheeseburger (Price: 100 rupees)\n");
    printf("2 - Veggie Burger (Price: 90 rupees)\n");
    printf("3 - Chicken Burger (Price: 120 rupees)\n");
    printf("4 - Double Burger (Price: 150 rupees)\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: price = 100.0; break;
        case 2: price = 90.0; break;
        case 3: price = 120.0; break;
        case 4: price = 150.0; break;
        default:
            printf("Invalid choice!\n");
            return;
    }

    printf("Please enter the quantity of burger: ");
    scanf("%d", &quantity);

    if (quantity > *remaining_burger) {
        printf("Sorry, we don't have enough burger in stock. You can buy up to %d burger.\n", *remaining_burger);
        return;
    }

    *remaining_burger -= quantity;
    *total_cost += price * quantity;
    *amount -= price * quantity;

    printf("You bought %d burger(s) for %.2f rupees. Remaining balance: %d rupees.\n", quantity, price * quantity, *amount);
}

void handlePastaPurchase(int *remaining_pasta, float *total_cost, int *amount) {
    int choice, quantity;
    float price = 0.0;

    printf("Select a pasta:\n");
    printf("1 - Spaghetti (Price: 150 rupees)\n");
    printf("2 - Penne (Price: 160 rupees)\n");
    printf("3 - Fettuccine (Price: 170 rupees)\n");
    printf("4 - Macaroni (Price: 140 rupees)\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: price = 150.0; break;
        case 2: price = 160.0; break;
        case 3: price = 170.0; break;
        case 4: price = 140.0; break;
        default:
            printf("Invalid choice!\n");
            return;
    }

    printf("Please enter the quantity of pasta: ");
    scanf("%d", &quantity);

    if (quantity > *remaining_pasta) {
        printf("Sorry, we don't have enough pasta in stock. You can buy up to %d pasta.\n", *remaining_pasta);
        return;
    }

    *remaining_pasta -= quantity;
    *total_cost += price * quantity;
    *amount -= price * quantity;

    printf("You bought %d pasta(s) for %.2f rupees. Remaining balance: %d rupees.\n", quantity, price * quantity, *amount);
}

void handleSandwichPurchase(int *remaining_sandwich, float *total_cost, int *amount) {
    int choice, quantity;
    float price = 0.0;

    printf("Select a sandwich:\n");
    printf("1 - Club Sandwich (Price: 80 rupees)\n");
    printf("2 - Veg Sandwich (Price: 70 rupees)\n");
    printf("3 - Chicken Sandwich (Price: 90 rupees)\n");
    printf("4 - Grilled Sandwich (Price: 100 rupees)\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: price = 80.0; break;
        case 2: price = 70.0; break;
        case 3: price = 90.0; break;
        case 4: price = 100.0; break;
        default:
            printf("Invalid choice!\n");
            return;
    }

    printf("Please enter the quantity of sandwich: ");
    scanf("%d", &quantity);

    if (quantity > *remaining_sandwich) {
        printf("Sorry, we don't have enough sandwich in stock. You can buy up to %d sandwich.\n", *remaining_sandwich);
        return;
    }

    *remaining_sandwich -= quantity;
    *total_cost += price * quantity;
    *amount -= price * quantity;

    printf("You bought %d sandwich(es) for %.2f rupees. Remaining balance: %d rupees.\n", quantity, price * quantity, *amount);
}

void handleSaladPurchase(int *remaining_salad, float *total_cost, int *amount) {
    int choice, quantity;
    float price = 0.0;

    printf("Select a salad:\n");
    printf("1 - Caesar Salad (Price: 50 rupees)\n");
    printf("2 - Greek Salad (Price: 60 rupees)\n");
    printf("3 - Garden Salad (Price: 40 rupees)\n");
    printf("4 - Pasta Salad (Price: 70 rupees)\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: price = 50.0; break;
        case 2: price = 60.0; break;
        case 3: price = 40.0; break;
        case 4: price = 70.0; break;
        default:
            printf("Invalid choice!\n");
            return;
    }

    printf("Please enter the quantity of salad: ");
    scanf("%d", &quantity);

    if (quantity > *remaining_salad) {
        printf("Sorry, we don't have enough salad in stock. You can buy up to %d salad.\n", *remaining_salad);
        return;
    }

    *remaining_salad -= quantity;
    *total_cost += price * quantity;
    *amount -= price * quantity;

    printf("You bought %d salad(s) for %.2f rupees. Remaining balance: %d rupees.\n", quantity, price * quantity, *amount);
}

void displayDenominationBreakdown(int amount) {
    int denominations[] = {500, 200, 100, 50, 20, 10, 5, 2, 1};
    int count[9] = {0};

    for (int i = 0; i < 9; i++) {
        if (amount >= denominations[i]) {
            count[i] = amount / denominations[i];
            amount %= denominations[i];
        }
    }

    printf("\nDenomination breakdown:\n");
    for (int i = 0; i < 9; i++) {
        if (count[i] > 0) {
            printf("%d rupees: %d\n", denominations[i], count[i]);
        }
    }
}

void displayUpdatedStock(int remaining_pizza, int remaining_burger, int remaining_pasta,
                         int remaining_sandwich, int remaining_salad) {
    printf("\nUpdated available products:\n");
    printf("Pizza: %d\n", remaining_pizza);
    printf("Burger: %d\n", remaining_burger);
    printf("Pasta: %d\n", remaining_pasta);
    printf("Sandwich: %d\n", remaining_sandwich);
    printf("Salad: %d\n", remaining_salad);
}

void applyDiscountAndFinalize(float total_cost, int *amount) {
    float discount_percentage = 0.0;

    if (total_cost >= 1000) {
        discount_percentage = 10.0;
    } else if (total_cost >= 500) {
        discount_percentage = 5.0;
    }

    float discount_amount = (discount_percentage / 100) * total_cost;
    float final_amount = total_cost - discount_amount;
    *amount -= final_amount;

    printf("******BILL!!******\nThank you for shopping with us!\n");
    printf("Total amount spent: %.2f rupees.\n", total_cost);
    printf("Discount applied: %.2f%%\n", discount_percentage);
    printf("Amount after discount: %.2f rupees.\n", final_amount);
    printf("The remaining balance is %d rupees.\n", *amount);
}

void handleItemRemoval(float *total_cost, int *amount, int *remaining_pizza, int *remaining_burger,
                       int *remaining_pasta, int *remaining_sandwich, int *remaining_salad) {
    char item;
    int quantity;
    float price = 0.0;

    printf("Enter the food item you want to remove:\n");
    printf("p - Pizza\nb - Burger\nP - Pasta\ns - Sandwich\nS - Salad\n");
    printf("Enter your choice: ");
    scanf(" %c", &item);

    switch (item) {
        case 'p':
            price = 200.0;
            break;
        case 'b':
            price = 100.0;
            break;
        case 'P':
            price = 150.0;
            break;
        case 's':
            price = 80.0;
            break;
        case 'S':
            price = 50.0;
            break;
        default:
            printf("Invalid product choice.\n");
            return;
    }

    printf("Enter the quantity of %c you want to remove: ", item);
    scanf("%d", &quantity);

    switch (item) {
        case 'p':
            if (quantity > *remaining_pizza) {
                printf("You have less than %d pizza(s) in stock. You can only remove up to %d.\n", quantity, *remaining_pizza);
                return;
            }
            *remaining_pizza += quantity;
            break;
        case 'b':
            if (quantity > *remaining_burger) {
                printf("You have less than %d burger(s) in stock. You can only remove up to %d.\n", quantity, *remaining_burger);
                return;
            }
            *remaining_burger += quantity;
            break;
        case 'P':
            if (quantity > *remaining_pasta) {
                printf("You have less than %d pasta(s) in stock. You can only remove up to %d.\n", quantity, *remaining_pasta);
                return;
            }
            *remaining_pasta += quantity;
            break;
        case 's':
            if (quantity > *remaining_sandwich) {
                printf("You have less than %d sandwich(es) in stock. You can only remove up to %d.\n", quantity, *remaining_sandwich);
                return;
            }
            *remaining_sandwich += quantity;
            break;
        case 'S':
            if (quantity > *remaining_salad) {
                printf("You have less than %d salad(s) in stock. You can only remove up to %d.\n", quantity, *remaining_salad);
                return;
            }
            *remaining_salad += quantity;
            break;
    }

    *total_cost -= (price * quantity);
    *amount += (price * quantity);
    printf("Removed %d %c(s). New total cost: %.2f, New balance: %d.\n", quantity, item, *total_cost, *amount);
}
