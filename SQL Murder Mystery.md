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
WHERE LOWER(type) = 'murder'
  AND date = '20180115'
  AND LOWER(city) = 'sql city';
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

   - Witness #1: **Morty Schapiro** with **id** 14887, **license_id** 118009, and **ssn** 111564949.
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

  - Witness #1: **Morty Schapiro** with **id** 14887, **license_id** 118009, and **ssn** 111564949.
  - Witness #2: **Annabel Miller** with **id** 16371, **license_id** 490173, and **ssn** 318771143.

## 5. Interrogating the Witnesses ✍

With our witnesses identified, it’s time to review their statements. What did they see? Who was there? Are their stories airtight, or do we have contradictions to unravel?
By examining their testimonies in the database, we’ll extract key details that could expose our suspect—or lead us deeper into the mystery. 

We will use the SELECT * statement with IN operator to search our interview table and retrieve the interviews from our witnesses using their person_id:

```` sql
SELECT *
FROM interview
WHERE person_id IN ('14887', '16371');
````

An alternative more advanced solution would be to use JOIN to get the transcripts instead:

```` sql
SELECT person.name, interview.transcript
FROM person
JOIN interview
ON person.id = interview.person_id
WHERE person.id = 14887 OR person.id = 16371;
````

![image](https://github.com/user-attachments/assets/3cce2078-bf0d-4696-ac84-9c9c3d3dd018)

Here are the transcripts from our witnesses:

  - I heard a gunshot and then saw a man run out. He had a **"Get Fit Now Gym" bag**. The membership number on the bag started with **"48Z"**. Only **gold members** have those bags. The man got into a car with a plate that included **"H42W"**.
  - I saw the murder happen, and I recognized the killer from my gym when I was working out last week on **January the 9th**.

From this we get new clues about the crime:

  - A man
  - Gold member of "Get Fit Now Gym" with a membership no. starting with "48Z"
  - Spotted at the gym on 9 January
  - Car plate number containing "H42W"

## 6. Finding the murderer 🕵️‍♂️

With our current clues we could slowly follow the trail of details to find our murder suspect by checking each indentifier, but we could instead pinpoint our suspect with a single SQL query that combines all the identifiers and give us a suspect that meets all of them. For complex questions that simultaneously require data from multiple different tables, we will be using JOIN statements, it helps us to consider data by joining different tables into a single table.
 
```` sql
SELECT 	person.name, get_fit_now_member.membership_status, 
		    get_fit_now_check_in.check_in_date, get_fit_now_check_in.membership_id,
		    drivers_license.plate_number
FROM person 
JOIN get_fit_now_member
ON get_fit_now_member.person_id = person.id
JOIN get_fit_now_check_in
ON get_fit_now_check_in.membership_id = get_fit_now_member.id
JOIN drivers_license
ON drivers_license.id = person.license_id
WHERE get_fit_now_check_in.membership_id LIKE '48Z%'
	AND get_fit_now_check_in.check_in_date = '20180109'
	AND get_fit_now_member.membership_status = 'gold'
	AND drivers_license.plate_number LIKE '%H42W%';
````

![image](https://github.com/user-attachments/assets/3bcfa37e-1f51-4344-98b6-af9092a65c33)

We have found the murderer... it's **Jeremy Bowers**!

## 7. Submitting our report 📌

Now let us hand over our report to the detective by updating the database with our main suspect, Jeremy Bowers.

```` sql
INSERT INTO solution VALUES(1, 'Jeremy Bowers');

SELECT value
FROM solution;
````

![image](https://github.com/user-attachments/assets/84e7647b-8b53-40b6-9b78-127ecf4ef236)

Congrats, you found the murderer! But wait, there's more... **If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries.** Use this same INSERT statement with your new suspect to check your answer.

Hold on, there's more...

## 8. Who is the mastermind? 🧠

Let us first analyze what our murderer has to say:

```` sql
SELECT interview.transcript
FROM interview JOIN person
ON interview.person_id = person.id
WHERE person.name = 'Jeremy Bowers';
````

![image](https://github.com/user-attachments/assets/b1388889-3879-4643-b060-de86ecff98fd)

Murderer's interview:

_I was hired by a woman with a lot of money. I don't know her name but I know she's around **5'5" (65") or 5'7" (67")**. She has **red hair** and she drives a **Tesla Model S**. I know that she attended the **SQL Symphony Concert 3 times in December 2017**._

Here is our clues about our mastermind:
  
  - The mastermind is a female with red hair, approximately 5'5" (65") or 5'7" (67") tall
  - She drives a Tesla Model S
  - She attended the SQL Symphony Concert 3 times in December 2017

What an interesting turn of events... 🤔

With one last query, we shall determine exactly who our mastermind just like how we found our murderer.

```` sql
SELECT 	person.name, drivers_license.*
FROM person 
JOIN drivers_license
ON drivers_license.id = person.license_id
JOIN facebook_event_checkin
ON facebook_event_checkin.person_id = person.id
WHERE drivers_license.gender = 'female'
	AND drivers_license.height BETWEEN 65 AND 67
	AND drivers_license.hair_color = 'red'
	AND drivers_license.car_make = 'Tesla'
	AND drivers_license.car_model = 'Model S'
	AND facebook_event_checkin.date LIKE '201712%'
GROUP BY facebook_event_checkin.event_name
HAVING COUNT(LOWER(facebook_event_checkin.event_name) = 'sql symphony concert') > 1
ORDER BY facebook_event_checkin.event_name;
````

![image](https://github.com/user-attachments/assets/c4320b11-bb0c-4e39-b6c2-899c41297432)

We have found the true mastermind! It was Miranda Priestly all along. Now let's verify this by putting her name into the database.

```` sql
INSERT INTO solution VALUES (1, 'Miranda Priestly');

SELECT value
FROM solution;
````

![image](https://github.com/user-attachments/assets/cdc8ac60-7de2-4bc3-b13c-d89af819eb9f)

_Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!_

Elementary, my dear Watson.
