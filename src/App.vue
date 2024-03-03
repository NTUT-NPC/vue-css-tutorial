<script setup>
import "https://kit.fontawesome.com/f4ab973a80.js"
import Comment from "@/components/Comment.vue"
import {ref} from "vue"

const comments = ref([])
const newCommentAuthor = ref("")
const newCommentText = ref("")

function getComments() {
  fetch("http://localhost:8080/comments")
      .then(response => response.json())
      .then(fetchedComments => {
        console.log("Comments:", fetchedComments)
        comments.value = fetchedComments
      })
}

getComments() // Fetch comments when the app starts

function postComment(event) {
  event.preventDefault() // Prevent the form from submitting
  console.log("Adding comment:", newCommentText.value)

  fetch("http://localhost:8080/comments", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      author: newCommentAuthor.value,
      content: newCommentText.value,
      created: new Date().toISOString(),
    })
  })

  // Clear the new comment field
  newCommentAuthor.value = ""
  newCommentText.value = ""

  // Fetch the comments again to update the list
  getComments()
}
</script>

<template>
  <main>
    <h1>Chenghsuan Ho</h1>

    <h2>About me</h2>
    <img src="https://picsum.photos/200" alt="My profile picture">
    <p>Born in 2003, I'm probably the worst programmer the world has seen since then.</p>
    <ul class="bio">
      <li><i class="fa-solid fa-cake-candles"></i><span>12 May, 2003</span></li>
      <li><i class="fa-solid fa-school"></i><span>Taipei Tech, Electronic</span></li>
      <li><i class="fa-solid fa-suitcase"></i><span>Lightenform</span></li>
    </ul>

    <h2>Projects</h2>
    <ul class="project-list">
      <li>
        <h3>libITC</h3>
        <p>VHDL library for the 108th to 112nd ITC hardware</p>
      </li>
      <li>
        <h3>Emily</h3>
        <p>Discord.js assistant for the NTUT Programming Club. Originally written in Java.</p>
      </li>
      <li>
        <h3>flash51</h3>
        <p>A 8051 flash tool built for the Principles and Applications of Microcomputers course.</p>
      </li>
      <li>
        <h3>HarmonyFlow</h3>
        <p>Cross-platform, responsive ChatGPT front end written in Flutter.</p>
      </li>
    </ul>

    <h2>Socials</h2>
    <ul class="social-list">
      <li><a href="https://www.facebook.com/chenghsuandottxt/"><i class="fa-brands fa-facebook"></i></a></li>
      <li><a href="https://www.instagram.com/chenghsuan.jpg/"><i class="fa-brands fa-instagram"></i></a></li>
      <li><a href="https://is.gd/rileytwitter"><i class="fa-brands fa-twitter"></i></a></li>
    </ul>

    <h2>Comments</h2>
    <article v-for="comment in comments">
      <Comment
          v-bind:key="comment.id"
          v-bind:author="comment.author"
          v-bind:date="new Date(comment.created)"
          v-bind:content="comment.content"
      />
      <hr>
    </article>

    <form v-on:submit="postComment" class="new-comment">
      <textarea v-model="newCommentText" placeholder="Add a comment..."></textarea>
      <div class="new-comment-footer">
        <span>By <input v-model="newCommentAuthor" type="text" placeholder="Your name"></span>
        <button type="submit">Submit</button>
      </div>
    </form>

    <!-- Being able to scroll past the end aids the presentation -->
    <div style="height: 1000px"></div>
  </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans&family=Rubik+Maps&display=swap');

h1 {
  font-family: 'Rubik Maps', sans-serif;
  font-weight: 400;
}

p,
li {
  font-family: 'Josefin Sans', sans-serif;
}

.project-list {
  padding-left: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.project-list > li {
  list-style-type: none;
  width: 200px;
  padding: 1em;
  margin: 1em;
  border: 1px solid black;
}

main {
  max-width: 768px;
  margin: 0 auto;
}

.social-list {
  padding-left: 0;
  display: flex;
  gap: 0.5em;
  justify-content: center;
}

.social-list > li {
  list-style-type: none;
}

.bio {
  padding-left: 0;
}

.bio > li > i {
  width: 40px;
  text-align: center;
}

.bio > li > span {
  flex-grow: 1;
}

.new-comment > textarea {
  width: 100%;
  resize: vertical;
  margin-bottom: 0.5rem;
}

.new-comment-footer {
  display: flex;
  justify-content: space-between;
}
</style>
