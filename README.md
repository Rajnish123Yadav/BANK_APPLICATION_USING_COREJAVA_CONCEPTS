# BANK_APPLICATION_USING_COREJAVA_CONCEPTS

import java.util.Scanner;

class InsufficientFundsException extends Exception {
	InsufficientFundsException(String s) {
		super(s);
	}
}

class InvalidAmountException extends Exception {
	InvalidAmountException(String s) {
		super(s);
	}
}

class AccountNotFoundException extends Exception {
	AccountNotFoundException(String s) {
		super(s);
	}
}

class LoanNotAllowedException extends Exception {
	LoanNotAllowedException(String s) {
		super(s);
	}
}

interface Bank {
	public void deposite(double amount) throws InvalidAmountException;
	public double withdral(double amount) throws InsufficientFundsException, InvalidAmountException;
	public void transfer(BankAccount toAccount, double amount) throws InsufficientFundsException, AccountNotFoundException, InvalidAmountException;
	public void applyForLoan(double amount) throws LoanNotAllowedException, InvalidAmountException;
	public double getBalance();
}

class BankAccount implements Bank {
	private long accountNumber;
	private double balance;
	
	
	public BankAccount(long accountNumber, double balance) {
		super();
		this.accountNumber = accountNumber;
		this.balance = balance;
	}
	@Override
	public void deposite(double amount) throws InvalidAmountException {
		if(amount <= 0) {
			throw new InvalidAmountException("Error: Invalid Amount");
		}else {
			balance += amount;
			System.out.println("Balance deposited."+balance);
		}
		
	}
	@Override
	public double withdral(double amount) throws InsufficientFundsException, InvalidAmountException {
		if(amount <= 0) {
			throw new InvalidAmountException("Error: Invalid Amount");
		}else if(amount > balance) {
			throw new InsufficientFundsException("Error: InsufficientFunds");
		}else {
			System.out.println("successful withdral Rs."+amount);
			balance -= amount;
			return amount;
		}
	}
	@Override
	public void transfer(BankAccount toAccount, double amount) throws InsufficientFundsException, AccountNotFoundException, InvalidAmountException {
		if (toAccount.equals(toAccount)) {
	        throw new AccountNotFoundException("Error: Account Not Found");
	    }
	    if (amount <= 0) {
	        throw new InvalidAmountException("Error: Invalid Amount");
	    }
	    if (amount > balance) {
	        throw new InsufficientFundsException("Error: Insufficient Funds");
	    }
	    balance -= amount;
	    toAccount.deposite(amount);
	    System.out.println("Successfully transferred amount: " + amount);
	}
	
	@Override
	public void applyForLoan(double amount) throws LoanNotAllowedException, InvalidAmountException {
		if (amount <= 0) {
	        throw new InvalidAmountException("Error: Invalid Amount");
	    }
	    if (amount > 50000) {
	        throw new LoanNotAllowedException("Error: Loan Not Allowed");
	    }
	    System.out.println("Loan application successful for: " + amount);
	}
	
	@Override
	public double getBalance() {
		return balance;
	}
}

class Customer {
	private String Name;
	private BankAccount account;
	public Customer(String name, BankAccount account) {
		super();
		Name = name;
		this.account = account;
	}
	public BankAccount getAccount() {
		return account;
	} 
	
	
}

class Bank_Application {//ATM Class 

	public static void main(String[] args) throws InsufficientFundsException, AccountNotFoundException, LoanNotAllowedException {
		
		System.out.println("1. Deposit"
				+ "\n"+ "2. Withdraw"
				+ "\n3. Transfer"
				+ "\n4. Loan Application"
				+ "\n5. Check Balance"
				+ "\n6. Exit");
			
		try(Scanner sc = new Scanner(System.in);) {
			System.out.println("\nEnter user account number: ");
			long account_Number = sc.nextLong();
			System.out.print("Enter initial balance:");
			double balance = sc.nextDouble();
			
			BankAccount BankAccount = new BankAccount(account_Number,balance);

			System.out.print("Enter your choice: ");
			int choice = sc.nextInt();
			switch (choice) {
            case 1:
                System.out.print("Enter deposit amount: ");
                double depositAmount = sc.nextDouble();
                BankAccount.deposite(depositAmount);
                break;

            case 2:
                System.out.print("Enter withdrawal amount: ");
                double withdrawAmount = sc.nextDouble();
                System.out.println("Withdrawn: " + BankAccount.withdral(withdrawAmount));
                break;

            case 3:
                System.out.print("Enter user account number: ");
                long AccountNumber = sc.nextLong();
                BankAccount Account = new BankAccount(AccountNumber, 0); // Dummy recipient account
                System.out.print("Enter transfer amount: ");
                double transferAmount = sc.nextDouble();
                BankAccount.transfer(Account, transferAmount);
                break;

            case 4:
                System.out.print("Enter loan amount: ");
                double loanAmount = sc.nextDouble();
                BankAccount.applyForLoan(loanAmount);
                break;

            case 5:
                System.out.println("Current Balance: " + BankAccount.getBalance());
                break;

            case 6:
                System.out.println("Exiting...");
                sc.close();
                System.exit(0);

            default:
                System.out.println("Invalid choice, please try again.");
			}
			
		} 
		
			catch(InvalidAmountException | InsufficientFundsException | AccountNotFoundException | LoanNotAllowedException e) {
				System.out.println("Error: "+e.getMessage());
			} 
		
		finally 
		{
			System.out.println("opereation successfully.");
		}
		
	}
	
}
