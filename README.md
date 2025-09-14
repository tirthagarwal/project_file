FILENAME = "expenses.txt"

# -----------------------------
# Add a new expense
# -----------------------------
def add_expense():
    category = input("Enter category (e.g., Food, Travel): ")
    amount = input("Enter amount: ")
    date = input("Enter date (YYYY-MM-DD): ")

    file = open(FILENAME, "a")
    file.write(category + "," + amount + "," + date + "\n")
    file.close()

    print("Expense added successfully!\n")

# -----------------------------
# View all expenses
# -----------------------------
def view_expenses():
    try:
        file = open(FILENAME, "r")
        lines = file.readlines()
        file.close()
    except:
        print("\nNo expenses recorded.\n")
        return

    if len(lines) == 0:
        print("\nNo expenses recorded.\n")
        return

    print("\nAll Expenses:")
    for i, line in enumerate(lines, start=1):
        parts = line.strip().split(",")
        if len(parts) == 3:
            category = parts[0]
            amount = parts[1]
            date = parts[2]
            print(str(i) + ". Category: " + category + " | Amount: " + amount + " | Date: " + date)
    print("")

# -----------------------------
# Summary by category (all time)
# -----------------------------
def category_summary():
    try:
        file = open(FILENAME, "r")
        lines = file.readlines()
        file.close()
    except:
        print("No expenses recorded.\n")
        return

    if len(lines) == 0:
        print("No expenses recorded.\n")
        return

    categories = {}
    total = 0

    for line in lines:
        parts = line.strip().split(",")
        if len(parts) == 3:
            category = parts[0]
            amount = float(parts[1])
            total += amount
            if category not in categories:
                categories[category] = 0
            categories[category] += amount

    print("\nSummary (All Time):")
    print("Total Expenses:", total)
    print("By Category:")
    for category in categories:
        print(category + ":", categories[category])
    print("")

# -----------------------------
# Expenses over a period
# -----------------------------
def period_summary():
    try:
        file = open(FILENAME, "r")
        lines = file.readlines()
        file.close()
    except:
        print("No expenses recorded.\n")
        return

    if len(lines) == 0:
        print("No expenses recorded.\n")
        return

    start_date = input("Enter start date (YYYY-MM-DD): ")
    end_date = input("Enter end date (YYYY-MM-DD): ")

    total = 0
    categories = {}

    print("\nExpenses from " + start_date + " to " + end_date + ":")
    for line in lines:
        parts = line.strip().split(",")
        if len(parts) == 3:
            category = parts[0]
            amount = float(parts[1])
            date = parts[2]

            if start_date <= date <= end_date:   # simple string comparison
                print("Category: " + category + " | Amount: " + str(amount) + " | Date: " + date)
                total += amount
                if category not in categories:
                    categories[category] = 0
                categories[category] += amount

    if total == 0:
        print("No expenses in this period.\n")
        return

    print("\nTotal Expenses in this period:", total)
    print("By Category:")
    for category in categories:
        print(category + ":", categories[category])
    print("")

# -----------------------------
# Main Menu
# -----------------------------
def main():
    while True:
        print("Welcome to Personal Expense Tracker!")
        print("1. Add Expense")
        print("2. View All Expenses")
        print("3. Summary by Category (All Time)")
        print("4. Expenses Over a Period")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            category_summary()
        elif choice == "4":
            period_summary()
        elif choice == "5":
            print("Exiting... Goodbye!")
            break
        else:
            print("Invalid choice! Try again.\n")

# -----------------------------
# Run program
# -----------------------------
main()
