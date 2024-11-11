<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <RouterLink class="navbar-brand" to="/">
        <img
          src="C:\Users\giber\Desktop\bs21inf5\sem07-project-GianBeram\engaging-quiz-sign-vector-bold-eyecatching-design-interactive-educational-graphics_579306-15541.avif"
          alt="Quiz"
          width="24"
          height="24"
          class="d-inline-block align-text-top"
        />
        Quiz
      </RouterLink>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbar"
        aria-controls="navbar"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div id="navbar" class="collapse navbar-collapse">
        <ul class="navbar-nav">
          <li class="nav-item">
            <RouterLink class="nav-link" to="/about">
              <i class="bi bi-info-square"></i>
              À propos
            </RouterLink>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <RouterView />
</template>
