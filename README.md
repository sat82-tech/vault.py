import hashlib
import random
import string
import json
import os

VAULT_FILE = "vault_data.json"

def hash_master_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

def load_vault():
    if not os.path.exists(VAULT_FILE):
        return {}
    with open(VAULT_FILE, "r") as f:
        return json.load(f)

def save_vault(data):
    with open(VAULT_FILE, "w") as f:
        json.dump(data, f, indent=4)

def generate_strong_password(length=16):
    chars = string.ascii_letters + string.digits + "!@#$%^&*()_+"
    return "".join(random.choice(chars) for _ in range(length))

def main():
    print("==========================================")
    print(" 🚀 SATWIK STAR: SECURE PASSWORD VAULT CLI ")
    print("==========================================")
    
    vault = load_vault()
    
    while True:
        print("\n1. Store a Password")
        print("2. Retrieve a Password")
        print("3. Generate Strong Password")
        print("4. Exit")
        
        choice = input("\nSelect an option (1-4): ")
        
        if choice == '1':
            service = input("Enter Service Name (e.g., GitHub, Gmail): ").strip()
            pwd = input("Enter Password to Store: ").strip()
            vault[service] = hash_master_password(pwd)
            save_vault(vault)
            print(f"✅ Success! Password for '{service}' securely hashed and stored.")
            
        elif choice == '2':
            service = input("Enter Service Name to Search: ").strip()
            if service in vault:
                print(f"🔒 Stored Hash for '{service}': {vault[service]}")
            else:
                print(f"❌ No records found for '{service}'.")
                
        elif choice == '3':
            secure_pwd = generate_strong_password()
            print(f"✨ Generated Enterprise-Grade Password: {secure_pwd}")
            
        elif choice == '4':
            print("👋 Exiting Vault. Keep building big, Satwik Star!")
            break
        else:
            print("❌ Invalid option! Choose between 1 and 4.")

if __name__ == "__main__":
    main()
