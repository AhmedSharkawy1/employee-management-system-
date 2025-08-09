# employee-management-system-
This Python code implements an employee management system that interacts with a CSV file. The system allows adding, viewing, updating, deleting, and searching employees.
import csv

class EmployeeManager:
    def __init__(self, file_name='employees.csv'):
        self.file_name = file_name
        self.employees = {}
        self.load_employees()

    def load_employees(self):
        try:
            with open(self.file_name, mode='r', newline='') as file:
                reader = csv.DictReader(file)
                for row in reader:
                    self.employees[row['ID']] = {
                        'Name': row['Name'],
                        'Position': row['Position'],
                        'Salary': row['Salary'],
                        'Email': row['Email']
                    }
            print(f"Loaded {len(self.employees)} employees from {self.file_name}")
        except FileNotFoundError:
            # Create new file if doesn't exist
            with open(self.file_name, mode='w', newline='') as file:
                writer = csv.writer(file)
                writer.writerow(['ID', 'Name', 'Position', 'Salary', 'Email'])
            print(f"Created new data file: {self.file_name}")

    def save_employees(self):
        """Save employee data from memory to CSV file"""
        with open(self.file_name, mode='w', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(['ID', 'Name', 'Position', 'Salary', 'Email'])
            for employee_id, data in self.employees.items():
                writer.writerow([
                    employee_id,
                    data['Name'],
                    data['Position'],
                    data['Salary'],
                    data['Email']
                ])
        print(f"Data saved to {self.file_name}")

    def add_employee(self):
        """Add a new employee with validation"""
        print("\n-* Add New Employee *-")

        while True:
            employee_id = input("Enter Employee ID: ").strip()
            if not employee_id:
                print("Error: ID can't be empty!")
                continue
            if employee_id in self.employees:
                print("Error: ID already exists!")
                continue
            break

        name = input("Enter Name: ").strip()
        while not name:
            print("Error: Name cannot be empty!")
            name = input("Enter Name: ").strip()

        position = input("Enter Position: ").strip()
        while not position:
            print("Error: Position cannot be empty!")
            position = input("Enter Position: ").strip()

        while True:
            salary = input("Enter Salary: ").strip()
            if not salary.replace('.', '').isdigit():
                print("Error: Salary must be a number!")
                continue
            break

        email = input("Enter Email: ").strip()
        while not email:
            print("Error: Email cannot be empty!")
            email = input("Enter Email: ").strip()

        self.employees[employee_id] = {
            'Name': name,
            'Position': position,
            'Salary': salary,
            'Email': email
        }
        self.save_employees()
        print(f"Success: Employee {employee_id} added!")

    def view_all_employees(self):
        print("\n--- All Employees ---")
        if not self.employees:
            print("No employees found.")
            return

        for employee_id, data in self.employees.items():
            print(f"\nID: {employee_id}")
            print(f"Name: {data['Name']}")
            print(f"Position: {data['Position']}")
            print(f"Salary: {float(data['Salary'] )} EGP")
            print(f"Email: {data['Email']}")
        print(f"\nTotal: {len(self.employees)} employees")

    def update_employee(self):
        print("\n--- Update Employee ---")
        employee_id = input("Enter Employee ID to update: ").strip()

        if employee_id not in self.employees:
            print("Error: Employee not found!")
            return

        employee = self.employees[employee_id]
        print("\nCurrent Details:")
        print(f"1. Name: {employee['Name']}")
        print(f"2. Position: {employee['Position']}")
        print(f"3. Salary: {employee['Salary']}")
        print(f"4. Email: {employee['Email']}")

        print("\nEnter new values (leave blank to keep current):")

        # Get updated fields
        name = input(f"Name [{employee['Name']}]: ").strip()
        position = input(f"Position [{employee['Position']}]: ").strip()

        while True:
            salary = input(f"Salary [{employee['Salary']}]: ").strip()
            if salary and not salary.replace('.', '').isdigit():
                print("Error: Salary must be a number!")
                continue
            break

        email = input(f"Email [{employee['Email']}]: ").strip()

        # Apply updates
        if name: employee['Name'] = name
        if position: employee['Position'] = position
        if salary: employee['Salary'] = salary
        if email: employee['Email'] = email

        self.save_employees()
        print(f"Success: Employee {employee_id} updated!")

    def delete_employee(self):
        print("\n--- Delete Employee ---")
        employee_id = input("Enter Employee ID to delete: ").strip()

        if employee_id not in self.employees:
            print("Error: Employee not found!")
            return

        confirm = input(f"Confirm delete {employee_id}? (y/n): ").lower()
        if confirm == 'y':
            del self.employees[employee_id]
            self.save_employees()
            print(f"Success: Employee {employee_id} deleted!")
        else:
            print("Deletion cancelled.")

    def search_employee(self):
        print("\n--- Search Employee ---")
        employee_id = input("Enter Employee ID to search: ").strip()

        if employee_id not in self.employees:
            print("Error: Employee not found!")
            return

        employee = self.employees[employee_id]
        print("\nEmployee Details:")
        print(f"ID: {employee_id}")
        print(f"Name: {employee['Name']}")
        print(f"Position: {employee['Position']}")
        print(f"Salary: {float(employee['Salary'])} EGP")
        print(f"Email: {employee['Email']}")

def display_menu():
    print("\n" + "=" * 50)
    print("EMPLOYEE DATA MANAGEMENT SYSTEM".center(50))
    print("=" * 50)
    print("1. Add Employee")
    print("2. View All Employees")
    print("3. Update Employee")
    print("4. Delete Employee")
    print("5. Search Employee")
    print("6. Exit")
    print("=" * 50)


def main():
    print("Initializing Employee Management System...")
    manager = EmployeeManager()

    while True:
        display_menu()
        choice = input("\nEnter your choice (1-6): ").strip()

        if choice == '1':
            manager.add_employee()
        elif choice == '2':
            manager.view_all_employees()
        elif choice == '3':
            manager.update_employee()
        elif choice == '4':
            manager.delete_employee()
        elif choice == '5':
            manager.search_employee()
        elif choice == '6':
            print("\nThank you for using my system created by Eng/ Ahmed said . Goodbye (Full marke 😁)")
            break
        else:
            print("\nInvalid input\t Please enter 1-6")

        input("\nPress Enter to continue..")

if __name__ == "__main__":
    main()
