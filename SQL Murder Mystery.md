# SQL Murder Mystery: Cracking the Case with Data

<img alt="image" src="https://github.com/user-attachments/assets/2255d75a-aadd-4b26-b1c8-24ec75b03587">

A crime has been committed, and the only way to solve it is by digging into the database! In this **SQL Murder Mystery**, I take on the role of a data detective, using logical thinking and precise SQL queries to track down the suspect.

By carefully examining tables, filtering through crime reports, and uncovering key details, I piece together the evidence to solve the case. This project showcases my ability to **write efficient SQL queries, analyze structured data, and think critically**—all while having some fun cracking the mystery!

The database holds the truth—let’s uncover it! 🔍💻

## Clues at the Crime Scene Report

Let's start with the excerpt provided to us:

A crime has taken place and the detective needs your help. The detective gave you the crime scene report, but you somehow lost it. You vaguely remember that **the crime was a ​murder**​ that occurred sometime on **​Jan.15, 2018​** and that it took place in ​**SQL City**​. Start by retrieving the corresponding crime scene report from the police department’s database. 

From this we get very important clues 🧩 that will be crucial to help us find our suspect:
  - The crime is a murder.
  - The crime occured on 15 January 2018.
  - The crime took place in SQL City.

## Exploring the Database Structure

<img width="1008" alt="Screenshot 2024-03-25 at 12 12 38 PM" src="https://github.com/user-attachments/assets/27256018-3e85-4e61-adb3-fb07051a0ec8" />

Before cracking the case, every great detective studies the crime scene—and in this investigation, the **Entity-Relationship Diagram (ERD)** is my map to the truth. This blueprint reveals the key tables, their connections, and the crucial evidence hidden within the database.

By analyzing the structure, I can pinpoint the most relevant records, trace relationships between suspects and events, and uncover where the real clues might be buried. A well-planned investigation is the key to solving any mystery, and this step ensures that no detail slips through the cracks.

## Crime Scene Report

Our first step would be to start at the crime scene report table. We'll use a simple SELECT * statement to view the contents of the table:

```` sql
SELECT *
FROM crime_scene_report
LIMIT 5;
````
![image](https://github.com/user-attachments/assets/781c9829-98a1-4bb2-a820-ccba5661ae90)









