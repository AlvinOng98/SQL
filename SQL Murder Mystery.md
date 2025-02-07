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

## 1. Crime Scene Report 📝

Our first step would be to start at the crime scene report table. We'll use a simple SELECT * statement to view the contents of the table:

```` sql
SELECT *
FROM crime_scene_report
LIMIT 5;
````
![image](https://github.com/user-attachments/assets/781c9829-98a1-4bb2-a820-ccba5661ae90)

Now we can see the fields where we can plug in our clues to help us narrow down our search.
  - The crime is a murder -> type field
  - The crime occured on 15 January 2018 -> date field
  - The crime took place in SQL City -> city field

## 2. Filter Crime Scene Reports using our clues 🕵️‍♂️

```` sql
SELECT *
FROM crime_scene_report
WHERE type = 'murder'
  AND date = '20180115'
  AND city = 'SQL City';
````

![image](https://github.com/user-attachments/assets/9a3cafe5-fc1c-4d32-853b-985b9ca31fa3)

With the clues given, we have found our singular crime scene report. The report describes:

_Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave"._

We've uncovered a critical lead—**two witnesses** were present at the crime scene. Their testimonies could either crack the case wide open or send us chasing shadows. It’s time to track them down, sift through their statements, and see if the truth is hiding in plain sight.

## 3. Following the lead 🕵️‍♂️

  - Witness #1: The first witness **lives at the last house on "Northwestern Dr**.
  - Witness #2: The second witness, **Annabel, lives somewhere on "Franklin Ave**.

The crime scene report has led us to two key witnesses—potential gold mines of information. They might hold the missing pieces to this puzzle, but we’ll need to dig into the database to find out exactly what they know.
The next step? Tracking them down, interrogating their statements, and seeing if their stories hold up. Let’s fire up some SQL queries and see where the data leads!

We can find our first witness who lives at the last house on Northwestern Drive in the person table using WHERE and ORDER BY statements:

```` sql
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 5;
````

The query searches for witnesses living on Northwestern Drive and orders the results by address number in descending order, as he lives on the last house, helping us find the our first witness.

![image](https://github.com/user-attachments/assets/61ef9a4b-3fe7-4313-a135-049860644ca4)

   - Witness #1:** Morty Schapiro** with **id** 14887, **license_id** 118009, and **ssn** 111564949.
   - Witness #2: The second witness, **Annabel, lives somewhere on "Franklin Ave**.

## 4. Following the lead pt. 2 🕵️‍♂️

Now we will look for a witness named "Annabel" residing on "Franklin Ave" street. For this query, we will be using the **LIKE** statement to look for any witness that has "Annabel" in the name:

```` sql
SELECT *
FROM person
WHERE name LIKE '%Annabel%'
  AND address_street_name = 'Franklin Ave';
````

![image](https://github.com/user-attachments/assets/ca963004-639d-41d9-bbc0-e2999c4226aa)

  - Witness #1:**Morty Schapiro** with **id** 14887, **license_id** 118009, and **ssn** 111564949.
  - Witness #2:**Annabel Miller** with **id** 16371, **license_id** 490173, and **ssn** 318771143.

## 5. Interrogating the Witnesses ✍

With our witnesses identified, it’s time to review their statements. What did they see? Who was there? Are their stories airtight, or do we have contradictions to unravel?
By examining their testimonies in the database, we’ll extract key details that could expose our suspect—or lead us deeper into the mystery. Every word matters, and the truth is waiting to be uncovered.
Let’s dive into the witness interviews and see where the evidence takes us! 



