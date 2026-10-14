```dataview
TABLE length(rows) AS "Total Solved", 
      filter(rows, (r) => r.difficulty = "Easy") AS "Easy",
      filter(rows, (r) => r.difficulty = "Medium") AS "Medium",
      filter(rows, (r) => r.difficulty = "Hard") AS "Hard"
	FROM #dsa/problem
WHERE !contains(file.folder, "Templates")
GROUP BY pattern
```


---
