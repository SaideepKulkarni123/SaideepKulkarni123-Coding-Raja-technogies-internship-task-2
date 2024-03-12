import json
import os

# Function to load existing data from a file or create a new one
def load_data():
    if os.path.exists("budget_data.json"):
        with open("budget_data.json", "r") as file:
            return json.load(file)
    else:
        return {"income": 0, "expenses": [], "categories": {}}

# Function to save data to a file
def save_data(data):
    with open("budget_data.json", "w") as file:
        json.dump(data, file)

# Function to add income
def add_income(data, amount):
    data["income"] += amount

# Function to add an expense
def add_expense(data, category, amount):
    data["expenses"].append({"category": category, "amount": amount})
    data["categories"].setdefault(category, 0)
    data["categories"][category] += amount

# Function to calculate remaining budget
def calculate_budget(data):
    total_expenses = sum(expense["amount"] for expense in data["expenses"])
    remaining_budget = data["income"] - total_expenses
    return remaining_budget

# Function to analyze expenses
def analyze_expenses(data):
    categories = data["categories"]
    for category, amount in categories.items():
        print(f"{category}: ${amount}")

# Main function
def main():
    data = load_data()

    while True:
        print("\n1. Add Income")
        print("2. Add Expense")
        print("3. View Remaining Budget")
        print("4. Analyze Expenses")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            amount = float(input("Enter income amount: $"))
            add_income(data, amount)
            save_data(data)
        elif choice == "2":
            category = input("Enter expense category: ")
            amount = float(input("Enter expense amount: $"))
            add_expense(data, category, amount)
            save_data(data)
        elif choice == "3":
            remaining_budget = calculate_budget(data)
            print(f"Remaining budget: ${remaining_budget}")
        elif choice == "4":
            analyze_expenses(data)
        elif choice == "5":
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
    
