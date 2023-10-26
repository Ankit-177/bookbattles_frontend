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
<html>
<body>



<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = "Description for Biography";
</script>

</body>
</html>