
```dataview
TABLE file.mtime AS "Last Modified", file.folder AS "Folder"
FROM #CS144
SORT file.name ASC

```

| Tutorial No. | Percentage |
| ------------ | ---------- |
| 1            | 90         |
| AVG          | 90         |
<!-- TBLFM: @>$2=mean(@I$>..@-1$>) -->



| Assignment | Percentage | Weight | Weighted Score |
| ---------- | ---------- | ------ | -------------- |
| Tuts       | 90         | 0.20   | 18             |
| Project    | 0          | 0.30   | 0              |
| A1         | 0          | 0.25   | 0              |
| A2         | 0          | 0.25   | 0              |
| Avg        |            |        | 18             |
<!-- TBLFM: $>=($2*$3) -->
<!-- TBLFM: @>$>=sum(@I$>..@-1$>) -->




