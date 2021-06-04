COUNTING CHARS

```sqlite
SELECT pub_name,LENGTH(pub_name) 
FROM publisher
WHERE LENGTH(pub_name)>=20;
```

