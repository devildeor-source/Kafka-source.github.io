<script>
    const chatArea = document.querySelector('section');
    const textarea = document.querySelector('textarea');
    const sendBtn = document.querySelector('button');

    async function sendMessage() {
        const msg = textarea.value.trim();
        if (!msg) return;

        // Add user message to UI
        appendMessage(msg, 'user');
        textarea.value = '';

        // Send to Python Backend
        const response = await fetch('/chat', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ message: msg })
        });
        
        const data = await response.json();
        
        // Add AI response to UI
        appendMessage(data.response, 'ai');
    }

    function appendMessage(text, sender) {
        const div = document.createElement('div');
        div.className = sender === 'user' ? 'flex gap-4 max-w-3xl ml-auto flex-row-reverse' : 'flex gap-4 max-w-3xl';
        const color = sender === 'user' ? 'bg-indigo-600' : 'glass';
        const icon = sender === 'user' ? 'U' : '<i class="fa-solid fa-robot"></i>';
        
        div.innerHTML = `
            <div class="w-8 h-8 rounded flex-shrink-0 flex items-center justify-center ${sender === 'user' ? 'bg-gray-600' : 'bg-indigo-500'}">${icon}</div>
            <div class="${color} p-4 rounded-2xl ${sender === 'user' ? 'rounded-tr-none text-white' : 'rounded-tl-none'}">${text}</div>
        `;
        chatArea.appendChild(div);
        chatArea.scrollTop = chatArea.scrollHeight; // Auto-scroll
    }

    sendBtn.addEventListener('click', sendMessage);
</script>
