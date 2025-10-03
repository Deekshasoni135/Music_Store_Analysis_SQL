# 🎵 Music_Store_Analysis_SQL

## 📌 Overview
This project analyzes a Music Store database using SQL to derive insights into sales, customer behavior, and music trends. The goal is to answer real-world business questions and help in data-driven decision making.

## ❓Business Problem
The project addresses the following questions:
- Who are the top customers based on revenue?
- Which genres and artists generate the highest sales?
- What are the most popular tracks and albums?
- Which countries contribute the most to overall sales?

## 🛠️ Tools & Technologies
- SQL – Data analysis and querying
- PostgreSQL – Database management system
- pgAdmin / DBeaver – Query execution & ERD visualization

## 📂 Dataset
The database contains multiple tables:
- 🎤 Artist, 🎶 Album, 🎼 Track, 🎧 Genre, 📀 MediaType
- 🛒 Invoice, 📋 InvoiceLine
- 👨‍💼 Customer, 👩‍💻 Employee
- 📑 Playlist, 🎵 PlaylistTrack

## 🗂️ Schema (ERD)
The database schema follows this structure:
<img width="710" height="574" alt="MusicDatabaseSchema" src="https://github.com/user-attachments/assets/10f1efd0-5774-4c44-9530-f608ca031e91" />

## 📊 SQL Analysis: Complex Queries & Business Problem Solving
This project goes beyond simple SQL queries by focusing on real business use cases and solving them with complex SQL techniques such as:
- Joins (INNER, LEFT, SELF joins) to combine multiple tables.
- Aggregations (SUM, COUNT, AVG, MAX, MIN) for sales & revenue analysis.
- Subqueries & Nested Queries for customer segmentation.
- Window Functions (RANK(), ROW_NUMBER(), DENSE_RANK()) to rank customers, artists, and tracks.
- CTEs (Common Table Expressions) → for breaking down complex problems into manageable steps, improving query readability and reusability.

### Sample SQL Business Problems Solved
Here are some example business problems addressed in this project:

**1. Senior-most Employee by Job Title**
```sql
SELECT * FROM employee
ORDER BY levels DESC
LIMIT 1
```
🔹 Identifies the senior-most employee in the company based on their job title.

**2. City with the Best Customers**
```sql
SELECT 
      billing_city,
	  SUM(total) AS Total_invoices
FROM invoice
GROUP BY billing_city
ORDER BY Total_invoices DESC
LIMIT 1
```
🔹 Helps decide where to host a promotional music festival, since it returns the city with the highest revenue.

**3. Rock Music Listeners**
```sql
SELECT DISTINCT email, first_name, last_name
FROM customer 
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN (
     SELECT track_id FROM track 
	 JOIN genre ON track.genre_id = genre.genre_id
	 WHERE genre.name LIKE 'Rock'
)
ORDER BY email;
```
🔹 Provides a list of all Rock music listeners for targeted promotions.

**4. Top 10 Rock Artists by Track Count**
```sql
SELECT 
      artist.artist_id, 
	  artist.name, 
	  COUNT(artist.artist_id) AS No_of_songs
FROM track
JOIN album ON track.album_id = album.album_id
JOIN artist ON album.artist_id = artist.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name = 'Rock'
GROUP BY artist.artist_id 
ORDER BY No_of_songs DESC
LIMIT 10
```
🔹 Identifies the top rock artists/bands based on the number of Rock tracks they produced.

**5. Most Popular Genre per Country**
```sql
WITH popular_genre AS
(
     SELECT 
	       COUNT(invoice_line.quantity) AS Purchase,
		   customer.country,
		   genre.name,
		   genre.genre_id,
		   ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo
	 FROM invoice_line
	 JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
	 JOIN customer ON customer.customer_id = invoice.customer_id
	 JOIN track ON track.track_id = invoice_line.track_id
	 JOIN genre ON genre.genre_id = track.genre_id
	 GROUP BY customer.country, genre.name, genre.genre_id
	 ORDER BY 2 ASC, 1 DESC
)
SELECT * FROM popular_genre
WHERE RowNo <= 1
```
🔹 Shows the most popular music genre in each country, helping with regionalized marketing strategies.

## 📌 Future Scope
- Build interactive dashboards in Power BI/Tableau.
- Create automated reports for business users.
- Apply customer segmentation & predictive analytics.
