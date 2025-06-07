# Task 1
#include <stdio.h>
#include <string.h>

#define MAX_STUDENTS 100
#define MAX_SUBJECTS 10
#define MAX_NAME_LEN 50

typedef struct {
    char name[MAX_NAME_LEN];
    float scores[MAX_SUBJECTS];
    float average;
} Student;

void inputStudentData(Student students[], int *studentCount, int *subjectCount);
void calculateAverages(Student students[], int studentCount, int subjectCount);
void findHighestLowest(Student students[], int studentCount, Student *highest, Student *lowest);
void displaySummary(Student students[], int studentCount, int subjectCount, Student highest, Student lowest);

int main() {
    Student students[MAX_STUDENTS];
    Student highest, lowest;
    int studentCount, subjectCount;

    inputStudentData(students, &studentCount, &subjectCount);
    calculateAverages(students, studentCount, subjectCount);
    findHighestLowest(students, studentCount, &highest, &lowest);
    displaySummary(students, studentCount, subjectCount, highest, lowest);

    return 0;
}

void inputStudentData(Student students[], int *studentCount, int *subjectCount) {
    printf("Enter number of students: ");
    scanf("%d", studentCount);
    printf("Enter number of subjects per student: ");
    scanf("%d", subjectCount);

    for (int i = 0; i < *studentCount; i++) {
        printf("\nEnter name of student #%d: ", i + 1);
        scanf(" %[^\n]", students[i].name);

        for (int j = 0; j < *subjectCount; j++) {
            printf("  Enter score for subject #%d: ", j + 1);
            scanf("%f", &students[i].scores[j]);
        }
    }
}

void calculateAverages(Student students[], int studentCount, int subjectCount) {
    for (int i = 0; i < studentCount; i++) {
        float sum = 0;
        for (int j = 0; j < subjectCount; j++) {
            sum += students[i].scores[j];
        }
        students[i].average = sum / subjectCount;
    }
}

void findHighestLowest(Student students[], int studentCount, Student *highest, Student *lowest) {
    *highest = students[0];
    *lowest = students[0];

    for (int i = 1; i < studentCount; i++) {
        if (students[i].average > highest->average) {
            *highest = students[i];
        }
        if (students[i].average < lowest->average) {
            *lowest = students[i];
        }
    }
}

void displaySummary(Student students[], int studentCount, int subjectCount, Student highest, Student lowest) {
    printf("\n--- Student Summary ---\n");
    for (int i = 0; i < studentCount; i++) {
        printf("\nName: %s\n", students[i].name);
        for (int j = 0; j < subjectCount; j++) {
            printf("  Subject #%d: %.2f\n", j + 1, students[i].scores[j]);
        }
        printf("  Average Score: %.2f\n", students[i].average);
    }

    printf("\n--- Performance Summary ---\n");
    printf("Highest Performer: %s (Average: %.2f)\n", highest.name, highest.average);
    printf("Lowest Performer : %s (Average: %.2f)\n", lowest.name, lowest.average);
}
