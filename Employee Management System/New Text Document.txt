#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100

// Updated Employee Structure
struct Employee {
    int empID;
    char name[50];
    char department[50];
    float salary;
    char address[100];
    char contactNumber[15];
    char dateOfBirth[11];  // Format: YYYY-MM-DD
    char bloodGroup[5];
    char email[50];
    char gender[10];
};

void addEmployee();
void viewEmployee(int empID);
void updateEmployee(int empID);
void deleteEmployee(int empID);
void displayAllEmployees();

void addEmployee() {
    FILE *file = fopen("employee.txt", "a");
    if (file == NULL) {
        printf("Unable to open file.\n");
        return;
    }

    struct Employee emp;

    printf("Enter Employee ID: ");
    scanf("%d", &emp.empID);
    getchar();  // clear newline

    printf("Enter Name: ");
    fgets(emp.name, sizeof(emp.name), stdin);
    emp.name[strcspn(emp.name, "\n")] = '\0';

    printf("Enter Department: ");
    fgets(emp.department, sizeof(emp.department), stdin);
    emp.department[strcspn(emp.department, "\n")] = '\0';

    printf("Enter Salary: ");
    scanf("%f", &emp.salary);
    getchar();  // clear newline after scanf

    printf("Enter Address: ");
    fgets(emp.address, sizeof(emp.address), stdin);
    emp.address[strcspn(emp.address, "\n")] = '\0';

    printf("Enter Contact Number: ");
    fgets(emp.contactNumber, sizeof(emp.contactNumber), stdin);
    emp.contactNumber[strcspn(emp.contactNumber, "\n")] = '\0';

   

    printf("Enter Blood Group: ");
    fgets(emp.bloodGroup, sizeof(emp.bloodGroup), stdin);
    emp.bloodGroup[strcspn(emp.bloodGroup, "\n")] = '\0';

    printf("Enter Email: ");
    fgets(emp.email, sizeof(emp.email), stdin);
    emp.email[strcspn(emp.email, "\n")] = '\0';

    printf("Enter Gender: ");
    fgets(emp.gender, sizeof(emp.gender), stdin);
    emp.gender[strcspn(emp.gender, "\n")] = '\0';
    
     printf("Enter Date of Birth (YYYY/MM/DD): ");
    fgets(emp.dateOfBirth, sizeof(emp.dateOfBirth), stdin);
    emp.dateOfBirth[strcspn(emp.dateOfBirth, "\n")] = '\0';

    fprintf(file, "%d|%s|%s|%.2f|%s|%s|%s|%s|%s|%s\n",
        emp.empID, emp.name, emp.department, emp.salary, emp.address,
        emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);

    fclose(file);
    printf("Employee added successfully!\n");
}

void viewEmployee(int empID) {
    FILE *file = fopen("employee.txt", "r");
    if (file == NULL) {
        printf("Unable to open file.\n");
        return;
    }

    struct Employee emp;
    int found = 0;
    char line[512];

    while (fgets(line, sizeof(line), file)) {
        sscanf(line, "%d|%49[^|]|%49[^|]|%f|%99[^|]|%14[^|]|%10[^|]|%4[^|]|%49[^|]|%9[^\n]",
            &emp.empID, emp.name, emp.department, &emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);

        if (emp.empID == empID) {
            printf("\nEmployee Details:\n");
            printf("ID: %d\n", emp.empID);
            printf("Name: %s\n", emp.name);
            printf("Department: %s\n", emp.department);
            printf("Salary: %.2f\n", emp.salary);
            printf("Address: %s\n", emp.address);
            printf("Contact Number: %s\n", emp.contactNumber);
            printf("Date of Birth: %s\n", emp.dateOfBirth);
            printf("Blood Group: %s\n", emp.bloodGroup);
            printf("Email: %s\n", emp.email);
            printf("Gender: %s\n", emp.gender);
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("Employee with ID %d not found.\n", empID);
    }

    fclose(file);
}

void updateEmployee(int empID) {
    FILE *file = fopen("employee.txt", "r");
    FILE *temp = fopen("temp.txt", "w");

    if (!file || !temp) {
        printf("Error opening files.\n");
        return;
    }

    struct Employee emp;
    int found = 0;
    char line[512];

    while (fgets(line, sizeof(line), file)) {
        sscanf(line, "%d|%49[^|]|%49[^|]|%f|%99[^|]|%14[^|]|%10[^|]|%4[^|]|%49[^|]|%9[^\n]",
            &emp.empID, emp.name, emp.department, &emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);

        if (emp.empID == empID) {
            found = 1;
            printf("Enter new Name: ");
            getchar();
            fgets(emp.name, sizeof(emp.name), stdin);
            emp.name[strcspn(emp.name, "\n")] = '\0';

            printf("Enter new Department: ");
            fgets(emp.department, sizeof(emp.department), stdin);
            emp.department[strcspn(emp.department, "\n")] = '\0';

            printf("Enter new Salary: ");
            scanf("%f", &emp.salary);
            getchar();

            printf("Enter new Address: ");
            fgets(emp.address, sizeof(emp.address), stdin);
            emp.address[strcspn(emp.address, "\n")] = '\0';

            printf("Enter new Contact Number: ");
            fgets(emp.contactNumber, sizeof(emp.contactNumber), stdin);
            emp.contactNumber[strcspn(emp.contactNumber, "\n")] = '\0';

            

            printf("Enter new Blood Group: ");
            fgets(emp.bloodGroup, sizeof(emp.bloodGroup), stdin);
            emp.bloodGroup[strcspn(emp.bloodGroup, "\n")] = '\0';

            printf("Enter new Email: ");
            fgets(emp.email, sizeof(emp.email), stdin);
            emp.email[strcspn(emp.email, "\n")] = '\0';

            printf("Enter new Gender: ");
            fgets(emp.gender, sizeof(emp.gender), stdin);
            emp.gender[strcspn(emp.gender, "\n")] = '\0';
            
            printf("Enter new Date of Birth: ");
            fgets(emp.dateOfBirth, sizeof(emp.dateOfBirth), stdin);
            emp.dateOfBirth[strcspn(emp.dateOfBirth, "\n")] = '\0';
        }

        fprintf(temp, "%d|%s|%s|%.2f|%s|%s|%s|%s|%s|%s\n",
            emp.empID, emp.name, emp.department, emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);
    }

    fclose(file);
    fclose(temp);

    remove("employee.txt");
    rename("temp.txt", "employee.txt");

    if (found)
        printf("Employee updated successfully.\n");
    else
        printf("Employee with ID %d not found.\n", empID);
}

void deleteEmployee(int empID) {
    FILE *file = fopen("employee.txt", "r");
    FILE *temp = fopen("temp.txt", "w");

    if (!file || !temp) {
        printf("Error opening files.\n");
        return;
    }

    struct Employee emp;
    char line[512];
    int found = 0;

    while (fgets(line, sizeof(line), file)) {
        sscanf(line, "%d|%49[^|]|%49[^|]|%f|%99[^|]|%14[^|]|%10[^|]|%4[^|]|%49[^|]|%9[^\n]",
            &emp.empID, emp.name, emp.department, &emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);

        if (emp.empID != empID) {
            fprintf(temp, "%d|%s|%s|%.2f|%s|%s|%s|%s|%s|%s\n",
                emp.empID, emp.name, emp.department, emp.salary, emp.address,
                emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);
        } else {
            found = 1;
        }
    }

    fclose(file);
    fclose(temp);

    remove("employee.txt");
    rename("temp.txt", "employee.txt");

    if (found)
        printf("Employee deleted successfully.\n");
    else
        printf("Employee with ID %d not found.\n", empID);
}

void displayAllEmployees() {
    FILE *file = fopen("employee.txt", "r");
    if (!file) {
        printf("Unable to open file.\n");
        return;
    }

    struct Employee emp;
    char line[512];

    printf("\nAll Employee Records:\n");
    while (fgets(line, sizeof(line), file)) {
        sscanf(line, "%d|%49[^|]|%49[^|]|%f|%99[^|]|%14[^|]|%10[^|]|%4[^|]|%49[^|]|%9[^\n]",
            &emp.empID, emp.name, emp.department, &emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);

        printf("\nID: %d\nName: %s\nDepartment: %s\nSalary: %.2f\nAddress: %s\nContact: %s\nDOB: %s\nBlood Group: %s\nEmail: %s\nGender: %s\n",
            emp.empID, emp.name, emp.department, emp.salary, emp.address,
            emp.contactNumber, emp.dateOfBirth, emp.bloodGroup, emp.email, emp.gender);
    }

    fclose(file);
}

int main() {
    int choice, empID;

    while (1) {
        printf("\n=== Employee Management System ===\n");
        printf("1. Add Employee\n");
        printf("2. View Employee\n");
        printf("3. Update Employee\n");
        printf("4. Delete Employee\n");
        printf("5. Display All Employees\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addEmployee();
                break;
            case 2:
                printf("Enter Employee ID to view: ");
                scanf("%d", &empID);
                viewEmployee(empID);
                break;
            case 3:
                printf("Enter Employee ID to update: ");
                scanf("%d", &empID);
                updateEmployee(empID);
                break;
            case 4:
                printf("Enter Employee ID to delete: ");
                scanf("%d", &empID);
                deleteEmployee(empID);
                break;
            case 5:
                displayAllEmployees();
                break;
            case 6:
                printf("Exiting program...\n");
                exit(0);
            default:
                printf("Invalid choice. Try again.\n");
        }
    }

    return 0;
}
