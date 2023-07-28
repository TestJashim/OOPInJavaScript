# OOPInJavaScript

```Javascript
/**
 * OOP Practice in JavaScript by MD. Jashim Uddin.
 */

// Abstract Transaction class
class Transaction{
    constructor(amount){
        this.amount = amount;
    }

    execute(account){
        // Abstract method to be implemented by subclasses.
    }
}

// Deposit class -Transaction subclass
class Deposit extends Transaction {
    execute(account){
        account.deposit(this.amount);
    }
}

// Withdrawal class - Transaction subclass
class Withdrawal extends Transaction {
    execute(account){
        account.withdraw(this.amount);
    }
}

// Customer class definition
class Customer {
    constructor(id, name, email){
        this.id = id;
        this.name = name;
        this.email = email;
        this.accounts = [];
    }

    addAccount(account){
        this.accounts.push(account);
    }

    getAccount(accountNumber){
        return this.accounts.find((account) => account.number === accountNumber);
    }

    removeAccount(accountNumber){
        this.accounts = this.accounts.filter((account) => account.number !== accountNumber);
    }

    getTotalBalance(){
        return this.accounts.reduce((total, account) => total + account.balance, 0);
    }

    toString(){
        return `Customer ID: ${this.id}, Name: ${this.name}, Email: ${this.email}`;
    }
}

// Account class definaition
class Account{
    constructor(number, type, balance = 0){
        this.number = number;
        this.type = type;
        this._balance = balance;
    }

    get balance(){
        return this._balance;
    }

    deposit(amount){
        if(amount > 0){
            this._balance += amount;
            console.log(`Deposited TK ${amount} into Account ${this.number}.`);
        } else {
            console.log("Invalid amount for deposit.");
        }
    }

    withdraw(amount){
        if(amount > 0 && this._balance >= amount){
            this._balance -= amount;
            console.log(`Withdrew TK ${amount} from Account ${this.number}.`);
        } else {
            console.log("Insufficient balance or invalid amount for withdrawal.");
        }
    }

    toString(){
        return `Account Number: ${this.number}, Type: ${this.type}, Balance: TK ${this._balance}`;
    }
}

// SavingsAccount class - Inheritance from Account class
class SavingsAccount extends Account{
    constructor(number, balance = 0, interestRate = 0.01){
        super(number, "Savings", balance);
        this.interestRate = interestRate;
    }

    addInterest(){
        const interestAmount = this.balance * this.interestRate;
        this.deposit(interestAmount);
        console.log(`Interest of TK ${interestAmount.toFixed(2)} added to Account ${this.number}.`);
    }
}


/**
 * Main Program
 */

const customer1 = new Customer(1, "Jashim Uddin", "jashim@example.com");
const customer2 = new Customer(2, "Rasel Ahmed", "rasel@example.com");

const account1 = new Account("A001", "Checking", 10000);
const account2 = new SavingsAccount("A002", 5000);
//const account2 = new SavingsAccount("A002", 50, 0.01); 

customer1.addAccount(account1);
customer2.addAccount(account2);

const depositTransaction = new Deposit(5000);
depositTransaction.execute(account1);

const withdrawalTransaction = new Withdrawal(2000);
withdrawalTransaction.execute(account1);

account2.addInterest();

console.log(customer1.toString());
console.log(account1.toString());
console.log(customer2.toString());
console.log(account2.toString());

customer1.removeAccount("A001");

console.log("Total balnce for Customer 1: ", customer1.getTotalBalance());
console.log("Total balnce for Customer 2: ", customer2.getTotalBalance());


```
