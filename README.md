#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Employee struct
struct Employee {
    char id[20];
    char name[50];
    char gender[20];
    char branch[30];
    char phone[20];
};

// Function prototypes
void addEmployee();
void showEmployee();
void searchEmployee();
void deleteEmployee();
void modifyEmployee();
void basicInfo();
void contactInfo();
void maleList();
void femaleList();
void dhakaList();
void otherDistrictList();
void mainBranchList();
void otherBranchList();

// Remove newline character if it exists
void removeNewline(char *str) {
    size_t len = strlen(str);
    if (str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}

// Display employee details
void displayEmployee(struct Employee e) {
    printf("ID:%s | Name:%s | Gender:%s | Branch:%s | Phone:%s\n", e.id, e.name, e.gender, e.branch, e.phone);
}

// Add new employee
void addEmployee() {
    FILE *fp = fopen("employees.txt", "a");
    if (!fp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    int n, i;
    printf("How many employees to add? ");
    scanf("%d", &n);
    getchar(); // To consume the newline character left by scanf

    for (i = 0; i < n; i++) {
        printf("Enter information for Employee %d:\n", i + 1);

        printf("ID: ");
        gets(e.id);  // Using gets instead of fgets for id

        printf("Name: ");
        gets(e.name);  // Using gets instead of fgets for name

        printf("Gender: ");
        gets(e.gender);  // Using gets instead of fgets for gender

        printf("Branch: ");
        gets(e.branch);  // Using gets instead of fgets for branch

        printf("Phone: ");
        gets(e.phone);  // Using gets instead of fgets for phone

        // Write to file using %d
        fprintf(fp, "%s\n%s\n%s\n%s\n%s\n", e.id, e.name, e.gender, e.branch, e.phone);
    }

    fclose(fp);
    printf("%d employees added successfully!\n", n);
}

// Show all employees
void showEmployee() {
    FILE *fp = fopen("employees.txt", "r");
    if (!fp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    int count = 0;
    while (
        fgets(e.id, sizeof(e.id), fp) &&
        fgets(e.name, sizeof(e.name), fp) &&
        fgets(e.gender, sizeof(e.gender), fp) &&
        fgets(e.branch, sizeof(e.branch), fp) &&
        fgets(e.phone, sizeof(e.phone), fp)
    ) {
        removeNewline(e.id);
        removeNewline(e.name);
        removeNewline(e.gender);
        removeNewline(e.branch);
        removeNewline(e.phone);
        displayEmployee(e);
        count++;
    }

    if (!count) {
        printf("No employees found.\n");
    }

    fclose(fp);
}

// Search by ID
void searchEmployee() {
    FILE *fp = fopen("employees.txt", "r");
    if (!fp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    char id[20];
    int found = 0;
    printf("Search ID: ");
    fgets(id, sizeof(id), stdin);
    removeNewline(id);

    while (
        fgets(e.id, sizeof(e.id), fp) &&
        fgets(e.name, sizeof(e.name), fp) &&
        fgets(e.gender, sizeof(e.gender), fp) &&
        fgets(e.branch, sizeof(e.branch), fp) &&
        fgets(e.phone, sizeof(e.phone), fp)
    ) {
        removeNewline(e.id);
        removeNewline(e.name);
        removeNewline(e.gender);
        removeNewline(e.branch);
        removeNewline(e.phone);

        if (strcmp(e.id, id) == 0) {
            displayEmployee(e);
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("Employee not found.\n");
    }

    fclose(fp);
}

// Delete by ID
void deleteEmployee() {
    FILE *fp = fopen("employees.txt", "r");
    FILE *tp = fopen("temp.txt", "w");
    if (!fp || !tp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    char id[20];
    int found = 0;
    printf("Delete ID: ");
    fgets(id, sizeof(id), stdin);
    removeNewline(id);

    while (
        fgets(e.id, sizeof(e.id), fp) &&
        fgets(e.name, sizeof(e.name), fp) &&
        fgets(e.gender, sizeof(e.gender), fp) &&
        fgets(e.branch, sizeof(e.branch), fp) &&
        fgets(e.phone, sizeof(e.phone), fp)
    ) {
        removeNewline(e.id);
        removeNewline(e.name);
        removeNewline(e.gender);
        removeNewline(e.branch);
        removeNewline(e.phone);

        if (strcmp(e.id, id) != 0) {
            fprintf(tp, "%s\n%s\n%s\n%s\n%s\n", e.id, e.name, e.gender, e.branch, e.phone);
        } else {
            found = 1;
        }
    }

    fclose(fp);
    fclose(tp);

    remove("employees.txt");
    rename("temp.txt", "employees.txt");

    if (found) {
        printf("Employee deleted successfully.\n");
    } else {
        printf("Employee not found.\n");
    }
}

// Modify by ID
void modifyEmployee() {
    FILE *fp = fopen("employees.txt", "r");
    FILE *tp = fopen("temp.txt", "w");
    if (!fp || !tp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    char id[20];
    int found = 0;
    printf("Modify ID: ");
    fgets(id, sizeof(id), stdin);
    removeNewline(id);

    while (
        fgets(e.id, sizeof(e.id), fp) &&
        fgets(e.name, sizeof(e.name), fp) &&
        fgets(e.gender, sizeof(e.gender), fp) &&
        fgets(e.branch, sizeof(e.branch), fp) &&
        fgets(e.phone, sizeof(e.phone), fp)
    ) {
        removeNewline(e.id);
        removeNewline(e.name);
        removeNewline(e.gender);
        removeNewline(e.branch);
        removeNewline(e.phone);

        if (strcmp(e.id, id) == 0) {
            printf("New Name: "); fgets(e.name, sizeof(e.name), stdin); removeNewline(e.name);
            printf("New Gender: "); fgets(e.gender, sizeof(e.gender), stdin); removeNewline(e.gender);
            printf("New Branch: "); fgets(e.branch, sizeof(e.branch), stdin); removeNewline(e.branch);
            printf("New Phone: "); fgets(e.phone, sizeof(e.phone), stdin); removeNewline(e.phone);
            found = 1;
        }

        fprintf(tp, "%s\n%s\n%s\n%s\n%s\n", e.id, e.name, e.gender, e.branch, e.phone);
    }

    fclose(fp);
    fclose(tp);

    remove("employees.txt");
    rename("temp.txt", "employees.txt");

    if (found) {
        printf("Employee modified successfully.\n");
    } else {
        printf("Employee not found.\n");
    }
}

// 9. Female Employee
void femaleList() {
    FILE *fp = fopen("employees.txt", "r");
    if (!fp) {
        printf("Error opening file!\n");
        return;
    }

    struct Employee e;
    int found = 0;
    while (
        fgets(e.id, sizeof(e.id), fp) &&
        fgets(e.name, sizeof(e.name), fp) &&
        fgets(e.gender, sizeof(e.gender), fp) &&
        fgets(e.branch, sizeof(e.branch), fp) &&
        fgets(e.phone, sizeof(e.phone), fp)
    ) {
        removeNewline(e.gender);  // Remove any newline characters from gender
        if (strcmp(e.gender, "Female") == 0) {  // Check if gender is "Female"
            removeNewline(e.id);
            removeNewline(e.name);
            removeNewline(e.branch);
            removeNewline(e.phone);
            displayEmployee(e);  // Display female employee info
            found = 1;
        }
    }

    if (!found) {
        printf("No female employees found.\n");
    }

    fclose(fp);
}

// Main function
int main() {
    int c;
    while (1) {
        printf("\nPlease select an option from the menu below:\n");
        printf("1. Add Employee\n");
        printf("2. Delete Employee\n");
        printf("3. Modify Employee\n");
        printf("4. Show All Employees\n");
        printf("5. Search Employee by ID\n");
        printf("6. Basic Information\n");
        printf("7. Contact Information\n");
        printf("8. List Male Employees\n");
        printf("9. List Female Employees\n");
        printf("10. List Employees from Dhaka\n");
        printf("11. List Employees from Other Districts\n");
        printf("12. List Employees from Main Branch\n");
        printf("13. List Employees from Other Branches\n");
        printf("0. Exit\n");
        printf("\nChoice: ");
        scanf("%d", &c);
        getchar(); // Consume newline character from previous input

        switch (c) {
            case 1: addEmployee(); break;
            case 2: deleteEmployee(); break;
            case 3: modifyEmployee(); break;
            case 4: showEmployee(); break;
            case 5: searchEmployee(); break;
            case 6: basicInfo(); break;
            case 7: contactInfo(); break;
            case 8: maleList(); break;
            case 9: femaleList(); break;
            case 10: dhakaList(); break;
            case 11: otherDistrictList(); break;
            case 12: mainBranchList(); break;
            case 13: otherBranchList(); break;
            case 0: exit(0);
            default: printf("Invalid choice. Please try again.\n");
        }
    }
    return 0;
}
