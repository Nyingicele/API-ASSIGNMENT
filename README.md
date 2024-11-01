from abc import ABC, abstractmethod

# Abstract base class for Accounts
class Account(ABC):
    def __init__(self, owner, balance=0):
        self.__owner = owner
        self.__balance = balance

    @abstractmethod
    def deposit(self, amount):
        pass

    @abstractmethod
    def withdraw(self, amount):
        pass

    def get_balance(self):
        return self.__balance

    def get_owner(self):
        return self.__owner


# Savings Account class
class SavingsAccount(Account):
    def __init__(self, owner, balance=0, interest_rate=0.02):
        super().__init__(owner, balance)
        self.__interest_rate = interest_rate

    def deposit(self, amount):
        if amount > 0:
            self._Account__balance += amount
            print(f"{amount} deposited. New balance: {self.get_balance()}")
        else:
            print("Deposit amount must be positive.")

    def withdraw(self, amount):
        if 0 < amount <= self.get_balance():
            self._Account__balance -= amount
            print(f"{amount} withdrawn. New balance: {self.get_balance()}")
        else:
            print("Insufficient funds or invalid amount.")

    def apply_interest(self):
        interest = self.get_balance() * self.__interest_rate
        self.deposit(interest)
        print(f"Interest applied: {interest}. New balance: {self.get_balance()}")


# Checking Account class
class CheckingAccount(Account):
    def __init__(self, owner, balance=0, overdraft_limit=0):
        super().__init__(owner, balance)
        self.__overdraft_limit = overdraft_limit

    def deposit(self, amount):
        if amount > 0:
            self._Account__balance += amount
            print(f"{amount} deposited. New balance: {self.get_balance()}")
        else:
            print("Deposit amount must be positive.")

    def withdraw(self, amount):
        if amount > 0 and (amount <= self.get_balance() + self.__overdraft_limit):
            self._Account__balance -= amount
            print(f"{amount} withdrawn. New balance: {self.get_balance()}")
        else:
            print("Insufficient funds or invalid amount.")


# User class
class User:
    def __init__(self, name):
        self.name = name
        self.accounts = []

    def add_account(self, account):
        self.accounts.append(account)
        print(f"Account added for {self.name}.")

    def show_accounts(self):
        print(f"{self.name}'s Accounts:")
        for account in self.accounts:
            print(f"- {type(account).__name__}: Balance: {account.get_balance()}")

# Main program
def main():
    # Creating users
    user1 = User("Alice")
    user2 = User("Bob")

    # Creating accounts
    savings = SavingsAccount(user1.name, 1000)
    checking = CheckingAccount(user2.name, 500, overdraft_limit=200)

    # Adding accounts to users
    user1.add_account(savings)
    user2.add_account(checking)

    # User operations
    savings.deposit(500)
    savings.withdraw(200)
    savings.apply_interest()

    checking.deposit(300)
    checking.withdraw(800)  # This will use overdraft

    # Show account balances
    user1.show_accounts()
    user2.show_accounts()

if __name__ == "__main__":
    main()
