import random
import string

# Simple dictionary list for demonstration purposes
dictionary_list = ['password', '123456', '123456789', 'qwerty', 'abc123', 'monkey', '1234567', 'letmein']

# Function to generate a random password
def generate_password():
    length = random.randint(8, 16)  # Choose a random length for the password
    characters = string.ascii_letters + string.digits + string.punctuation  # Pool of characters to choose from
    password = ''.join(random.choice(characters) for i in range(length))  # Generate a random password
    return password

# Function to check if the password is acceptable
def is_acceptable(password):
    if any(char in string.punctuation for char in password) and password not in dictionary_list:
        return True
    else:
        return False

# Main loop to generate and archive acceptable passwords
archived_passwords = []
iterations = 40  # Minimum number of iterations

for _ in range(iterations):
    password = generate_password()
    if is_acceptable(password):
        archived_passwords.append(password)  # Archive the password if it's acceptable

# Display the archived passwords in a neat manner
print("Archived Passwords:")
for i, password in enumerate(archived_passwords, start=1):
    print(f"{i}. {password}")
