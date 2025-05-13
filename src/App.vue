<template>
  <div class="app">
    <header class="app-header">
      <div class="container header-content">
        <div class="logo">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
            <path d="M3 9h18" />
            <path d="M9 21V9" />
          </svg>
          <h1 class="app-title">QR Code Generator</h1>
        </div>
        <nav class="nav-buttons">
          <button
            class="nav-button"
            :class="{ active: !show }"
            @click="show = false"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="18"
              height="18"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
              <polyline points="9 22 9 12 15 12 15 22" />
            </svg>
            <span>Home</span>
          </button>
          <button
            class="nav-button"
            :class="{ active: show }"
            @click="showHistory"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="18"
              height="18"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <circle cx="12" cy="12" r="10" />
              <polyline points="12 6 12 12 16 14" />
            </svg>
            <span>History</span>
          </button>
        </nav>
      </div>
    </header>

    <main class="app-main">
      <div class="container">
        <transition name="fade" mode="out-in">
          <home v-if="!show" @childtoparent="fromChild" />
          <history v-else :Sent="ChildData" />
        </transition>
      </div>
    </main>

    <footer class="app-footer">
      <div class="container footer-content">
        <p>
          &copy; {{ new Date().getFullYear() }} QR Code Generator. All rights
          reserved.
        </p>
        <div class="social-links">
          <a
            href="https://github.com/madildev"
            target="_blank"
            rel="noopener noreferrer"
            class="social-link"
            aria-label="GitHub"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"
              />
            </svg>
          </a>
          <a
            href="https://www.linkedin.com/in/madildev"
            target="_blank"
            rel="noopener noreferrer"
            class="social-link"
            aria-label="LinkedIn"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"
              />
              <rect x="2" y="9" width="4" height="12" />
              <circle cx="4" cy="4" r="2" />
            </svg>
          </a>
        </div>
      </div>
    </footer>
  </div>
</template>
<script>
import Home from "../src/views/Home.vue";
import History from "../src/views/History.vue";

export default {
  name: "App",
  components: {
    Home,
    History
  },
  data() {
    return {
      ChildData: [],
      show: false
    };
  },
  methods: {
    fromChild(value) {
      this.ChildData = value;
      console.log(this.ChildData);
    },
    showHistory() {
      this.show = true;
    }
  }
};
</script>
<style>
:root {
  .app-main {
    flex: 1;
    padding: 2.5rem 0;
    background-color: var(--background);
    width: 100%;
    max-width: 100%;
    overflow: hidden;
  }

  /* Base colors */
  --color-primary: #4f46e5;
  --color-primary-hover: #4338ca;
  --color-secondary: #10b981;
  --color-danger: #ef4444;
  --color-warning: #f59e0b;
  --color-success: #10b981;
  --color-info: #3b82f6;

  /* Background colors */
  --bg-primary: #f8fafc;
  --bg-secondary: #f1f5f9;
  --bg-tertiary: #e2e8f0;
  --bg-card: #ffffff;

  /* Text colors */
  --text-primary: #1e293b;
  --text-secondary: #475569;
  --text-muted: #64748b;
  --text-light: #94a3b8;
  --text-on-primary: #ffffff;

  /* Border colors */
  --border-light: #e2e8f0;
  --border-medium: #cbd5e1;
  --border-dark: #94a3b8;

  /* Shadow */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);

  /* Border radius */
  --radius-sm: 0.25rem;
  --radius: 0.5rem;
  --radius-md: 0.75rem;
  --radius-lg: 1rem;
  --radius-full: 9999px;

  /* Transitions */
  --transition: all 0.2s ease-in-out;

  /* Spacing */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-5: 1.25rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-20: 5rem;
  --space-24: 6rem;
  --space-32: 8rem;
  --space-40: 10rem;
  --space-48: 12rem;
  --space-56: 14rem;
  --space-64: 16rem;

  /* Typography */
  --font-sans: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, sans-serif;
  --font-mono: "Fira Code", "SFMono-Regular", Menlo, Monaco, Consolas,
    "Liberation Mono", "Courier New", monospace;

  /* Z-index */
  --z-dropdown: 1000;
  --z-sticky: 1020;
  --z-fixed: 1030;
  --z-modal-backdrop: 1040;
  --z-offcanvas: 1050;
  --z-modal: 1060;
  --z-popover: 1070;
  --z-tooltip: 1080;

  /* Container */
  --container-sm: 640px;
  --container-md: 768px;
  --container-lg: 1024px;
  --container-xl: 1280px;
  --container-2xl: 1536px;
  --primary: #6366f1;
  --primary-hover: #4f46e5;
  --background: #f1f5f9;
  --card-bg: #ffffff;
  --header-bg: #ffffff;
  --footer-bg: #f8fafc;
  --text: #1e293b;
  --text-light: #64748b;
  --border: #e2e8f0;
  --radius: 0.75rem;
  --shadow: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --header-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, sans-serif;
  background-color: var(--background);
  color: var(--text);
  line-height: 1.5;
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Header Styles */
.app-header {
  background-color: var(--bg-card);
  box-shadow: var(--shadow-sm);
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: var(--z-sticky);
  border-bottom: 1px solid var(--border-light);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-6);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  color: var(--color-primary);
  text-decoration: none;
  transition: var(--transition);
}

.logo:hover {
  opacity: 0.9;
}

.logo svg {
  width: 1.75rem;
  height: 1.75rem;
  flex-shrink: 0;
}

.app-title {
  font-size: 1.375rem;
  font-weight: 700;
  margin: 0;
  background: linear-gradient(135deg, var(--color-primary), var(--color-info));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  letter-spacing: -0.025em;
  line-height: 1.2;
}

.nav-buttons {
  display: flex;
  gap: var(--space-3);
  align-items: center;
}

.nav-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius);
  font-weight: 500;
  font-size: 0.9375rem;
  color: var(--text-secondary);
  background: transparent;
  border: 1px solid transparent;
  cursor: pointer;
  transition: var(--transition);
  white-space: nowrap;
  text-decoration: none;
}

.nav-button:hover {
  background-color: var(--bg-secondary);
  color: var(--text-primary);
  transform: translateY(-1px);
  box-shadow: var(--shadow-sm);
}

.nav-button.active {
  background-color: rgba(79, 70, 229, 0.1);
  color: var(--color-primary);
  font-weight: 600;
}

.nav-button svg {
  width: 1.25rem;
  height: 1.25rem;
  flex-shrink: 0;
}

/* Main Content */
.app-main {
  flex: 1;
  padding: 2.5rem 0;
  background-color: var(--background);
  width: 100%;
  max-width: 100%;
  overflow: hidden;
}

.container {
  width: 100%;
  margin: 0 auto;
  padding: 0 var(--space-6);
  max-width: var(--container-xl);
}

/* Responsive container */
@media (min-width: 640px) {
  .container {
    max-width: var(--container-sm);
  }
}

@media (min-width: 768px) {
  .container {
    max-width: var(--container-md);
  }
}

@media (min-width: 1024px) {
  .container {
    max-width: var(--container-lg);
  }
}

@media (min-width: 1280px) {
  .container {
    max-width: var(--container-xl);
  }
}

@media (min-width: 1536px) {
  .container {
    max-width: var(--container-2xl);
  }
}

/* Utility classes */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* Focus styles */
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}

/* Buttons */
button {
  font-family: inherit;
  font-size: 100%;
  line-height: inherit;
  margin: 0;
  padding: 0;
  text-transform: none;
  -webkit-appearance: button;
  background-color: transparent;
  background-image: none;
  cursor: pointer;
  border: none;
}

button:focus {
  outline: 2px solid transparent;
  outline-offset: 2px;
}

/* Links */
a {
  color: var(--color-primary);
  text-decoration: none;
  transition: var(--transition);
}

a:hover {
  color: var(--color-primary-hover);
  text-decoration: underline;
}

/* Footer Styles */
.app-footer {
  background-color: var(--footer-bg);
  border-top: 1px solid var(--border-light);
  padding: 1.5rem 0;
  margin-top: auto;
  flex-shrink: 0;
  width: 100%;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.app-footer p {
  color: var(--text-light);
  font-size: 0.875rem;
  margin: 0;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-link {
  color: var(--text-light);
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  background-color: var(--card-bg);
  box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
}

.social-link:hover {
  color: var(--primary);
  transform: translateY(-1px);
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
}

.social-link svg {
  width: 1.25rem;
  height: 1.25rem;
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Responsive Design */
@media (max-width: 768px) {
  .header-content {
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
  }

  .app-title {
    font-size: 1.25rem;
  }

  .nav-buttons {
    width: 100%;
    justify-content: center;
  }

  .footer-content {
    flex-direction: column;
    text-align: center;
    gap: 0.5rem;
  }
}
</style>
