```dataview
TABLE difficulty, pattern, sheet, last_reviewed
FROM #dsa/problem
WHERE !contains(file.folder, "Templates")
SORT next_review ASC
```

---
