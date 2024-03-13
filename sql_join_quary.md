1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```
SELECT
  `students`.`id`,
  `students`.`name`,
  `students`.`surname`,
  `students`.`date_of_birth`,
  `students`.`fiscal_code`,
  `students`.`enrolment_date`,
  `students`.`registration_number`,
  `students`.`email`

FROM `students`

INNER JOIN `degrees`
ON `degrees`.`id` = `students`.`degree_id`

WHERE `degrees`.`name` = "Corso di Laurea in Economia";
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze

```
SELECT
	`degrees`.`id`,
    `degrees`.`name`,
    `degrees`.`level`,
    `degrees`.`address`,
    `degrees`.`email`,
    `degrees`.`website`

FROM `degrees`

INNER JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`

WHERE `departments`.`name` = "Dipartimento di Neuroscienze"
AND `degrees`.`level` = "magistrale";
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```
SELECT
	`courses`.*,
	`course_teacher`.`teacher_id`

FROM `courses`

INNER JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

INNER JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`

WHERE `teachers`.`name` = "Fulvio"
AND `teachers`.`surname` = "Amato";


# OR #


SELECT `courses`.*

FROM `courses`

INNER JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

WHERE `course_teacher`.`teacher_id` = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome

```
SELECT
	`students`.`id`,
  `students`.`name`,
  `students`.`surname`,
  `students`.`date_of_birth`,
  `students`.`fiscal_code`,
  `students`.`enrolment_date`,
  `students`.`registration_number`,
  `students`.`email`,
  `degrees`.`name` AS `degree`,
  `degrees`.`level` AS `degree_level`,
  `degrees`.`address` AS `degree_address`,
  `degrees`.`email` AS `degree_email`,
  `degrees`.`website` AS `degree_website`,
  `departments`.`name` AS `degree_department`

FROM `students`

INNER JOIN `degrees`
ON `degrees`.`id` = `students`.`degree_id`

INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`

ORDER BY `students`.`name` ASC, `students`.`surname` ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```
SELECT
	`degrees`.`id`,
    `degrees`.`name` AS `degree_name`,
    `degrees`.`level`,
    `degrees`.`address`,
    `degrees`.`email`,
    `degrees`.`website`,
    `courses`.`name` AS `course_name`,
    CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher`


FROM `degrees`

INNER JOIN `courses`
ON `courses`.`degree_id` = `degrees`.`id`

INNER JOIN `course_teacher`
ON `course_teacher`.`course_id` = `courses`.`id`

INNER JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
ORDER BY `degrees`.`id` ASC, `courses`.`name` ASC;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)

```
SELECT DISTINCT `teachers`.*

FROM `teachers`

INNER JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`

INNER JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`

INNER JOIN `degrees`
ON `degrees`.`id` = `courses`.`degree_id`

INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`

WHERE `departments`.`name` = "Dipartimento di Matematica";
```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18

```
SELECT
	CONCAT(`students`.`name`, ' ', `students`.`surname`) AS `student`,
    `courses`.`name`,
	COUNT(*) AS `attempt`,
    MAX(`exam_student`.`vote`) AS `max_vote`

FROM `exam_student`

INNER JOIN `students`
ON `students`.`id` = `exam_student`.`student_id`

INNER JOIN `exams`
ON `exams`.`id` = `exam_student`.`exam_id`

INNER JOIN `courses`
ON `courses`.`id` = `exams`.`course_id`

GROUP BY `students`.`id`, `courses`.`id`

HAVING `max_vote` >= 18;
```
