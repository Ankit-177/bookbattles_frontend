---
layout: post
hide: false
comments : true
title: BookBattles - Blind Book Date
description: Random book generator - a fun way to pick up a new book!
type: ccc
permalink: /basics/blinddate
---

<html>

<head>
    <title>Random Book Generator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f2f2f2;
            padding: 20px;
        }

        #book-box {
            background-color: #fff;
            border-radius: 5px;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        }

        #new-book {
            background-color: #007BFF;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
        }

        #new-book:hover {
            background-color: #0056b3;
        }
    </style>
</head>

<body>
    <div id="book-box">
        <h1>Random Book Generator</h1>
        <p id="book">Click the button to get a random book recommendation.</p>
        <button id="new-book">New Book</button>
    </div>

    <script>
        const books = [
            "To Kill a Mockingbird by Harper Lee",
            "1984 by George Orwell",
            "The Hobbit by J.R.R. Tolkien",
            "Harry Potter and the Sorcerer's Stone by J.K. Rowling",
            "The Lord of the Rings by J.R.R. Tolkien",
            "The Love Hypothesis by Ali Hazelwood",
            "The Spanish Love Deception by Elena Armas",
            "The Alchemist by Paulo Coelho"
        ];

        const bookElement = document.getElementById("book");
        const newBookButton = document.getElementById("new-book");

        newBookButton.addEventListener("click", function () {
            if (books.length === 0) {
                bookElement.textContent = "No books available.";
            } else {
                const randomIndex = Math.floor(Math.random() * books.length);
                bookElement.textContent = "How about reading: " + books[randomIndex];
            }
        });
    </script>
</body>

</html>





