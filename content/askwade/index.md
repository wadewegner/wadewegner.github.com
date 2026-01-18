---
title: "Ask Wade"
draft: false
weight: 0
noComment: true
---

<style>
.chat-container {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 300px);
  min-height: 500px;
  max-width: 800px;
  margin: 0 auto;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  overflow: hidden;
  background: #fafafa;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.message {
  display: flex;
  gap: 12px;
  max-width: 85%;
  animation: fadeIn 0.3s ease-in-out;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.message.user {
  align-self: flex-end;
  flex-direction: row-reverse;
}

.message.assistant {
  align-self: flex-start;
}

.message-avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 14px;
  flex-shrink: 0;
}

.message.user .message-avatar {
  background: #4a4a4a;
  color: white;
}

.message.assistant .message-avatar {
  background: #00b8d4;
  color: white;
}

.message-content {
  padding: 12px 16px;
  border-radius: 18px;
  line-height: 1.5;
  font-size: 15px;
}

.message.user .message-content {
  background: #4a4a4a;
  color: white;
  border-bottom-right-radius: 4px;
}

.message.assistant .message-content {
  background: white;
  color: #363636;
  border: 1px solid #e0e0e0;
  border-bottom-left-radius: 4px;
}

.message-content p {
  margin: 0 0 8px 0;
}

.message-content p:last-child {
  margin-bottom: 0;
}

.message-content pre {
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 12px;
  border-radius: 8px;
  overflow-x: auto;
  margin: 8px 0;
}

.message-content code {
  font-family: 'SF Mono', Monaco, 'Cascadia Code', monospace;
  font-size: 13px;
}

.message-content code:not(pre code) {
  background: rgba(0,0,0,0.08);
  padding: 2px 6px;
  border-radius: 4px;
}

.message.user .message-content code:not(pre code) {
  background: rgba(255,255,255,0.2);
}

.chat-input-container {
  padding: 16px 24px;
  background: white;
  border-top: 1px solid #e0e0e0;
}

.chat-input-wrapper {
  display: flex;
  gap: 12px;
  align-items: flex-end;
}

.chat-input {
  flex: 1;
  border: 1px solid #e0e0e0;
  border-radius: 24px;
  padding: 12px 20px;
  font-size: 15px;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
  resize: none;
  min-height: 48px;
  max-height: 150px;
  font-family: inherit;
  line-height: 1.5;
}

.chat-input:focus {
  border-color: #00b8d4;
  box-shadow: 0 0 0 2px rgba(0, 184, 212, 0.1);
}

.chat-input::placeholder {
  color: #999;
}

.send-button {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #00b8d4;
  color: white;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s, transform 0.1s;
  flex-shrink: 0;
}

.send-button:hover:not(:disabled) {
  background: #0097af;
}

.send-button:active:not(:disabled) {
  transform: scale(0.95);
}

.send-button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.send-button svg {
  width: 20px;
  height: 20px;
}

.typing-indicator {
  display: flex;
  gap: 4px;
  padding: 8px 0;
}

.typing-indicator span {
  width: 8px;
  height: 8px;
  background: #999;
  border-radius: 50%;
  animation: bounce 1.4s infinite ease-in-out;
}

.typing-indicator span:nth-child(1) { animation-delay: -0.32s; }
.typing-indicator span:nth-child(2) { animation-delay: -0.16s; }
.typing-indicator span:nth-child(3) { animation-delay: 0s; }

@keyframes bounce {
  0%, 80%, 100% { transform: scale(0); }
  40% { transform: scale(1); }
}

.welcome-message {
  text-align: center;
  color: #666;
  padding: 40px 20px;
}

.welcome-message h2 {
  font-size: 24px;
  margin-bottom: 8px;
  color: #363636;
}

.welcome-message p {
  font-size: 15px;
  margin: 0;
}

.error-message {
  color: #cc0000;
  font-size: 14px;
  padding: 8px 12px;
  background: #fff0f0;
  border-radius: 8px;
  margin-top: 8px;
}

@media (max-width: 768px) {
  .chat-container {
    height: calc(100vh - 250px);
    min-height: 400px;
    border-radius: 0;
    border-left: none;
    border-right: none;
  }
  
  .chat-messages {
    padding: 16px;
  }
  
  .message {
    max-width: 90%;
  }
  
  .chat-input-container {
    padding: 12px 16px;
  }
}
</style>

<div class="chat-container">
  <div class="chat-messages" id="chatMessages">
    <div class="welcome-message">
      <h2>Hi, I'm Wade's AI assistant</h2>
      <p>Ask me anything about Wade, his work, or his interests.</p>
    </div>
  </div>
  <div class="chat-input-container">
    <div class="chat-input-wrapper">
      <textarea 
        class="chat-input" 
        id="chatInput" 
        placeholder="Type your message..." 
        rows="1"
      ></textarea>
      <button class="send-button" id="sendButton" aria-label="Send message">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <line x1="22" y1="2" x2="11" y2="13"></line>
          <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
        </svg>
      </button>
    </div>
    <div id="errorContainer"></div>
  </div>
</div>

{{< rawhtml >}}
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
<script>
(function() {
  var chatMessages = document.getElementById('chatMessages');
  var chatInput = document.getElementById('chatInput');
  var sendButton = document.getElementById('sendButton');
  var errorContainer = document.getElementById('errorContainer');
  
  var isLoading = false;
  var conversationHistory = [];

  chatInput.addEventListener('input', function() {
    this.style.height = 'auto';
    this.style.height = Math.min(this.scrollHeight, 150) + 'px';
  });

  chatInput.addEventListener('keydown', function(e) {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  });

  sendButton.addEventListener('click', sendMessage);

  function escapeHtml(text) {
    var div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }

  function formatMessage(text) {
    // Remove surrounding quotes if present
    if (text.charAt(0) === '"' && text.charAt(text.length - 1) === '"') {
      text = text.slice(1, -1);
    }
    // Convert escaped characters
    text = text.replace(/\\n/g, '\n');
    text = text.replace(/\\"/g, '"');
    text = text.replace(/\\'/g, "'");
    text = text.replace(/\\\\/g, '\\');
    
    // Use marked.js to parse markdown
    return marked.parse(text);
  }

  function addMessage(content, isUser) {
    var welcome = chatMessages.querySelector('.welcome-message');
    if (welcome) welcome.remove();

    var messageDiv = document.createElement('div');
    messageDiv.className = 'message ' + (isUser ? 'user' : 'assistant');
    
    var avatar = document.createElement('div');
    avatar.className = 'message-avatar';
    avatar.textContent = isUser ? 'You' : 'W';
    
    var contentDiv = document.createElement('div');
    contentDiv.className = 'message-content';
    contentDiv.innerHTML = isUser ? escapeHtml(content) : formatMessage(content);
    
    messageDiv.appendChild(avatar);
    messageDiv.appendChild(contentDiv);
    chatMessages.appendChild(messageDiv);
    
    scrollToBottom();
    return messageDiv;
  }

  function addTypingIndicator() {
    var messageDiv = document.createElement('div');
    messageDiv.className = 'message assistant';
    messageDiv.id = 'typingIndicator';
    
    var avatar = document.createElement('div');
    avatar.className = 'message-avatar';
    avatar.textContent = 'W';
    
    var contentDiv = document.createElement('div');
    contentDiv.className = 'message-content';
    
    var indicator = document.createElement('div');
    indicator.className = 'typing-indicator';
    for (var i = 0; i < 3; i++) {
      indicator.appendChild(document.createElement('span'));
    }
    contentDiv.appendChild(indicator);
    
    messageDiv.appendChild(avatar);
    messageDiv.appendChild(contentDiv);
    chatMessages.appendChild(messageDiv);
    
    scrollToBottom();
  }

  function removeTypingIndicator() {
    var indicator = document.getElementById('typingIndicator');
    if (indicator) indicator.remove();
  }

  function scrollToBottom() {
    chatMessages.scrollTop = chatMessages.scrollHeight;
  }

  function showError(message) {
    var errorDiv = document.createElement('div');
    errorDiv.className = 'error-message';
    errorDiv.textContent = message;
    errorContainer.innerHTML = '';
    errorContainer.appendChild(errorDiv);
    setTimeout(function() {
      errorContainer.innerHTML = '';
    }, 5000);
  }

  function setLoading(loading) {
    isLoading = loading;
    sendButton.disabled = loading;
    chatInput.disabled = loading;
  }

  function buildPromptWithHistory(newMessage) {
    var prompt = '';
    for (var i = 0; i < conversationHistory.length; i++) {
      var msg = conversationHistory[i];
      if (msg.role === 'user') {
        prompt += 'User: ' + msg.content + '\n\n';
      } else {
        prompt += 'Assistant: ' + msg.content + '\n\n';
      }
    }
    prompt += 'User: ' + newMessage;
    return prompt;
  }

  function sendMessage() {
    var message = chatInput.value.trim();
    if (!message || isLoading) return;

    chatInput.value = '';
    chatInput.style.height = 'auto';
    errorContainer.innerHTML = '';

    addMessage(message, true);

    setLoading(true);
    addTypingIndicator();

    var fullPrompt = buildPromptWithHistory(message);
    conversationHistory.push({ role: 'user', content: message });

    fetch('/.netlify/functions/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ prompt: fullPrompt })
    })
    .then(function(response) {
      removeTypingIndicator();
      if (!response.ok) throw new Error('Failed to get response');
      return response.json();
    })
    .then(function(data) {
      var assistantMessage;
      if (typeof data === 'string') {
        assistantMessage = data;
      } else {
        assistantMessage = data.response || data.message || data.output || data.text || data.content || String(data);
      }
      conversationHistory.push({ role: 'assistant', content: assistantMessage });
      addMessage(assistantMessage, false);
    })
    .catch(function(error) {
      removeTypingIndicator();
      showError('Sorry, something went wrong. Please try again.');
      console.error('Chat error:', error);
    })
    .finally(function() {
      setLoading(false);
      chatInput.focus();
    });
  }

  chatInput.focus();
})();
</script>
{{< /rawhtml >}}
