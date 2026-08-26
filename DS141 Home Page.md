```dataview
TABLE file.mtime AS "Last Modified", file.folder AS "Folder"
FROM #DS141 
SORT file.name ASC

```




| Tutorial No. | Percentage |
| ------------ | ---------- |
| 1            | 100        |
| 2            | 93.333     |
| AVG          | 96.6665    |
<!-- TBLFM: @>$2=mean(@I$>..@-1$>) -->

| Assignment | Percentage | Weight | Weighted Score |
| ---------- | ---------- | ------ | -------------- |
| Tuts       | 96.67      | 0.1    | 9.667          |
| A1         |            | 0.35   | 0              |
| A2         |            | 0.55   | 0              |
| Avg        |            |        | 9.667          |
<!-- TBLFM: $>=($2*$3) -->
<!-- TBLFM: @>$>=sum(@I$>..@-1$>) -->