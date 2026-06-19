---
title: "Contact"
draft: false
weight: 0
noComment: true
---

<p class="contact-intro">For general questions, collaborations, speaking, or anything that does not fit the coaching form, send me a note here.</p>

You can also find me on [LinkedIn](https://linkedin.com/in/wadewegner/), [GitHub](https://github.com/wadewegner), and [Strava](https://www.strava.com/athletes/27524150).

<section class="contact-cta" id="contact-me" aria-labelledby="contact-cta-title">
  <div class="contact-cta-copy">
    <h2 id="contact-cta-title">Send a message</h2>
    <p>Share what you want to talk about and the best way to get back to you.</p>
  </div>

<form name="contact-me" method="POST" data-netlify="true" class="contact-form">
  <input type="hidden" name="form-name" value="contact-me">

  <div class="field">
    <label class="label" for="contact-name">Name</label>
    <div class="control">
      <input class="input" id="contact-name" type="text" name="name" autocomplete="name" required>
    </div>
  </div>

  <div class="field">
    <label class="label" for="contact-email">Email</label>
    <div class="control">
      <input class="input" id="contact-email" type="email" name="email" autocomplete="email" required>
    </div>
  </div>

  <div class="field">
    <label class="label" for="contact-topic">Topic</label>
    <div class="control">
      <select class="select" id="contact-topic" name="topic" required>
        <option value="">Select one</option>
        <option value="general">General question</option>
        <option value="developer-platforms">Developer platforms or AI</option>
        <option value="speaking">Speaking or community</option>
        <option value="coaching-follow-up">Coaching follow-up</option>
      </select>
    </div>
  </div>

  <div class="field">
    <label class="label" for="contact-message">Message</label>
    <div class="control">
      <textarea class="textarea" id="contact-message" name="message" placeholder="What would you like to talk about?" required></textarea>
    </div>
  </div>

  <div class="field">
    <div class="control">
      <button class="button is-link" type="submit">Send message</button>
    </div>
  </div>
</form>
</section>
