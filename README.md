SYSTEM DESIGN 
✅ Class 1: User
Responsibility:
Stores user details such as username and password.
Attributes:
username
password
Methods:
setUsername()
setPassword()
getUsername()
getPassword()
✅ Class 2: PasswordSecurity
Responsibility:
Checks whether the password entered is strong.
Password Strength Rules: ✔ Minimum 8 characters
✔ Contains uppercase letter
✔ Contains lowercase letter
✔ Contains number
✔ Contains special character (@, #, $, etc)
Methods:
checkLength()
hasUpperCase()
hasLowerCase()
hasDigit()
hasSpecialCharacter()
isStrongPassword()
✅ Class 3: AuthenticationSystem
Responsibility:
Handles login and verifies user credentials.
Methods:
registerUser()
loginUser()
authenticate()

CODE IMPLEMENTATION 
import hashlib

# ---------------- PASSWORD SECURITY CLASS ----------------
class PasswordSecurity:

    # Hash password using SHA-256
    def hash_password(self, password):
        return hashlib.sha256(password.encode()).hexdigest()

    # Check password strength
    def check_strength(self, password):

        if len(password) < 8:
            print("Password must be at least 8 characters.")
            return False

        if not any(char.isupper() for char in password):
            print("Password must contain at least one uppercase letter.")
            return False

        if not any(char.islower() for char in password):
            print("Password must contain at least one lowercase letter.")
            return False

        if not any(char.isdigit() for char in password):
            print("Password must contain at least one number.")
            return False

        return True


# ---------------- USER CLASS ----------------
class User:
    def __init__(self, username, hashed_password):
        self.username = username
        self.hashed_password = hashed_password


# ---------------- AUTHENTICATION SYSTEM ----------------
class AuthenticationSystem:

    def __init__(self):
        self.security = PasswordSecurity()
        self.user = None

    # Register Method
    def register(self):

        username = input("Enter username: ")
        password = input("Enter password: ")

        if self.security.check_strength(password):

            hashed_password = self.security.hash_password(password)
            self.user = User(username, hashed_password)

            print("Registration successful!")
        else:
            print("Strong password required.")

    # Login Method
    def login(self):

        username = input("Enter username: ")
        password = input("Enter password: ")

        hashed_input = self.security.hash_password(password)

        if self.user and username == self.user.username and hashed_input == self.user.hashed_password:
            print("Login successful! Access granted.")
        else:
            print("Login failed! Access denied.")


# ---------------- MAIN PROGRAM ----------------
system = AuthenticationSystem()

while True:

    print("\n==== AUTHENTICATION SYSTEM ====")
    print("1. Register")
    print("2. Login")
    print("3. Exit")

    option = input("Choose option: ")

    if option == "1":
        system.register()

    elif option == "2":
        system.login()

    elif option == "3":
        print("Exiting system...")
        break

    else:
        print("Invalid option!")
