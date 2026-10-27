```dataview
TABLE file.mtime AS "Last Modified", file.folder AS "Folder"
FROM #val
SORT file.name ASC
```

