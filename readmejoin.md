1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia:

SELECT `students`.`name`,`surname`,`date_of_birth`, `degrees`.`name`AS `degree_name` FROM `students` INNER JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id` WHERE `degrees`.`name` = 'Corso di Laurea in Economia';