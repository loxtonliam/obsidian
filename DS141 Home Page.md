```dataview
TABLE file.mtime AS "Last Modified", file.folder AS "Folder"
FROM #DS141 
SORT file.name ASC

```




| Tutorial No. | Percentage |
| ------------ | ---------- |
| 1            |            |
| AVG          | 0          |
<!-- TBLFM: @>$2=mean(@I$>..@-1$>) -->

| Assignment | Percentage | Weight | Weighted Score |
| ---------- | ---------- | ------ | -------------- |
| Tuts       |            | 0.1    | 0              |
| A1         |            | 0.35   | 0              |
| A2         |            | 0.55   | 0              |
| Avg        |            |        | 0              |
<!-- TBLFM: $>=($2*$3) -->
<!-- TBLFM: @>$>=sum(@I$>..@-1$>) -->