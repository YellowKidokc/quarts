

TABLE rows.file.link AS "Files"
FROM ""
FLATTEN file.tags AS tag
GROUP BY tag
SORT tag ASC

