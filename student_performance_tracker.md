# Student Performance Tracker

## Overview

This project is a **Student Performance Tracker** that allows users to input student names and their scores for different subjects. It calculates each student's average score, determines if they have passed, and provides an overall class average. This is useful for quickly tracking student performance in a structured and easily readable format.

## Project Structure

- **Classes**:
  - `Student`: Represents individual students, with methods to calculate their average score and determine if they are passing.
  - `PerformanceTracker`: Manages multiple `Student` instances, calculates class average, and displays a summary of all students' performance.
- **Main Program Flow**:
  - Prompts the user to enter student names and their scores in three subjects.
  - Allows input validation for names and scores.
  - Calculates individual student averages, determines their passing status, and calculates the class average.
  - Displays a performance report for each student and the class.

## Code Breakdown

### 1. `Student` Class

The `Student` class is designed to store individual student data and provide methods for calculating their average score and checking their pass/fail status.

#### Constructor (`__init__`)

- **Parameters**:
  - `name`: A `str` representing the student's name.
  - `scores`: A `list[int]` containing integer scores for each subject.
- **Attributes**:
  - `self.name`: Stores the name of the student.
  - `self.scores`: Stores a list of scores for each subject.

```python
class Student:
  def __init__(self, name: str, scores: list[int]):
    self.name: str = name
    self.scores: list[int] = scores
```

#### Method: `calculate_average()`

- Calculates and returns the average score for the student.
- Uses `sum()` for total the scores and `len()` for divides by the number of scores.

```python
def calculate_average(self):
    average_score = sum(self.scores) / len(self.scores)
    return average_score
```

#### Method: `is_passing(passing_score=40)`

- Determines if the student is passing all subjects.
- Default `passing_score` is set to 40. It returns `True` if all scores are equal or above the threshold, otherwise `False`.

```python
def is_passing(self, passing_score: int = 40):
    status = all(score >= passing_score for score in self.scores)
    return status
```

### 2. `PerformanceTracker` Class

The `PerformanceTracker` class manages a collection of Student instances, calculates the class average, and displays a performance report.

#### Constructor (`__init__`)

- **Attributes**:
  - `self.students`: A `dict` to store student names as keys and `Student` objects as values.

```python
class PerformanceTracker:
  def __init__(self):
    self.students: dict = {}
```

#### Method: `add_student(name, scores)`

- Adds a `Student` instance to `self.students` if scores are provided; otherwise, it alerts the user that no scores were entered.

```python
def add_student(self, name, scores):
    if scores:
      self.students[name] = Student(name, scores)
    else:
      print(f"No scores entered for {name}. Student not added.")
```

#### Method: `calculate_class_average()`

- Calculates the class average score by averaging all students' average scores.
- If no students are present, it alerts the user and returns `None`.

```python
def calculate_class_average(self):
    if not self.students:
      print("No students have been added. Cannot calculate class average.")
      return None

    total_scores = sum(student.calculate_average() for student in self.students.values())
    class_average = total_scores / len(self.students)
    return class_average
```

#### Method: `display_student_performance()`

- Displays each student’s name, average score, and pass/fail status.
- Also displays the overall class average at the end.

```python
def display_student_performance(self):

    if not self.students:
      print("No students to display. Please add students first.")
      return

    print("\n...............Student Performance Report...............\n")
    for name, student in self.students.items():
      average_score = student.calculate_average()
      status = "Pass" if student.is_passing() else "Needs Improvement"
      print(f"Name: {name} \nAverage Score: {average_score:.2f} \nStatus: {status}\n")

    class_average = self.calculate_class_average()
    if class_average is not None:
      print(f"\nClass Average Score: {class_average:.2f}")
```

### 3. Main Program Flow

The main loop handles user input and validation, managing the addition of students and their scores. Here is a step-by-step breakdown:

#### 1. Prompt for Student Data:

- Collect each student’s name and scores for three subjects.
- Validate that the name contains only alphabetic characters.
- Ensure scores are entered as numbers.

#### 2. Add Student Data:

- Add each valid student to `PerformanceTracker`.

#### 3. Generate Report:

- Display individual and class performance.

```python
tracker = PerformanceTracker()

while True:

    while True:
       try:

            name = input("Enter student's name (or type 'done' to finish): ")
            print(f"\n\nEnter student's name (or type 'done' to finish): {name}")
            if name.lower() == 'done':
               break

            if not name.isalpha():
               raise ValueError("Invalid name. Please enter a name with alphabetic characters only.")

            break

       except ValueError as e:
            print(e)

    if name.lower() == 'done':
        break

    scores: list[int] = []
    for subject in ["Maths", "Science", "English"]:
       while True:
          try:
              score: int = int(input(f"Enter {name}'s score in {subject}: "))
              print(f"Enter {name}'s score in {subject}: {score}")
              scores.append(score)
              break

          except ValueError:
              print(f"Invalid input. Please enter only numeric values for scores.")

    tracker.add_student(name, scores)

tracker.display_student_performance()
```

### Usage Instructions

#### 1. Run the Program:

- Start the program by executing the script in a Python environment.

#### 2. Enter Student Information:

- Type each student's name.
- Enter scores for three subjects: Maths, Science, and English.
- Type `done` when you finish entering students.

#### 3. View the Performance Report:

- The program will print each student’s performance report, including their average score and pass/fail status.
- The class average score will also be displayed at the end.

### Example Output

```python
_______________Welcome to the Student Performance Tracker_______________

Enter student's name (or type 'done' to finish): Memoona
Enter Memoona's score in Maths: 99
Enter Memoona's score in Science: 97
Enter Memoona's score in English: 90


Enter student's name (or type 'done' to finish): Abeera
Enter Abeera's score in Maths: 89
Enter Abeera's score in Science: 87
Enter Abeera's score in English: 76


Enter student's name (or type 'done' to finish): Rameen
Enter Rameen's score in Maths: 56
Enter Rameen's score in Science: 88
Enter Rameen's score in English: 34


Enter student's name (or type 'done' to finish): done

...............Student Performance Report...............

Name: Memoona
Average Score: 95.33
Status: Pass

Name: Abeera
Average Score: 84.00
Status: Pass

Name: Rameen
Average Score: 59.33
Status: Needs Improvement


Class Average Score: 79.56

```
