# A Toy Database

Here is a toy database which models an online music store.  In this database, we have falling into three categories: information about the company, information about customers and their orders, and information about the products being sold.

You'll be editing the `queries.sql` file, which consists of a series of
"plain English" sentences.  Your job is to translate those sentences into a
single SQL `SELECT` query.  Don't worry if you can't do it — take your best shot!

We've set up our own PostgreSQL server that you'll be connecting to.

## Contents

- [Contents](#Contents)
- [Exercises](#Exercises)
- [PostgreSQL](#PostgreSQL)
  - [Connecting To PostgreSQL](#Connecting-To-PostgreSQL)
  - [Exiting The PostgreSQL Shell](#Exiting-The-PostgreSQL-Shell)
- [The Database Tables](#The-Database-Tables)
  - [Information About The Products](#Information-About-The-Products)
  - [Information About Customers and Their Orders](#Information-About-Customers-and-Their-Orders)
  - [Information About The Company](#Information-About-The-Company)
- [One Key Idea: Normalization](#One-Key-Idea-Normalization)
- [Useful Resources](#Useful-Resources)

## Exercises

The exercises are contained in individual `.sql` files. They are meant to be completed in the following order:

1. `queries.sql` — Basic SQL queries on a single table
1. `group_by.sql` — Single-table queries using `GROUP BY`
1. `join.sql` — Two-table queries using `JOIN`
1. `left_join.sql` — Two-table queries using `LEFT JOIN`
1. `multi_join.sql` — Queries against more than two tables simultaneously

## PostgreSQL

### Connecting To PostgreSQL

Connect to database by running this in your terminal:

```console
psql -h sql-exercises.20bits.com -U adjacent_student adjacent_sql_exercises
```

You will be prompted for a password. Ask your instructor for this information.

Once inside, type the following at the PostgreSQL prompt

```console
adjacent_sql_exercises> \dt
```

to list all the tables in the database.  To see the schema for a specific table, e.g., the `invoices` table, you can use PostgreSQL's `\d` command:

```text
adjacent_sql_exercises> \d invoices
```

If you see a colon `:` at the bottom of the screen, that means there's more to see. You can scroll up and down using the arrow keys. You can exit the scrolling interface by pressing the `q` key.

Don't be afraid to explore the data in the tables to get a better feeling for how it's formatted.  It's impossible to damage your database as long as you're only running `SELECT` queries.  For example, what does the data in the invoices table look like?  Well, let's look at a 5 rows

```sql
SELECT * FROM invoices LIMIT 5;
```

That's what we mean by "explore."

### Exiting The PostgreSQL Shell

When you're ready to exit the PostgreSQL shell you can press `Ctrl+D` or run the following command:

```console
\q
```

## The Database Tables

Remember: relational databases are organized into tables.  Each table has a "schema", which dictates the fields (aka columns) that the data in the table can or must contain.  The data itself is stored as records (aka rows).

Here are the tables, organized by high-level purpose.

### Information About The Products

1. `albums` are the albums our store is selling
2. `artists` are all the artists
3. `tracks` are all the tracks, containing a column that tells us which album the track belongs to.
4. `media_types` tell us what format a track is in, e.g., MP3, Apple Audio, etc.
5. `genres` are a list of genres.
6. `playlists` are, well, playlists.  A play list has many tracks and a single track can belong to many playlists.
7. `playlist_tracks` tells us which tracks belong to which playlists

### Information About Customers and Their Orders

1. `customers` are our customer records and they may or may not have an assigned "customer support representative"
2. `invoices` are a list of specific customer orders
3. `invoice_lines` are the line-items on specific invoices

### Information About The Company

1. `employees` contains information about our company's employees.  This is mostly used to assign support representatives to customers.

## One Key Idea: Normalization

One of the key ideas in how relational databases like PostgreSQL and MySQL organize data is that we try to minimize redundancy by using references to other data rather than duplicating that data between different tables.

For example, every track belongs to one (and only one) album.  An album has its own associated information, like album title, publication date, and so on.  Each track also has its own associated information, like track title, track position, and duration.  We want to be able to ask questions like "What is the title of the album on which track X appears?"

If you imagine a spreadsheet with a bunch of track listings, one way to achieve this would be to have an "Album Title" column and to answer this question we would just look at the value in the "Album Title" column for track X.  Every track on the same album would have the same value in the "Album Title" column, although heaven help us if there are two separate albums with the same title!

This is not how we store information in a relational database.  Rather than storing album-related information in the same table as track-related information, we store album-related information in an "albums" table and track-related information in a "tracks" table.  We assign each track and album a unique ID.  In the "tracks" table we would when have an `album_id` column containing the unique album ID as a *reference* or *pointer* to the relevant row in the "albums" table.

Excel can do this, but it is too cumbersome for the most common tasks.  In a relational database like PostgreSQL or MySQL, however, it is much easier to deal with.  In fact, SQL (the language) is counting on you storing your data this way.

## Useful Resources

- <http://stackoverflow.com/questions/17946221/sql-join-and-different-types-of-joins>
- <http://en.wikipedia.org/wiki/Join_(SQL)>
