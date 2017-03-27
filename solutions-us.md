Select authors where location isn't Nazareth including null:

  `SELECT * FROM authors WHERE location IS DISTINCT FROM 'Nazareth';`

Select authors when location !== null:

  `SELECT * FROM authors WHERE location IS NOT NULL;`

select authors by name (wistle in this case) within surname (case insensitive):

  `SELECT * FROM authors WHERE LOWER(surname) LIKE LOWER('%wistle%');`

or

  `SELECT * FROM authors WHERE surname ILIKE '%wistle%';`

Select publisher name of a specific book:
```
 SELECT publishers.name FROM publishers
 INNER JOIN books ON books.publisher_id = publishers.id
 WHERE books.name = 'Python Made Easy';
```

Show all books by publisher 'No Starch Press':
```
SELECT books.name FROM books
INNER JOIN publishers ON books.publisher_id = publishers.id
WHERE publishers.name = 'No Starch Press';
```

List all books with their authors (one author per row):
```
SELECT books.name, authors.first_name, authors.surname FROM books
INNER JOIN book_authors ON books.id = book_authors.book_id
INNER JOIN authors ON authors.id = book_authors.author_id
ORDER BY books.name;
```

List all book by an author:
```
SELECT books.name FROM books
INNER JOIN book_authors ON books.id = book_authors.book_id
INNER JOIN authors ON authors.id = book_authors.author_id
WHERE LOWER(authors.first_name) = LOWER('ted');
```

List all authors who have authored at least 3 books:
```
SELECT authors.first_name, authors.surname FROM authors
INNER JOIN book_authors ON book_authors.author_id = authors.id
GROUP BY authors.id
HAVING COUNT(book_authors.author_id) > 2;
```

List the number of books published by one publisher:
```
SELECT publishers.name, COUNT(publishers.id) FROM publishers
INNER JOIN books ON books.publisher_id = publishers.id
GROUP BY publishers.id;
```

List books released after 1-jan-1996 and count how many authors.
```
SELECT books.name, COUNT(book_authors.author_id) as num_authors FROM books
INNER JOIN book_authors ON book_authors.book_id = books.id
WHERE books.release_date > '1-Jan-1996'
GROUP BY books.name
ORDER BY num_authors DESC;
```

What's the highest number of authors per book? The lowest?
```
SELECT COUNT(book_authors.author_id)
FROM books
INNER JOIN book_authors ON book_authors.book_id = books.id
GROUP BY books.name
ORDER BY count DESC
LIMIT 1;
```
or
```
SELECT COUNT(book_authors.author_id)
FROM books
INNER JOIN book_authors ON book_authors.book_id = books.id
GROUP BY books.name
ORDER BY count
LIMIT 1;
```
