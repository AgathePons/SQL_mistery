# SQL Murder Mistery

## Start, find the rapport
**Rapport**

```sql
SELECT * FROM crime_scene_report WHERE city='SQL City' AND type='murder' AND date=20180115;
```

Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

## First witness

```sql
SELECT * FROM interview JOIN person ON person.id=interview.person_id WHERE 
(person.address_street_name='Franklin Ave' AND person.name LIKE '%Annabel%');
```

I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

## Second witness

```sql
SELECT * FROM interview JOIN person ON person.id=interview.person_id WHERE 
(person.address_street_name='Northwestern Dr') 
ORDER BY person.address_number DESC
LIMIT 1;
```

I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

## Investigate...

```sql
SELECT * FROM person 
JOIN get_fit_now_member 
ON get_fit_now_member.person_id=person.id
JOIN get_fit_now_check_in
ON get_fit_now_member.id=get_fit_now_check_in.membership_id
JOIN drivers_license
ON drivers_license.id=person.license_id
WHERE get_fit_now_check_in.check_in_date=20180109 
AND get_fit_now_member.membership_status='gold'
AND get_fit_now_member.id LIKE '48Z%'
AND drivers_license.plate_number LIKE '%H42W%'
LIMIT 5;
```

Jeremy Bowers

Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.

## Jeremy Bowers's interview

```sql
SELECT * FROM interview 
JOIN person 
ON interview.person_id=person.id
WHERE person.name='Jeremy Bowers';
```

I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

## Find the mastermind

```sql
SELECT * FROM person
JOIN facebook_event_checkin
ON facebook_event_checkin.person_id=person.id
JOIN drivers_license
ON drivers_license.id=person.license_id
WHERE facebook_event_checkin.event_name='SQL Symphony Concert'
AND drivers_license.height BETWEEN 65 AND 67
AND drivers_license.hair_color='red'
AND drivers_license.gender='female'
AND drivers_license.car_make='Tesla'
AND drivers_license.car_model='Model S';
```

Miranda Priestly

Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!