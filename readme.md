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

    SELECT `degrees`.*, `departments`.`name` FROM `degrees` INNER JOIN `departments` ON `departments`.`id` = `degrees`.`department_id` WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name`= 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44):
