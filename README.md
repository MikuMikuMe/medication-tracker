# medication-tracker

Creating a comprehensive medication tracker in Python involves a few key features like adding medications, scheduling doses, and providing notifications for due doses. For simplicity, this program will be a console-based application using Python. We'll use `datetime` for scheduling and create a simple loop for time-based notifications. Persistent storage could be added later using a database or file storage system.

Below is a sample Python program implementing this functionality:

```python
import datetime
import time

class Medication:
    def __init__(self, name, dosage, frequency):
        self.name = name
        self.dosage = dosage
        self.frequency = frequency  # in hours
        self.last_taken = None

    def take_medication(self):
        self.last_taken = datetime.datetime.now()
        print(f"{self.name} taken at {self.last_taken.strftime('%Y-%m-%d %H:%M:%S')}.")

    def next_due(self):
        if self.last_taken:
            return self.last_taken + datetime.timedelta(hours=self.frequency)
        return None

class MedicationTracker:
    def __init__(self):
        self.medications = []

    def add_medication(self, name, dosage, frequency):
        try:
            medication = Medication(name, dosage, frequency)
            self.medications.append(medication)
            print(f"Medication {name} added successfully.")
        except Exception as e:
            print(f"Error adding medication: {e}")

    def check_medications(self):
        current_time = datetime.datetime.now()
        for med in self.medications:
            next_due_time = med.next_due()
            if next_due_time and current_time >= next_due_time:
                print(f"Time to take your {med.name} (Dosage: {med.dosage}). Next due was at: {next_due_time.strftime('%Y-%m-%d %H:%M:%S')}")
            elif not next_due_time:
                print(f"Medication {med.name} has not been taken yet.")

    def take_medication(self, name):
        for med in self.medications:
            if med.name == name:
                med.take_medication()
                return
        print(f"Medication {name} not found.")

def main():
    tracker = MedicationTracker()

    while True:
        print("\nMedication Tracker Menu:")
        print("1. Add medication")
        print("2. Take medication")
        print("3. Check medication schedule")
        print("4. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            name = input("Enter medication name: ")
            dosage = input("Enter dosage: ")
            frequency = input("Enter frequency in hours: ")

            try:
                frequency = int(frequency)
                tracker.add_medication(name, dosage, frequency)
            except ValueError:
                print("Invalid input for frequency. Please enter an integer.")

        elif choice == '2':
            name = input("Enter the name of the medication you took: ")
            tracker.take_medication(name)

        elif choice == '3':
            tracker.check_medications()

        elif choice == '4':
            print("Exiting the Medication Tracker. Stay healthy!")
            break

        else:
            print("Invalid choice. Please try again.")

        # Simulating checking the medication schedule, every minute in this example
        time.sleep(60)

if __name__ == "__main__":
    main()
```

### Key Features:

1. **Adding Medications:** You can add medications with the name, dosage, and frequency (in hours) with proper error handling for invalid input.

2. **Taking Medications:** You can record when you take the medication, updating the last taken time.

3. **Checking Schedules:** The application checks and notifies you if it's time to take any medication.

4. **User Interaction:** Simple menu interface to navigate options.

This program provides a basic structure to build on further, adding features like persistent storage, GUI elements, or advanced scheduling notifications.