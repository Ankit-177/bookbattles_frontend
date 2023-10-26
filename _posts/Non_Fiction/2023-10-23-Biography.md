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
                            // Note: Removed the inline onclick event, we'll add it dynamically in JavaScript
                            ratingCell.innerHTML = '<div class="stars" data-book-id="' + book.id + '">' +
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

                    // Initialize DataTables after filling the table
                    $('#demo').DataTable();
                })
                .catch(error => console.error('Error fetching data:', error));
        }

        function generateStarRatingHTML(bookId, rating) {
            const starsHTML = Array.from({ length: 5 }, (_, index) => {
                const starValue = index + 1;
                const isActive = starValue <= rating ? 'active' : '';
                return `<span class="star ${isActive}" data-value="${starValue}">&#9733;</span>`;
            }).join('');

            return `<div class="stars" data-book-id="${bookId}">${starsHTML}</div>`;
        }

        function rateBook(event) {
            if (event.target.matches('.star')) {
                const rating = event.target.getAttribute('data-value');
                const bookId = event.currentTarget.getAttribute('data-book-id');
                sendRatingToBackend(bookId, rating);
            }
        }

        function sendRatingToBackend(bookId, rating) {
            const backendEndpoint = 'https://bookbattles.stu.nighthawkcodingsociety.com/api/review/'; // Replace with your actual rating endpoint
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
                updateStarRating(bookId, rating); // Update the displayed star rating after submitting a new rating
            })
            .catch(error => {
                console.error('Error sending rating:', error);
            });
        }

        function updateStarRating(bookId, rating) {
            const starsContainer = document.querySelector(`[data-book-id="${bookId}"]`);
            const stars = starsContainer.querySelectorAll('.star');

            stars.forEach((star, index) => {
                const starValue = index + 1;
                const isActive = starValue <= rating ? 'active' : '';
                star.className = `star ${isActive}`;
            });
        }

        // Delegate the event handling to the document level
        document.addEventListener('click', function(event) {
            if (event.target.matches('.stars')) {
                rateBook(event);
            }
        });

        // Call fillTable after defining functions
        fillTable();
        
        // Add a click event listener for the star rating component
        document.body.addEventListener('click', rateBook);
    </script>
</body>
</html>