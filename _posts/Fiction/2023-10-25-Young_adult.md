---
layout: post
hide: false
comments : true
title: Young Adult
description: A genre of fiction books.
type: ccc
permalink: /basics/youngadult
---

{% include nav_basics.html %}

<html>
<head>
    <!-- load jQuery and DataTables output style and scripts -->
    <!-- The line below styles the table -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
    <!-- Brings out a tool from jQuery to help change the way the table looks -->
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>var define = null;</script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
</head>
<body>
    <table id="demo" class="table">
        <thead>
            <tr>
                <th>Book Author</th>
                <th>Book Title</th>
                <th>Cover</th>
                <th>Blurb</th>
            </tr>
        </thead>
        <tbody id="tabledata">

        </tbody>
    </table>

    <script>
        var api = 'https://bookbattles.stu.nighthawkcodingsociety.com/api/books/';
        var tableBody = document.getElementById("tabledata");

        function fillTable() {
            fetch(api)
                .then(response => response.json())
                .then(data => {
                    data.forEach(function(book) {
                        if (book.genres.includes("young adult")) {
                            var table_row = document.createElement("tr");
                            var author = document.createElement("td");
                            var title = document.createElement("td");
                            var coverCell = document.createElement("td");
                            var blurb = document.createElement("td");

                            author.innerHTML = book.book_author;
                            title.innerHTML = book.book_title;
                            coverCell.innerHTML = '<img src="' + book.cover_url + '" alt="Book Cover" style="width:50px;height:50px;">';
                            blurb.innerHTML = book.blurb;

                            table_row.appendChild(author);
                            table_row.appendChild(title);
                            table_row.appendChild(coverCell);
                            table_row.appendChild(blurb);

                            tableBody.appendChild(table_row);
                        }
                    });

                    $('#demo').DataTable();
                })
                .catch(error => console.error('Error fetching data:', error));
        }

        fillTable();
    </script>
</body>
</html>