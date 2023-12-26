# SQL Murder - Walktrough

![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/addb4fa9-5d03-48cc-86d5-df87737eb11a)

# Overview

This challenge consists in finding the Murder of a case in SQL City. The first 
thing showed in the challenge is the relationshiops of all the tables from the 
database:

![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/50bd2d4b-8b2b-4ef8-aada-0b2e04e1a338)

The schema above shows that there is an interesting table named crime_scene_report, which have 
descriptions from the witnesses about many crimes.

In the primary text of the challenge it says that the crime was made in SQL City, it was a murder and 
it was on Jan.15, 2018.

So, the first step was to check the crime report filtered by this values.

# Step by step

1. `SELECT * FROM crime_scene_report WHERE city='SQL City' AND type='murder' AND date=20180115`
   **Return**: Security footage shows that there were 2 witnesses. The first witness lives at
   the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

   So, I decided to check first for the person who lives in the last house of the Northwestern Dr Street.
   
2. `SELECT name, MAX(address_number) FROM person WHERE address_street_name='Northwestern Dr'`
  **Return**: Morty Schapiro | 4919

  Knowing his name, the next step was to find his interview in interview table.

3. `SELECT * FROM person JOIN interview ON person.id = interview.person_id WHERE name='Morty Schapiro'`
   **Return**: I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z".
   Only gold members have those bags. The man got into a car with a plate that included "H42W".

  Using LIKE we can search for the plate number of the car on drivers_license table.

4. `SELECT * FROM person JOIN drivers_license ON person.license_id = drivers_license.id WHERE plate_number LIKE '%H42W%'`
  **Return**: ![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/8f4fff8a-e464-4e78-b2ae-42508d8b6087)

  Observing the output we see that the murderer can be one of this three people. SO I searched for the git fit now member id too.

5. `SELECT * FROM person JOIN get_fit_now_member ON person.id = get_fit_now_member.person_id WHERE get_fit_now_member.id LIKE '48Z%'`
   
   **Return**: ![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/e3e4be5f-08c6-42b7-885d-3f2717665dd2)

   Looking this return and the past, we can infer that the murder can only possible be **Jeremy Bowers**. So i tried to insert his name in the
   solution table.
6. `INSERT INTO solution VALUES (1, 'Jeremy Bowers');`
   `SELECT value FROM solution;`
   **Return**: ![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/859f083b-e95b-4f72-8261-5fbce1b334bb)

   So the murderer is **Jeremy Bowers**!. But, there is someone that have contracted him to do the job.
   The first thing to do is to search for the interview of Jeremy Bowers.

7. `SELECT * FROM person JOIN interview ON person.id = interview.person_id WHERE name='Jeremy Bowers'`
   **Return**: I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67").
   She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

   So I focused to find someone's ID that have visited the SQL Symphony Concert 3 times.

8. `SELECT person_id FROM facebook_event_checkin WHERE event_name='SQL Symphony Concert' AND date LIKE '201712%' ORDER BY person_id DESC;`
    
   **Return**: ![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/91ef8b52-be92-4230-82dd-b965688dccc5)

   In the output we can see that only one person was in the concert for three times. So, the last thing to do is to find the person name by this ID (99716).

9. `SELECT name FROM person WHERE id=99716`
    **Return**: Miranda Priestly

    So, the person who have contracted Jeremy Bowers as **Miranda Priestly**

10. `INSERT INTO solution VALUES (1, 'Miranda Priestly');`
    ` SELECT value FROM solution;`
    **Return**: ![image](https://github.com/Klesse/sql-murder-solution/assets/62315031/37bf65f2-7b3b-4c22-ad18-25ac31d1cb49)


# Conclusion

**Jeremy Bowers** murdered someone in SQL City on 01/15/2018 by command of **Miranda Priestly**.

# Author

[Pedro Malandrin Klesse](https://www.github.com/Klesse)





