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
