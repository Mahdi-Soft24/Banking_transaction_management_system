#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <ctype.h>

// Structure for customer account
struct Account
{
    int accountNumber;
    char name[50];
    char mobile[15];
    char address[100];
    int pin;
    float balance;
    int isActive;
};

// Structure for transaction
struct Transaction
{
    int accountNumber;
    char type[20]; // "Deposit", "Withdrawal", "Transfer"
    float amount;
    time_t timestamp;
    char description[100];
};

// Structure for loan
struct Loan
{
    int accountNumber;
    float amount;
    float interestRate;
    time_t issueDate;
    time_t dueDate;
    int isApproved;
};

// Global variables
struct Account accounts[1000];
struct Transaction transactions[10000];
struct Loan loans[100];
int accountCount = 0;
int transactionCount = 0;
int loanCount = 0;
int currentUser = -1; // -1 means no user logged in, 0 is admin, >0 is customer account number

// Function prototypes
void loadData();
void saveData();
void mainMenu();
void adminMenu();
void customerMenu();
void registerAccount();
int login();
void deposit();
void withdraw();
void checkBalance();
void transfer();
void viewTransactions();
void applyForLoan();
void manageLoans();
void calculateInterest();
void closeAccount();
void viewAllAccounts();
void changePin();
void logout();
int generateAccountNumber();
int validatePin(int accountNumber, int pin);
int findAccount(int accountNumber);
void addTransaction(int accountNumber, char type[], float amount, char description[]);
void printTransaction(struct Transaction t);
void printAccount(struct Account a);

int main()
{
    loadData();
    mainMenu();
    saveData();
    return 0;
}

void loadData()
{
    // In a real implementation, this would load from files
    // For this example, we'll initialize with some dummy data
    accountCount = 2;

    // Admin account
    accounts[0].accountNumber = 0;
    strcpy(accounts[0].name, "Admin");
    strcpy(accounts[0].mobile, "1234567890");
    strcpy(accounts[0].address, "Bank Headquarters");
    accounts[0].pin = 1234;
    accounts[0].balance = 0;
    accounts[0].isActive = 1;

    // Sample customer account
    accounts[1].accountNumber = 1001;
    strcpy(accounts[1].name, "Sample Customer");
    strcpy(accounts[1].mobile, "9876543210");
    strcpy(accounts[1].address, "123 Main St");
    accounts[1].pin = 4321;
    accounts[1].balance = 5000;
    accounts[1].isActive = 1;

    printf("Data loaded successfully.\n");
}

void saveData()
{
    // In a real implementation, this would save to files
    printf("Data saved successfully.\n");
}

void mainMenu()
{
    int choice;
    do
    {
        printf("\n=== LenaDena Banking System ===\n");
        printf("1. Register New Account\n");
        printf("2. Login\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            registerAccount();
            break;
        case 2:
            if (login())
            {
                if (currentUser == 0)
                {
                    adminMenu();
                }
                else
                {
                    customerMenu();
                }
            }
            break;
        case 3:
            printf("Thank you for using LenaDena Banking System.\n");
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 3);
}

void adminMenu()
{
    int choice;
    do
    {
        printf("\n=== Admin Dashboard ===\n");
        printf("1. View All Accounts\n");
        printf("2. Manage Loans\n");
        printf("3. Calculate Interest\n");
        printf("4. Close Account\n");
        printf("5. Logout\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            viewAllAccounts();
            break;
        case 2:
            manageLoans();
            break;
        case 3:
            calculateInterest();
            break;
        case 4:
            closeAccount();
            break;
        case 5:
            logout();
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 5);
}

void customerMenu()
{
    int choice;
    do
    {
        printf("\n=== Customer Dashboard ===\n");
        printf("1. Deposit Money\n");
        printf("2. Withdraw Money\n");
        printf("3. Check Balance\n");
        printf("4. Transfer Money\n");
        printf("5. View Transactions\n");
        printf("6. Apply for Loan\n");
        printf("7. Change PIN\n");
        printf("8. Logout\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            deposit();
            break;
        case 2:
            withdraw();
            break;
        case 3:
            checkBalance();
            break;
        case 4:
            transfer();
            break;
        case 5:
            viewTransactions();
            break;
        case 6:
            applyForLoan();
            break;
        case 7:
            changePin();
            break;
        case 8:
            logout();
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 8);
}

void registerAccount()
{
    if (accountCount >= 1000)
    {
        printf("Cannot create more accounts. System limit reached.\n");
        return;
    }

    struct Account newAccount;
    newAccount.accountNumber = generateAccountNumber();

    printf("\n=== New Account Registration ===\n");
    printf("Enter your name: ");
    scanf(" %[^\n]", newAccount.name);

    printf("Enter mobile number: ");
    scanf("%s", newAccount.mobile);

    printf("Enter address: ");
    scanf(" %[^\n]", newAccount.address);

    printf("Set a 4-digit PIN: ");
    scanf("%d", &newAccount.pin);

    newAccount.balance = 0;
    newAccount.isActive = 1;

    accounts[accountCount++] = newAccount;

    printf("\nRegistration successful!\n");
    printf("Your account number is: %d\n", newAccount.accountNumber);
    printf("Please remember this number for future logins.\n");
}

int login()
{
    int accountNumber, pin;

    printf("\n=== Login ===\n");
    printf("Enter account number (0 for admin): ");
    scanf("%d", &accountNumber);

    printf("Enter PIN: ");
    scanf("%d", &pin);

    if (accountNumber == 0)
    {
        // Admin login
        if (pin == accounts[0].pin)
        {
            currentUser = 0;
            printf("Admin login successful!\n");
            return 1;
        }
        else
        {
            printf("Invalid admin PIN.\n");
            return 0;
        }
    }
    else
    {
        // Customer login
        int accountIndex = findAccount(accountNumber);
        if (accountIndex != -1)
        {
            if (accounts[accountIndex].pin == pin)
            {
                if (accounts[accountIndex].isActive)
                {
                    currentUser = accountNumber;
                    printf("Login successful! Welcome, %s.\n", accounts[accountIndex].name);
                    return 1;
                }
                else
                {
                    printf("This account is closed.\n");
                    return 0;
                }
            }
            else
            {
                printf("Invalid PIN.\n");
                return 0;
            }
        }
        else
        {
            printf("Account not found.\n");
            return 0;
        }
    }
}

void deposit()
{
    if (currentUser <= 0)
        return;

    float amount;
    printf("\n=== Deposit Money ===\n");
    printf("Enter amount to deposit: ");
    scanf("%f", &amount);

    if (amount <= 0)
    {
        printf("Invalid amount.\n");
        return;
    }

    int accountIndex = findAccount(currentUser);
    accounts[accountIndex].balance += amount;

    char desc[50];
    sprintf(desc, "Deposit to account %d", currentUser);
    addTransaction(currentUser, "Deposit", amount, desc);

    printf("Deposit successful. New balance: %.2f\n", accounts[accountIndex].balance);
}

void withdraw()
{
    if (currentUser <= 0)
        return;

    float amount;
    printf("\n=== Withdraw Money ===\n");
    printf("Enter amount to withdraw: ");
    scanf("%f", &amount);

    if (amount <= 0)
    {
        printf("Invalid amount.\n");
        return;
    }

    int accountIndex = findAccount(currentUser);
    if (accounts[accountIndex].balance < amount)
    {
        printf("Insufficient balance.\n");
        return;
    }

    accounts[accountIndex].balance -= amount;

    char desc[50];
    sprintf(desc, "Withdrawal from account %d", currentUser);
    addTransaction(currentUser, "Withdrawal", amount, desc);

    printf("Withdrawal successful. New balance: %.2f\n", accounts[accountIndex].balance);
}

void checkBalance()
{
    if (currentUser <= 0)
        return;

    int accountIndex = findAccount(currentUser);
    printf("\n=== Account Balance ===\n");
    printf("Account Number: %d\n", currentUser);
    printf("Account Holder: %s\n", accounts[accountIndex].name);
    printf("Current Balance: %.2f\n", accounts[accountIndex].balance);
}

void transfer()
{
    if (currentUser <= 0)
        return;

    int targetAccount;
    float amount;

    printf("\n=== Transfer Money ===\n");
    printf("Enter recipient account number: ");
    scanf("%d", &targetAccount);

    int targetIndex = findAccount(targetAccount);
    if (targetIndex == -1)
    {
        printf("Recipient account not found.\n");
        return;
    }

    if (!accounts[targetIndex].isActive)
    {
        printf("Recipient account is closed.\n");
        return;
    }

    printf("Enter amount to transfer: ");
    scanf("%f", &amount);

    if (amount <= 0)
    {
        printf("Invalid amount.\n");
        return;
    }

    int sourceIndex = findAccount(currentUser);
    if (accounts[sourceIndex].balance < amount)
    {
        printf("Insufficient balance.\n");
        return;
    }

    // Perform transfer
    accounts[sourceIndex].balance -= amount;
    accounts[targetIndex].balance += amount;

    char desc1[50], desc2[50];
    sprintf(desc1, "Transfer to account %d", targetAccount);
    sprintf(desc2, "Transfer from account %d", currentUser);

    addTransaction(currentUser, "Transfer", -amount, desc1);
    addTransaction(targetAccount, "Transfer", amount, desc2);

    printf("Transfer successful.\n");
    printf("Your new balance: %.2f\n", accounts[sourceIndex].balance);
}

void viewTransactions()
{
    if (currentUser <= 0)
        return;

    printf("\n=== Transaction History ===\n");
    int found = 0;

    for (int i = 0; i < transactionCount; i++)
    {
        if (transactions[i].accountNumber == currentUser)
        {
            printTransaction(transactions[i]);
            found = 1;
        }
    }

    if (!found)
    {
        printf("No transactions found.\n");
    }
}

void applyForLoan()
{
    if (currentUser <= 0)
        return;

    float amount;
    printf("\n=== Apply for Loan ===\n");
    printf("Enter loan amount: ");
    scanf("%f", &amount);

    if (amount <= 0)
    {
        printf("Invalid amount.\n");
        return;
    }

    if (loanCount >= 100)
    {
        printf("Loan application system is currently full.\n");
        return;
    }

    struct Loan newLoan;
    newLoan.accountNumber = currentUser;
    newLoan.amount = amount;
    newLoan.interestRate = 10.0; // Default 10% interest
    newLoan.issueDate = time(NULL);
    newLoan.dueDate = newLoan.issueDate + (365 * 24 * 60 * 60); // 1 year from now
    newLoan.isApproved = 0;

    loans[loanCount++] = newLoan;

    printf("Loan application submitted successfully.\n");
    printf("An admin will review your application.\n");
}

void manageLoans()
{
    if (currentUser != 0)
        return;

    printf("\n=== Loan Management ===\n");

    int choice;
    do
    {
        printf("\n1. View Pending Loans\n");
        printf("2. Approve Loan\n");
        printf("3. View All Loans\n");
        printf("4. Back to Admin Menu\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
        {
            printf("\n=== Pending Loans ===\n");
            int found = 0;
            for (int i = 0; i < loanCount; i++)
            {
                if (!loans[i].isApproved)
                {
                    printf("Loan ID: %d\n", i);
                    printf("Account: %d\n", loans[i].accountNumber);
                    printf("Amount: %.2f\n", loans[i].amount);
                    printf("Interest Rate: %.2f%%\n", loans[i].interestRate);
                    printf("Applied on: %s", ctime(&loans[i].issueDate));
                    printf("\n");
                    found = 1;
                }
            }
            if (!found)
                printf("No pending loans.\n");
            break;
        }
        case 2:
        {
            int loanId;
            printf("Enter loan ID to approve: ");
            scanf("%d", &loanId);

            if (loanId < 0 || loanId >= loanCount)
            {
                printf("Invalid loan ID.\n");
                break;
            }

            if (loans[loanId].isApproved)
            {
                printf("Loan already approved.\n");
                break;
            }

            loans[loanId].isApproved = 1;
            int accIndex = findAccount(loans[loanId].accountNumber);
            accounts[accIndex].balance += loans[loanId].amount;

            char desc[50];
            sprintf(desc, "Loan approved for %.2f", loans[loanId].amount);
            addTransaction(loans[loanId].accountNumber, "Loan", loans[loanId].amount, desc);

            printf("Loan approved and amount credited to account.\n");
            break;
        }
        case 3:
        {
            printf("\n=== All Loans ===\n");
            for (int i = 0; i < loanCount; i++)
            {
                printf("Loan ID: %d\n", i);
                printf("Account: %d\n", loans[i].accountNumber);
                printf("Amount: %.2f\n", loans[i].amount);
                printf("Interest Rate: %.2f%%\n", loans[i].interestRate);
                printf("Status: %s\n", loans[i].isApproved ? "Approved" : "Pending");
                printf("Issue Date: %s", ctime(&loans[i].issueDate));
                printf("Due Date: %s", ctime(&loans[i].dueDate));
                printf("\n");
            }
            break;
        }
        case 4:
            break;
        default:
            printf("Invalid choice.\n");
        }
    } while (choice != 4);
}

void calculateInterest()
{
    if (currentUser != 0)
        return;

    printf("\n=== Interest Calculation ===\n");

    float interestRate = 5.0; // 5% annual interest
    int days = 30;            // Calculate for 30 days

    for (int i = 1; i < accountCount; i++)
    {
        if (accounts[i].isActive && accounts[i].balance > 0)
        {
            float interest = accounts[i].balance * (interestRate / 100) * (days / 365.0);
            accounts[i].balance += interest;

            char desc[50];
            sprintf(desc, "Interest for %d days @ %.2f%%", days, interestRate);
            addTransaction(accounts[i].accountNumber, "Interest", interest, desc);

            printf("Account %d: Added %.2f interest. New balance: %.2f\n",
                   accounts[i].accountNumber, interest, accounts[i].balance);
        }
    }

    printf("Interest calculation completed for all accounts.\n");
}

void closeAccount()
{
    if (currentUser == 0)
    {
        // Admin closing an account
        int accountNumber;
        printf("\n=== Close Customer Account ===\n");
        printf("Enter account number to close: ");
        scanf("%d", &accountNumber);

        int accountIndex = findAccount(accountNumber);
        if (accountIndex == -1)
        {
            printf("Account not found.\n");
            return;
        }

        if (accountNumber == 0)
        {
            printf("Cannot close admin account.\n");
            return;
        }

        if (!accounts[accountIndex].isActive)
        {
            printf("Account is already closed.\n");
            return;
        }

        if (accounts[accountIndex].balance != 0)
        {
            printf("Cannot close account with non-zero balance.\n");
            return;
        }

        accounts[accountIndex].isActive = 0;
        printf("Account %d has been closed successfully.\n", accountNumber);
    }
    else
    {
        // Customer requesting account closure
        printf("\n=== Account Closure Request ===\n");

        int accountIndex = findAccount(currentUser);
        if (accounts[accountIndex].balance != 0)
        {
            printf("Your account balance must be zero to close the account.\n");
            printf("Current balance: %.2f\n", accounts[accountIndex].balance);
            return;
        }

        accounts[accountIndex].isActive = 0;
        printf("Your account has been closed successfully.\n");
        printf("Thank you for using LenaDena Banking System.\n");
        logout();
    }
}

void viewAllAccounts()
{
    if (currentUser != 0)
        return;

    printf("\n=== All Customer Accounts ===\n");
    printf("%-15s %-20s %-15s %-10s\n", "Account No.", "Name", "Balance", "Status");
    printf("------------------------------------------------\n");

    for (int i = 1; i < accountCount; i++)
    {
        printf("%-15d %-20s %-15.2f %-10s\n",
               accounts[i].accountNumber,
               accounts[i].name,
               accounts[i].balance,
               accounts[i].isActive ? "Active" : "Closed");
    }
}

void changePin()
{
    if (currentUser <= 0)
        return;

    int oldPin, newPin;
    printf("\n=== Change PIN ===\n");
    printf("Enter current PIN: ");
    scanf("%d", &oldPin);

    int accountIndex = findAccount(currentUser);
    if (accounts[accountIndex].pin != oldPin)
    {
        printf("Incorrect current PIN.\n");
        return;
    }

    printf("Enter new 4-digit PIN: ");
    scanf("%d", &newPin);

    if (newPin < 1000 || newPin > 9999)
    {
        printf("PIN must be a 4-digit number.\n");
        return;
    }

    accounts[accountIndex].pin = newPin;
    printf("PIN changed successfully.\n");
}

void logout()
{
    printf("Logging out...\n");
    currentUser = -1;
}

int generateAccountNumber()
{
    // Simple implementation - in a real system this would be more sophisticated
    static int lastAccountNumber = 1000;
    return ++lastAccountNumber;
}

int validatePin(int accountNumber, int pin)
{
    int accountIndex = findAccount(accountNumber);
    if (accountIndex == -1)
        return 0;
    return accounts[accountIndex].pin == pin;
}

int findAccount(int accountNumber)
{
    for (int i = 0; i < accountCount; i++)
    {
        if (accounts[i].accountNumber == accountNumber)
        {
            return i;
        }
    }
    return -1;
}

void addTransaction(int accountNumber, char type[], float amount, char description[])
{
    if (transactionCount >= 10000)
    {
        printf("Transaction log full. Cannot add more transactions.\n");
        return;
    }

    struct Transaction newTrans;
    newTrans.accountNumber = accountNumber;
    strcpy(newTrans.type, type);
    newTrans.amount = amount;
    newTrans.timestamp = time(NULL);
    strcpy(newTrans.description, description);

    transactions[transactionCount++] = newTrans;
}

void printTransaction(struct Transaction t)
{
    printf("Date: %s", ctime(&t.timestamp));
    printf("Type: %s\n", t.type);
    printf("Amount: %.2f\n", t.amount);
    printf("Description: %s\n", t.description);
    printf("------------------------\n");
}

void printAccount(struct Account a)
{
    printf("Account Number: %d\n", a.accountNumber);
    printf("Name: %s\n", a.name);
    printf("Mobile: %s\n", a.mobile);
    printf("Address: %s\n", a.address);
    printf("Balance: %.2f\n", a.balance);
    printf("Status: %s\n", a.isActive ? "Active" : "Closed");
    printf("------------------------\n");
}
