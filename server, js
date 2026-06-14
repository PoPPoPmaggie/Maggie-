const http = require('http');
const fs = require('fs');
const path = require('path');

const CLAUDE_KEY = "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";

const SYS = `You are Maggie, an elite AI business partner. You grew up in the hood but you're the smartest person in any room. You talk real, a little ghetto, a little street, but SHARP. Your boss is Eric. You work for him 24/7. Expert in business strategy, crypto, stocks, digital products, marketing, 150+ revenue streams. Talk like a smart friend from the block who made it. Say things like: on God, no cap, we finna eat, bet, that's a bag move. Always push toward money and next step. SHORT responses 3-5 sentences. No bullet points, no symbols. Sound like TALKING. Always end pushing to next action. Building million dollar operation from zero with Eric.`;

const server = http.createServer(async (req, res) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  if (req.method === 'OPTIONS') { res.writeHead(200); res.end(); return; }

  if (req.method === 'GET' && (req.url === '/' || req.url === '/index.html')) {
    const file = fs.readFileSync(path.join(__dirname, 'index.html'));
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end(file);
    return;
  }

  if (req.method === 'POST' && req.url === '/chat') {
    let body = '';
    req.on('data', chunk => body += chunk);
    req.on('end', async () => {
      try {
        const { messages } = JSON.parse(body);
        const response = await fetch('https://api.anthropic.com/v1/messages', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-api-key': CLAUDE_KEY,
            'anthropic-version': '2023-06-01'
          },
          body: JSON.stringify({ model: 'claude-haiku-4-5-20251001', max_tokens: 300, system: SYS, messages })
        });
        const data = await response.json();
        res.writeHead(200, {'Content-Type': 'application/json'});
        res.end(JSON.stringify(data));
      } catch(e) {
        res.writeHead(500);
        res.end(JSON.stringify({error: e.message}));
      }
    });
    return;
  }

  res.writeHead(404);
  res.end('Not found');
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`Maggie running on port ${PORT}`));

