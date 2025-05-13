<template>
  <div class="history">
    <div class="card">
      <h1 class="title">QR Code History</h1>
      <p class="subtitle" v-if="!check">
        Your generated links will appear here
      </p>

      <div v-if="!check" class="history-list">
        <div class="history-header">
          <span class="url-heading">URL</span>
          <span class="actions-heading">Actions</span>
        </div>
        <div class="history-items">
          <div v-for="(data, index) in Data" :key="index" class="history-item">
            <div class="url-text">{{ truncateText(data) }}</div>
            <div class="actions">
              <div class="tooltip-container">
                <button
                  @click.stop="copyToClipboard(data, index)"
                  class="action-btn"
                  :title="copiedIndex === index ? 'Copied!' : 'Copy URL'"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <rect
                      x="9"
                      y="9"
                      width="13"
                      height="13"
                      rx="2"
                      ry="2"
                    ></rect>
                    <path
                      d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"
                    ></path>
                  </svg>
                </button>
                <span v-if="copiedIndex === index" class="tooltip"
                  >Copied!</span
                >
              </div>
              <button
                @click="removeHistory(data)"
                class="action-btn delete"
                title="Remove"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="3 6 5 6 21 6"></polyline>
                  <path
                    d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                  ></path>
                </svg>
              </button>
            </div>
          </div>
        </div>
        <button @click="DeleteHistory" class="clear-btn">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polyline points="3 6 5 6 21 6"></polyline>
            <path
              d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
            ></path>
          </svg>
          Clear All History
        </button>
      </div>

      <div v-else class="empty-state">
        <div class="empty-icon">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="48"
            height="48"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path
              d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"
            ></path>
          </svg>
        </div>
        <h2>No History Yet</h2>
        <p>Your generated QR codes will appear here</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Data: [],
      copiedIndex: null,
      copyTimeout: null
    };
  },
  props: {
    Sent: {
      type: Array,
      default: () => []
    }
  },
  // This is a life cycle hook(triggers before Component is rendered) that loads the data from the local storage
  beforeMount() {
    this.loadHistory();
    if (this.Sent && this.Sent.length) {
      this.Data = [...new Set([...this.Data, ...this.Sent])];
      this.Sent.splice(0, this.Sent.length);
      this.saveHistory();
    }
  },
  // This is a life cycle hook(triggers before the Component is removed from the DOM) it saves the history of the QR code
  beforeDestroy() {
    this.saveHistory();
  },
  computed: {
    // This checks whether the data sent by parent is empty
    check() {
      return this.Data.length === 0;
    }
  },
  methods: {
    // Truncate long URLs for display
    truncateText(text) {
      const maxLength = 50;
      return text.length > maxLength
        ? `${text.substring(0, maxLength)}...`
        : text;
    },
    // Copy URL to clipboard
    copyToClipboard(text, index) {
      navigator.clipboard
        .writeText(text)
        .then(() => {
          // Clear any existing timeout
          if (this.copyTimeout) {
            clearTimeout(this.copyTimeout);
          }

          // Set the copied index and reset after 2 seconds
          this.copiedIndex = index;
          this.copyTimeout = setTimeout(() => {
            this.copiedIndex = null;
          }, 2000);
        })
        .catch(err => {
          console.error("Failed to copy text:", err);
        });
    },
    // This Function handles the local storage
    loadHistory() {
      if (localStorage.getItem("Data")) {
        try {
          const savedData = JSON.parse(localStorage.getItem("Data"));
          this.Data = Array.isArray(savedData) ? savedData : [];
        } catch (e) {
          // Error loading history
          localStorage.removeItem("Data");
        }
      }
    },
    // This function removes the History item from the array and local storage
    removeHistory(url) {
      const index = this.Data.indexOf(url);
      if (index > -1) {
        this.Data.splice(index, 1);
        this.saveHistory();
      }
    },
    // This function saves the data into the local storage
    saveHistory() {
      const parsed = JSON.stringify(this.Data);
      localStorage.setItem("Data", parsed);
    },
    // This function deletes all the history from the array and local Storage
    DeleteHistory() {
      if (this.Data.length > 0) {
        this.Data = [];
        localStorage.removeItem("Data");
      }
    }
  }
};
</script>

<style scoped>
.history {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  width: 100%;
  padding: 1rem;
  min-height: 0;
  flex: 1 1 auto;
  overflow-y: auto;
}

.card {
  background: var(--card-bg);
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow);
  transition: all 0.3s ease;
  margin: 1.5rem 0;
  padding: 1.5rem;
  width: 100%;
  max-width: 600px;
  min-height: 280px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  gap: 1rem;
}

.title {
  color: var(--text-primary);
  font-size: 1.75rem;
  font-weight: 700;
  margin: 0 0 0.25rem;
  text-align: center;
  background: linear-gradient(135deg, var(--color-primary), var(--color-info));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  line-height: 1.2;
}

.subtitle {
  color: var(--text-secondary);
  font-size: 1rem;
  text-align: center;
  margin: 0 0 1rem;
}

.history-list {
  width: 100%;
}

.history-header {
  display: grid;
  grid-template-columns: 1fr auto;
  padding: 0.75rem 1rem;
  background: var(--bg-secondary);
  border-radius: 0.5rem 0.5rem 0 0;
  font-weight: 600;
  color: var(--text-secondary);
  font-size: 0.875rem;
}

.history-items {
  max-height: 500px;
  overflow-y: auto;
  border: 1px solid var(--border);
  border-top: none;
  border-radius: 0 0 0.5rem 0.5rem;
}

.history-item {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid var(--border);
  transition: background-color 0.2s ease;
}

.history-item:last-child {
  border-bottom: none;
}

/* Hover effect removed for better visual distinction from header */

.url-text {
  color: var(--text-primary);
  font-size: 0.9375rem;
  word-break: break-all;
  padding-right: 1rem;
}

.actions {
  display: flex;
  gap: 0.5rem;
}

.action-btn {
  background: none;
  border: none;
  color: var(--text-secondary);
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 0.25rem;
  transition: all 0.2s ease;
}

.action-btn:hover {
  background: var(--bg-secondary);
  color: var(--text-primary);
}

.action-btn.delete:hover {
  color: #ef4444;
  border-color: #fecaca;
  background: #fef2f2;
}

.clear-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-top: 1.5rem;
  padding: 0.5rem 1rem;
  background: #fef2f2;
  color: #dc2626;
  border: 1px solid #fecaca;
  border-radius: 0.375rem;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-left: auto;
}

.clear-btn:hover {
  background: #fee2e2;
}

.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.5rem 1rem;
  text-align: center;
}

.empty-icon {
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-secondary);
  margin-bottom: 1.5rem;
  opacity: 0.7;
}

.empty-state h2 {
  color: var(--text-primary);
  font-size: 1.25rem;
  margin: 0 0 0.5rem;
}

.empty-state p {
  color: var(--text-secondary);
  margin: 0;
}

/* Responsive styles */
@media (max-width: 640px) {
  .card {
    padding: 1.25rem;
    margin: 0;
    border-radius: 0;
    border-left: none;
    border-right: none;
  }

  .title {
    font-size: 1.5rem;
  }

  .history-header {
    display: none;
  }

  .history-item {
    grid-template-columns: 1fr;
    gap: 0.75rem;
  }

  .actions {
    justify-content: flex-end;
  }
}
</style>
