# student-report-card-generator

import csv

INPUT_CSV = 'student_marks.csv'
OUTPUT_CSV = 'final_report_card.csv'

def calculate_feedback(percentage):
    if percentage > 90:
        return "Excellent"
    elif 75 <= percentage <= 90:
        return "Good"
    elif 50 <= percentage < 75:
        return "Average"
    else:
        return "Needs Improvement"

def read_student_marks(filename):
    students = []
    with open(filename, newline='') as csvfile:
        reader = csv.DictReader(csvfile)
        for row in reader:
            students.append(row)
    return students

def print_report_card(students):
    print("----- Report Card -----")
    for student in students:
        name = student['Name']
        marks = [int(student[subj]) for subj in ['Math', 'Science', 'English', 'History', 'Computer']]
        total = sum(marks)
        percentage = (total / 500) * 100
        feedback = calculate_feedback(percentage)
        print(f"Name: {name}")
        print(f"Total: {total} / 500")
        print(f"Percentage: {percentage:.1f}%")
        print(f"Feedback: {feedback}\n")

def write_final_report_card(students, filename):
    fieldnames = ['Name', 'Math', 'Science', 'English', 'History', 'Computer', 'Total', 'Percentage', 'Feedback']
    with open(filename, 'w', newline='') as csvfile:
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()
        for student in students:
            marks = [int(student[subj]) for subj in ['Math', 'Science', 'English', 'History', 'Computer']]
            total = sum(marks)
            percentage = (total / 500) * 100
            feedback = calculate_feedback(percentage)
            row = {**student,
                   'Total': total,
                   'Percentage': f"{percentage:.1f}",
                   'Feedback': feedback}
            writer.writerow(row)

def main():
    students = read_student_marks(INPUT_CSV)
    print_report_card(students)
    write_final_report_card(students, OUTPUT_CSV)

if __name__ == "__main__":
    main()
