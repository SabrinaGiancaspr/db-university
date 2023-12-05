1. Selezionare tutti gli studenti nati nel 1990 (160):
    SELECT * FROM ` students ` WHERE YEAR(date_of_birth) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479):
    SELECT * FROM `courses` WHERE cfu > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni:
    SELECT * FROM `students` WHERE TIMESTAMPDIFF(YEAR, date_of_birth, CURDATE()) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286):
    SELECT * FROM `courses` WHERE year = 1 AND period = 'I semestre';

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21):
    SELCET * FROM `exams`  WHERE DATE(date) = '2020-06-20' AND HOUR(hour) > 14;

6. Selezionare tutti i corsi di laurea magistrale (38):
    SELECT * FROM `degrees` WHERE level = 'magistrale';

7. Da quanti dipartimenti è composta l'università? (12):
    SELECT COUNT(*) AS `departments_count` FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
    SELECT * FROM `teachers` WHERE phone IS NULL OR phone = '';



<!-- join -->

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia:

    SELECT `students`.`name`,`surname`,`date_of_birth`, `degrees`.`name`AS `degree_name` FROM `students` INNER JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id` WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze:

SELECT `degrees`.*, `departments`.`name` FROM `degrees` INNER JOIN `departments` ON `departments``id` = `degrees`.`department_id` WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name`='Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44):

SELECT CONCAT(`teachers`.`name`,' ', `teachers`.`surname`) AS `full_name`, `teachers`.`id`, `courses`.`name` AS `course_name`,`course_teacher`.`course_id` FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id` INNER JOIN `courses` ON `courses`.`id` = `course_teacher`.`teacher_id` WHERE `teachers`.`id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome:

SELECT CONCAT(`surname`,' ',`students`.`name`) AS `full_name`, `degrees`.*, `departments`.* FROM `students` INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` ORDER BY `full_name` ASC;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti:

SELECT * FROM `degrees` INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54):

SELECT DISTINCT `teachers`.*, `departments`.`name` AS 'departments_name' FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id` INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id` INNER JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` WHERE `departments`.`name` = 'Dipartimento di Matematica';

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18:

SELECT `students`.`id` AS 'students_id', `courses`.`name` AS 'courses_name', COUNT(`exams`.`id`) AS `tentativi`, MAX(`exam_student`.`vote`) AS `voto_massimo` FROM `students` INNER JOIN `exam_student` ON `students`.`id`= `exam_student`.`student_id` INNER JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id` INNER JOIN `courses` ON `exams`.`course_id` = `courses`.`id` WHERE `exam_student`.`vote` >= 18 GROUP BY `students`.`id`, `courses`.`id`;

<!-- group by  --> 

1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(`enrolment_date`) AS `anno_iscrizione`, COUNT(*) AS `iscritti_per_anno` FROM `students` GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT `teachers`.`office_address`, COUNT(*) AS `stesso_ufficio` FROM `teachers` GROUP BY `teachers`.`office_address`;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT `exam_id`, AVG(`vote`) AS `media_voto` FROM `exam_student` GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `departments`.`name`, COUNT(*) AS `numero_corsi_laurea` FROM `degrees` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` GROUP BY `department_id`;