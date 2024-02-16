import random
from datetime import datetime, timedelta
import uuid
import json

# Enhanced Data Collector with detailed product purchase information
def generate_random_date(start_year=1990, end_year=2020):
    start_date = datetime(start_year, 1, 1)
    end_date = datetime(end_year, 12, 31)
    time_between_dates = end_date - start_date
    random_number_of_days = random.randrange(time_between_dates.days)
    return (start_date + timedelta(days=random_number_of_days)).strftime('%Y-%m-%d')

def generate_random_address():
    states = ['NY', 'CA', 'FL', 'TX', 'PA']
    return f"{random.randint(100, 999)} Main St, {random.choice(states)}"

def generate_product_purchased():
    products = [
        {"order": f"order{random.randint(100,999)}", "vendor": f"vendor{random.randint(1,10)}",
         "productID": f"ID-{random.randint(1000,9999)}", "quantity": random.randint(1,5),
         "dateOfOrder": generate_random_date(2015, 2020), "region": random.choice(["North", "South", "East", "West"])}
        for _ in range(random.randint(1,3))  # Each user can have multiple product purchases
    ]
    return products

def data_collector(num_records):
    users = []
    for _ in range(num_records):
        user = {
            "username": f"user{random.randint(1, 10000)}",
            "password": f"pass{random.randint(1000, 9999)}",
            "birthdate": generate_random_date(1950, 2000),
            "address": generate_random_address(),
            "social_security_number": f"{random.randint(100, 999)}-{random.randint(10, 99)}-{random.randint(1000, 9999)}",
            "productPurchased": generate_product_purchased(),
            "salesperson": f"sales{random.randint(1, 10)}"
        }
        users.append(user)
    return users

# Key/Value Pair Storage
def store_data(users):
    user_store = {}
    for user in users:
        user_id = str(uuid.uuid4())  # Generating a unique ID for each user
        user_store[user_id] = user
    return user_store

# Advanced Search Engine
def search_users(data_store, search_type, search_value):
    results = []
    for user_id, user_data in data_store.items():
        if search_type == 'state':
            address_state = user_data['address'].split(", ")[1]
            if search_value == address_state:
                results.append((user_id, user_data))
        elif search_type == 'salesperson':
            if user_data['salesperson'] == search_value:
                results.append((user_id, user_data))
    return results

# Neatly print search results
def print_search_results(results):
    for user_id, user_data in results:
        print(f"\nUser ID: {user_id}")
        print(json.dumps(user_data, indent=4))

# Main execution with interactive search
if __name__ == "__main__":
    # Generate and store user data
    num_records = 10
    users = data_collector(num_records)
    user_store = store_data(users)

    while True:
        # Ask the user what type of search they want to perform
        print("\nSearch Options:")
        print("1. Search by state")
        print("2. Search by salesperson")
        print("3. Exit")
        choice = input("Enter your choice (1/2/3): ")

        if choice == '1':
            search_state = input("Enter the state to search for (e.g., NY): ")
            search_results = search_users(user_store, 'state', search_state)
            print(f"\nSearch Results for state {search_state}:")
            print_search_results(search_results)
        
        elif choice == '2':
            search_salesperson = input("Enter the salesperson to search for (e.g., sales1): ")
            search_results = search_users(user_store, 'salesperson', search_salesperson)
            print(f"\nSearch Results for salesperson {search_salesperson}:")
            print_search_results(search_results)
        
        elif choice == '3':
            print("Exiting search...")
            break
        else:
            print("Invalid choice. Please enter 1, 2, or 3.")
