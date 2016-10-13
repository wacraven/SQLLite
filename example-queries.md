#Example Queries

##Requirements for each query
1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
2. Provide a query only showing the Customers from Brazil.
3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
4. Provide a query showing only the Employees who are Sales Agents.
5. Provide a query showing a unique list of billing countries from the Invoice table.
6. Provide a query showing the invoices of customers who are from Brazil.
7. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
8. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
9. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?
10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
11. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)
12. Provide a query that includes the track name with each invoice line item.
13. Provide a query that includes the purchased track name AND artist name with each invoice line item.
14. Provide a query that shows the # of invoices per country. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)
15. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resultant table.
16. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
17. Provide a query that shows all Invoices but includes the # of invoice line items.
18. Provide a query that shows total sales made by each sales agent.
19. Which sales agent made the most in sales in 2009?
20. Which sales agent made the most in sales in 2010?
21. Which sales agent made the most in sales over all?
22. Provide a query that shows the # of customers assigned to each sales agent.
23. Provide a query that shows the total sales per country. Which country's customers spent the most?
24. Provide a query that shows the most purchased track of 2013.
25. Provide a query that shows the top 5 most purchased tracks over all.
26. Provide a query that shows the top 3 best selling artists.
27. Provide a query that shows the most purchased Media Type.

##Queries
1.
```
SELECT FirstName || " " || LastName AS "Name", CustomerId, Country FROM Customer
Where Country != "USA"
```
2.
```
SELECT FirstName || " " || LastName AS "Name" FROM Customer
Where Country = "Brazil"
```
3.
```
SELECT FirstName || " " || LastName AS "Name", Invoice.InvoiceId, Invoice.InvoiceDate, Invoice.BillingCountry FROM Customer
JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
Where Country = "Brazil"
```
4.
```
SELECT FirstName || " " || LastName AS "Name" FROM Employee
WHERE Title LIKE "sales %"
```
5.
```
SELECT BillingCountry FROM Invoice
GROUP BY BillingCountry
```
6.
```
SELECT * FROM Invoice
WHERE BillingCountry = "Brazil"
```
7.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", Invoice.* FROM Invoice
JOIN Customer on Invoice.CustomerId = Customer.CustomerId
JOIN Employee on Customer.SupportRepId = Employee.EmployeeId
```
8.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", Customer.FirstName || " " || Customer.LastName AS "Customer Name", Invoice.BillingCountry, Invoice.Total FROM Invoice
JOIN Customer on Invoice.CustomerId = Customer.CustomerId
JOIN Employee on Customer.SupportRepId = Employee.EmployeeId
```
9.
```
SELECT COUNT(Total) AS "2009 # of Sales", SUM(Total) AS "2009 Total Sales" FROM Invoice
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2009'

SELECT COUNT(Total) AS "2011 # of Sales", SUM(Total) AS "2011 Total Sales" FROM Invoice
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2011'
```
10.
```
SELECT COUNT(InvoiceId) AS "Total items on Invoice 37" FROM InvoiceLine
WHERE InvoiceId = 37
```
11.
```
SELECT InvoiceId, COUNT(InvoiceId) AS "Items on invoice" FROM InvoiceLine
GROUP BY InvoiceId
```
12.
```
SELECT Track.Name AS "Track Name", InvoiceLine.InvoiceLineId, InvoiceLine.InvoiceId AS "Invoice #" FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
```
13.
```
SELECT Track.Name AS "Track Name", Artist.Name AS "Artist Name", InvoiceLine.InvoiceLineId, InvoiceLine.InvoiceId AS "Invoice #" FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
```
14.
```
SELECT BillingCountry AS "Country", COUNT(BillingCountry) AS "# of Invoices" FROM Invoice
GROUP BY BillingCountry
```
15.
```
SELECT Playlist.Name, COUNT(PlaylistTrack.PlaylistId) AS "# of tracks" FROM Playlist
JOIN PlaylistTrack ON Playlist.PlaylistId = PlaylistTrack.PlaylistId
GROUP BY PlaylistTrack.PlaylistId
```
16.
```
SELECT Track.Name AS "Track", Album.Title AS "Album", MediaType.Name AS "Media Type", Genre.Name AS "Genre" FROM Track
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
JOIN Genre ON Track.GenreId = Genre.GenreId
```
17.
```
SELECT Invoice.*, COUNT(InvoiceLine.InvoiceId) AS "# of items" FROM Invoice
JOIN InvoiceLine ON InvoiceLine.InvoiceId = Invoice.InvoiceId
GROUP BY InvoiceLine.InvoiceId
```
18.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", SUM(Invoice.Total) AS "Total value of all sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice on Invoice.CustomerId = Customer.CustomerId
GROUP BY Customer.SupportRepId
```
19.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", SUM(Invoice.Total) AS "Total value of all sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice on Invoice.CustomerId = Customer.CustomerId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2009'
GROUP BY Customer.SupportRepId
```
20.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", SUM(Invoice.Total) AS "Total value of all sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice on Invoice.CustomerId = Customer.CustomerId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2010'
GROUP BY Customer.SupportRepId
```
21.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", SUM(Invoice.Total) AS "Total value of all sales" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice on Invoice.CustomerId = Customer.CustomerId
GROUP BY Customer.SupportRepId
```
22.
```
SELECT Employee.FirstName || " " || Employee.LastName AS "Agent Name", COUNT(Customer.SupportRepId) AS "# of Customers" FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY Customer.SupportRepId
```
23.
```
SELECT BillingCountry, SUM(Total) AS Total FROM Invoice
GROUP BY BillingCountry
ORDER BY Total DESC
```
24.
```
SELECT Track.Name , COUNT(InvoiceLine.TrackId) AS CountSold FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2013'
GROUP BY InvoiceLine.TrackId
ORDER BY CountSold DESC
LIMIT 1
```
25.
```
SELECT Track.Name , COUNT(InvoiceLine.TrackId) AS CountSold FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
GROUP BY InvoiceLine.TrackId
ORDER BY CountSold DESC
LIMIT 5
```
26.
```
SELECT Artist.Name , COUNT(InvoiceLine.TrackId) AS CountSold FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
GROUP BY Artist.ArtistId
ORDER BY CountSold DESC
LIMIT 3
```
27.
```
SELECT MediaType.Name , COUNT(InvoiceLine.TrackId) AS CountSold FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
GROUP BY MediaType.MediaTypeId
ORDER BY CountSold DESC
```
