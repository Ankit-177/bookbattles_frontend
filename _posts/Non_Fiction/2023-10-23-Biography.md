---
layout: post
hide: false
comments : true
title: Non-fiction
description: An introduction to basic HTML, and resources to learn more.
type: ccc
permalink: /basics/Biography
---
<!-- the line below 'pulls' the info from the file nav_non_fiction.html to create a table-->
{% include nav_basics.html %}
<!-- This is where the code goes for the subpage of the subpage 'non_fiction'-->
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- load jQuery and DataTables output style and scripts -->
    <!-- The line below styles the table -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
    <!-- Brings out a tool from jQuery to help change the way the table looks -->
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>var define = null;</script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
    <style>
        .stars {
            font-size: 16px;
            cursor: pointer;
        }
        .stars span {
            color: gray;
        }
        .stars span:hover,
        .stars span.active {
            color: gold;
        }
    </style>
</head>
<body>
    <table id="demo" class="table">
        <thead>
            <tr>
                <th>Book Author</th>
                <th>Book Title</th>
                <th>Rating</th>
                <th>Cover</th>
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
                        if (book.genres.includes("self help")) {
                            var table_row = document.createElement("tr");
                            var author = document.createElement("td");
                            var title = document.createElement("td");
                            var ratingCell = document.createElement("td");
                            var coverCell = document.createElement("td");

                            author.innerHTML = book.book_author;
                            title.innerHTML = book.book_title;
                            ratingCell.innerHTML = '<div class="stars" onclick="rateBook(event)" data-book-id="' + book.id + '">' +
                                '<span class="star" data-value="1">&#9733;</span>' +
                                '<span class="star" data-value="2">&#9733;</span>' +
                                '<span class="star" data-value="3">&#9733;</span>' +
                                '<span class="star" data-value="4">&#9733;</span>' +
                                '<span class="star" data-value="5">&#9733;</span>' +
                                '</div>';
                            coverCell.innerHTML = '<img src="' + book.cover_url + '" alt="Book Cover" style="width:50px;height:50px;">';

                            table_row.appendChild(author);
                            table_row.appendChild(title);
                            table_row.appendChild(ratingCell);
                            table_row.appendChild(coverCell);

                            tableBody.appendChild(table_row);
                        }
                    });

                    $('#demo').DataTable();
                })
                .catch(error => console.error('Error fetching data:', error));
        }

        function rateBook(event) {
            if (event.target.matches('.star')) {
                const rating = event.target.getAttribute('data-value');
                const bookId = event.currentTarget.getAttribute('data-book-id');
                sendRatingToBackend(bookId, rating);
            }
        }

        function sendRatingToBackend(bookId, rating) {
            const backendEndpoint = 'YOUR_BACKEND_RATING_ENDPOINT'; // Replace with your actual rating endpoint
            fetch(backendEndpoint, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ bookId: bookId, rating: rating }),
            })
            .then(response => {
                if (!response.ok) {
                    throw new Error('Failed to send rating to the backend');
                }
                return response.json();
            })
            .then(data => {
                console.log('Rating sent successfully:', data);
            })
            .catch(error => {
                console.error('Error sending rating:', error);
            });
        }

        fillTable();
    </script>
</body>
</html>
