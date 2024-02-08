## Task1
```
CREATE VIEW v_persons_female AS (
  SELECT * FROM person WHERE gender = 'female'
)
SELECT * FROM v_persons_female;

CREATE VIEW v_persons_male AS (
  SELECT * FROM person WHERE gender = 'male'
)
SELECT * FROM v_persons_male;
SELECT name FROM v_persons_female
UNION ALL
SELECT name FROM v_persons_male ORDER BY name
```
![image](https://github.com/KlagT/SQL-08.02-/assets/90261661/d1ea64db-4e15-4aae-b887-97af2388a943)
---

## Task2
```
CREATE VIEW dates AS(
  SELECT gen_dates::date FROM generate_series('2022-01-01', '2022-01-31', interval '1 day') as gen_dates ORDER BY gen_dates)
SELECT * FROM dates
```
![image](https://github.com/KlagT/SQL-08.02-/assets/90261661/e3daf67c-ce0c-4dee-9587-9f3152ff49d6)
---

## Task3
```
WITH v_generated_dates AS (
    -- Your logic to generate a list of dates, assuming it's available as a view
    -- For example, using generate_series in PostgreSQL:
    SELECT generate_series('2022-01-01'::date, '2022-01-31'::date, interval '1 day')::date AS generated_date
)

SELECT
    p.person_id,
    vd.generated_date AS missing_date
FROM
    v_generated_dates vd
CROSS JOIN
    (SELECT DISTINCT person_id FROM person_visits) p
LEFT JOIN
    person_visits pv ON vd.generated_date = pv.visit_date AND p.person_id = pv.person_id
WHERE
    vd.generated_date BETWEEN '2022-01-01' AND '2022-01-31'
    AND pv.visit_date IS NULL
ORDER BY
    p.person_id, vd.generated_date;
```
![image](https://github.com/KlagT/SQL-08.02-/assets/90261661/40ec9e6b-ce35-41d2-91f9-25e1e21e6f47)

---

## Task4
```
CREATE VIEW v_symmetric_union AS
SELECT person_id
FROM (
    SELECT person_id
    FROM person_visits
    WHERE visit_date = '2022-01-02'
    EXCEPT
    SELECT person_id
    FROM person_visits
    WHERE visit_date = '2022-01-06'  
    UNION    
    SELECT person_id
    FROM person_visits
    WHERE visit_date = '2022-01-06'
    EXCEPT
    SELECT person_id
    FROM person_visits
    WHERE visit_date = '2022-01-02'
) AS symmetric_union
ORDER BY person_id;
```
![image](https://github.com/KlagT/SQL-08.02-/assets/90261661/bbf6056e-e10e-49b6-b9f7-e53525b9f844)


---
