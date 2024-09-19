---
layout: post
title: Javascript Github Pages Hacks
description:  Javascript cell hack.
type: issues
comments: true
---

# Javascript

<script>
var user_name = prompt("Please enter your name homie:");
fetch(`https://api.agify.io?name=${user_name}`)
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
</script>