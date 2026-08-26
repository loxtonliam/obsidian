



| Tutorial No. | Percentage |
| ------------ | ---------- |
| 1            | 100        |
| 2            | 80         |
| AVG          | 90         |
<!-- TBLFM: @>$2=mean(@I$>..@-1$>) -->

| Assignment | Percentage | Weight | Weighted Score |
| ---------- | ---------- | ------ | -------------- |
| Tuts       | 90         | 0.1    | 9              |
| A1         |            | 0.45   | 0              |
| A2         |            | 0.45   | 0              |
| Avg        |            |        | 9              |
<!-- TBLFM: $>=($2*$3) -->
<!-- TBLFM: @>$>=sum(@I$>..@-1$>) -->