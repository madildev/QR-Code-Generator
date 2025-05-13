<template>
  <div class="home">
    <div class="card">
      <h1 class="title">QR Code Generator</h1>
      <p class="subtitle">Generate QR codes for any URL or text</p>

      <div class="input-group">
        <input
          type="url"
          v-model.trim="url"
          @keydown.enter.prevent="getSource"
          @keyup="toParent"
          placeholder="Enter URL to generate QR code..."
          class="url-input"
          required
        />
        <button @click="getSource" class="generate-btn">
          <span>Generate</span>
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
              d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h7"
            />
            <path d="M16 8 5 19" />
            <path d="M21 3h-6" />
            <path d="M21 3v6" />
          </svg>
        </button>
      </div>

      <div v-if="img_src" class="qr-code-container" @click="toggleCodeSnippet">
        <div class="qr-code">
          <img :src="img_src" alt="QR Code" />
          <div class="qr-overlay">
            <span class="show-code-text">Click for the code</span>
          </div>
        </div>
      </div>

      <div v-if="img_src && showCodeSnippet" class="code-snippet">
        <div class="snippet-header">
          <span>HTML Code</span>
          <button @click="copyToClipboard" class="copy-btn">
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
              <rect x="9" y="9" width="13" height="13" rx="2" ry="2" />
              <path
                d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"
              />
            </svg>
            {{ copySuccess ? "Copied!" : "Copy" }}
          </button>
        </div>
        <pre class="code-block">
<code>&lt;img src="{{ img_src }}" alt="QR Code" /&gt;</code>
        </pre>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "Home",
      base_url:
        "https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=",
      url: "",
      img_src: "",
      values: [],
      show: false,
      copySuccess: false,
      showCodeSnippet: false
    };
  },
  methods: {
    // This function handles the QR code generation
    getSource(e) {
      if (e) e.preventDefault();
      if (this.url.trim()) {
        this.img_src = `${this.base_url}${encodeURIComponent(this.url.trim())}`;
        this.values.push(this.url);
        this.show = false;
      }
    },
    // This function is a emits an event "childtoparent" and send data
    toParent() {
      this.$emit("childtoparent", this.values);
    },
    // Toggle display of the code snippet
    toggleCodeSnippet() {
      this.showCodeSnippet = !this.showCodeSnippet;
    },

    display() {
      this.show = !this.show;
    },
    // Copy code to clipboard
    async copyToClipboard() {
      try {
        await navigator.clipboard.writeText(
          `<img src="${this.img_src}" alt="QR Code" />`
        );
        this.copySuccess = true;
        setTimeout(() => {
          this.copySuccess = false;
        }, 2000);
      } catch (err) {
        console.error("Failed to copy text: ", err);
      }
    }
  }
};
</script>

<style scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  width: 100%;
  padding: 1rem;
  background-color: var(--background);
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

.input-group {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin: 0.5rem 0 1rem 0;
  width: 100%;
  max-width: 100%;
}

.url-input {
  flex: 1;
  min-width: 0;
  padding: 0.75rem 1rem;
  font-size: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 0.5rem;
  background: white;
  color: #1e293b;
  transition: border-color 0.2s ease;
}

.url-input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2);
}

.generate-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 0.5rem;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.generate-btn:hover {
  background: #2563eb;
  transform: translateY(-1px);
}

.generate-btn:active {
  transform: translateY(0);
}

.qr-code-container {
  position: relative;
  width: 100%;
  max-width: 200px;
  margin: 0 auto 1rem;
  cursor: pointer;
}

.qr-code {
  position: relative;
  width: 100%;
  transition: all 0.3s ease;
  border-radius: 0.5rem;
  overflow: hidden;
}

.qr-code img {
  max-width: 100%;
  height: auto;
  display: block;
  transition: all 0.3s ease;
}

.qr-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  border-radius: 0.5rem;
}

.qr-code:hover .qr-overlay {
  opacity: 1;
}

.qr-code:hover img {
  filter: brightness(0.7);
}

.show-code-text {
  background: #3b82f6;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.code-snippet {
  background: #1e293b;
  border-radius: var(--radius);
  overflow: hidden;
  margin: 1rem 0 0;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  max-width: 100%;
  box-sizing: border-box;
}

.snippet-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  background: #0f172a;
  color: #94a3b8;
  font-size: 0.8125rem;
  font-weight: 500;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.copy-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.1);
  color: #e2e8f0;
  border: none;
  border-radius: 0.375rem;
  padding: 0.375rem 0.75rem;
  font-size: 0.75rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.copy-btn:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-1px);
}

.copy-btn:active {
  transform: translateY(0);
}

.code-block {
  padding: 1rem;
  margin: 0;
  overflow-x: auto;
  font-family: "Fira Code", "SFMono-Regular", Menlo, Monaco, Consolas,
    "Liberation Mono", "Courier New", monospace;
  font-size: 0.875rem;
  line-height: 1.5;
  color: #e2e8f0;
}

.code-block code {
  white-space: pre;
}

.title {
  font-size: 1.75rem;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 0.25rem 0;
  text-align: center;
  background: linear-gradient(135deg, var(--color-primary), var(--color-info));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  line-height: 1.2;
}

.subtitle {
  font-size: 1rem;
  color: var(--text-light);
  margin: 0 0 1rem 0;
  text-align: center;
}

/* Responsive adjustments */
@media (max-width: 640px) {
  .card {
    padding: 1.25rem;
    margin: 0;
    border-radius: 0;
    border-left: none;
    border-right: none;
    min-height: calc(100vh - 180px);
  }

  .title {
    font-size: 1.5rem;
    margin-top: 0;
  }

  .subtitle {
    font-size: 1rem;
    margin-bottom: 1rem;
  }

  .input-group {
    flex-direction: column;
    gap: 0.5rem;
  }

  .url-input {
    width: 100%;
  }

  .generate-btn {
    width: 100%;
    justify-content: center;
  }

  .qr-code {
    margin: 1rem 0;
  }

  .code-snippet {
    margin: 1rem 0 0;
  }
}
</style>
