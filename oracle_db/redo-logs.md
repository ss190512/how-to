## Check switching frequency of redo logs.
```
SELECT
    TO_CHAR(first_time, 'YYYY-MM-DD') day,
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '00', 1, 0)), '999') "00",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '01', 1, 0)), '999') "01",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '02', 1, 0)), '999') "02",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '03', 1, 0)), '999') "03",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '04', 1, 0)), '999') "04",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '05', 1, 0)), '999') "05",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '06', 1, 0)), '999') "06",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '07', 1, 0)), '999') "07",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '08', 1, 0)), '999') "0",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '09', 1, 0)), '999') "09",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '10', 1, 0)), '999') "10",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '11', 1, 0)), '999') "11",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '12', 1, 0)), '999') "12",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '13', 1, 0)), '999') "13",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '14', 1, 0)), '999') "14",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '15', 1, 0)), '999') "15",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '16', 1, 0)), '999') "16",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '17', 1, 0)), '999') "17",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '18', 1, 0)), '999') "18",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '19', 1, 0)), '999') "19",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '20', 1, 0)), '999') "20",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '21', 1, 0)), '999') "21",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '22', 1, 0)), '999') "22",
    TO_CHAR(SUM(DECODE(TO_CHAR(first_time, 'HH24'), '23', 1, 0)), '999') "23"
FROM v$log_history
GROUP BY TO_CHAR(first_time, 'YYYY-MM-DD')
ORDER BY day;
```
## Status of redo-logs.
```
SELECT
    l.group#,
    l.thread#,
    f.member,
    l.archived,
    l.status,
    ( bytes / 1024 / 1024 ) size_mb
FROM
    v$log       l,
    v$logfile   f
WHERE
    f.group# = l.group#
ORDER BY
    1, 2;
```
## Script to generate scripts for re-creating redo-logs (setup needed size for the new_size variable).
```
SET SERVEROUTPUT ON;

DECLARE
    new_size            VARCHAR2(100) := '2g';
    rm_command          VARCHAR2(200);
    create_gr_command   VARCHAR2(500);
    tmp_thread          VARCHAR2(10);
    CURSOR cur_gr IS
    SELECT group#, thread# FROM v$log;
BEGIN
    FOR tmp_rec IN cur_gr LOOP
        dbms_output.put_line('------------------------');
        dbms_output.put_line('alter system switch logfile;');
        dbms_output.put_line('alter system archive log current;');
        dbms_output.put_line('alter database drop logfile group '
                             || tmp_rec.group#
                             || ';');
        SELECT
            'rm '
            ||
                LISTAGG('"'
                        || member
                        || '"', ' ') WITHIN GROUP(
                    ORDER BY
                        member
                )
            || '' members
        INTO rm_command
        FROM
            ( SELECT f.group#, f.member
                FROM v$logfile f
            )
        WHERE group# = tmp_rec.group#;

        dbms_output.put_line(rm_command);
        SELECT
            'alter database add logfile thread '
            || tmp_rec.thread#
            || ' group '
            || tmp_rec.group#
            || ' ('
            ||
                LISTAGG(''''
                        || TRIM(member)
                        || '''', ',') WITHIN GROUP(
                    ORDER BY
                        member
                )
            || ') size '
            || new_size
            || ';' members
        INTO create_gr_command
        FROM
            (   SELECT l.group#, l.thread#, f.member
                FROM
                    v$log       l,
                    v$logfile   f
                WHERE f.group# = l.group#
                ORDER BY 1, 2
            )
        WHERE
            group# = tmp_rec.group#;

        dbms_output.put_line(create_gr_command);
    END LOOP;
END;
```
