# Bundle del Proyecto para IA: minipet

## Estructura de Archivos
- borrar/minigames.js
- borrar/pet.js
- borrar/store.js
- borrar/styles.css
- css/base.css
- css/escenarios.css
- css/juegos.css
- css/mascota.css
- data/dialogues.json
- desarrollo/CONTEXTO-PROYECTO.md
- desarrollo/escenarios-demo.html
- desarrollo/escenarios-demo_3.html
- desarrollo/prueba-accesorios-visual.html
- documentacion/LEEME.md
- img/accessories/bow_tie.svg
- img/accessories/bunny_ears.svg
- img/accessories/cat_ears.svg
- img/accessories/chef_hat.svg
- img/accessories/fairy_wings.svg
- img/accessories/flower_pink.svg
- img/accessories/glasses_cool.svg
- img/accessories/hat_crown.svg
- img/accessories/headphones.svg
- img/accessories/magic_wand.svg
- img/accessories/ninja_band.svg
- img/accessories/party_hat.svg
- img/accessories/pirate_hat.svg
- img/accessories/ribbon_pink.svg
- img/accessories/shower_cap.svg
- img/accessories/snorkel.svg
- img/accessories/space_helmet.svg
- img/accessories/star_glasses.svg
- img/accessories/winter_beanie.svg
- img/accessories/winter_scarf.svg
- img/accessories/wizard_hat.svg
- index.html
- js/app.js
- js/catalogo.js
- js/juegos/atrapa-dulces.js
- js/juegos/burbujas-magicas.js
- js/juegos/explota-globos.js
- js/juegos/juegos-base.js
- js/juegos/runner-magico.js
- js/juegos/simon-memoria.js
- js/juegos/torre-postres.js
- js/mascota-estado.js
- js/mascota-visual.js
- js/particulas.js
- js/tienda.js
- manifest.json
- readme.md
- sw.js

---

## Código Fuente

### Archivo: `borrar/minigames.js`
```javascript
const Minigames = {
  // Estado de un minijuego en curso. Si el usuario cierra el modal o
  // navega a otra pestaña mientras `inProgress` es true, `exitHandler()`
  // se ejecuta para limpiar listeners/timers y devolver las monedas
  // ganadas hasta ese momento, en vez de perderlas en silencio.
  inProgress: false,
  exitHandler: null,

  forceExitIfActive() {
    if (this.inProgress && this.exitHandler) {
      const earned = this.exitHandler();
      this.inProgress = false;
      this.exitHandler = null;
      if (earned > 0) {
        PetState.addCoins(earned);
      }
    }
  },

  // Menú Principal de Minijuegos (6 Opciones Kawaii)
 renderMenu() {
  const content = document.getElementById('modal-content');
  content.innerHTML = `
    <h2 style="text-align:center; color:#6a1b9a; margin-bottom:4px;">🎮 Minijuegos</h2>
    <p style="margin-bottom:14px; font-size:0.85rem; color:#7b1fa2; text-align:center; font-weight:600;">
      ¡Gana monedas y haz feliz a tu mascota!
    </p>
    <div class="grid-container">
      <div class="item-card" onclick="Minigames.startCatchGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🎈</span>
        <span class="card-title">Atrapa Dulces</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startPopGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🌸</span>
        <span class="card-title">Explotar Globos</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startCakeGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🍰</span>
        <span class="card-title">Torre de Postres</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startSimonGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🧠</span>
        <span class="card-title">Simón Memoria</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startRunnerGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🦄</span>
        <span class="card-title">Runner Mágico</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startBubbleGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">✨</span>
        <span class="card-title">Burbujas Mágicas</span>
        <button class="card-btn">Jugar</button>
      </div>
    </div>
  `;
},

  // Conteo Regresivo Estilo Kawaii
  showCountdown(onComplete) {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <div style="display:flex; flex-direction:column; align-items:center; justify-content:center; height:260px;">
        <div id="countdown-num" class="kawaii-countdown">3</div>
        <p id="countdown-sub" style="font-weight:bold; color:#ab47bc; margin-top:10px;">¡Prepárate! ✨</p>
      </div>
    `;

    let count = 3;
    AudioEffects.playTone(523, 'sine', 0.15);

    const interval = setInterval(() => {
      count--;
      const numEl = document.getElementById('countdown-num');
      const subEl = document.getElementById('countdown-sub');

      if (!numEl) {
        clearInterval(interval);
        return;
      }

      if (count > 0) {
        numEl.innerText = count;
        numEl.style.animation = 'none';
        numEl.offsetHeight;
        numEl.style.animation = 'popNum 0.6s ease-out';
        AudioEffects.playTone(523 + (3 - count) * 100, 'sine', 0.15);
      } else if (count === 0) {
        numEl.innerText = '¡A JUGAR! 🌸';
        numEl.style.fontSize = '2rem';
        subEl.innerText = '✨ ¡Diviértete! ✨';
        AudioEffects.playTone(880, 'triangle', 0.3);
      } else {
        clearInterval(interval);
        onComplete();
      }
    }, 900);
  },

  // Pantalla de Resultado Kawaii (reemplaza los alert() nativos)
  showResult(title, emoji, score, restartFn, isRecord = false) {
    // El juego terminó de forma natural: ya no hay nada que cobrar al salir
    this.inProgress = false;
    this.exitHandler = null;

    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <div class="result-screen">
        <div class="result-emoji">${emoji}</div>
        <h2 class="result-title">${title}</h2>
        ${isRecord ? '<div class="result-record">🏆 ¡Nuevo récord!</div>' : ''}
        <div class="result-coins-box">
          <span>🪙</span>
          <span id="result-coin-num">0</span>
        </div>
        <div class="result-actions">
          <button class="card-btn result-btn-again" id="result-again">🔁 Jugar de nuevo</button>
          <button class="card-btn result-btn-menu" id="result-menu">🎮 Menú</button>
        </div>
      </div>
    `;

    PetState.addCoins(score);

    // Animación de conteo de monedas ganadas
    const numEl = document.getElementById('result-coin-num');
    let current = 0;
    const step = Math.max(1, Math.ceil(score / 25));
    const counter = setInterval(() => {
      current = Math.min(score, current + step);
      numEl.innerText = current;
      if (current >= score) clearInterval(counter);
    }, 25);

    document.getElementById('result-again').onclick = () => restartFn();
    document.getElementById('result-menu').onclick = () => Minigames.renderMenu();
  },

  // =========================================================
  // MEJOR PUNTAJE POR JUEGO (persistido en localStorage)
  // =========================================================
  BEST_SCORES_KEY: 'minipetBestScores',

  getBestScore(gameId) {
    try {
      const data = JSON.parse(localStorage.getItem(this.BEST_SCORES_KEY) || '{}');
      return data[gameId] || 0;
    } catch (e) {
      return 0;
    }
  },

  // Guarda el puntaje si es un nuevo récord. Devuelve true si lo fue.
  saveBestScore(gameId, score) {
    try {
      const data = JSON.parse(localStorage.getItem(this.BEST_SCORES_KEY) || '{}');
      if (score > (data[gameId] || 0)) {
        data[gameId] = score;
        localStorage.setItem(this.BEST_SCORES_KEY, JSON.stringify(data));
        return true;
      }
    } catch (e) {
      // Si localStorage falla, seguimos sin romper el juego
    }
    return false;
  },

  // =========================================================
  // HUD REUTILIZABLE (marcador + mejor puntaje + tiempo opcional)
  // =========================================================
  renderHUD(icon, best, timeLeft) {
    return `
      <div class="game-hud">
        <span class="hud-chip">${icon} <span id="hud-score">0</span></span>
        <span class="hud-chip hud-best">⭐ <span id="hud-best">${best}</span></span>
        ${timeLeft !== undefined ? `<span class="hud-chip hud-timer">⏰ <span id="hud-timer">${timeLeft}</span>s</span>` : ''}
      </div>
    `;
  },

  updateHudScore(score) {
    const el = document.getElementById('hud-score');
    if (el) el.innerText = score;
  },

  updateHudTimer(t) {
    const el = document.getElementById('hud-timer');
    if (el) el.innerText = t;
  },

  // Factor de escala entre la resolución lógica del canvas (width/height)
  // y su tamaño real en pantalla (que ahora es responsive vía CSS).
  // Necesario para que los toques/clics caigan en el punto correcto
  // sin importar qué tan grande o chico se vea el canvas.
  getCanvasScale(canvas) {
    const rect = canvas.getBoundingClientRect();
    return {
      x: canvas.width / rect.width,
      y: canvas.height / rect.height
    };
  },

  // =========================================================
  // 1. ATRAPA DULCES KAWAII (Soporte Táctil)
  // =========================================================
  startCatchGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('catch');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🎈 Atrapa Dulces</h3>
        ${Minigames.renderHUD('🍬', best)}
        <canvas id="gameCanvas" width="300" height="300" style="background:linear-gradient(180deg, #e1f5fe 0%, #f3e5f5 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ce93d8; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Arrastra el dedo o usa ⬅️ ➡️ para moverte</p>
      `;

      const canvas = document.getElementById('gameCanvas');
      const ctx = canvas.getContext('2d');
      const CANDY_EMOJIS = ['🍬', '🍭', '🍡', '🍫'];
      let basketX = 120;
      let itemX = Math.random() * 260 + 10;
      let itemY = 0;
      let currentCandy = CANDY_EMOJIS[Math.floor(Math.random() * CANDY_EMOJIS.length)];
      let score = 0;
      let gameOver = false;

      // Teclado
      const keys = {};
      const handleKeyDown = (e) => keys[e.key] = true;
      const handleKeyUp = (e) => keys[e.key] = false;
      window.addEventListener('keydown', handleKeyDown);
      window.addEventListener('keyup', handleKeyUp);

      // Controles Táctiles (Móvil / Tablet)
      const handleTouch = (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const touchX = (e.touches[0].clientX - rect.left) * scale.x;
        basketX = Math.max(0, Math.min(240, touchX - 30));
      };
      canvas.addEventListener('touchstart', handleTouch, { passive: false });
      canvas.addEventListener('touchmove', handleTouch, { passive: false });

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        window.removeEventListener('keydown', handleKeyDown);
        window.removeEventListener('keyup', handleKeyUp);
        canvas.removeEventListener('touchstart', handleTouch);
        canvas.removeEventListener('touchmove', handleTouch);
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleKeyDown);
          window.removeEventListener('keyup', handleKeyUp);
          canvas.removeEventListener('touchstart', handleTouch);
          canvas.removeEventListener('touchmove', handleTouch);
          const isRecord = Minigames.saveBestScore('catch', score);
          Minigames.showResult('¡Juego terminado!', '🎈', score, () => Minigames.startCatchGame(), isRecord);
          return;
        }

        if (keys['ArrowLeft'] && basketX > 5) basketX -= 6;
        if (keys['ArrowRight'] && basketX < 235) basketX += 6;

        itemY += 3.8;

        if (itemY >= 260 && itemX >= basketX - 10 && itemX <= basketX + 60) {
          score += 5;
          Minigames.updateHudScore(score);
          AudioEffects.playTone(800, 'sine', 0.05);
          itemY = 0;
          itemX = Math.random() * 250 + 10;
          currentCandy = CANDY_EMOJIS[Math.floor(Math.random() * CANDY_EMOJIS.length)];
        }

        if (itemY > 300) gameOver = true;

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        // Cesta
        ctx.font = '38px sans-serif';
        ctx.fillText('🧺', basketX + 30, 285);

        // Dulce cayendo
        ctx.font = '26px sans-serif';
        ctx.fillText(currentCandy, itemX, itemY);

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  },

  // =========================================================
  // 2. EXPLOTAR GLOBOS KAWAII (Soporte Táctil)
  // =========================================================
  startPopGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('pop');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🌸 Explotar Globos</h3>
        ${Minigames.renderHUD('🎈', best, 20)}
        <canvas id="popCanvas" width="300" height="300" style="background:linear-gradient(180deg, #fff3e0 0%, #fce4ec 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ff80ab; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">¡Toca los globos antes de que suban!</p>
      `;

      const canvas = document.getElementById('popCanvas');
      const ctx = canvas.getContext('2d');
      let balloons = [];
      let score = 0;
      let timeLeft = 20;
      let stopped = false;

      const timerInterval = setInterval(() => {
        timeLeft--;
        Minigames.updateHudTimer(timeLeft);
        if (timeLeft <= 0) {
          clearInterval(timerInterval);
          const isRecord = Minigames.saveBestScore('pop', score);
          Minigames.showResult('¡Tiempo!', '🌸', score, () => Minigames.startPopGame(), isRecord);
        }
      }, 1000);

      const handlePop = (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const clientX = e.touches ? e.touches[0].clientX : e.clientX;
        const clientY = e.touches ? e.touches[0].clientY : e.clientY;
        const mouseX = (clientX - rect.left) * scale.x;
        const mouseY = (clientY - rect.top) * scale.y;

        balloons.forEach((b, index) => {
          const dist = Math.hypot(b.x - mouseX, b.y - mouseY);
          if (dist < b.radius + 10) { // Margen extra para dedos en pantalla táctil
            score += 5;
            Minigames.updateHudScore(score);
            AudioEffects.playTone(900, 'triangle', 0.08);
            balloons.splice(index, 1);
          }
        });
      };

      canvas.addEventListener('touchstart', handlePop, { passive: false });
      canvas.onclick = handlePop;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        stopped = true;
        clearInterval(timerInterval);
        canvas.removeEventListener('touchstart', handlePop);
        canvas.onclick = null;
        return score;
      };

      function loop() {
        if (stopped || timeLeft <= 0) return;

        if (Math.random() < 0.05 && balloons.length < 6) {
          balloons.push({
            x: Math.random() * 260 + 20,
            y: 320,
            radius: 18,
            speed: Math.random() * 1.5 + 1.2
          });
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        balloons.forEach((b, index) => {
          b.y -= b.speed;

          ctx.font = `${b.radius * 2}px sans-serif`;
          ctx.fillText('🎈', b.x, b.y);

          if (b.y < -20) balloons.splice(index, 1);
        });

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  },

  // =========================================================
  // 3. TORRE DE POSTRES KAWAII (Soporte Táctil)
  // =========================================================
  startCakeGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('cake');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🍰 Torre de Postres</h3>
        ${Minigames.renderHUD('🍰', best)}
        <canvas id="cakeCanvas" width="300" height="300" style="background:#f3e5f5; border-radius:16px; margin:6px auto; display:block; border:3px solid #b388ff; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca la pantalla para apilar el pastel</p>
      `;

      const canvas = document.getElementById('cakeCanvas');
      const ctx = canvas.getContext('2d');
      let layers = [{ x: 100, width: 100, y: 270 }];
      let currentX = 0;
      let currentSpeed = 3;
      let score = 0;
      let gameOver = false;

      const dropLayer = (e) => {
        if (e) e.preventDefault();
        if (gameOver) return;
        const prev = layers[layers.length - 1];
        const diff = currentX - prev.x;

        if (Math.abs(diff) > prev.width) {
          gameOver = true;
          const isRecord = Minigames.saveBestScore('cake', score);
          Minigames.showResult('¡Se cayó la torre!', '🍰', score, () => Minigames.startCakeGame(), isRecord);
          return;
        }

        score += 10;
        Minigames.updateHudScore(score);
        AudioEffects.playTone(700, 'sine', 0.1);
        const newWidth = prev.width - Math.abs(diff);
        const newX = diff > 0 ? currentX : prev.x;

        layers.push({ x: newX, width: newWidth, y: prev.y - 25 });
        currentX = 0;

        if (layers.length > 8) {
          layers.forEach(l => l.y += 25);
        }
      };

      canvas.addEventListener('touchstart', dropLayer, { passive: false });
      canvas.onclick = dropLayer;

      const handleSpace = (e) => { if (e.code === 'Space') dropLayer(); };
      window.addEventListener('keydown', handleSpace);

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        canvas.removeEventListener('touchstart', dropLayer);
        canvas.onclick = null;
        window.removeEventListener('keydown', handleSpace);
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleSpace);
          return;
        }

        currentX += currentSpeed;
        const prevWidth = layers[layers.length - 1].width;
        if (currentX > 300 - prevWidth || currentX < 0) currentSpeed *= -1;

        ctx.clearRect(0, 0, canvas.width, canvas.height);

        layers.forEach((l, i) => {
          ctx.fillStyle = i % 2 === 0 ? '#ff80ab' : '#80cbc4';
          ctx.beginPath();
          ctx.roundRect(l.x, l.y, l.width, 22, 6);
          ctx.fill();
        });

        const currentY = layers[layers.length - 1].y - 25;
        ctx.fillStyle = '#ffd54f';
        ctx.beginPath();
        ctx.roundRect(currentX, currentY, prevWidth, 22, 6);
        ctx.fill();

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  },

  // =========================================================
  // 4. SIMÓN DICE KAWAII (Soporte Táctil)
  // =========================================================
  startSimonGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('simon');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🎯 Simón Memoria</h3>
        ${Minigames.renderHUD('🧠', best)}
        <p id="simon-status" style="margin-top:5px; text-align:center; font-weight:bold; color:#7e57c2;">¡Memoriza la secuencia!</p>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px; margin:20px 0;">
          <button id="simon-0" onclick="Minigames.simonClick(0)" style="height:75px; background:#ff80ab; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #f50057; touch-action:manipulation;"></button>
          <button id="simon-1" onclick="Minigames.simonClick(1)" style="height:75px; background:#80deea; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #00acc1; touch-action:manipulation;"></button>
          <button id="simon-2" onclick="Minigames.simonClick(2)" style="height:75px; background:#b388ff; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #673ab7; touch-action:manipulation;"></button>
          <button id="simon-3" onclick="Minigames.simonClick(3)" style="height:75px; background:#ffe082; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #ffb300; touch-action:manipulation;"></button>
        </div>
      `;

      this.simonSequence = [];
      this.userSequence = [];
      this.simonScore = 0;
      this.nextSimonRound();

      Minigames.inProgress = true;
      Minigames.exitHandler = () => Minigames.simonScore * 10;
    });
  },

  nextSimonRound() {
    this.userSequence = [];
    this.simonSequence.push(Math.floor(Math.random() * 4));
    document.getElementById('simon-status').innerText = `Ronda ${this.simonSequence.length} ✨`;
    
    let i = 0;
    const interval = setInterval(() => {
      if (i >= this.simonSequence.length) {
        clearInterval(interval);
        return;
      }
      this.flashSimonButton(this.simonSequence[i]);
      i++;
    }, 700);
  },

  flashSimonButton(index) {
    const btn = document.getElementById(`simon-${index}`);
    if (!btn) return;
    const freqs = [350, 450, 550, 650];
    AudioEffects.playTone(freqs[index], 'sine', 0.15);
    btn.style.opacity = '0.3';
    setTimeout(() => btn.style.opacity = '1', 350);
  },

  simonClick(index) {
    this.flashSimonButton(index);
    this.userSequence.push(index);

    const currentIndex = this.userSequence.length - 1;
    if (this.userSequence[currentIndex] !== this.simonSequence[currentIndex]) {
      const reward = this.simonScore * 10;
      const isRecord = Minigames.saveBestScore('simon', this.simonScore);
      Minigames.showResult('¡Te equivocaste!', '🧠', reward, () => Minigames.startSimonGame(), isRecord);
      return;
    }

    if (this.userSequence.length === this.simonSequence.length) {
      this.simonScore++;
      Minigames.updateHudScore(this.simonScore);
      setTimeout(() => this.nextSimonRound(), 1000);
    }
  },

  // =========================================================
  // 5. RUNNER MÁGICO KAWAII (Soporte Táctil)
  // =========================================================
  startRunnerGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('runner');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🦄 Runner Mágico</h3>
        ${Minigames.renderHUD('🦄', best)}
        <canvas id="runnerCanvas" width="300" height="280" style="background:linear-gradient(180deg, #fff9c4 0%, #f3e5f5 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ffd54f; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:300/280;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca la pantalla o Espacio para saltar</p>
      `;

      const canvas = document.getElementById('runnerCanvas');
      const ctx = canvas.getContext('2d');

      let playerY = 210;
      let playerVY = 0;
      let isJumping = false;
      let obstacleX = 300;
      let score = 0;
      let gameOver = false;

      const jump = (e) => {
        if (e) e.preventDefault();
        if (!isJumping) {
          playerVY = -11;
          isJumping = true;
          AudioEffects.playTone(550, 'square', 0.08);
        }
      };

      const handleKeyDown = (e) => { if (e.code === 'Space') jump(); };
      window.addEventListener('keydown', handleKeyDown);
      canvas.addEventListener('touchstart', jump, { passive: false });
      canvas.onclick = jump;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        window.removeEventListener('keydown', handleKeyDown);
        canvas.removeEventListener('touchstart', jump);
        canvas.onclick = null;
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleKeyDown);
          canvas.removeEventListener('touchstart', jump);
          const isRecord = Minigames.saveBestScore('runner', score);
          Minigames.showResult('¡Juego terminado!', '🦄', score, () => Minigames.startRunnerGame(), isRecord);
          return;
        }

        playerY += playerVY;
        playerVY += 0.65;

        if (playerY >= 210) {
          playerY = 210;
          isJumping = false;
        }

        obstacleX -= 4.2;
        if (obstacleX < -25) {
          obstacleX = 300;
          score += 10;
          Minigames.updateHudScore(score);
        }

        if (obstacleX < 45 && obstacleX > 15 && playerY > 180) gameOver = true;

        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Suelo
        ctx.fillStyle = '#e1bee7';
        ctx.fillRect(0, 240, 300, 40);

        // Personaje
        ctx.fillStyle = '#ff4081';
        ctx.beginPath();
        ctx.arc(35, playerY + 15, 15, 0, Math.PI * 2);
        ctx.fill();

        // Obstáculo
        ctx.fillStyle = '#ffb300';
        ctx.beginPath();
        ctx.arc(obstacleX + 10, 225, 12, 0, Math.PI * 2);
        ctx.fill();

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  },

  // =========================================================
  // 6. BURBUJAS KAWAII (Soporte Táctil)
  // =========================================================
  startBubbleGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('bubble');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">✨ Burbujas Mágicas</h3>
        ${Minigames.renderHUD('✨', best, 15)}
        <canvas id="bubbleCanvas" width="300" height="300" style="background:linear-gradient(180deg, #e0f2f1 0%, #e8eaf6 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #80cbc4; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca las burbujas para atraparlas</p>
      `;

      const canvas = document.getElementById('bubbleCanvas');
      const ctx = canvas.getContext('2d');
      let bubbles = [];
      let score = 0;
      let timeLeft = 15;
      let stopped = false;

      const timer = setInterval(() => {
        timeLeft--;
        Minigames.updateHudTimer(timeLeft);
        if (timeLeft <= 0) {
          clearInterval(timer);
          const isRecord = Minigames.saveBestScore('bubble', score);
          Minigames.showResult('¡Tiempo!', '✨', score, () => Minigames.startBubbleGame(), isRecord);
        }
      }, 1000);

      const handleTouchBubble = (e) => {
        if (e) e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const clientX = e.touches ? e.touches[0].clientX : e.clientX;
        const clientY = e.touches ? e.touches[0].clientY : e.clientY;
        const mx = (clientX - rect.left) * scale.x;
        const my = (clientY - rect.top) * scale.y;

        bubbles.forEach((b, i) => {
          if (Math.hypot(b.x - mx, b.y - my) < b.r + 8) { // Margen táctil
            score += b.isGolden ? 15 : 5;
            Minigames.updateHudScore(score);
            AudioEffects.playTone(b.isGolden ? 1000 : 750, 'sine', 0.1);
            bubbles.splice(i, 1);
          }
        });
      };

      canvas.addEventListener('touchstart', handleTouchBubble, { passive: false });
      canvas.onclick = handleTouchBubble;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        stopped = true;
        clearInterval(timer);
        canvas.removeEventListener('touchstart', handleTouchBubble);
        canvas.onclick = null;
        return score;
      };

      function loop() {
        if (stopped || timeLeft <= 0) return;

        if (Math.random() < 0.08 && bubbles.length < 7) {
          const isGolden = Math.random() < 0.3;
          bubbles.push({
            x: Math.random() * 250 + 25,
            y: Math.random() * 220 + 40,
            r: Math.random() * 8 + 16,
            isGolden: isGolden
          });
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        bubbles.forEach(b => {
          ctx.font = `${b.r * 2}px sans-serif`;
          ctx.fillText(b.isGolden ? '🌟' : '🫧', b.x, b.y);
        });

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  }
};
```

### Archivo: `borrar/pet.js`
```javascript
const PetState = {
  coins: 500,
  happiness: 100,
  hunger: 100,
  energy: 100,
  petColor: '#ff80ab',
  petShape: 'shape-circle',
  petSize: 'medium',
  glitterEnabled: false,
  glitterIntensity: 0,
  isSick: false,
  isSleeping: false,
  inventory: ['bow_tie', 'bg_living'],
  equippedAccessory: 'bow_tie',
  equippedBackground: 'bg_living',
  looks: [],
  dialogues: {},

  async init() {
    this.loadData();
    await this.loadDialogues();
    this.updateUI();
    setInterval(() => this.tick(), 8000);
    this.startWandering();
  },

  async loadDialogues() {
    try {
      const response = await fetch('data/dialogues.json');
      this.dialogues = await response.json();
    } catch (e) {
      this.dialogues = {
        happy: ['❤️ ¡Me encanta estar contigo!', '✨ ¡Hoy es un día genial!', '🌸 ¡Eres mi persona favorita!'],
        hungry: ['🍕 ¡Tengo hambre! ¡Comida rica!', '🍗 Mmm... ¿Hay algo para picar?'],
        sick: ['🥺 Me siento mal... ¿Me das medicina?', '🤒 Me dolió la pancita...'],
        tired: ['😴 Tengo sueño... ¡A dormir!', '💤 Mis ojitos se cierran...'],
        bored: ['🎮 ¿Jugamos un minijuego?', '🎈 ¡Estoy aburrida! Vamos a jugar.'],
        sleeping: ['💤 Zzz... Descansando...', '🌙 Zzz... Soñando con caramelos...']
      };
    }
  },

  getRandomPhrase(category) {
    const list = this.dialogues[category] || ['❤️ ¡Hola!'];
    const index = Math.floor(Math.random() * list.length);
    return list[index];
  },

  getDialogue(key, fallback) {
    return this.dialogues.alerts?.[key] || fallback;
  },

  showDialogue(message) {
    const thought = document.getElementById('pet-thought');
    if (!thought) return;
    thought.innerText = message;
    clearTimeout(this.dialogueTimeout);
    this.dialogueTimeout = setTimeout(() => this.updateFaceAndThought(), 3000);
  },

  startWandering() {
    clearInterval(this.wanderingTimer);
    this.moveRandomly();
    this.wanderingTimer = setInterval(() => {
      // Solo se mueve si NO está durmiendo ni enferma
      if (!this.isSleeping && !this.isSick) {
        this.moveRandomly();
      }
    }, 5000);
  },

  moveRandomly() {
    const petEl = document.getElementById('pet');
    const roomEl = document.getElementById('pet-room');
    if (!petEl || !roomEl) return;

    const roomWidth = roomEl.clientWidth;
    const petWidth = petEl.offsetWidth;
    const minX = 20;
    const maxX = Math.max(minX, roomWidth - petWidth - 20);

    const targetX = Math.floor(Math.random() * (maxX - minX + 1)) + minX;

    petEl.classList.add('walking');
    petEl.style.left = `${targetX}px`;

    setTimeout(() => {
      petEl.classList.remove('walking');
    }, 2500);
  },

  updateUI() {
    document.getElementById('coin-count').innerText = this.coins;
    document.getElementById('happy-count').innerText = this.happiness;
    document.getElementById('hunger-count').innerText = this.hunger;
    document.getElementById('energy-count').innerText = this.energy;

    const petEl = document.getElementById('pet');
    if (petEl) {
      petEl.style.backgroundColor = this.petColor;
      // currentColor: lo usan las formas decorativas (orejitas, nube, corazón)
      petEl.style.color = this.petColor;
      // No pisa 'walking'/'happy-jump' u otras clases temporales que ya tenga el elemento
      petEl.className = petEl.className.replace(/\bshape-\S+/g, '').trim();
      petEl.classList.add(this.petShape || 'shape-circle');
      petEl.className = petEl.className.replace(/\bsize-\S+/g, '').trim();
      petEl.classList.add(`size-${this.petSize || 'medium'}`);
      const glitterEl = document.getElementById('pet-glitter');
      if (glitterEl) {
        glitterEl.classList.toggle('enabled', this.glitterIntensity > 0);
        glitterEl.style.setProperty('--glitter-opacity', (0.1 + this.glitterIntensity / 100 * 0.9).toFixed(2));
      }
    }

    const roomEl = document.getElementById('pet-room');
    if (roomEl) {
      roomEl.className = `room ${this.equippedBackground}`;
    }

    if (typeof Particles !== 'undefined') {
      Particles.render(this.equippedBackground);
    }

    const accessoryEl = document.getElementById('pet-accessory');
    accessoryEl.className = 'accessory';
    accessoryEl.style.cssText = '';

    if (this.equippedAccessory) {
      const item = Catalogo.items.find(i => i.id === this.equippedAccessory);
      if (item) {
        accessoryEl.innerHTML = `<img src="${item.image}" alt="${item.name}">`;
        accessoryEl.classList.add(`pos-${item.type}`);
        accessoryEl.classList.add(`item-${item.id}`);
        const fit = Catalogo.accessoryFits?.[item.id];
        if (fit) {
          const accessoryImg = accessoryEl.querySelector('img');
          accessoryImg.style.setProperty('--accessory-scale', fit.scale);
          accessoryImg.style.setProperty('--accessory-y', `${fit.y}px`);
          accessoryImg.style.setProperty('--accessory-x', `${fit.x || 0}px`);
          accessoryImg.style.setProperty('--accessory-rotate', `${fit.rotate || 0}deg`);
        }
      }
    } else {
      accessoryEl.innerHTML = '';
    }

    this.updateFaceAndThought();
  },

  updateFaceAndThought() {
    const face = document.getElementById('kawaii-face');
    const thought = document.getElementById('pet-thought');
    const extraProp = document.getElementById('extra-prop');
    
    if (!face || !thought) return;

    face.className = 'kawaii-face';
    extraProp.innerText = '';

    if (this.isSleeping) {
      face.classList.add('sleeping');
      thought.innerText = this.getRandomPhrase('sleeping');
      extraProp.innerText = '💤';
    } else if (this.isSick) {
      face.classList.add('sick');
      thought.innerText = this.getRandomPhrase('sick');
      extraProp.innerText = '🌡️';
    } else if (this.hunger < 30) {
      face.classList.add('hungry');
      thought.innerText = this.getRandomPhrase('hungry');
      extraProp.innerText = '💧';
    } else if (this.energy < 30) {
      face.classList.add('tired');
      thought.innerText = this.getRandomPhrase('tired');
    } else if (this.happiness < 40) {
      face.classList.add('bored');
      thought.innerText = this.getRandomPhrase('bored');
    } else {
      face.classList.add('happy');
      thought.innerText = this.getRandomPhrase('happy');
    }
  },

  petPet() {
    if (this.isSleeping) return;
    this.happiness = Math.min(100, this.happiness + 10);
    const petEl = document.getElementById('pet');
    petEl.classList.add('happy-jump');
    setTimeout(() => petEl.classList.remove('happy-jump'), 500);

    for (let i = 0; i < 3; i++) {
      const heart = document.createElement('div');
      heart.className = 'heart-particle';
      heart.innerText = '❤️';
      const offsetX = (Math.random() - 0.5) * 60;
      heart.style.setProperty('--dx', `${offsetX}px`);
      heart.style.left = `calc(50% + ${offsetX / 2}px)`;
      heart.style.top = '10px';
      petEl.appendChild(heart);
      setTimeout(() => heart.remove(), 800);
    }

    AudioEffects.playTone(600, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  },

  feed() {
    if (this.isSleeping) return;
    if (this.hunger >= 100) {
      this.showDialogue(this.getDialogue('full', 'No preciso comer ahora.'));
      return;
    }
    this.hunger = Math.min(100, this.hunger + 30);
    AudioEffects.playEat();
    this.saveData();
    this.updateUI();
  },

  heal() {
    if (!this.isSick) {
      this.showDialogue(this.getDialogue('healthy', 'No preciso un doctor ahora.'));
      return;
    }
    this.isSick = false;
    this.happiness = Math.min(100, this.happiness + 20);
    AudioEffects.playHeal();
    this.saveData();
    this.updateUI();
  },

  sleep() {
    this.isSleeping = !this.isSleeping;
    
    // Si se duerme, quita la animación de caminar de inmediato
    const petEl = document.getElementById('pet');
    if (petEl) petEl.classList.remove('walking');

    if (this.isSleeping) {
      this.energy = 100;
      AudioEffects.playTone(300, 'sine', 0.3);
    }
    this.saveData();
    this.updateUI();
  },

  setColor(color) {
    this.petColor = color;
    this.saveData();
    this.updateUI();
  },

  setShape(shapeId) {
    this.petShape = shapeId;
    AudioEffects.playTone(700, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  },

  setSize(size) {
    if (!['small', 'medium', 'large'].includes(size)) return;
    this.petSize = size;
    AudioEffects.playTone(700, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  },

  setGlitterIntensity(intensity) {
    this.glitterIntensity = Math.max(0, Math.min(100, Number(intensity) || 0));
    this.glitterEnabled = this.glitterIntensity > 0;
    this.saveData();
    this.updateUI();
  },

  setGlitterEnabled(enabled) {
    this.glitterEnabled = Boolean(enabled);
    this.saveData();
    this.updateUI();
  },

  equipItem(itemId) {
    const item = Catalogo.items.find(i => i.id === itemId);
    if (!item) return;

    if (item.type === 'bg') {
      this.equippedBackground = (this.equippedBackground === itemId) ? 'bg_living' : itemId;
    } else {
      this.equippedAccessory = (this.equippedAccessory === itemId) ? null : itemId;
    }

    AudioEffects.playTone(800, 'sine', 0.08);
    this.saveData();
    this.updateUI();
  },

  // Guarda la combinación actual (accesorio + fondo + color) como un "look".
  // Máximo 6 guardados: al superar el límite se descarta el más viejo.
  saveLook(name) {
    if (!this.looks) this.looks = [];
    const look = {
      id: 'look_' + Date.now(),
      name: (name && name.length > 0) ? name : `Look ${this.looks.length + 1}`,
      accessory: this.equippedAccessory,
      background: this.equippedBackground,
      color: this.petColor
    };
    this.looks.push(look);
    if (this.looks.length > 6) this.looks.shift();
    AudioEffects.playCoin();
    this.saveData();
  },

  applyLook(lookId) {
    const look = (this.looks || []).find(l => l.id === lookId);
    if (!look) return;
    this.equippedAccessory = look.accessory;
    this.equippedBackground = look.background;
    this.petColor = look.color;
    AudioEffects.playTone(800, 'sine', 0.08);
    this.saveData();
    this.updateUI();
  },

  deleteLook(lookId) {
    this.looks = (this.looks || []).filter(l => l.id !== lookId);
    this.saveData();
  },

  addCoins(amount) {
    this.coins += amount;
    if (amount > 0) {
      this.happiness = Math.min(100, this.happiness + 15);
      AudioEffects.playCoin();
    }
    this.saveData();
    this.updateUI();
  },

  tick() {
    if (this.isSleeping) {
      this.energy = Math.min(100, this.energy + 10);
      if (this.energy === 100) this.isSleeping = false;
    } else {
      this.hunger = Math.max(0, this.hunger - 3);
      this.happiness = Math.max(0, this.happiness - 3);
      this.energy = Math.max(0, this.energy - 2);

      if (!this.isSick && (this.hunger < 40 || this.happiness < 40) && Math.random() < 0.2) {
        this.isSick = true;
      }
    }
    this.saveData();
    this.updateUI();
  },

  saveData() {
    localStorage.setItem('virtualPetData', JSON.stringify({
      coins: this.coins,
      happiness: this.happiness,
      hunger: this.hunger,
      energy: this.energy,
      petColor: this.petColor,
      petShape: this.petShape,
      petSize: this.petSize,
      glitterEnabled: this.glitterEnabled,
      glitterIntensity: this.glitterIntensity,
      isSick: this.isSick,
      inventory: this.inventory,
      equippedAccessory: this.equippedAccessory,
      equippedBackground: this.equippedBackground,
      looks: this.looks
    }));
  },

  loadData() {
    const saved = localStorage.getItem('virtualPetData');
    if (saved) {
      const data = JSON.parse(saved);
      this.coins = data.coins ?? 100;
      this.happiness = data.happiness ?? 100;
      this.hunger = data.hunger ?? 100;
      this.energy = data.energy ?? 100;
      this.petColor = data.petColor || '#ff80ab';
      this.petShape = data.petShape || 'shape-circle';
      this.petSize = ['small', 'medium', 'large'].includes(data.petSize) ? data.petSize : 'medium';
      const savedGlitterIntensity = data.glitterIntensity ?? (data.glitterEnabled ? 50 : 0);
      this.glitterIntensity = Math.max(0, Math.min(100, Number(savedGlitterIntensity)));
      this.glitterEnabled = this.glitterIntensity > 0;
      this.isSick = data.isSick ?? false;
      this.inventory = data.inventory || ['bow_tie', 'bg_living'];
      this.equippedAccessory = data.equippedAccessory || null;
      this.equippedBackground = data.equippedBackground || 'bg_living';
      this.looks = data.looks || [];
    }
  }
};
```

### Archivo: `borrar/store.js`
```javascript
const Store = {
  storeCategory: 'all',

  // Las formas de mascota disponibles (elegibles en el Armario)
  shapes: [
    { id: 'shape-circle',   name: 'Círculo',   emoji: '⚪' },
    { id: 'shape-mochi',    name: 'Mochi',      emoji: '🍡' },
    { id: 'shape-onigiri',  name: 'Gotita',     emoji: '💧' },
    { id: 'shape-squircle', name: 'Squircle',   emoji: '🔲' },
    { id: 'shape-dumpling', name: 'Dumpling',   emoji: '🥟' },
    //{ id: 'shape-capsule',  name: 'Cápsula',    emoji: '💊' },
    //{ id: 'shape-heart',    name: 'Corazón',    emoji: '❤️' },
    //{ id: 'shape-cathead',  name: 'Animalito',  emoji: '🐱' },
    //{ id: 'shape-cloud',    name: 'Nube',       emoji: '☁️' }
  ],

  items: [
    // -----------------------------------------------------------------
    // ACCESORIOS (Organizados para combinar con los escenarios)
    // -----------------------------------------------------------------
    // Básicos y Fiesta (Living, Dormitorio, Playroom)
    { id: 'hat_crown', name: 'Corona Real', price: 50, image: 'img/accessories/hat_crown.svg', type: 'head', category: 'acc' },
    { id: 'party_hat', name: 'Gorro Fiesta', price: 25, image: 'img/accessories/party_hat.svg', type: 'head', category: 'acc' },
    { id: 'ribbon_pink', name: 'Lazo Rosado', price: 15, image: 'img/accessories/ribbon_pink.svg', type: 'head', category: 'acc' },
    { id: 'bow_tie', name: 'Moño Coquette', price: 20, image: 'img/accessories/bow_tie.svg', type: 'neck', category: 'acc' },
    { id: 'headphones', name: 'Auriculares Gato', price: 40, image: 'img/accessories/headphones.svg', type: 'ears', category: 'acc' },
    { id: 'cat_ears', name: 'Orejitas Violeta', price: 45, image: 'img/accessories/cat_ears.svg', type: 'ears', category: 'acc' },
    { id: 'bunny_ears', name: 'Orejas Conejo', price: 40, image: 'img/accessories/bunny_ears.svg', type: 'ears', category: 'acc' },

    // Para "Playa Tropical" / "Oasis del Desierto"
    { id: 'glasses_cool', name: 'Lentes Corazón', price: 30, image: 'img/accessories/glasses_cool.svg', type: 'eyes', category: 'acc' },
    { id: 'star_glasses', name: 'Lentes Estrella', price: 35, image: 'img/accessories/star_glasses.svg', type: 'eyes', category: 'acc' },
    { id: 'pirate_hat', name: 'Gorro Pirata', price: 55, image: 'img/accessories/pirate_hat.svg', type: 'head', category: 'acc' },
    { id: 'snorkel_mask', name: 'Visor de Buceo', price: 45, image: 'img/accessories/snorkel.svg', type: 'eyes', category: 'acc' },

    // Para "Cocina del Chef"
    { id: 'chef_hat', name: 'Gorro de Chef', price: 35, image: 'img/accessories/chef_hat.svg', type: 'head', category: 'acc' },

    // Para "Parque Mágico" / "Bosque Encantado" / "Castillo Real"
    { id: 'flower_pink', name: 'Flor Primavera', price: 18, image: 'img/accessories/flower_pink.svg', type: 'head', category: 'acc' },
    { id: 'wizard_hat', name: 'Gorro de Mago', price: 60, image: 'img/accessories/wizard_hat.svg', type: 'head', category: 'acc' },
    { id: 'magic_wand', name: 'Varita Mágica', price: 60, image: 'img/accessories/magic_wand.svg', type: 'neck', category: 'acc' },
    { id: 'fairy_wings', name: 'Alas de Hada', price: 75, image: 'img/accessories/fairy_wings.svg', type: 'neck', category: 'acc' },

    // Para "Dojo Ninja"
    { id: 'ninja_band', name: 'Vanda Ninja', price: 50, image: 'img/accessories/ninja_band.svg', type: 'head', category: 'acc' },

    // Para "Estación Espacial"
    { id: 'space_helmet', name: 'Casco Espacial', price: 90, image: 'img/accessories/space_helmet.svg', type: 'head', category: 'acc' },

    // Para "Montaña Nevada" / "Campamento Nocturno"
    { id: 'winter_beanie', name: 'Gorro de Lana', price: 30, image: 'img/accessories/winter_beanie.svg', type: 'head', category: 'acc' },
    { id: 'winter_scarf', name: 'Bufanda Calientita', price: 25, image: 'img/accessories/winter_scarf.svg', type: 'neck', category: 'acc' },

    // Para "Baño de Burbujas"
    { id: 'shower_cap', name: 'Gorro de Baño', price: 20, image: 'img/accessories/shower_cap.svg', type: 'head', category: 'acc' },

    // -----------------------------------------------------------------
    // ESCENARIOS / FONDOS (Categoría 'bg')
    // El campo `preview` reproduce el gradiente real de css/escenarios.css
    // para mostrar una miniatura fiel en vez de un emoji genérico.
    // -----------------------------------------------------------------
    // Básicos
    { id: 'bg_living', name: 'Sala de Estar', price: 0, image: '🛋️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%), linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%);' },
    { id: 'bg_bedroom', name: 'Dormitorio', price: 40, image: '🛏️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%), radial-gradient(circle at 80% 20%, #fff 2px, transparent 3px), radial-gradient(circle at 20% 35%, #fff 2px, transparent 3px), linear-gradient(180deg, #311b92 0%, #512da8 72%);' },
    { id: 'bg_playroom', name: 'Sala de Juegos', price: 60, image: '🧸', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #80deea 70%, #4dd0e1 100%), radial-gradient(#ff80ab 15%, transparent 16%), radial-gradient(#ffd54f 15%, transparent 16%), #f3e5f5; background-size: 100% 100%, 20px 20px, 20px 20px, 100% 100%; background-position: 0 0, 0 0, 10px 10px, 0 0;' },
    { id: 'bg_park', name: 'Parque Mágico', price: 80, image: '🌳', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #aed581 68%, #7cb342 100%), linear-gradient(180deg, #81d4fa 0%, #e1f5fe 68%);' },
    { id: 'bg_beach', name: 'Playa Tropical', price: 50, image: '🏖️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%), linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%);' },

    // Naturaleza y Exteriores
    { id: 'bg_forest', name: 'Bosque Encantado', price: 90, image: '🌲', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%), linear-gradient(180deg, #aed581 0%, #558b2f 65%);' },
    { id: 'bg_mountain', name: 'Montaña Nevada', price: 100, image: '🏔️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #cfd8dc 70%, #90a4ae 100%), linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);' },
    { id: 'bg_camping', name: 'Campamento Nocturno', price: 120, image: '⛺', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #2e7d32 72%, #1b5e20 100%), radial-gradient(circle at 75% 20%, #fff9c4 3px, transparent 4px), radial-gradient(circle at 25% 15%, #fff9c4 2px, transparent 3px), linear-gradient(180deg, #0d1b2a 0%, #1b263b 72%);' },
    { id: 'bg_desert', name: 'Oasis del Desierto', price: 110, image: '🏜️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #f57c00 68%, #e65100 100%), linear-gradient(180deg, #ffe0b2 0%, #ffcc80 68%);' },

    // Interiores y Temáticos
    { id: 'bg_kitchen', name: 'Cocina del Chef', price: 45, image: '🍳', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%), linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%);' },
    { id: 'bg_bathroom', name: 'Baño de Burbujas', price: 35, image: '🛁', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #b2ebf2 70%, #4dd0e1 100%), linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);' },
    { id: 'bg_dojo', name: 'Dojo Ninja', price: 130, image: '⛩️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #8d6e63 70%, #4e342e 100%), linear-gradient(180deg, #efebe9 0%, #d7ccc8 70%);' },
    { id: 'bg_disco', name: 'Pista de Baile', price: 150, image: '🪩', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #311b92 70%, #1a237e 100%), linear-gradient(135deg, #4a148c 0%, #880e4f 50%, #311b92 100%);' },

    // Fantásticos y Especiales
    { id: 'bg_space', name: 'Estación Espacial', price: 200, image: '🚀', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 75%, #263238 75%, #102027 100%), radial-gradient(circle at 50% 30%, #37474f 0%, #000000 100%);' },
    { id: 'bg_castle', name: 'Castillo Real', price: 250, image: '🏰', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #9c27b0 70%, #4a148c 100%), linear-gradient(180deg, #f3e5f5 0%, #ce93d8 70%);' },
    { id: 'bg_underwater', name: 'Mundo Submarino', price: 180, image: '🪸', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #004d40 70%, #00251a 100%), linear-gradient(180deg, #00838f 0%, #006064 70%);' },
    { id: 'bg_volcano', name: 'Guarida Volcánica', price: 300, image: '🌋', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #3e2723 70%, #1b0000 100%), linear-gradient(180deg, #bf360c 0%, #dd2c00 70%);' }
  ],

  accessoryFits: {
    hat_crown: { scale: 1.25, y: -4 },
    party_hat: { scale: 1.2, y: -4 },
    ribbon_pink: { scale: 1.15, y: -2, rotate: -4 },
    chef_hat: { scale: 1.25, y: -4 },
    wizard_hat: { scale: 1.25, y: -5 },
    pirate_hat: { scale: 1.2, y: -3 },
    space_helmet: { scale: 1.2, y: -2 },
    winter_beanie: { scale: 1.2, y: -3 },
    shower_cap: { scale: 1.2, y: -2 },
    flower_pink: { scale: 1.1, y: 0 },
    headphones: { scale: 1.15, y: 0 },
    cat_ears: { scale: 1.1, y: -2 },
    bunny_ears: { scale: 1.15, y: -3 },
    glasses_cool: { scale: 1.15, y: 1, x: 0 },
    star_glasses: { scale: 1.15, y: 1, x: 0 },
    snorkel_mask: { scale: 1.2, y: 1 },
    bow_tie: { scale: 1.25, y: -2 },
    magic_wand: { scale: 1.1, y: -4, x: 3, rotate: -8 },
    fairy_wings: { scale: 1.2, y: -3, x: 0 },
    ninja_band: { scale: 1.15, y: 0 },
    winter_scarf: { scale: 1.25, y: -3 }
  },

  // Ícono de la card: mini-preview del gradiente real para fondos,
  // ícono SVG para accesorios.
  renderIcon(item) {
    if (item.category === 'bg') {
      return `<div class="bg-preview" style="${item.preview}"><span class="bg-preview-badge">${item.image}</span></div>`;
    }
    return `<img src="${item.image}" class="card-icon" alt="${item.name}">`;
  },

  renderStore(category = this.storeCategory) {
    this.storeCategory = category;
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">🛍️ Tienda de Objetos</h2>
      <div class="grid-container"></div>
      <div class="store-tabs" role="tablist" aria-label="Categorías de la tienda">
        <button class="store-tab ${category === 'all' ? 'active' : ''}" data-category="all">Todos</button>
        <button class="store-tab ${category === 'acc' ? 'active' : ''}" data-category="acc">Accesorios</button>
        <button class="store-tab ${category === 'bg' ? 'active' : ''}" data-category="bg">Escenarios</button>
      </div>
    `;

    content.querySelectorAll('.store-tab').forEach(tab => {
      tab.onclick = () => this.renderStore(tab.dataset.category);
    });
    
    const container = content.querySelector('.grid-container');
    
    this.items.filter(item => category === 'all' || item.category === category).forEach(item => {
      const isOwned = PetState.inventory.includes(item.id);
      const card = document.createElement('div');
      card.className = 'item-card';

      card.innerHTML = `
        ${this.renderIcon(item)}
        <span class="card-title">${item.name}</span>
        <span class="card-price">${item.price === 0 ? 'Gratis' : '🪙 ' + item.price}</span>
        <button onclick="Store.buyItem('${item.id}')" ${isOwned ? 'disabled' : ''} class="card-btn ${isOwned ? 'owned' : ''}">
          ${isOwned ? 'Comprado' : 'Comprar'}
        </button>
      `;
      container.appendChild(card);
    });
  },

  renderPetCustomization() {
    return `
      <section class="pet-customization-panel" aria-label="Personalizar Mi Pet">
        <h3>Mi Pet</h3>
        <div class="wardrobe-top-row">
          <div style="margin: 8px 0; text-align:center;">
            <p class="wardrobe-label">Color de Piel:</p>
            <div class="color-picker">
              <button class="color-option ${PetState.petColor === '#ff80ab' ? 'selected' : ''}" onclick="PetState.setColor('#ff80ab'); Store.renderMyPet();" aria-label="Color rosa" style="background:#ff80ab;"></button>
              <button class="color-option ${PetState.petColor === '#b388ff' ? 'selected' : ''}" onclick="PetState.setColor('#b388ff'); Store.renderMyPet();" aria-label="Color violeta" style="background:#b388ff;"></button>
              <button class="color-option ${PetState.petColor === '#80cbc4' ? 'selected' : ''}" onclick="PetState.setColor('#80cbc4'); Store.renderMyPet();" aria-label="Color turquesa" style="background:#80cbc4;"></button>
            </div>
          </div>
          <div style="margin: 8px 0; text-align:center;">
            <p class="wardrobe-label">Tamaño:</p>
            <div id="size-picker" class="size-picker">
              <button class="card-btn size-option ${PetState.petSize === 'small' ? 'equipped' : ''}" data-size="small" aria-label="Tamaño chico">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(0.6);"></span></span>
              </button>
              <button class="card-btn size-option ${PetState.petSize === 'medium' ? 'equipped' : ''}" data-size="medium" aria-label="Tamaño mediano">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(0.8);"></span></span>
              </button>
              <button class="card-btn size-option ${PetState.petSize === 'large' ? 'equipped' : ''}" data-size="large" aria-label="Tamaño grande">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(1);"></span></span>
              </button>
            </div>
          </div>
        </div>
        <div style="margin: 4px 0 8px; text-align:center;">
          <p class="wardrobe-label">Forma:</p>
          <div id="shape-picker" class="shape-picker"></div>
        </div>
        <div class="glitter-intensity">
          <label for="glitter-intensity">✨ Brillantina: <output id="glitter-intensity-value">${PetState.glitterIntensity}%</output></label>
          <input id="glitter-intensity" type="range" min="0" max="100" value="${PetState.glitterIntensity}">
        </div>
      </section>
    `;
  },

  bindPetCustomization(content) {
    const shapePicker = content.querySelector('#shape-picker');
    this.shapes.forEach(shape => {
      const isSelected = PetState.petShape === shape.id;
      const btn = document.createElement('button');
      btn.className = `card-btn shape-option ${isSelected ? 'selected' : ''}`;
      btn.setAttribute('aria-label', `Forma ${shape.name}`);
      btn.innerHTML = `${shape.emoji}<br>${shape.name}`;
      btn.onclick = () => { PetState.setShape(shape.id); Store.renderMyPet(); };
      shapePicker.appendChild(btn);
    });

    content.querySelectorAll('.size-option').forEach(btn => {
      btn.onclick = () => { PetState.setSize(btn.dataset.size); Store.renderMyPet(); };
    });

    const glitterIntensity = content.querySelector('#glitter-intensity');
    const glitterIntensityValue = content.querySelector('#glitter-intensity-value');
    glitterIntensity.oninput = event => {
      glitterIntensityValue.value = `${event.target.value}%`;
      glitterIntensityValue.textContent = `${event.target.value}%`;
      PetState.setGlitterIntensity(event.target.value);
    };
  },

  renderMyPet() {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">🐾 Mi Pet</h2>
      ${this.renderPetCustomization()}
    `;
    this.bindPetCustomization(content);
  },

  renderCollection(title, category, includeLooks, rerender) {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">${title}</h2>
      ${includeLooks ? `<div style="margin: 4px 0 8px; text-align:center;">
        <p style="font-size:0.8rem; color:#666; margin-bottom:4px; font-weight:bold;">⭐ Mis Looks:</p>
        <div id="looks-list" style="display:flex; flex-wrap:wrap; gap:6px; justify-content:center; margin-bottom:8px;"></div>
        <button onclick="Store.saveCurrentLook()" class="card-btn" style="width:auto; padding:6px 16px; background:#7e57c2;">💾 Guardar look actual</button>
      </div>` : ''}
      <hr style="border:none; border-top:1px solid #eee; margin:8px 0;">
      <div class="grid-container"></div>
    `;

    if (includeLooks) {
      const looksList = content.querySelector('#looks-list');
      if (!PetState.looks || PetState.looks.length === 0) {
        looksList.innerHTML = `<span style="font-size:0.72rem; color:#999;">Todavía no guardaste ningún look</span>`;
      } else {
        PetState.looks.forEach(look => {
          const chip = document.createElement('div');
          chip.className = 'look-chip';
          chip.innerHTML = `
            <span class="look-chip-name">${look.name}</span>
            <div class="look-chip-actions">
              <button onclick="PetState.applyLook('${look.id}'); Store.renderAccessories();" title="Aplicar look">✨</button>
              <button onclick="PetState.deleteLook('${look.id}'); Store.renderAccessories();" title="Borrar look">🗑️</button>
            </div>
          `;
          looksList.appendChild(chip);
        });
      }
    }

    const container = content.querySelector('.grid-container');

    PetState.inventory.forEach(itemId => {
      const item = this.items.find(i => i.id === itemId);
      if (!item || item.category !== category) return;

      const isEquipped = item.type === 'bg' 
        ? PetState.equippedBackground === item.id 
        : PetState.equippedAccessory === item.id;

      const card = document.createElement('div');
      card.className = 'item-card';

      card.innerHTML = `
        ${this.renderIcon(item)}
        <span class="card-title">${item.name}</span>
        <button class="card-btn ${isEquipped ? 'equipped' : ''}">
          ${isEquipped ? '✨ Usando' : 'Poner'}
        </button>
      `;
      card.querySelector('.card-btn').onclick = () => {
        PetState.equipItem(item.id);
        rerender();
      };
      container.appendChild(card);
    });
  },

  renderAccessories() {
    this.renderCollection('🎒 Accesorios', 'acc', true, () => this.renderAccessories());
  },

  renderScenery() {
    this.renderCollection('🏡 Escenario', 'bg', false, () => this.renderScenery());
  },

  saveCurrentLook() {
    const name = prompt('Nombre para este look:', `Look ${(PetState.looks ? PetState.looks.length : 0) + 1}`);
    if (name === null) return; // El usuario canceló
    PetState.saveLook(name.trim());
    this.renderAccessories();
  },

  buyItem(itemId) {
    const item = this.items.find(i => i.id === itemId);
    if (item && (PetState.coins >= item.price || item.price === 0) && !PetState.inventory.includes(itemId)) {
      PetState.coins -= item.price;
      PetState.inventory.push(itemId);
      AudioEffects.playCoin();
      PetState.saveData();
      PetState.updateUI();
      this.renderStore(this.storeCategory);
    } else if (item && PetState.coins < item.price) {
      alert('¡No tienes suficientes monedas!');
    }
  }
};
```

### Archivo: `borrar/styles.css`
```css
/* =========================================================
   RESETEO GLOBAL Y OPTIMIZACIÓN TÁCTIL (MÓVIL / TABLET)
   ========================================================= */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

body {
  background-color: #f0e6ff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  overscroll-behavior: none;
  touch-action: manipulation;
}

/* =========================================================
   CONTENEDOR PRINCIPAL (MARCO DE APP MÓVIL)
   ========================================================= */
#app-container {
  width: 100%;
  max-width: 380px;
  height: 100vh;
  max-height: 660px;
  background: #ffffff;
  border-radius: 24px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  border: 4px solid #d4b2ff;
  touch-action: manipulation;
}

/* =========================================================
   BARRA SUPERIOR Y GLOBO DE PENSAMIENTO
   ========================================================= */
.top-bar {
  display: flex;
  justify-content: space-around;
  padding: 12px 8px;
  background: #fdf8ff;
  border-bottom: 2px solid #f0e6ff;
  z-index: 5;
}

.stat-box {
  background: #ffffff;
  padding: 6px 10px;
  border-radius: 12px;
  font-size: 0.82rem;
  font-weight: bold;
  color: #4a148c;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  border: 1px solid #e1bee7;
}

.thought-bubble {
  position: absolute;
  top: 62px;
  left: 50%;
  transform: translateX(-50%);
  background: #ffffff;
  padding: 8px 16px;
  border-radius: 18px;
  font-size: 0.85rem;
  font-weight: 600;
  color: #4a148c;
  border: 2px solid #ce93d8;
  box-shadow: 0 4px 10px rgba(0,0,0,0.08);
  z-index: 10;
  max-width: 80%;
  text-align: center;
  width: max-content;  /* Hace que la burbuja tome el ancho de la frase en una sola línea... */
  max-width: 92%;      /* ...pero si la frase supera el 92% de la pantalla, la obliga a bajar de renglón */
}

/* =========================================================
   ESCENARIOS DETALLADOS CON MUEBLES FIJOS (CSS PURO)
   ========================================================= */
.room {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  transition: background 0.4s ease;
  overflow: hidden;
}

/* 1. Sala de Estar: Lámpara de pie y Cuadro */
.room.bg_living {
  background: 
    linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%),
    linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%);
}

.room.bg_living::before {
  content: '';
  position: absolute;
  left: 20px;
  bottom: 28%;
  width: 35px;
  height: 110px;
  background: 
    radial-gradient(ellipse at top, #ffee58 60%, transparent 65%),
    linear-gradient(to right, transparent 45%, #8d6e63 45%, #8d6e63 55%, transparent 55%);
  background-size: 100% 35px, 100% 100%;
  background-repeat: no-repeat;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
}

.room.bg_living::after {
  content: '🖼️';
  font-size: 1.8rem;
  position: absolute;
  right: 25px;
  top: 18%;
  padding: 4px;
  background: #ffffff;
  border: 3px solid #8d6e63;
  border-radius: 6px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

/* 2. Dormitorio: Ventana con Luna y Cama */
.room.bg_bedroom {
  background: 
    linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%),
    radial-gradient(circle at 80% 20%, #fff 2px, transparent 3px),
    radial-gradient(circle at 20% 35%, #fff 2px, transparent 3px),
    linear-gradient(180deg, #311b92 0%, #512da8 72%);
}

.room.bg_bedroom::before {
  content: '🌙';
  font-size: 1.6rem;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 20px;
  top: 15%;
  width: 55px;
  height: 55px;
  background: #1a237e;
  border: 4px solid #ffffff;
  border-radius: 50%;
  box-shadow: 0 0 12px rgba(255,255,255,0.3);
}

.room.bg_bedroom::after {
  content: '';
  position: absolute;
  right: -10px;
  bottom: 25%;
  width: 90px;
  height: 50px;
  background: 
    radial-gradient(ellipse at center, #ffffff 50%, transparent 55%),
    linear-gradient(to right, #ff80ab, #f50057);
  background-size: 40px 20px, 100% 35px;
  background-position: 10px 0, 0 15px;
  background-repeat: no-repeat;
  border-radius: 12px 0 0 8px;
}

/* 3. Sala de Juegos: Repisa con Juguetes y Pelota */
.room.bg_playroom {
  background: 
    linear-gradient(to bottom, transparent 70%, #80deea 70%, #4dd0e1 100%),
    radial-gradient(#ff80ab 15%, transparent 16%),
    radial-gradient(#ffd54f 15%, transparent 16%),
    #f3e5f5;
  background-size: 100% 100%, 40px 40px, 40px 40px, 100% 100%;
  background-position: 0 0, 0 0, 20px 20px, 0 0;
}

.room.bg_playroom::before {
  content: '🧸🤖';
  font-size: 1.2rem;
  letter-spacing: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 15px;
  top: 22%;
  width: 85px;
  height: 12px;
  background: #ffb74d;
  border-radius: 4px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.room.bg_playroom::after {
  content: '⚽';
  font-size: 1.8rem;
  position: absolute;
  right: 20px;
  bottom: 20%;
  filter: drop-shadow(0 4px 4px rgba(0,0,0,0.15));
}

/* 4. Parque Mágico: Sol Giratorio y Árbol */
.room.bg_park {
  background: 
    linear-gradient(to bottom, transparent 68%, #aed581 68%, #7cb342 100%),
    linear-gradient(180deg, #81d4fa 0%, #e1f5fe 68%);
}

.room.bg_park::before {
  content: '☀️';
  font-size: 2.2rem;
  position: absolute;
  right: 20px;
  top: 12%;
  animation: spinSun 20s linear infinite;
}

@keyframes spinSun {
  100% { transform: rotate(360deg); }
}

.room.bg_park::after {
  content: '🌳';
  font-size: 3.8rem;
  position: absolute;
  left: -10px;
  bottom: 22%;
  filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.12));
}

/* 5. Playa Tropical */
.room.bg_beach {
  background: 
    linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%),
    linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%);
}

.room.bg_beach::before {
  content: '🌴';
  font-size: 3.8rem;
  position: absolute;
  right: -5px;
  bottom: 22%;
  filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.15));
}

.room.bg_beach::after {
  content: '🏖️';
  font-size: 2.5rem;
  position: absolute;
  left: 15px;
  bottom: 24%;
  filter: drop-shadow(0 3px 5px rgba(0,0,0,0.1));
}

/* 6. Bosque Encantado */
.room.bg_forest {
  background: 
    linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%),
    linear-gradient(180deg, #aed581 0%, #558b2f 65%);
}

.room.bg_forest::before {
  content: '🌲';
  font-size: 3.5rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_forest::after {
  content: '🍄';
  font-size: 1.8rem;
  position: absolute;
  right: 25px;
  bottom: 20%;
}

/* 7. Montaña Nevada */
.room.bg_mountain {
  background: 
    linear-gradient(to bottom, transparent 70%, #cfd8dc 70%, #90a4ae 100%),
    linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);
}

.room.bg_mountain::before {
  content: '🏔️';
  font-size: 4rem;
  position: absolute;
  right: -10px;
  top: 15%;
  opacity: 0.8;
}

.room.bg_mountain::after {
  content: '☃️';
  font-size: 2.2rem;
  position: absolute;
  left: 20px;
  bottom: 22%;
}

/* 8. Campamento Nocturno */
.room.bg_camping {
  background: 
    linear-gradient(to bottom, transparent 72%, #2e7d32 72%, #1b5e20 100%),
    radial-gradient(circle at 75% 20%, #fff9c4 3px, transparent 4px),
    radial-gradient(circle at 25% 15%, #fff9c4 2px, transparent 3px),
    linear-gradient(180deg, #0d1b2a 0%, #1b263b 72%);
}

.room.bg_camping::before {
  content: '⛺';
  font-size: 2.8rem;
  position: absolute;
  left: 15px;
  bottom: 22%;
}

.room.bg_camping::after {
  content: '🔥';
  font-size: 2rem;
  position: absolute;
  right: 35px;
  bottom: 21%;
}

/* 9. Oasis del Desierto */
.room.bg_desert {
  background: 
    linear-gradient(to bottom, transparent 68%, #f57c00 68%, #e65100 100%),
    linear-gradient(180deg, #ffe0b2 0%, #ffcc80 68%);
}

.room.bg_desert::before {
  content: '🏜️';
  font-size: 3.2rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_desert::after {
  content: '🐪';
  font-size: 2.4rem;
  position: absolute;
  right: 15px;
  bottom: 22%;
}

/* 10. Cocina del Chef */
.room.bg_kitchen {
  background: 
    linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%),
    linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%);
}

.room.bg_kitchen::before {
  content: '🍳';
  font-size: 2.5rem;
  position: absolute;
  left: 20px;
  top: 20%;
}

.room.bg_kitchen::after {
  content: '🍕';
  font-size: 2rem;
  position: absolute;
  right: 25px;
  bottom: 22%;
}

/* 11. Baño de Burbujas */
.room.bg_bathroom {
  background: 
    linear-gradient(to bottom, transparent 70%, #b2ebf2 70%, #4dd0e1 100%),
    linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);
}

.room.bg_bathroom::before {
  content: '🛁';
  font-size: 3rem;
  position: absolute;
  right: 15px;
  bottom: 22%;
}

.room.bg_bathroom::after {
  content: '🦆';
  font-size: 1.8rem;
  position: absolute;
  left: 25px;
  bottom: 22%;
}

/* 12. Dojo Ninja */
.room.bg_dojo {
  background: 
    linear-gradient(to bottom, transparent 70%, #8d6e63 70%, #4e342e 100%),
    linear-gradient(180deg, #efebe9 0%, #d7ccc8 70%);
}

.room.bg_dojo::before {
  content: '⛩️';
  font-size: 3.5rem;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 15%;
}

/* 13. Pista de Baile Neo */
.room.bg_disco {
  background: 
    linear-gradient(to bottom, transparent 70%, #311b92 70%, #1a237e 100%),
    linear-gradient(135deg, #4a148c 0%, #880e4f 50%, #311b92 100%);
}

.room.bg_disco::before {
  content: '🪩';
  font-size: 3rem;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  animation: floatHeart 2s ease-in-out infinite alternate;
}

/* 14. Estación Espacial */
.room.bg_space {
  background: 
    linear-gradient(to bottom, transparent 75%, #263238 75%, #102027 100%),
    radial-gradient(circle at 50% 30%, #37474f 0%, #000000 100%);
}

.room.bg_space::before {
  content: '🚀';
  font-size: 3rem;
  position: absolute;
  right: 20px;
  top: 15%;
}

.room.bg_space::after {
  content: '🪐';
  font-size: 2.8rem;
  position: absolute;
  left: 15px;
  top: 25%;
}

/* 15. Castillo Real */
.room.bg_castle {
  background: 
    linear-gradient(to bottom, transparent 70%, #9c27b0 70%, #4a148c 100%),
    linear-gradient(180deg, #f3e5f5 0%, #ce93d8 70%);
}

.room.bg_castle::before {
  content: '🏰';
  font-size: 3.5rem;
  position: absolute;
  right: 15px;
  top: 15%;
}

/* 16. Mundo Submarino */
.room.bg_underwater {
  background: 
    linear-gradient(to bottom, transparent 70%, #004d40 70%, #00251a 100%),
    linear-gradient(180deg, #00838f 0%, #006064 70%);
}

.room.bg_underwater::before {
  content: '🪸';
  font-size: 3rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_underwater::after {
  content: '🐠';
  font-size: 2rem;
  position: absolute;
  right: 20px;
  top: 30%;
}

/* 17. Guarida Volcánica */
.room.bg_volcano {
  background: 
    linear-gradient(to bottom, transparent 70%, #3e2723 70%, #1b0000 100%),
    linear-gradient(180deg, #bf360c 0%, #dd2c00 70%);
}

.room.bg_volcano::before {
  content: '🌋';
  font-size: 3.5rem;
  position: absolute;
  right: 10px;
  top: 15%;
}

/* =========================================================
   SISTEMA DE PARTÍCULAS DINÁMICAS
   ========================================================= */
#bg-particles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  z-index: 2;
}

.particle {
  position: absolute;
  font-size: 1.2rem;
  opacity: 0.8;
}

/* Animaciones de Partículas */
@keyframes fall-slow {
  0% { top: -8%; opacity: 0; transform: rotate(0deg); }
  15% { opacity: 0.9; }
  85% { opacity: 0.9; }
  100% { top: 104%; opacity: 0; transform: rotate(280deg); }
}

@keyframes float-up {
  0% { bottom: -8%; opacity: 0; transform: scale(0.7); }
  15% { opacity: 0.85; }
  85% { opacity: 0.85; }
  100% { bottom: 104%; opacity: 0; transform: scale(1.1); }
}

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(0.8); }
  50% { opacity: 1; transform: scale(1.3); }
}

.fall-slow { animation: fall-slow linear infinite; animation-fill-mode: both; }
.float-up { animation: float-up linear infinite; animation-fill-mode: both; }
.twinkle { animation: twinkle ease-in-out infinite; animation-fill-mode: both; }

/* =========================================================
   SPRITE DE LA MASCOTA Y MOVIMIENTO AUTÓNOMO
   ========================================================= */
.pet-sprite {
  width: 120px;
  height: 120px;
  --pet-scale: 1;
  border-radius: 50%;
  position: absolute;
  cursor: pointer;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
  background-image: radial-gradient(circle at 32% 26%, rgba(255,255,255,0.55) 0%, rgba(255,255,255,0) 45%);
  transform: scale(var(--pet-scale));
  transition: left 2.5s ease-in-out, transform 0.2s ease, background-color 0.3s ease, width 0.3s ease, height 0.3s ease, border-radius 0.3s ease;
  z-index: 4;
}

.pet-sprite:active {
  transform: scale(calc(var(--pet-scale) * 0.95));
}

.pet-glitter {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  border-radius: inherit;
  clip-path: inherit;
  opacity: 0;
  mix-blend-mode: screen;
  background-image:
    radial-gradient(circle at 18% 24%, rgba(255,255,255,0.95) 0 1.5px, transparent 2px),
    radial-gradient(circle at 72% 18%, rgba(255,255,255,0.8) 0 1px, transparent 1.8px),
    radial-gradient(circle at 42% 68%, rgba(255,255,255,0.75) 0 1.2px, transparent 2px),
    radial-gradient(circle at 86% 76%, rgba(255,255,255,0.9) 0 1.4px, transparent 2.2px);
  background-size: 38px 38px, 52px 52px, 46px 46px, 61px 61px;
  animation: glitter-drift 4s linear infinite;
}

.pet-glitter.enabled {
  opacity: var(--glitter-opacity, 0.55);
}

.pet-glitter::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  background: linear-gradient(120deg, transparent 25%, rgba(255,255,255,0.4) 45%, transparent 62%);
  background-size: 220% 100%;
  animation: slime-shine 3.8s ease-in-out infinite;
}

@keyframes glitter-drift {
  from { background-position: 0 0, 12px 4px, 0 0, 18px 12px; }
  to { background-position: 38px 38px, -40px 52px, 46px -46px, -43px 61px; }
}

@keyframes slime-shine {
  0%, 100% { background-position: 180% 0; opacity: 0.25; }
  45%, 60% { background-position: -80% 0; opacity: 0.75; }
}

.pet-sprite.size-small { --pet-scale: 0.7; }
.pet-sprite.size-medium { --pet-scale: 0.9; }
.pet-sprite.size-large { --pet-scale: 1.1; }

.wardrobe-top-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  align-items: start;
}

.pet-customization-panel {
  border: 1px solid #eadff2;
  border-radius: 12px;
  padding: 4px 8px 2px;
  background: rgba(255,255,255,0.72);
}

.pet-customization-panel h3 {
  margin: 0;
  text-align: center;
  color: #6a1b9a;
  font-size: 0.78rem;
}

.glitter-intensity {
  display: flex;
  align-items: center;
  gap: 7px;
  margin: 0 10px 8px;
  color: #6a1b9a;
  font-size: 0.7rem;
  font-weight: bold;
}

.glitter-intensity label {
  min-width: 76px;
}

.glitter-intensity input[type="range"] {
  flex: 1;
  accent-color: #ff4081;
}

.wardrobe-label {
  margin: 0 0 5px;
  color: #666;
  font-size: 0.8rem;
  font-weight: bold;
}

.color-picker {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.color-option {
  width: 28px;
  height: 28px;
  padding: 0;
  border: 2px solid #fff;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
}

.color-option.selected,
.shape-option.selected {
  outline: 2px solid #ff4081;
  outline-offset: 2px;
}

.shape-picker {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 6px;
  max-width: 280px;
  margin: 0 auto;
}

.shape-picker .shape-option {
  background: transparent;
  color: #6a1b9a;
  border: 2px solid transparent;
  padding: 6px 2px;
  font-size: 0.68rem;
}

.size-picker {
  display: flex;
  justify-content: center;
  gap: 8px;
  max-width: none;
  margin: 0 auto;
}

.size-picker .size-option {
  flex: 0 0 28px;
  width: 28px;
  height: 28px;
  padding: 0;
  background: transparent;
  color: inherit;
  border: 2px solid transparent;
  border-radius: 50%;
}

.size-preview {
  display: block;
  position: relative;
  width: 24px;
  height: 24px;
  overflow: visible;
}

.size-preview .pet-sprite {
  position: absolute;
  left: 50%;
  top: 50%;
  margin: -14px 0 0 -14px;
  pointer-events: none;
}

.size-picker .size-option.equipped {
  background: transparent;
  border-color: transparent;
  outline: 2px solid #ff4081;
  outline-offset: 1px;
}

/* =========================================================
   FORMAS DE LA MASCOTA (8 opciones elegibles en el Armario)
   ========================================================= */
.pet-sprite.shape-circle {
  width: 120px; height: 120px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-mochi {
  width: 138px; height: 96px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-onigiri {
  width: 120px; height: 118px;
  border-radius: 50% 50% 40% 40% / 62% 62% 30% 30%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-squircle {
  width: 120px; height: 120px;
  border-radius: 28%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-dumpling {
  width: 140px;
  height: 95px;
  border-radius: 50% 50% 35% 35% / 65% 65% 35% 35%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-dumpling::before {
  content: '';
  position: absolute;
  top: -5px;
  left: 50%;
  transform: translateX(-50%);
  width: 22px;
  height: 14px;
  background-color: inherit;
  border-radius: 50%;
  box-shadow:
    -18px 2px 0 -1px currentColor,
    18px 2px 0 -1px currentColor,
    -34px 8px 0 -3px currentColor,
    34px 8px 0 -3px currentColor;
}

.pet-sprite.shape-capsule {
  width: 92px; height: 142px;
  border-radius: 46px;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

/* Corazón vía clip-path (evita el truco de rotar el elemento,
   que chocaría con las animaciones de caminar/saltar) */
.pet-sprite.shape-heart {
  width: 120px; height: 112px;
  border-radius: 0;
  box-shadow: none;
  clip-path: path("M60,110 C15,82 0,54 0,32 C0,12 16,0 35,0 C50,0 60,9 60,20 C60,9 70,0 85,0 C104,0 120,12 120,32 C120,54 105,82 60,110 Z");
}

/* Cabeza de animalito: círculo + orejitas triangulares por pseudo-elementos */
.pet-sprite.shape-cathead {
  width: 120px; height: 120px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}
.pet-sprite.shape-cathead::before,
.pet-sprite.shape-cathead::after {
  content: '';
  position: absolute;
  top: -15px;
  width: 0;
  height: 0;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-bottom: 24px solid currentColor;
}
.pet-sprite.shape-cathead::before { left: 10px; transform: rotate(-12deg); }
.pet-sprite.shape-cathead::after { right: 10px; transform: rotate(12deg); }

/* Nube esponjosa: círculo base + bultos extra vía box-shadow (sin elementos de más) */
.pet-sprite.shape-cloud {
  width: 128px; height: 100px;
  border-radius: 50%;
  clip-path: none;
  box-shadow:
    0 8px 15px rgba(0,0,0,0.12),
    -34px 8px 0 -10px currentColor,
    34px 8px 0 -10px currentColor,
    -16px -16px 0 -16px currentColor,
    16px -16px 0 -16px currentColor;
}

/* Las siluetas del selector usan una base pequeña para que las tres
   escalas queden completas dentro de sus tarjetas. */
.size-preview .pet-sprite.shape-squircle {
  width: 28px;
  height: 28px;
  margin: -14px 0 0 -14px;
  background-color: #ff80ab;
  transform-origin: center;
}

.pet-sprite.walking {
  animation: walkWiggle 0.6s infinite alternate ease-in-out;
}

@keyframes walkWiggle {
  0% { transform: scale(var(--pet-scale)) translateY(0) rotate(-2deg); }
  100% { transform: scale(var(--pet-scale)) translateY(-6px) rotate(2deg); }
}

.happy-jump {
  animation: jump 0.5s ease !important;
}

@keyframes jump {
  0%, 100% { transform: scale(var(--pet-scale)) translateY(0); }
  50% { transform: scale(var(--pet-scale)) translateY(-25px); }
}

.kawaii-face {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.eyes {
  display: flex;
  gap: 28px;
  margin-top: 10px;
}

.eye {
  width: 14px;
  height: 14px;
  background: #333;
  border-radius: 50%;
  position: relative;
}

.sparkle {
  color: #fff;
  font-size: 8px;
  position: absolute;
  top: -2px;
  right: -1px;
}

.cheeks {
  display: flex;
  gap: 40px;
  position: absolute;
  top: 48%;
}

.cheek {
  width: 14px;
  height: 8px;
  background: #ff4081;
  opacity: 0.4;
  border-radius: 50%;
}

.mouth {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-top: 2px;
}

.extra-prop {
  position: absolute;
  top: -10px;
  right: -5px;
  font-size: 1.2rem;
}

/* EXPRESIONES FACIALES */
.kawaii-face.sleeping .eye {
  height: 3px !important;
  border-radius: 2px !important;
  background: #333 !important;
  margin-top: 15px !important;
}

.kawaii-face.sleeping .sparkle {
  display: none !important;
}

.kawaii-face.sick .eye {
  height: 4px;
  border-radius: 2px;
}

/* =========================================================
   ACCESORIOS Y PROPORCIONES ESPECÍFICAS
   ========================================================= */
.accessory {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 5;
}

.accessory img {
  position: absolute;
  object-fit: contain;
  left: 50%;
  transform: translateX(calc(-50% + var(--accessory-x, 0px))) scale(var(--accessory-scale, 1)) rotate(var(--accessory-rotate, 0deg));
  transform-origin: center center;
}

/* Sombreros y Coronas (Cabeza) */
.accessory.pos-head img {
  top: calc(var(--accessory-top, -28px) + var(--accessory-y, 0px));
  width: 65px;
  height: 65px;
  --accessory-scale: 1.1;
}

/* Lentes y Gafas (Ojos) */
.accessory.pos-eyes img {
  top: calc(var(--accessory-top, 26px) + var(--accessory-y, 0px));
  width: 50px;
  height: 35px;
  --accessory-scale: 1.05;
}

/* Diademas y Orejas Grandes (Orejas) */
.accessory.pos-ears img {
  top: calc(var(--accessory-top, -22px) + var(--accessory-y, 0px));
  width: 85px;
  height: 60px;
  --accessory-scale: 1.1;
}

/* Moños y Varitas (Cuello) */
.accessory.pos-neck img {
  top: calc(var(--accessory-top, 78px) + var(--accessory-y, 0px));
  width: 45px;
  height: 40px;
  --accessory-scale: 1.1;
}

/* Las formas anchas necesitan accesorios algo mayores y más bajos. */
.pet-sprite.shape-dumpling .accessory.pos-head img,
.pet-sprite.shape-mochi .accessory.pos-head img {
  --accessory-scale: 1.3;
  --accessory-top: -25px;
}

.pet-sprite.shape-dumpling .accessory.pos-eyes img,
.pet-sprite.shape-mochi .accessory.pos-eyes img {
  --accessory-scale: 1.15;
  --accessory-top: 22px;
}

.pet-sprite.shape-dumpling .accessory.pos-ears img,
.pet-sprite.shape-mochi .accessory.pos-ears img {
  --accessory-scale: 1.2;
  --accessory-top: -18px;
}

.pet-sprite.shape-dumpling .accessory.pos-neck img,
.pet-sprite.shape-mochi .accessory.pos-neck img {
  --accessory-scale: 1.15;
  --accessory-top: 55px;
}

.heart-particle {
  position: absolute;
  font-size: 1.2rem;
  animation: floatHeart 0.8s ease-out forwards;
  pointer-events: none;
  z-index: 12;
}

@keyframes floatHeart {
  0% { transform: translate(0, 0) scale(0.6); opacity: 1; }
  100% { transform: translate(var(--dx), -50px) scale(1.2); opacity: 0; }
}

/* =========================================================
   NAVEGACIÓN INFERIOR Y MODALES (CATÁLOGO GRID 2 COLUMNAS)
   ========================================================= */
.bottom-nav {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 6px;
  padding: 10px;
  background: #fdf8ff;
  border-top: 2px solid #f0e6ff;
  z-index: 5;
}

.bottom-nav button {
  background: #ffffff;
  border: 2px solid #e1bee7;
  border-radius: 14px;
  padding: 8px 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  transition: transform 0.1s, background-color 0.2s;
}

.bottom-nav button:active {
  transform: scale(0.95);
  background: #f3e5f5;
}

.btn-icon { font-size: 1.2rem; }
.btn-text { font-size: 0.72rem; font-weight: bold; color: #4a148c; margin-top: 2px; }

.modal {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: rgba(255, 255, 255, 0.96);
  z-index: 20;
  padding: 20px 14px;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}

.modal.hidden { display: none; }

.modal::-webkit-scrollbar { width: 5px; }
.modal::-webkit-scrollbar-thumb { background: #ce93d8; border-radius: 5px; }

.close-btn {
  align-self: flex-end;
  background: #ff80ab;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 12px;
  font-weight: bold;
  cursor: pointer;
  margin-bottom: 8px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  max-height: 420px;
  overflow-y: auto;
  padding: 6px;
  margin-top: 8px;
}

.grid-container::-webkit-scrollbar { width: 4px; }
.grid-container::-webkit-scrollbar-thumb { background: #ce93d8; border-radius: 4px; }

.store-tabs {
  display: flex;
  gap: 6px;
  position: absolute;
  left: 14px;
  right: 14px;
  bottom: 132px;
  padding: 7px 0;
  max-width: none;
  background: rgba(255,255,255,0.98);
  border-top: 1px solid #eadff2;
  z-index: 3;
}

.store-tab {
  flex: 1;
  padding: 6px 4px;
  border: 1px solid #d1c4e9;
  border-radius: 10px;
  background: #fff;
  color: #6a1b9a;
  font-size: 0.72rem;
  font-weight: bold;
  cursor: pointer;
}

.store-tab.active {
  background: #ff80ab;
  border-color: #ff4081;
  color: #fff;
}

.item-card {
  background: #fdf8ff;
  border: 2px solid #e1bee7;
  border-radius: 16px;
  padding: 10px 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  gap: 4px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.05);
}

.card-icon { width: 45px; height: 45px; object-fit: contain; }
.card-title { font-size: 0.8rem; font-weight: bold; color: #4a148c; text-align: center; }
.card-price { font-size: 0.75rem; font-weight: 600; color: #7b1fa2; }

/* Mini-preview real del escenario en la card de la tienda/armario */
.bg-preview {
  width: 100%;
  height: 52px;
  border-radius: 10px;
  position: relative;
  overflow: hidden;
  border: 2px solid rgba(0,0,0,0.08);
}

.bg-preview-badge {
  position: absolute;
  bottom: 2px;
  right: 4px;
  font-size: 1.05rem;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,0.4));
}

/* Looks guardados (combos de accesorio + fondo + color) */
.look-chip {
  background: #fff;
  border: 2px solid #d1c4e9;
  border-radius: 12px;
  padding: 4px 8px;
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.72rem;
  font-weight: bold;
  color: #4a148c;
}

.look-chip-actions { display: flex; gap: 2px; }
.look-chip-actions button {
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 0.85rem;
  padding: 0 2px;
}

.card-btn {
  width: 100%;
  padding: 6px 0;
  border-radius: 10px;
  border: none;
  background: #b388ff;
  color: white;
  font-weight: bold;
  font-size: 0.75rem;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
}

.card-btn:active { transform: scale(0.96); }
.card-btn.owned { background: #ccc; cursor: default; }
.card-btn.equipped { background: #ff4081; }

/* =========================================================
   ANIMACIÓN CONTEO REGRESIVO MINIJUEGOS
   ========================================================= */
.kawaii-countdown {
  font-size: 4rem;
  font-weight: 800;
  color: #ff4081;
  text-shadow: 2px 2px 0px #ba68c8, 4px 4px 0px #f0e6ff;
  animation: popNum 0.6s ease-out;
}

/* =========================================================
   HUD DE MINIJUEGOS (marcador + mejor puntaje + tiempo)
   ========================================================= */
.game-hud {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin: 4px auto 2px;
  flex-wrap: wrap;
}

.hud-chip {
  background: #fff3e0;
  border: 2px solid #ffd54f;
  border-radius: 14px;
  padding: 3px 10px;
  font-size: 0.78rem;
  font-weight: 800;
  color: #4a148c;
  box-shadow: 0 2px 0 #ffca28;
}

.hud-best {
  background: #ede7f6;
  border-color: #b39ddb;
  color: #4527a0;
  box-shadow: 0 2px 0 #9575cd;
}

.hud-timer {
  background: #ffebee;
  border-color: #ef9a9a;
  color: #c62828;
  box-shadow: 0 2px 0 #e57373;
}

.result-record {
  background: #fff9c4;
  border: 2px solid #ffd54f;
  color: #f57f17;
  border-radius: 14px;
  padding: 4px 14px;
  font-weight: 800;
  font-size: 0.85rem;
  margin-bottom: 10px;
  animation: resultBounce 1s ease-in-out infinite;
}

/* =========================================================
   PANTALLA DE RESULTADO DE MINIJUEGOS
   ========================================================= */
.result-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
  animation: resultPop 0.4s ease-out;
}

.result-emoji {
  font-size: 3.4rem;
  animation: resultBounce 1.2s ease-in-out infinite;
}

.result-title {
  color: #6a1b9a;
  font-size: 1.15rem;
  margin: 8px 0 14px;
  text-align: center;
}

.result-coins-box {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #fff3e0;
  border: 2px solid #ffd54f;
  border-radius: 20px;
  padding: 8px 22px;
  font-size: 1.4rem;
  font-weight: 800;
  color: #e65100;
  box-shadow: 0 3px 0 #ffca28;
  margin-bottom: 22px;
}

.result-actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 80%;
}

.result-btn-again { background: #ff4081; }
.result-btn-menu { background: #b388ff; }

@keyframes resultPop {
  0% { transform: scale(0.85); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes resultBounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

@keyframes popNum {
  0% { transform: scale(0.3); opacity: 0; }
  70% { transform: scale(1.2); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
```

### Archivo: `css/base.css`
```css
/* =========================================================
   BASE.CSS — Reseteo, contenedor principal, barra superior,
   navegación inferior, sistema de modales y grillas de cards
   (tienda/armario). Todo lo estructural compartido por la app.
   ========================================================= */

/* =========================================================
   RESETEO GLOBAL Y OPTIMIZACIÓN TÁCTIL (MÓVIL / TABLET)
   ========================================================= */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

body {
  background-color: #f0e6ff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  overscroll-behavior: none;
  touch-action: manipulation;
}

/* =========================================================
   CONTENEDOR PRINCIPAL (MARCO DE APP MÓVIL)
   ========================================================= */
#app-container {
  width: 100%;
  max-width: 380px;
  height: 100vh;
  max-height: 660px;
  background: #ffffff;
  border-radius: 24px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  border: 4px solid #d4b2ff;
  touch-action: manipulation;
}

/* =========================================================
   BARRA SUPERIOR Y GLOBO DE PENSAMIENTO
   ========================================================= */
.top-bar {
  display: flex;
  justify-content: space-around;
  padding: 12px 8px;
  background: #fdf8ff;
  border-bottom: 2px solid #f0e6ff;
  z-index: 5;
}

.stat-box {
  background: #ffffff;
  padding: 6px 10px;
  border-radius: 12px;
  font-size: 0.82rem;
  font-weight: bold;
  color: #4a148c;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  border: 1px solid #e1bee7;
}

.thought-bubble {
  position: absolute;
  top: 62px;
  left: 50%;
  transform: translateX(-50%);
  background: #ffffff;
  padding: 8px 16px;
  border-radius: 18px;
  font-size: 0.85rem;
  font-weight: 600;
  color: #4a148c;
  border: 2px solid #ce93d8;
  box-shadow: 0 4px 10px rgba(0,0,0,0.08);
  z-index: 10;
  max-width: 80%;
  text-align: center;
  width: max-content;  /* Hace que la burbuja tome el ancho de la frase en una sola línea... */
  max-width: 92%;      /* ...pero si la frase supera el 92% de la pantalla, la obliga a bajar de renglón */
}


/* =========================================================
   NAVEGACIÓN INFERIOR Y MODALES (CATÁLOGO GRID 2 COLUMNAS)
   ========================================================= */
.bottom-nav {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 6px;
  padding: 10px;
  background: #fdf8ff;
  border-top: 2px solid #f0e6ff;
  z-index: 5;
}

.bottom-nav button {
  background: #ffffff;
  border: 2px solid #e1bee7;
  border-radius: 14px;
  padding: 8px 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  transition: transform 0.1s, background-color 0.2s;
}

.bottom-nav button:active {
  transform: scale(0.95);
  background: #f3e5f5;
}

.btn-icon { font-size: 1.2rem; }
.btn-text { font-size: 0.72rem; font-weight: bold; color: #4a148c; margin-top: 2px; }

.modal {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: rgba(255, 255, 255, 0.96);
  z-index: 20;
  padding: 20px 14px;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}

.modal.hidden { display: none; }

.modal::-webkit-scrollbar { width: 5px; }
.modal::-webkit-scrollbar-thumb { background: #ce93d8; border-radius: 5px; }

.close-btn {
  align-self: flex-end;
  background: #ff80ab;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 12px;
  font-weight: bold;
  cursor: pointer;
  margin-bottom: 8px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  max-height: 420px;
  overflow-y: auto;
  padding: 6px;
  margin-top: 8px;
}

.grid-container::-webkit-scrollbar { width: 4px; }
.grid-container::-webkit-scrollbar-thumb { background: #ce93d8; border-radius: 4px; }

.store-tabs {
  display: flex;
  gap: 6px;
  position: absolute;
  left: 14px;
  right: 14px;
  bottom: 132px;
  padding: 7px 0;
  max-width: none;
  background: rgba(255,255,255,0.98);
  border-top: 1px solid #eadff2;
  z-index: 3;
}

.store-tab {
  flex: 1;
  padding: 6px 4px;
  border: 1px solid #d1c4e9;
  border-radius: 10px;
  background: #fff;
  color: #6a1b9a;
  font-size: 0.72rem;
  font-weight: bold;
  cursor: pointer;
}

.store-tab.active {
  background: #ff80ab;
  border-color: #ff4081;
  color: #fff;
}

.item-card {
  background: #fdf8ff;
  border: 2px solid #e1bee7;
  border-radius: 16px;
  padding: 10px 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  gap: 4px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.05);
}

.card-icon { width: 45px; height: 45px; object-fit: contain; }
.card-title { font-size: 0.8rem; font-weight: bold; color: #4a148c; text-align: center; }
.card-price { font-size: 0.75rem; font-weight: 600; color: #7b1fa2; }

/* Mini-preview real del escenario en la card de la tienda/armario */
.bg-preview {
  width: 100%;
  height: 52px;
  border-radius: 10px;
  position: relative;
  overflow: hidden;
  border: 2px solid rgba(0,0,0,0.08);
}

.bg-preview-badge {
  position: absolute;
  bottom: 2px;
  right: 4px;
  font-size: 1.05rem;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,0.4));
}

/* Looks guardados (combos de accesorio + fondo + color) */
.look-chip {
  background: #fff;
  border: 2px solid #d1c4e9;
  border-radius: 12px;
  padding: 4px 8px;
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.72rem;
  font-weight: bold;
  color: #4a148c;
}

.look-chip-actions { display: flex; gap: 2px; }
.look-chip-actions button {
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 0.85rem;
  padding: 0 2px;
}

.card-btn {
  width: 100%;
  padding: 6px 0;
  border-radius: 10px;
  border: none;
  background: #b388ff;
  color: white;
  font-weight: bold;
  font-size: 0.75rem;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
}

.card-btn:active { transform: scale(0.96); }
.card-btn.owned { background: #ccc; cursor: default; }
.card-btn.equipped { background: #ff4081; }


```

### Archivo: `css/escenarios.css`
```css
/* =========================================================
   ESCENARIOS.CSS — Los 17 fondos (.room.bg_*) con sus muebles
   fijos en CSS puro, más el sistema de partículas ambientales
   (#bg-particles, .fall-slow/.float-up/.twinkle).
   ========================================================= */

/* =========================================================
   ESCENARIOS DETALLADOS CON MUEBLES FIJOS (CSS PURO)
   ========================================================= */
.room {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  transition: background 0.4s ease;
  overflow: hidden;
}

/* 1. Sala de Estar: Lámpara de pie y Cuadro */
.room.bg_living {
  background: 
    linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%),
    linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%);
}

.room.bg_living::before {
  content: '';
  position: absolute;
  left: 20px;
  bottom: 28%;
  width: 35px;
  height: 110px;
  background: 
    radial-gradient(ellipse at top, #ffee58 60%, transparent 65%),
    linear-gradient(to right, transparent 45%, #8d6e63 45%, #8d6e63 55%, transparent 55%);
  background-size: 100% 35px, 100% 100%;
  background-repeat: no-repeat;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
}

.room.bg_living::after {
  content: '🖼️';
  font-size: 1.8rem;
  position: absolute;
  right: 25px;
  top: 18%;
  padding: 4px;
  background: #ffffff;
  border: 3px solid #8d6e63;
  border-radius: 6px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

/* 2. Dormitorio: Ventana con Luna y Cama */
.room.bg_bedroom {
  background: 
    linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%),
    radial-gradient(circle at 80% 20%, #fff 2px, transparent 3px),
    radial-gradient(circle at 20% 35%, #fff 2px, transparent 3px),
    linear-gradient(180deg, #311b92 0%, #512da8 72%);
}

.room.bg_bedroom::before {
  content: '🌙';
  font-size: 1.6rem;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 20px;
  top: 15%;
  width: 55px;
  height: 55px;
  background: #1a237e;
  border: 4px solid #ffffff;
  border-radius: 50%;
  box-shadow: 0 0 12px rgba(255,255,255,0.3);
}

.room.bg_bedroom::after {
  content: '';
  position: absolute;
  right: -10px;
  bottom: 25%;
  width: 90px;
  height: 50px;
  background: 
    radial-gradient(ellipse at center, #ffffff 50%, transparent 55%),
    linear-gradient(to right, #ff80ab, #f50057);
  background-size: 40px 20px, 100% 35px;
  background-position: 10px 0, 0 15px;
  background-repeat: no-repeat;
  border-radius: 12px 0 0 8px;
}

/* 3. Sala de Juegos: Repisa con Juguetes y Pelota */
.room.bg_playroom {
  background: 
    linear-gradient(to bottom, transparent 70%, #80deea 70%, #4dd0e1 100%),
    radial-gradient(#ff80ab 15%, transparent 16%),
    radial-gradient(#ffd54f 15%, transparent 16%),
    #f3e5f5;
  background-size: 100% 100%, 40px 40px, 40px 40px, 100% 100%;
  background-position: 0 0, 0 0, 20px 20px, 0 0;
}

.room.bg_playroom::before {
  content: '🧸🤖';
  font-size: 1.2rem;
  letter-spacing: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  left: 15px;
  top: 22%;
  width: 85px;
  height: 12px;
  background: #ffb74d;
  border-radius: 4px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.room.bg_playroom::after {
  content: '⚽';
  font-size: 1.8rem;
  position: absolute;
  right: 20px;
  bottom: 20%;
  filter: drop-shadow(0 4px 4px rgba(0,0,0,0.15));
}

/* 4. Parque Mágico: Sol Giratorio y Árbol */
.room.bg_park {
  background: 
    linear-gradient(to bottom, transparent 68%, #aed581 68%, #7cb342 100%),
    linear-gradient(180deg, #81d4fa 0%, #e1f5fe 68%);
}

.room.bg_park::before {
  content: '☀️';
  font-size: 2.2rem;
  position: absolute;
  right: 20px;
  top: 12%;
  animation: spinSun 20s linear infinite;
}

@keyframes spinSun {
  100% { transform: rotate(360deg); }
}

.room.bg_park::after {
  content: '🌳';
  font-size: 3.8rem;
  position: absolute;
  left: -10px;
  bottom: 22%;
  filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.12));
}

/* 5. Playa Tropical */
.room.bg_beach {
  background: 
    linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%),
    linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%);
}

.room.bg_beach::before {
  content: '🌴';
  font-size: 3.8rem;
  position: absolute;
  right: -5px;
  bottom: 22%;
  filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.15));
}

.room.bg_beach::after {
  content: '🏖️';
  font-size: 2.5rem;
  position: absolute;
  left: 15px;
  bottom: 24%;
  filter: drop-shadow(0 3px 5px rgba(0,0,0,0.1));
}

/* 6. Bosque Encantado */
.room.bg_forest {
  background: 
    linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%),
    linear-gradient(180deg, #aed581 0%, #558b2f 65%);
}

.room.bg_forest::before {
  content: '🌲';
  font-size: 3.5rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_forest::after {
  content: '🍄';
  font-size: 1.8rem;
  position: absolute;
  right: 25px;
  bottom: 20%;
}

/* 7. Montaña Nevada */
.room.bg_mountain {
  background: 
    linear-gradient(to bottom, transparent 70%, #cfd8dc 70%, #90a4ae 100%),
    linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);
}

.room.bg_mountain::before {
  content: '🏔️';
  font-size: 4rem;
  position: absolute;
  right: -10px;
  top: 15%;
  opacity: 0.8;
}

.room.bg_mountain::after {
  content: '☃️';
  font-size: 2.2rem;
  position: absolute;
  left: 20px;
  bottom: 22%;
}

/* 8. Campamento Nocturno */
.room.bg_camping {
  background: 
    linear-gradient(to bottom, transparent 72%, #2e7d32 72%, #1b5e20 100%),
    radial-gradient(circle at 75% 20%, #fff9c4 3px, transparent 4px),
    radial-gradient(circle at 25% 15%, #fff9c4 2px, transparent 3px),
    linear-gradient(180deg, #0d1b2a 0%, #1b263b 72%);
}

.room.bg_camping::before {
  content: '⛺';
  font-size: 2.8rem;
  position: absolute;
  left: 15px;
  bottom: 22%;
}

.room.bg_camping::after {
  content: '🔥';
  font-size: 2rem;
  position: absolute;
  right: 35px;
  bottom: 21%;
}

/* 9. Oasis del Desierto */
.room.bg_desert {
  background: 
    linear-gradient(to bottom, transparent 68%, #f57c00 68%, #e65100 100%),
    linear-gradient(180deg, #ffe0b2 0%, #ffcc80 68%);
}

.room.bg_desert::before {
  content: '🏜️';
  font-size: 3.2rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_desert::after {
  content: '🐪';
  font-size: 2.4rem;
  position: absolute;
  right: 15px;
  bottom: 22%;
}

/* 10. Cocina del Chef */
.room.bg_kitchen {
  background: 
    linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%),
    linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%);
}

.room.bg_kitchen::before {
  content: '🍳';
  font-size: 2.5rem;
  position: absolute;
  left: 20px;
  top: 20%;
}

.room.bg_kitchen::after {
  content: '🍕';
  font-size: 2rem;
  position: absolute;
  right: 25px;
  bottom: 22%;
}

/* 11. Baño de Burbujas */
.room.bg_bathroom {
  background: 
    linear-gradient(to bottom, transparent 70%, #b2ebf2 70%, #4dd0e1 100%),
    linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);
}

.room.bg_bathroom::before {
  content: '🛁';
  font-size: 3rem;
  position: absolute;
  right: 15px;
  bottom: 22%;
}

.room.bg_bathroom::after {
  content: '🦆';
  font-size: 1.8rem;
  position: absolute;
  left: 25px;
  bottom: 22%;
}

/* 12. Dojo Ninja */
.room.bg_dojo {
  background: 
    linear-gradient(to bottom, transparent 70%, #8d6e63 70%, #4e342e 100%),
    linear-gradient(180deg, #efebe9 0%, #d7ccc8 70%);
}

.room.bg_dojo::before {
  content: '⛩️';
  font-size: 3.5rem;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 15%;
}

/* 13. Pista de Baile Neo */
.room.bg_disco {
  background: 
    linear-gradient(to bottom, transparent 70%, #311b92 70%, #1a237e 100%),
    linear-gradient(135deg, #4a148c 0%, #880e4f 50%, #311b92 100%);
}

.room.bg_disco::before {
  content: '🪩';
  font-size: 3rem;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  animation: floatHeart 2s ease-in-out infinite alternate;
}

/* 14. Estación Espacial */
.room.bg_space {
  background: 
    linear-gradient(to bottom, transparent 75%, #263238 75%, #102027 100%),
    radial-gradient(circle at 50% 30%, #37474f 0%, #000000 100%);
}

.room.bg_space::before {
  content: '🚀';
  font-size: 3rem;
  position: absolute;
  right: 20px;
  top: 15%;
}

.room.bg_space::after {
  content: '🪐';
  font-size: 2.8rem;
  position: absolute;
  left: 15px;
  top: 25%;
}

/* 15. Castillo Real */
.room.bg_castle {
  background: 
    linear-gradient(to bottom, transparent 70%, #9c27b0 70%, #4a148c 100%),
    linear-gradient(180deg, #f3e5f5 0%, #ce93d8 70%);
}

.room.bg_castle::before {
  content: '🏰';
  font-size: 3.5rem;
  position: absolute;
  right: 15px;
  top: 15%;
}

/* 16. Mundo Submarino */
.room.bg_underwater {
  background: 
    linear-gradient(to bottom, transparent 70%, #004d40 70%, #00251a 100%),
    linear-gradient(180deg, #00838f 0%, #006064 70%);
}

.room.bg_underwater::before {
  content: '🪸';
  font-size: 3rem;
  position: absolute;
  left: 10px;
  bottom: 22%;
}

.room.bg_underwater::after {
  content: '🐠';
  font-size: 2rem;
  position: absolute;
  right: 20px;
  top: 30%;
}

/* 17. Guarida Volcánica */
.room.bg_volcano {
  background: 
    linear-gradient(to bottom, transparent 70%, #3e2723 70%, #1b0000 100%),
    linear-gradient(180deg, #bf360c 0%, #dd2c00 70%);
}

.room.bg_volcano::before {
  content: '🌋';
  font-size: 3.5rem;
  position: absolute;
  right: 10px;
  top: 15%;
}

/* =========================================================
   SISTEMA DE PARTÍCULAS DINÁMICAS
   ========================================================= */
#bg-particles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  z-index: 2;
}

.particle {
  position: absolute;
  font-size: 1.2rem;
  opacity: 0.8;
}

/* Animaciones de Partículas */
@keyframes fall-slow {
  0% { top: -8%; opacity: 0; transform: rotate(0deg); }
  15% { opacity: 0.9; }
  85% { opacity: 0.9; }
  100% { top: 104%; opacity: 0; transform: rotate(280deg); }
}

@keyframes float-up {
  0% { bottom: -8%; opacity: 0; transform: scale(0.7); }
  15% { opacity: 0.85; }
  85% { opacity: 0.85; }
  100% { bottom: 104%; opacity: 0; transform: scale(1.1); }
}

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(0.8); }
  50% { opacity: 1; transform: scale(1.3); }
}

.fall-slow { animation: fall-slow linear infinite; animation-fill-mode: both; }
.float-up { animation: float-up linear infinite; animation-fill-mode: both; }
.twinkle { animation: twinkle ease-in-out infinite; animation-fill-mode: both; }


```

### Archivo: `css/juegos.css`
```css
/* =========================================================
   JUEGOS.CSS — Conteo regresivo, HUD (marcador/mejor puntaje/
   tiempo) y pantalla de resultado, compartidos por los 6
   minijuegos.
   ========================================================= */

/* =========================================================
   ANIMACIÓN CONTEO REGRESIVO MINIJUEGOS
   ========================================================= */
.kawaii-countdown {
  font-size: 4rem;
  font-weight: 800;
  color: #ff4081;
  text-shadow: 2px 2px 0px #ba68c8, 4px 4px 0px #f0e6ff;
  animation: popNum 0.6s ease-out;
}

/* =========================================================
   HUD DE MINIJUEGOS (marcador + mejor puntaje + tiempo)
   ========================================================= */
.game-hud {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin: 4px auto 2px;
  flex-wrap: wrap;
}

.hud-chip {
  background: #fff3e0;
  border: 2px solid #ffd54f;
  border-radius: 14px;
  padding: 3px 10px;
  font-size: 0.78rem;
  font-weight: 800;
  color: #4a148c;
  box-shadow: 0 2px 0 #ffca28;
}

.hud-best {
  background: #ede7f6;
  border-color: #b39ddb;
  color: #4527a0;
  box-shadow: 0 2px 0 #9575cd;
}

.hud-timer {
  background: #ffebee;
  border-color: #ef9a9a;
  color: #c62828;
  box-shadow: 0 2px 0 #e57373;
}

.result-record {
  background: #fff9c4;
  border: 2px solid #ffd54f;
  color: #f57f17;
  border-radius: 14px;
  padding: 4px 14px;
  font-weight: 800;
  font-size: 0.85rem;
  margin-bottom: 10px;
  animation: resultBounce 1s ease-in-out infinite;
}

/* =========================================================
   PANTALLA DE RESULTADO DE MINIJUEGOS
   ========================================================= */
.result-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
  animation: resultPop 0.4s ease-out;
}

.result-emoji {
  font-size: 3.4rem;
  animation: resultBounce 1.2s ease-in-out infinite;
}

.result-title {
  color: #6a1b9a;
  font-size: 1.15rem;
  margin: 8px 0 14px;
  text-align: center;
}

.result-coins-box {
  display: flex;
  align-items: center;
  gap: 6px;
  background: #fff3e0;
  border: 2px solid #ffd54f;
  border-radius: 20px;
  padding: 8px 22px;
  font-size: 1.4rem;
  font-weight: 800;
  color: #e65100;
  box-shadow: 0 3px 0 #ffca28;
  margin-bottom: 22px;
}

.result-actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 80%;
}

.result-btn-again { background: #ff4081; }
.result-btn-menu { background: #b388ff; }

@keyframes resultPop {
  0% { transform: scale(0.85); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes resultBounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

@keyframes popNum {
  0% { transform: scale(0.3); opacity: 0; }
  70% { transform: scale(1.2); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
```

### Archivo: `css/mascota.css`
```css
/* =========================================================
   MASCOTA.CSS — Sprite de la mascota, movimiento autónomo,
   las 8 formas elegibles (.shape-*), rostro/expresiones y
   posicionamiento de accesorios (.pos-*).
   ========================================================= */

/* =========================================================
   SPRITE DE LA MASCOTA Y MOVIMIENTO AUTÓNOMO
   ========================================================= */
.pet-sprite {
  width: 120px;
  height: 120px;
  --pet-scale: 1;
  border-radius: 50%;
  position: absolute;
  cursor: pointer;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
  background-image: radial-gradient(circle at 32% 26%, rgba(255,255,255,0.55) 0%, rgba(255,255,255,0) 45%);
  transform: scale(var(--pet-scale));
  transition: left 2.5s ease-in-out, transform 0.2s ease, background-color 0.3s ease, width 0.3s ease, height 0.3s ease, border-radius 0.3s ease;
  z-index: 4;
}

.pet-sprite:active {
  transform: scale(calc(var(--pet-scale) * 0.95));
}

.pet-glitter {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  border-radius: inherit;
  clip-path: inherit;
  opacity: 0;
  mix-blend-mode: screen;
  background-image:
    radial-gradient(circle at 18% 24%, rgba(255,255,255,0.95) 0 1.5px, transparent 2px),
    radial-gradient(circle at 72% 18%, rgba(255,255,255,0.8) 0 1px, transparent 1.8px),
    radial-gradient(circle at 42% 68%, rgba(255,255,255,0.75) 0 1.2px, transparent 2px),
    radial-gradient(circle at 86% 76%, rgba(255,255,255,0.9) 0 1.4px, transparent 2.2px);
  background-size: 38px 38px, 52px 52px, 46px 46px, 61px 61px;
  animation: glitter-drift 4s linear infinite;
}

.pet-glitter.enabled {
  opacity: var(--glitter-opacity, 0.55);
}

.pet-glitter::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  background: linear-gradient(120deg, transparent 25%, rgba(255,255,255,0.4) 45%, transparent 62%);
  background-size: 220% 100%;
  animation: slime-shine 3.8s ease-in-out infinite;
}

@keyframes glitter-drift {
  from { background-position: 0 0, 12px 4px, 0 0, 18px 12px; }
  to { background-position: 38px 38px, -40px 52px, 46px -46px, -43px 61px; }
}

@keyframes slime-shine {
  0%, 100% { background-position: 180% 0; opacity: 0.25; }
  45%, 60% { background-position: -80% 0; opacity: 0.75; }
}

.pet-sprite.size-small { --pet-scale: 0.7; }
.pet-sprite.size-medium { --pet-scale: 0.9; }
.pet-sprite.size-large { --pet-scale: 1.1; }

.wardrobe-top-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  align-items: start;
}

.pet-customization-panel {
  border: 1px solid #eadff2;
  border-radius: 12px;
  padding: 4px 8px 2px;
  background: rgba(255,255,255,0.72);
}

.pet-customization-panel h3 {
  margin: 0;
  text-align: center;
  color: #6a1b9a;
  font-size: 0.78rem;
}

.glitter-intensity {
  display: flex;
  align-items: center;
  gap: 7px;
  margin: 0 10px 8px;
  color: #6a1b9a;
  font-size: 0.7rem;
  font-weight: bold;
}

.glitter-intensity label {
  min-width: 76px;
}

.glitter-intensity input[type="range"] {
  flex: 1;
  accent-color: #ff4081;
}

.wardrobe-label {
  margin: 0 0 5px;
  color: #666;
  font-size: 0.8rem;
  font-weight: bold;
}

.color-picker {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.color-option {
  width: 28px;
  height: 28px;
  padding: 0;
  border: 2px solid #fff;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
}

.color-option.selected,
.shape-option.selected {
  outline: 2px solid #ff4081;
  outline-offset: 2px;
}

.shape-picker {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 6px;
  max-width: 280px;
  margin: 0 auto;
}

.shape-picker .shape-option {
  background: transparent;
  color: #6a1b9a;
  border: 2px solid transparent;
  padding: 6px 2px;
  font-size: 0.68rem;
}

.size-picker {
  display: flex;
  justify-content: center;
  gap: 8px;
  max-width: none;
  margin: 0 auto;
}

.size-picker .size-option {
  flex: 0 0 28px;
  width: 28px;
  height: 28px;
  padding: 0;
  background: transparent;
  color: inherit;
  border: 2px solid transparent;
  border-radius: 50%;
}

.size-preview {
  display: block;
  position: relative;
  width: 24px;
  height: 24px;
  overflow: visible;
}

.size-preview .pet-sprite {
  position: absolute;
  left: 50%;
  top: 50%;
  margin: -14px 0 0 -14px;
  pointer-events: none;
}

.size-picker .size-option.equipped {
  background: transparent;
  border-color: transparent;
  outline: 2px solid #ff4081;
  outline-offset: 1px;
}

/* =========================================================
   FORMAS DE LA MASCOTA (8 opciones elegibles en el Armario)
   ========================================================= */
.pet-sprite.shape-circle {
  width: 120px; height: 120px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-mochi {
  width: 138px; height: 96px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-onigiri {
  width: 120px; height: 118px;
  border-radius: 50% 50% 40% 40% / 62% 62% 30% 30%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-squircle {
  width: 120px; height: 120px;
  border-radius: 28%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-dumpling {
  width: 140px;
  height: 95px;
  border-radius: 50% 50% 35% 35% / 65% 65% 35% 35%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

.pet-sprite.shape-dumpling::before {
  content: '';
  position: absolute;
  top: -5px;
  left: 50%;
  transform: translateX(-50%);
  width: 22px;
  height: 14px;
  background-color: inherit;
  border-radius: 50%;
  box-shadow:
    -18px 2px 0 -1px currentColor,
    18px 2px 0 -1px currentColor,
    -34px 8px 0 -3px currentColor,
    34px 8px 0 -3px currentColor;
}

.pet-sprite.shape-capsule {
  width: 92px; height: 142px;
  border-radius: 46px;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}

/* Corazón vía clip-path (evita el truco de rotar el elemento,
   que chocaría con las animaciones de caminar/saltar) */
.pet-sprite.shape-heart {
  width: 120px; height: 112px;
  border-radius: 0;
  box-shadow: none;
  clip-path: path("M60,110 C15,82 0,54 0,32 C0,12 16,0 35,0 C50,0 60,9 60,20 C60,9 70,0 85,0 C104,0 120,12 120,32 C120,54 105,82 60,110 Z");
}

/* Cabeza de animalito: círculo + orejitas triangulares por pseudo-elementos */
.pet-sprite.shape-cathead {
  width: 120px; height: 120px;
  border-radius: 50%;
  clip-path: none;
  box-shadow: 0 8px 15px rgba(0,0,0,0.12);
}
.pet-sprite.shape-cathead::before,
.pet-sprite.shape-cathead::after {
  content: '';
  position: absolute;
  top: -15px;
  width: 0;
  height: 0;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-bottom: 24px solid currentColor;
}
.pet-sprite.shape-cathead::before { left: 10px; transform: rotate(-12deg); }
.pet-sprite.shape-cathead::after { right: 10px; transform: rotate(12deg); }

/* Nube esponjosa: círculo base + bultos extra vía box-shadow (sin elementos de más) */
.pet-sprite.shape-cloud {
  width: 128px; height: 100px;
  border-radius: 50%;
  clip-path: none;
  box-shadow:
    0 8px 15px rgba(0,0,0,0.12),
    -34px 8px 0 -10px currentColor,
    34px 8px 0 -10px currentColor,
    -16px -16px 0 -16px currentColor,
    16px -16px 0 -16px currentColor;
}

/* Las siluetas del selector usan una base pequeña para que las tres
   escalas queden completas dentro de sus tarjetas. */
.size-preview .pet-sprite.shape-squircle {
  width: 28px;
  height: 28px;
  margin: -14px 0 0 -14px;
  background-color: #ff80ab;
  transform-origin: center;
}

.pet-sprite.walking {
  animation: walkWiggle 0.6s infinite alternate ease-in-out;
}

@keyframes walkWiggle {
  0% { transform: scale(var(--pet-scale)) translateY(0) rotate(-2deg); }
  100% { transform: scale(var(--pet-scale)) translateY(-6px) rotate(2deg); }
}

.happy-jump {
  animation: jump 0.5s ease !important;
}

@keyframes jump {
  0%, 100% { transform: scale(var(--pet-scale)) translateY(0); }
  50% { transform: scale(var(--pet-scale)) translateY(-25px); }
}

.kawaii-face {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.eyes {
  display: flex;
  gap: 28px;
  margin-top: 10px;
}

.eye {
  width: 14px;
  height: 14px;
  background: #333;
  border-radius: 50%;
  position: relative;
}

.sparkle {
  color: #fff;
  font-size: 8px;
  position: absolute;
  top: -2px;
  right: -1px;
}

.cheeks {
  display: flex;
  gap: 40px;
  position: absolute;
  top: 48%;
}

.cheek {
  width: 14px;
  height: 8px;
  background: #ff4081;
  opacity: 0.4;
  border-radius: 50%;
}

.mouth {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-top: 2px;
}

.extra-prop {
  position: absolute;
  top: -10px;
  right: -5px;
  font-size: 1.2rem;
}

/* EXPRESIONES FACIALES */
.kawaii-face.sleeping .eye {
  height: 3px !important;
  border-radius: 2px !important;
  background: #333 !important;
  margin-top: 15px !important;
}

.kawaii-face.sleeping .sparkle {
  display: none !important;
}

.kawaii-face.sick .eye {
  height: 4px;
  border-radius: 2px;
}

/* =========================================================
   ACCESORIOS Y PROPORCIONES ESPECÍFICAS
   ========================================================= */
.accessory {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 5;
}

.accessory img {
  position: absolute;
  object-fit: contain;
  left: 50%;
  transform: translateX(calc(-50% + var(--accessory-x, 0px))) scale(var(--accessory-scale, 1)) rotate(var(--accessory-rotate, 0deg));
  transform-origin: center center;
}

/* Sombreros y Coronas (Cabeza) */
.accessory.pos-head img {
  top: calc(var(--accessory-top, -28px) + var(--accessory-y, 0px));
  width: 65px;
  height: 65px;
  --accessory-scale: 1.1;
}

/* Lentes y Gafas (Ojos) */
.accessory.pos-eyes img {
  top: calc(var(--accessory-top, 26px) + var(--accessory-y, 0px));
  width: 50px;
  height: 35px;
  --accessory-scale: 1.05;
}

/* Diademas y Orejas Grandes (Orejas) */
.accessory.pos-ears img {
  top: calc(var(--accessory-top, -22px) + var(--accessory-y, 0px));
  width: 85px;
  height: 60px;
  --accessory-scale: 1.1;
}

/* Moños y Varitas (Cuello) */
.accessory.pos-neck img {
  top: calc(var(--accessory-top, 78px) + var(--accessory-y, 0px));
  width: 45px;
  height: 40px;
  --accessory-scale: 1.1;
}

/* Las formas anchas necesitan accesorios algo mayores y más bajos. */
.pet-sprite.shape-dumpling .accessory.pos-head img,
.pet-sprite.shape-mochi .accessory.pos-head img {
  --accessory-scale: 1.3;
  --accessory-top: -25px;
}

.pet-sprite.shape-dumpling .accessory.pos-eyes img,
.pet-sprite.shape-mochi .accessory.pos-eyes img {
  --accessory-scale: 1.15;
  --accessory-top: 22px;
}

.pet-sprite.shape-dumpling .accessory.pos-ears img,
.pet-sprite.shape-mochi .accessory.pos-ears img {
  --accessory-scale: 1.2;
  --accessory-top: -18px;
}

.pet-sprite.shape-dumpling .accessory.pos-neck img,
.pet-sprite.shape-mochi .accessory.pos-neck img {
  --accessory-scale: 1.15;
  --accessory-top: 55px;
}

.heart-particle {
  position: absolute;
  font-size: 1.2rem;
  animation: floatHeart 0.8s ease-out forwards;
  pointer-events: none;
  z-index: 12;
}

@keyframes floatHeart {
  0% { transform: translate(0, 0) scale(0.6); opacity: 1; }
  100% { transform: translate(var(--dx), -50px) scale(1.2); opacity: 0; }
}


```

### Archivo: `data/dialogues.json`
```json
{
  "happy": [
    "✨ ¡Hoy es un día genial!",
    "🎶 ¡La la la!",
    "⭐ ¡Gracias por cuidarme tanto!",
    "🌈 ¡Tu eres divertido!",
    "🐥 ¡Me siento lleno de energía y alegría!",
    "🍭 ¡Eres el mejor compañero del mundo!",
    "🥳 ¡Hoy va a ser un día increíble!",
    "❤️ ¡Mi corazoncito brilla cuando jugamos!"
  ],
  "hungry": [
    "🍕 ¡Tengo hambre! ¡Comida rica!",
    "🍗 Mmm... ¿Hay algo para picar?",
    "🧁 ¡Mi pancita está rugiendo!",
    "🍎 ¡Un bocadito por favor!",
    "🍔 ¿Es la hora del almuerzo?",
    "🍓 ¡Se me antoja algo dulce!",
    "🥨 ¡Tengo un huequito en la pancita!",
    "🥞 ¿Me invitas un postrecito?"
  ],
  "sick": [
    "🥺 Me siento mal... ¿Me das medicina?",
    "🤒 Me dolió la pancita...",
    "💊 Necesito un remedio por favor...",
    "💧 No me siento muy bien hoy...",
    "🩹 ¿Me pones un curita y me das un abrazo?",
    "🌡️ Tengo un poquito de fiebre...",
    "🧸 Necesito mimos para recuperarme..."
  ],
  "tired": [
    "😴 Tengo sueño... ¡A dormir!",
    "💤 Mis ojitos se cierran...",
    "🌙 ¿Podemos tomar una siesta?",
    "🥱 Qué cansancio... a descansar.",
    "🧸 ¿Me acuestas en mi camita?",
    "🌌 Ya no puedo tener los ojos abiertos..."
  ],
  "bored": [
    "🎮 ¿Jugamos un minijuego?",
    "🎈 ¡Me aburro! Vamos a jugar.",
    "🪙 ¡Quiero ganar monedas para la tienda!",
    "🎲 ¡Se me ocurre una gran idea! ¿Jugamos?",
    "🎯 ¡Demuestra tus habilidades en un juego!",
    "🎨 ¡Hagamos algo divertido juntos!"
  ],
  "sleeping": [
    "💤 Zzz... Descansando...",
    "🌙 Zzz... Soñando con caramelos...",
    "🐑 Zzz... Contando obejitas...",
    "☁️ Zzz... Durmiendo en una nube esponjosa...",
    "💤 Zzz... Mmm... pastelitos..."
  ],
  "alerts": {
    "full": "No preciso comer ahora.",
    "healthy": "No preciso un doctor ahora."
  }
}
```

### Archivo: `desarrollo/CONTEXTO-PROYECTO.md`
```md
# Contexto del proyecto Minipet

## Resumen

`minipet` es una mascota virtual hecha con Vanilla JavaScript, HTML y CSS. No usa React, TypeScript ni un sistema de build. Es una PWA sencilla que puede ejecutarse como sitio estático y publicarse en Netlify.

La mascota puede:

- Comer, dormir y recibir medicina.
- Cambiar felicidad, hambre y energía.
- Cambiar color de piel.
- Cambiar forma.
- Cambiar tamaño.
- Equipar accesorios.
- Cambiar escenario/fondo.
- Guardar looks.
- Jugar minijuegos.
- Mostrar frases desde un archivo JSON.
- Guardar el progreso en `localStorage`.

## Archivos principales

- `index.html`: estructura principal, mascota, navegación inferior y modal.
- `css/styles.css`: layout, mascota, formas, animaciones, accesorios, modal y escenarios CSS.
- `js/app.js`: inicialización de sonidos, Service Worker y navegación entre modales.
- `js/pet.js`: estado principal `PetState`, necesidades, movimiento, persistencia y actualización visual.
- `js/store.js`: tienda, accesorios, escenarios, looks y modal `Mi Pet`.
- `js/minigames.js`: minijuegos y monedas.
- `js/particulas.js`: partículas de los escenarios.
- `data/dialogues.json`: frases de la mascota y mensajes hablados.
- `sw.js`: Service Worker y caché PWA.
- `manifest.json`: configuración PWA.

## Navegación actual

La barra inferior tiene dos filas de cuatro columnas:

Primera fila:

- Comer
- Medicina
- Dormir
- Juegos

Segunda fila:

- Mi Pet
- Accesorios
- Escenario
- Tienda

No debe volver a aparecer un botón llamado `Armario`. La personalización se dividió en `Mi Pet`, `Accesorios` y `Escenario`.

## Modal Mi Pet

`Mi Pet` abre `Store.renderMyPet()` desde `switchTab('my-pet')`.

Contiene únicamente:

- Color de piel.
- Tamaño.
- Forma.
- Control de `Brillantina`.

El control de brillantina es una barra de 0 a 100:

- `0`: desactivada.
- `1` a `99`: intensidad progresiva.
- `100`: máxima intensidad.

La intensidad se guarda como `PetState.glitterIntensity` en `localStorage`. El valor predeterminado es `0`.

La forma de la mascota se guarda en `PetState.petShape` y el tamaño en `PetState.petSize`.

Tamaños activos:

- `small`
- `medium`
- `large`

Formas activas en el selector:

- `shape-circle`
- `shape-mochi`
- `shape-onigiri`
- `shape-squircle`
- `shape-dumpling`

Hay otras formas CSS comentadas o preparadas, pero no todas están activas en el selector.

## Accesorios y escenarios

La antigua vista Armario fue eliminada.

### Accesorios

`Store.renderAccessories()` muestra solo artículos con:

```js
category: 'acc'
```

Incluye accesorios de cabeza, ojos, orejas y cuello/cuerpo. También conserva la gestión de Looks guardados.

### Escenario

`Store.renderScenery()` muestra solo artículos con:

```js
category: 'bg'
```

Los escenarios comprados se equipan mediante `PetState.equipItem()` y actualizan la clase del elemento `#pet-room`.

### Tienda

La Tienda tiene pestañas:

- Todos
- Accesorios
- Escenarios

`Store.storeCategory` conserva la pestaña activa después de comprar.

## Mascota y movimiento

La mascota principal es:

```html
<div id="pet" class="pet-sprite">
```

La cara, el accesorio y la capa de brillantina están dentro de ese elemento.

El movimiento autónomo se implementa en `PetState.startWandering()` y `moveRandomly()`:

- Hay un primer movimiento al iniciar.
- Luego intenta moverse cada 5 segundos.
- No se mueve cuando `isSleeping` es `true`.
- No se mueve cuando `isSick` es `true`.
- Esta condición debe conservarse.

Las animaciones `walking` y `happy-jump` deben conservar la variable `--pet-scale` para no romper el tamaño elegido.

## Accesorios: sistema de ajuste

Los accesorios se renderizan dentro de `#pet-accessory`.

Cada accesorio recibe una clase de posición:

- `pos-head`
- `pos-eyes`
- `pos-ears`
- `pos-neck`

También recibe una clase individual como `item-hat_crown`.

`Store.accessoryFits` permite ajustar cada accesorio con:

- `scale`
- `x`
- `y`
- `rotate`

Estos valores se aplican mediante variables CSS sobre la imagen del accesorio.

Las formas anchas como Mochi y Dumpling tienen ajustes CSS adicionales. Cualquier nuevo accesorio debe probarse con varias formas y tamaños.

## Brillantina slime

La capa visual es:

```html
<div id="pet-glitter" class="pet-glitter"></div>
```

La brillantina está apagada cuando la intensidad es `0`. La clase `.enabled` se aplica solo si `PetState.glitterIntensity > 0`.

El efecto usa:

- Puntos radiales.
- Reflejo diagonal.
- Animación de desplazamiento.
- Variable `--glitter-opacity`.

No debe convertirse en un efecto obligatorio: el usuario controla la intensidad desde `Mi Pet`.

## Diálogos

Las frases principales se cargan desde:

`data/dialogues.json`

Categorías actuales:

- `happy`
- `hungry`
- `sick`
- `tired`
- `bored`
- `sleeping`
- `alerts`

Los mensajes de comida y medicina se muestran en el globo `#pet-thought`, no mediante `alert()` del navegador.

Mensajes actuales:

- `No preciso comer ahora.`
- `No preciso un doctor ahora.`

Si el JSON no carga, `pet.js` tiene frases de respaldo para que el juego siga funcionando.

## Persistencia

La clave principal de `localStorage` es:

```text
virtualPetData
```

Incluye monedas, estadísticas, color, forma, tamaño, brillantina, enfermedad, inventario, accesorio equipado, escenario equipado y looks.

No eliminar campos existentes al modificar `saveData()` o `loadData()`.

## Service Worker

`sw.js` usa Network First y una caché versionada.

La versión actual es `v2.0.3`.

Si se modifican archivos principales de la aplicación, subir la versión de `CACHE_VERSION` para forzar la actualización en dispositivos que tengan la PWA instalada.

Los archivos de desarrollo no deben entrar en la caché de producción.

## Carpeta de desarrollo

Los archivos de pruebas y demos están en:

`desarrollo/`

Actualmente contiene documentación y demos visuales, como:

- `prueba-accesorios.md`
- `prueba-accesorios.csv`
- `prueba-accesorios-visual.html`
- `escenarios-demo.html`
- Este documento de contexto.

La carpeta está excluida del deploy mediante `.netlifyignore`:

```text
desarrollo/
```

Si se mueven archivos dentro de esta carpeta, revisar las rutas relativas de imágenes. Desde un HTML dentro de `desarrollo/`, las imágenes del proyecto se referencian normalmente como:

```text
../img/accessories/nombre.svg
```

## Demo de escenarios

`desarrollo/escenarios-demo.html` es una demo visual independiente. No representa todavía los escenarios reales de la aplicación.

Cada tarjeta muestra:

- `Antes`: referencia simple del escenario actual.
- `Propuesta`: versión visual mejorada.

La demo contiene los 17 escenarios actuales y sirve para aprobar la dirección visual antes de modificar `styles.css` o añadir recursos reales.

El Dojo Ninja se usa como referencia de calidad visual y tiene:

- Piso de madera.
- Torii construido por capas.
- Monte Fuji con cima nevada.
- Sol rojo.
- Mascota separada de los elementos importantes.

## Cómo agregar un minijuego

Los minijuegos se gestionan desde `js/minigames.js`. Para agregar uno nuevo:

1. Añadir una tarjeta dentro de `renderMenu()` con un icono, nombre y llamada a un método nuevo:

```html
<div class="item-card" onclick="Minigames.startNewGame()" style="cursor:pointer;">
	<span style="font-size:2rem;">🎯</span>
	<span class="card-title">Nuevo juego</span>
	<button class="card-btn">Jugar</button>
</div>
```

2. Crear `startNewGame()` dentro del objeto `Minigames`.
3. Usar `showCountdown()` si el juego necesita cuenta regresiva.
4. Marcar `inProgress = true` cuando comience.
5. Registrar `exitHandler` para limpiar timers, listeners y devolver monedas si el usuario cierra el modal.
6. Usar `renderHUD()` para mostrar puntuación, récord y tiempo.
7. Guardar récords con `getBestScore()` y `saveBestScore()` usando un identificador único.
8. Terminar con `showResult()` para entregar monedas y ofrecer volver a jugar o volver al menú.

Normalmente no hace falta tocar `index.html`: el menú se genera desde `minigames.js`. Solo agregar CSS si el juego necesita estilos propios en `styles.css` y actualizar `sw.js` si se incorporan imágenes o sonidos.

## Cómo agregar un escenario

Los escenarios se registran como objetos en `Store.items`, dentro de `js/store.js`:

```js
{
	id: 'bg_new_scene',
	name: 'Nuevo Escenario',
	price: 80,
	image: '🌿',
	type: 'bg',
	category: 'bg',
	preview: 'background: ...;'
}
```

Pasos:

1. Crear una clase CSS con el mismo identificador, por ejemplo `.room.bg_new_scene`, en `css/styles.css`.
2. Definir fondo, suelo, capas y objetos del ambiente mediante pseudo-elementos o elementos auxiliares.
3. Mantener a la mascota como protagonista y no taparla con los objetos principales.
4. Si necesita partículas, agregar el caso correspondiente en `js/particulas.js`.
5. Verificar que `renderScenery()` lo muestre automáticamente: filtra los artículos con `category: 'bg'`.
6. Añadir la imagen o recurso a `ASSETS_TO_CACHE` en `sw.js` si el escenario usa archivos externos.
7. Incrementar `CACHE_VERSION` cuando se modifique la aplicación publicada.

La clase del escenario debe coincidir con el valor usado por `PetState.equippedBackground`, porque `updateUI()` aplica esa clase al elemento `#pet-room`.

## Cómo agregar un accesorio

Los accesorios se registran en `Store.items` dentro de `js/store.js`:

```js
{
	id: 'new_accessory',
	name: 'Nuevo Accesorio',
	price: 40,
	image: 'img/accessories/new_accessory.svg',
	type: 'head',
	category: 'acc'
}
```

El campo `type` debe ser uno de los anclajes existentes:

- `head`: sombreros, coronas, flores y accesorios superiores.
- `eyes`: gafas y visores.
- `ears`: orejas y auriculares.
- `neck`: moños, bufandas, varitas y alas.

Pasos:

1. Crear el SVG o imagen dentro de `img/accessories/`.
2. Añadir el artículo a `Store.items` con `category: 'acc'`.
3. Añadirlo a `Store.accessoryFits` en `js/store.js` para definir `scale`, `x`, `y` y, si hace falta, `rotate`.
4. Probarlo con Círculo, Mochi y Dumpling.
5. Probarlo en Chico, Mediano y Grande.
6. Si una forma necesita una excepción, añadir una regla específica en `styles.css` sin romper el ajuste individual.
7. Añadir el recurso a `ASSETS_TO_CACHE` en `sw.js` si debe funcionar offline.

Ejemplo de ajuste:

```js
new_accessory: {
	scale: 1.15,
	x: 0,
	y: -3,
	rotate: 0
}
```

El accesorio se muestra automáticamente en `Accesorios` cuando pertenece al inventario y se equipa mediante `PetState.equipItem()`.

## Reglas de trabajo para otra IA

1. Leer este documento antes de explorar todos los archivos.
2. No modificar la aplicación principal si la petición es solo sobre la demo.
3. No eliminar cambios existentes sin confirmar.
4. Mantener las API públicas actuales de `PetState` y `Store`.
5. Preferir patrones ya existentes en el proyecto.
6. Validar `pet.js`, `store.js`, `app.js`, `styles.css` e `index.html` después de editar.
7. Si se cambia la aplicación principal, probar persistencia, navegación, movimiento y selección visual.
8. Si se cambia `sw.js`, incrementar `CACHE_VERSION`.
9. Mantener la condición de que la mascota no se mueva dormida o enferma.
10. Mantener la carpeta `desarrollo/` fuera del deploy de Netlify.

## Validación manual recomendada

- Abrir `Mi Pet` y cambiar color, forma y tamaño.
- Confirmar que los cambios persisten al recargar.
- Probar brillantina en `0`, `50` y `100`.
- Abrir Accesorios y probar cabeza, ojos, orejas y cuello.
- Probar accesorios con Círculo, Mochi y Dumpling.
- Abrir Escenario y cambiar fondos.
- Confirmar que la mascota se mueve cuando está activa.
- Confirmar que no se mueve dormida ni enferma.
- Abrir Tienda y cambiar entre Todos, Accesorios y Escenarios.
- Comprobar que los botones no ocultan contenido en móvil.

```

### Archivo: `desarrollo/escenarios-demo.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Demo visual de escenarios</title>
  <style>
    /* ==========================================================
       ESTILOS ORIGINALES (NO TOCADOS)
       ========================================================== */
    :root {
      --ink: #3d2756;
      --muted: #756982;
      --panel: #fffdfd;
      --line: #eadff1;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 24px;
      background: #f1eaff;
      color: var(--ink);
      font-family: Georgia, 'Times New Roman', serif;
    }
    header { max-width: 1100px; margin: 0 auto 22px; }
    h1 { margin: 0 0 6px; font-size: clamp(1.6rem, 4vw, 2.5rem); }
    header p { margin: 0; color: var(--muted); font-family: Arial, sans-serif; }
    .gallery {
      max-width: 1100px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 18px;
    }
    .scene-card {
      overflow: hidden;
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: 14px;
      box-shadow: 0 12px 30px rgba(72, 42, 93, 0.13);
    }
    .scene {
      position: relative;
      min-height: 260px;
      overflow: hidden;
      isolation: isolate;
    }
    .scene::after {
      content: '';
      position: absolute;
      inset: auto 0 0;
      height: 34%;
      z-index: 3;
      background: linear-gradient(180deg, transparent, rgba(28, 21, 43, 0.18));
      pointer-events: none;
    }
    .scene-label {
      padding: 12px 14px 14px;
      font-family: Arial, sans-serif;
    }
    .scene-label h2 { margin: 0 0 3px; font-size: 1rem; }
    .scene-label p { margin: 0; color: var(--muted); font-size: .82rem; }
    
    /* Mascota Original */
    .pet {
      position: absolute; z-index: 4; left: 50%; bottom: 28px; width: 74px; height: 74px;
      transform: translateX(-50%); border-radius: 50%; background: #ff80ab;
      box-shadow: 0 14px 16px rgba(30, 18, 35, .25), inset 12px 9px 0 rgba(255,255,255,.25);
    }
    .pet::before, .pet::after { content: ''; position: absolute; top: 28px; width: 9px; height: 12px; border-radius: 50%; background: #38253c; }
    .pet::before { left: 22px; }
    .pet::after { right: 22px; }
    .face { position: absolute; inset: 0; }
    .mouth { position: absolute; left: 50%; top: 47px; transform: translateX(-50%); color: #38253c; font-size: 12px; }
    .pet-shadow { position: absolute; z-index: 3; left: 50%; bottom: 19px; width: 115px; height: 18px; transform: translateX(-50%); background: rgba(34, 19, 38, .22); border-radius: 50%; filter: blur(5px); }
    
    .comparison { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: #eadff1; }
    .comparison .scene { min-height: 190px; }
    .version-tag { position: absolute; z-index: 20; top: 8px; left: 8px; padding: 3px 6px; border-radius: 6px; background: rgba(255,255,255,.95); color: #4a2b60; font: bold 10px Arial, sans-serif; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
    
    /* Escenarios originales del ANTES (Intactos) */
    .scene.old::after { display: none; }
    .scene.old .pet { display: block; left: 72%; }
    .scene.old .pet-shadow { display: block; left: 72%; }
    .scene.old.living-old { background: linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%), linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%); }
    .scene.old.living-old::before { content: '🛋️'; position: absolute; left: 18px; bottom: 26px; font-size: 48px; }
    .scene.old.bedroom-old { background: linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%), linear-gradient(180deg, #311b92 0%, #512da8 72%); }
    .scene.old.bedroom-old::before { content: '🌙'; position: absolute; right: 28px; top: 28px; font-size: 42px; }
    .scene.old.beach-old { background: linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%), linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%); }
    .scene.old.beach-old::before { content: '🏖️'; position: absolute; left: 50%; top: 45%; transform: translate(-50%, -50%); font-size: 48px; }
    .scene.old.forest-old { background: linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%), linear-gradient(180deg, #aed581 0%, #558b2f 65%); }
    .scene.old.forest-old::before { content: '🌲'; position: absolute; left: 50%; bottom: 25px; transform: translateX(-50%); font-size: 62px; }
    .scene.old.kitchen-old { background: linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%), linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%); }
    .scene.old.kitchen-old::before { content: '🍳'; position: absolute; left: 50%; top: 42%; transform: translate(-50%, -50%); font-size: 52px; }
    .scene.old.space-old { background: radial-gradient(circle at 50% 30%, #37474f 0%, #000 100%); }
    .scene.old.space-old::before { content: '🚀'; position: absolute; left: 50%; top: 42%; transform: translate(-50%, -50%); font-size: 52px; }
    .scene.old.playroom-old { background: linear-gradient(180deg, #f3e5f5 0 70%, #80deea 70%); }
    .scene.old.playroom-old::before { content: '🧸'; }
    .scene.old.park-old { background: linear-gradient(180deg, #81d4fa 0 68%, #7cb342 68%); }
    .scene.old.park-old::before { content: '🌳'; }
    .scene.old.mountain-old { background: linear-gradient(180deg, #e0f7fa 0 70%, #90a4ae 70%); }
    .scene.old.mountain-old::before { content: '🏔️'; }
    .scene.old.camping-old { background: linear-gradient(180deg, #0d1b2a 0 72%, #1b5e20 72%); }
    .scene.old.camping-old::before { content: '⛺'; }
    .scene.old.desert-old { background: linear-gradient(180deg, #ffe0b2 0 68%, #e65100 68%); }
    .scene.old.desert-old::before { content: '🏜️'; }
    .scene.old.bathroom-old { background: linear-gradient(180deg, #e0f7fa 0 70%, #4dd0e1 70%); }
    .scene.old.bathroom-old::before { content: '🛁'; }
    .scene.old.dojo-old { background: linear-gradient(180deg, #efebe9 0 70%, #4e342e 70%); }
    .scene.old.dojo-old::before { content: '⛩️'; }
    .scene.old.disco-old { background: linear-gradient(135deg, #4a148c, #880e4f 50%, #311b92); }
    .scene.old.disco-old::before { content: '🪩'; }
    .scene.old.castle-old { background: linear-gradient(180deg, #f3e5f5 0 70%, #4a148c 70%); }
    .scene.old.castle-old::before { content: '🏰'; }
    .scene.old.underwater-old { background: linear-gradient(180deg, #00838f 0 70%, #006064 70%); }
    .scene.old.underwater-old::before { content: '🪸'; }
    .scene.old.volcano-old { background: linear-gradient(180deg, #bf360c 0 70%, #1b0000 70%); }
    .scene.old.volcano-old::before { content: '🌋'; }

    @media (max-width: 720px) {
      body { padding: 14px; }
      .gallery { grid-template-columns: 1fr; }
    }

    /* ==========================================================
       NUEVO MOTOR DE PROPUESTAS (CSS Art Premium)
       ========================================================== */
    /* Capas base */
    .new-scene { box-shadow: inset 0 0 40px rgba(0,0,0,0.15); }
    .env-bg { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; }
    .env-mid { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 2; pointer-events: none; }
    .env-fg { position: absolute; bottom: 0; left: 0; width: 100%; height: 35%; z-index: 3; }
    
    /* Reposicionamiento de la mascota en propuestas para no tapar arte */
    .new-scene .pet, .new-scene .pet-shadow { z-index: 10; }
    .n-living .pet, .n-living .pet-shadow { left: 75%; }
    .n-bedroom .pet, .n-bedroom .pet-shadow { left: 75%; }
    .n-camping .pet, .n-camping .pet-shadow { left: 50%; }
    .n-dojo .pet, .n-dojo .pet-shadow { left: 80%; }
    .n-castle .pet, .n-castle .pet-shadow { left: 25%; }
    .n-desert .pet, .n-desert .pet-shadow { left: 70%; }
    .n-bathroom .pet, .n-bathroom .pet-shadow { left: 80%; }
    
    /* 1. Sala de Estar */
    .n-living .env-bg { background: linear-gradient(to bottom, #fdfbfb 0%, #ebedee 100%); border-bottom: 8px solid #dcdde1; }
    .n-living .env-fg { background: #d3b8ae; box-shadow: inset 0 15px 20px rgba(0,0,0,0.05); }
    .n-living .obj-window { position: absolute; top: 20px; left: 30px; width: 70px; height: 90px; background: linear-gradient(#81d4fa, #e1f5fe); border: 6px solid #fff; box-shadow: 0 10px 15px rgba(0,0,0,0.05), inset 0 0 15px rgba(255,255,255,0.8); }
    .n-living .obj-window::after { content:''; position:absolute; top: 40px; left:0; width: 100%; height: 6px; background: #fff; }
    .n-living .obj-sofa { position: absolute; bottom: 35px; left: 15px; width: 130px; height: 60px; background: #ff7675; border-radius: 15px 15px 5px 5px; box-shadow: inset -5px -10px 0 rgba(0,0,0,0.1), 0 15px 15px rgba(0,0,0,0.1); }
    .n-living .obj-sofa::before, .n-living .obj-sofa::after { content:''; position:absolute; bottom:0; width: 25px; height: 40px; background: #d63031; border-radius: 8px; }
    .n-living .obj-sofa::before { left: -10px; } .n-living .obj-sofa::after { right: -10px; }
    
    /* 2. Dormitorio */
    .n-bedroom .env-bg { background: linear-gradient(to bottom, #192a56 0%, #273c75 100%); }
    .n-bedroom .env-fg { background: #353b48; box-shadow: inset 0 20px 30px rgba(0,0,0,0.3); }
    .n-bedroom .obj-moon { position: absolute; top: 20px; right: 40px; width: 45px; height: 45px; background: #fbc531; border-radius: 50%; box-shadow: 0 0 30px #fbc531, inset -10px -5px 0 rgba(225, 177, 44, 0.5); }
    .n-bedroom .obj-bed { position: absolute; bottom: 35px; left: 20px; width: 150px; height: 50px; background: #8c7ae6; border-radius: 10px 10px 4px 4px; box-shadow: 0 15px 20px rgba(0,0,0,0.2); }
    .n-bedroom .obj-bed::after { content:''; position:absolute; bottom:0; left:0; width: 100%; height: 25px; background: #9c88ff; border-radius: 0 0 4px 4px;}
    .n-bedroom .obj-bed::before { content:''; position:absolute; top:-10px; left:10px; width: 50px; height: 25px; background: #f5f6fa; border-radius: 12px; box-shadow: 0 5px 10px rgba(0,0,0,0.1); }
    
    /* 3. Pista / Sala de Juegos */
    .n-playroom .env-bg { background: radial-gradient(circle at 50% 50%, #fff 0%, #f1f2f6 100%); border-bottom: 6px solid #ff4757; }
    .n-playroom .env-fg { background: repeating-conic-gradient(#7bed9f 0% 25%, #2ed573 0% 50%) 50% / 40px 40px; }
    .n-playroom .obj-block1 { position: absolute; bottom: 45px; left: 30px; width: 35px; height: 35px; background: #1e90ff; border: 3px solid #fff; box-shadow: 5px 5px 0 rgba(0,0,0,0.1); }
    .n-playroom .obj-block2 { position: absolute; bottom: 80px; left: 45px; width: 30px; height: 30px; background: #ffa502; border: 3px solid #fff; box-shadow: 5px 5px 0 rgba(0,0,0,0.1); }
    .n-playroom .obj-ball { position: absolute; bottom: 40px; left: 100px; width: 45px; height: 45px; background: radial-gradient(circle at 15px 15px, #ff4757, #ff6b81); border-radius: 50%; box-shadow: 5px 10px 15px rgba(0,0,0,0.15); }
    
    /* 4. Parque Mágico */
    .n-park .env-bg { background: linear-gradient(to bottom, #4b4b4b, #2d3436); }
    .n-park .env-fg { background: #00b894; box-shadow: inset 0 20px 30px rgba(0,0,0,0.2), 0 -10px 20px #55efc4; }
    .n-park .obj-shroom { position: absolute; bottom: 35px; left: 40px; width: 12px; height: 40px; background: #dfe6e9; border-radius: 6px; }
    .n-park .obj-shroom::before { content:''; position:absolute; top:-20px; left:-25px; width: 62px; height: 35px; background: #fd79a8; border-radius: 30px 30px 10px 10px; box-shadow: inset -5px -5px 0 rgba(0,0,0,0.1), 0 0 25px #fd79a8; }
    .n-park .obj-magic { position:absolute; width: 6px; height: 6px; background: #ffeaa7; border-radius: 50%; box-shadow: 0 0 15px 5px #ffeaa7; }
    
    /* 5. Playa Tropical */
    .n-beach .env-bg { background: linear-gradient(to bottom, #74b9ff, #0984e3 80%); }
    .n-beach .env-fg { background: #ffeaa7; box-shadow: inset 0 15px 20px rgba(0,0,0,0.05); }
    .n-beach .obj-sun { position: absolute; top: 20px; right: 30px; width: 60px; height: 60px; background: #fdcb6e; border-radius: 50%; box-shadow: 0 0 40px #fdcb6e; }
    .n-beach .obj-palm-trunk { position: absolute; bottom: 30px; left: 30px; width: 12px; height: 90px; background: #d63031; background: #b2bec3; background: #8d6e63; border-radius: 10px; transform: rotate(-10deg); box-shadow: -5px 5px 10px rgba(0,0,0,0.2); }
    .n-beach .obj-palm-leaves { position: absolute; bottom: 100px; left: 10px; width: 70px; height: 40px; background: #00b894; border-radius: 50%; box-shadow: 20px 10px 0 #00cec9, -15px 15px 0 #55efc4; }
    
    /* 6. Bosque Encantado */
    .n-forest .env-bg { background: linear-gradient(to bottom, #2d3436, #000); }
    .n-forest .env-fg { background: #013220; box-shadow: inset 0 25px 35px rgba(0,0,0,0.5); }
    .n-forest .obj-tree { position: absolute; bottom: 35%; width: 35px; height: 160px; background: #27ae60; border-radius: 15px 15px 0 0; box-shadow: inset -10px 0 20px rgba(0,0,0,0.3); }
    .n-forest .obj-tree.t1 { left: 10%; } .n-forest .obj-tree.t2 { right: 15%; background: #2ecc71; height: 120px; }
    .n-forest .obj-glow { position: absolute; width: 8px; height: 8px; background: #a29bfe; border-radius: 50%; box-shadow: 0 0 20px 8px #a29bfe; }
    
    /* 7. Montaña Nevada */
    .n-mountain .env-bg { background: linear-gradient(to bottom, #81ecec, #dfe6e9); }
    .n-mountain .env-fg { background: #fff; border-top: 5px solid #ecf0f1; box-shadow: inset 0 10px 20px rgba(116, 185, 255, 0.2); }
    .n-mountain .obj-mount { position: absolute; bottom: 35%; left: -20px; width: 0; height: 0; border-left: 90px solid transparent; border-right: 90px solid transparent; border-bottom: 140px solid #b2bec3; }
    .n-mountain .obj-mount::after { content:''; position:absolute; top: 20px; left: -30px; border-left: 30px solid transparent; border-right: 30px solid transparent; border-bottom: 40px solid #fff; }
    .n-mountain .obj-mount2 { position: absolute; bottom: 35%; right: 10px; border-left: 70px solid transparent; border-right: 70px solid transparent; border-bottom: 100px solid #636e72; z-index: -1; }
    
    /* 8. Campamento Nocturno */
    .n-camping .env-bg { background: linear-gradient(to bottom, #130f40, #30336b); }
    .n-camping .env-fg { background: #222f3e; box-shadow: inset 0 15px 30px rgba(0,0,0,0.4); }
    .n-camping .obj-tent { position: absolute; bottom: 35px; left: 20px; border-left: 50px solid transparent; border-right: 50px solid transparent; border-bottom: 80px solid #10ac84; filter: drop-shadow(10px 10px 10px rgba(0,0,0,0.3)); }
    .n-camping .obj-tent::after { content:''; position:absolute; top: 30px; left: -15px; border-left: 15px solid transparent; border-right: 15px solid transparent; border-bottom: 50px solid #01a3a4; }
    .n-camping .obj-fire { position: absolute; bottom: 35px; right: 30px; width: 35px; height: 35px; background: #ff9f43; border-radius: 50% 0 50% 50%; transform: rotate(-45deg); box-shadow: 0 0 30px 10px rgba(255, 159, 67, 0.4); }
    .n-camping .obj-fire::after { content:''; position:absolute; top: 8px; left: 8px; width: 15px; height: 15px; background: #feca57; border-radius: 50% 0 50% 50%; }
    
    /* 9. Oasis */
    .n-desert .env-bg { background: linear-gradient(to bottom, #ff9f43, #feca57); }
    .n-desert .env-fg { background: #f3a683; box-shadow: inset 0 20px 20px rgba(0,0,0,0.05); }
    .n-desert .obj-dune { position: absolute; bottom: 35%; right: -50px; width: 220px; height: 100px; background: #f19066; border-radius: 50% 50% 0 0; }
    .n-desert .obj-water { position: absolute; bottom: 20px; left: 20px; width: 130px; height: 35px; background: #54a0ff; border-radius: 50%; border: 3px solid #2e86de; box-shadow: inset 0 5px 10px rgba(0,0,0,0.2); }
    
    /* 10. Cocina */
    .n-kitchen .env-bg { background: repeating-linear-gradient(90deg, #f5f6fa, #f5f6fa 20px, #dcdde1 20px, #dcdde1 22px); }
    .n-kitchen .env-fg { background: repeating-conic-gradient(#7f8fa6 0% 25%, #f5f6fa 0% 50%) 50% / 40px 40px; }
    .n-kitchen .obj-counter { position: absolute; bottom: 35%; left: 0; width: 100%; height: 40px; background: #353b48; border-bottom: 6px solid #2f3640; box-shadow: 0 10px 15px rgba(0,0,0,0.2); }
    .n-kitchen .obj-fridge { position: absolute; bottom: 35%; left: 20px; width: 65px; height: 110px; background: #e1b12c; border-radius: 5px 5px 0 0; border: 3px solid #c23616; box-shadow: 10px 0 20px rgba(0,0,0,0.1); }
    
    /* 11. Baño */
    .n-bathroom .env-bg { background: linear-gradient(to bottom, #4bcffa, #0fbcf9); }
    .n-bathroom .env-fg { background: #d2dae2; }
    .n-bathroom .obj-tub { position: absolute; bottom: 30px; left: 20px; width: 150px; height: 60px; background: #fff; border-radius: 10px 10px 35px 35px; box-shadow: inset -10px -10px 15px rgba(0,0,0,0.05), 10px 15px 20px rgba(0,0,0,0.1); border-top: 6px solid #f5f6fa; }
    .n-bathroom .obj-bubble { position: absolute; border: 2px solid rgba(255,255,255,0.9); background: rgba(255,255,255,0.4); border-radius: 50%; box-shadow: inset 3px 3px 5px #fff; }
    
    /* 12. Dojo Ninja (Referencia de Calidad - Cero Emojis) */
    .n-dojo .env-bg { background: linear-gradient(to bottom, #ff9a9e 0%, #fecfef 100%); } /* Atardecer */
    /* Monte Fuji real (dibujado con CSS poligonal), asimétrico como en la realidad */
    .n-dojo .obj-fuji {
      position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%);
      width: 170px; height: 100px;
      background: #4a5568;
      clip-path: polygon(50% 0, 0 100%, 100% 100%);
    }
    /* Nieve del Fuji con recortes reales */
    .n-dojo .obj-fuji::after {
      content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 35px;
      background: #fff;
      clip-path: polygon(50% 0, 15% 100%, 30% 70%, 45% 100%, 65% 75%, 75% 100%, 85% 65%, 100% 100%);
    }
    .n-dojo .obj-sun { position: absolute; bottom: 45%; right: 20%; width: 55px; height: 55px; background: #e53e3e; border-radius: 50%; box-shadow: 0 0 25px #e53e3e; }
    /* Suelo: Líneas de madera limitadas estrictamente al área del suelo (fg) */
    .n-dojo .env-fg {
      background: #a0522d;
      background-image: repeating-linear-gradient(90deg, rgba(0,0,0,0.15) 0px, rgba(0,0,0,0.15) 2px, transparent 2px, transparent 25px);
      border-top: 4px solid #8b4513;
      box-shadow: inset 0 20px 20px rgba(0,0,0,0.2);
    }
    /* Torii con columnas gruesas, vigas y sombras */
    .n-dojo .obj-torii { position: absolute; bottom: 35%; left: 15px; width: 105px; height: 95px; }
    .n-dojo .obj-torii-col { position: absolute; bottom: 0; width: 12px; height: 85px; background: #c53030; box-shadow: inset -3px 0 5px rgba(0,0,0,0.2); }
    .n-dojo .obj-torii-col.left { left: 15px; } .n-dojo .obj-torii-col.right { right: 15px; }
    .n-dojo .obj-torii-beam1 { position: absolute; top: 10px; left: 0; width: 105px; height: 12px; background: #9b2c2c; border-radius: 6px 6px 0 0; box-shadow: 0 4px 6px rgba(0,0,0,0.2); }
    .n-dojo .obj-torii-beam2 { position: absolute; top: 30px; left: 10px; width: 85px; height: 8px; background: #9b2c2c; box-shadow: 0 4px 6px rgba(0,0,0,0.2); }
    
    /* 13. Pista de Baile */
    .n-disco .env-bg { background: #111; }
    .n-disco .env-fg {
      background: repeating-linear-gradient(90deg, #111 0, #111 20px, #e91e63 20px, #e91e63 22px),
                  repeating-linear-gradient(0deg, #111 0, #111 20px, #00bcd4 20px, #00bcd4 22px);
      transform: perspective(250px) rotateX(40deg); transform-origin: bottom;
      height: 50%; bottom: -10%; border-top: 2px solid #fff;
    }
    .n-disco .obj-beam { position: absolute; top: -20px; left: 20%; width: 100px; height: 200px; background: linear-gradient(to bottom, rgba(233,30,99,0.6), transparent); clip-path: polygon(50% 0, 0 100%, 100% 100%); mix-blend-mode: screen; }
    .n-disco .obj-beam2 { position: absolute; top: -20px; right: 20%; width: 100px; height: 200px; background: linear-gradient(to bottom, rgba(0,188,212,0.6), transparent); clip-path: polygon(50% 0, 0 100%, 100% 100%); mix-blend-mode: screen; }
    
    /* 14. Estación Espacial */
    .n-space .env-bg { background: #2f3640; border-bottom: 8px solid #192a56; }
    .n-space .env-fg { background: radial-gradient(circle, #718093 15%, transparent 16%), #353b48; background-size: 20px 20px; }
    .n-space .obj-window { position: absolute; top: 15px; left: 50%; transform: translateX(-50%); width: 140px; height: 90px; background: #000; border: 10px solid #dcdde1; border-radius: 40px; overflow: hidden; box-shadow: inset 0 0 20px rgba(255,255,255,0.2); }
    .n-space .obj-window::after { content:''; position:absolute; bottom: -15px; right: 10px; width: 45px; height: 45px; background: #e1b12c; border-radius: 50%; box-shadow: inset 10px 10px 15px rgba(0,0,0,0.5); }
    
    /* 15. Castillo Real */
    .n-castle .env-bg { background: #7f8fa6; background-image: radial-gradient(#dcdde1 10%, transparent 11%); background-size: 30px 30px; }
    .n-castle .env-fg { background: #353b48; }
    .n-castle .obj-carpet { position: absolute; bottom: 0; left: 50%; transform: translateX(-50%) perspective(150px) rotateX(40deg); width: 80px; height: 100%; background: #c23616; transform-origin: bottom; box-shadow: 0 0 15px rgba(0,0,0,0.5); }
    .n-castle .obj-throne { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); width: 55px; height: 80px; background: #fbc531; border-radius: 20px 20px 0 0; box-shadow: inset 0 -10px 20px rgba(0,0,0,0.3); }
    
    /* 16. Mundo Submarino */
    .n-underwater .env-bg { background: linear-gradient(to bottom, #0097e6, #273c75); }
    .n-underwater .env-fg { background: #e1b12c; box-shadow: inset 0 20px 30px rgba(0,0,0,0.2); }
    .n-underwater .obj-ray { position: absolute; top: 0; left: 25%; width: 50px; height: 100%; background: linear-gradient(to bottom, rgba(255,255,255,0.3), transparent); transform: rotate(20deg); }
    .n-underwater .obj-coral { position: absolute; bottom: 30%; left: 30px; width: 15px; height: 75px; background: #e84118; border-radius: 10px; box-shadow: 15px 15px 0 -2px #ff7f50; }
    
    /* 17. Guarida Volcánica */
    .n-volcano .env-bg { background: linear-gradient(to bottom, #2f3640, #192a56); }
    .n-volcano .env-fg { background: #111; }
    .n-volcano .obj-lava { position: absolute; bottom: 15px; left: 50%; transform: translateX(-50%); width: 150px; height: 35px; background: #e84118; border-radius: 50%; box-shadow: 0 0 25px #e84118, inset 0 0 15px #c23616; }
    .n-volcano .obj-rock { position: absolute; bottom: 35%; left: 20px; border-left: 40px solid transparent; border-right: 40px solid transparent; border-bottom: 70px solid #353b48; }

  </style>
</head>
<body>
  <header>
    <h1>Demo visual de escenarios</h1>
    <p>Propuesta de profundidad para Mi Pet: fondo lejano, plano medio, primer plano, iluminación y mascota protagonista.</p>
  </header>
  <main class="gallery">
    <!-- 1. Sala de Estar -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old living-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-living"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-window"></div><div class="obj-sofa"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Sala de estar</h2><p>Ventana, sofá, alfombra y luz cálida.</p></div>
    </article>

    <!-- 2. Dormitorio -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old bedroom-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-bedroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-moon"></div><div class="obj-bed"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Dormitorio nocturno</h2><p>Luna, estrellas y cama en capas.</p></div>
    </article>

    <!-- 3. Playa Tropical -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old beach-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-beach"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-sun"></div><div class="obj-palm-trunk"></div><div class="obj-palm-leaves"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Playa tropical</h2><p>Horizonte, agua, arena, palmera y luz solar.</p></div>
    </article>

    <!-- 4. Bosque Encantado -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old forest-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-forest"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tree t1"></div><div class="obj-tree t2"></div><div class="obj-glow" style="top:40px; left:60px;"></div><div class="obj-glow" style="top:70px; right:80px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Bosque encantado</h2><p>Montaña lejana, árboles cercanos y suelo profundo.</p></div>
    </article>

    <!-- 5. Cocina del Chef -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old kitchen-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-kitchen"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-fridge"></div><div class="obj-counter"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Cocina del chef</h2><p>Azulejos, muebles, mesada y objetos de cocina.</p></div>
    </article>

    <!-- 6. Estación Espacial -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old space-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-space"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-window"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Estación espacial</h2><p>Planeta, estrellas, plataforma y luces de ambiente.</p></div>
    </article>

    <!-- 7. Sala de Juegos -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old playroom-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-playroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-block1"></div><div class="obj-block2"></div><div class="obj-ball"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Sala de juegos</h2><p>Juguetes, burbujas de color y suelo acuático.</p></div>
    </article>

    <!-- 8. Parque Mágico -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old park-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-park"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-shroom"></div><div class="obj-magic" style="top:50px; left:80px;"></div><div class="obj-magic" style="top:30px; right:50px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Parque mágico</h2><p>Cielo abierto, árbol protagonista y césped en capas.</p></div>
    </article>

    <!-- 9. Montaña Nevada -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old mountain-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-mountain"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-mount2"></div><div class="obj-mount"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Montaña nevada</h2><p>Montañas lejanas, nieve y suelo frío con profundidad.</p></div>
    </article>

    <!-- 10. Campamento Nocturno -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old camping-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-camping"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tent"></div><div class="obj-fire"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Campamento nocturno</h2><p>Cielo oscuro, estrellas, pasto y tienda iluminada.</p></div>
    </article>

    <!-- 11. Oasis del Desierto -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old desert-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-desert"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-dune"></div></div>
          <div class="env-fg"><div class="obj-water"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Oasis del desierto</h2><p>Capas cálidas, oasis de agua y cactus en primer plano.</p></div>
    </article>

    <!-- 12. Baño de Burbujas -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old bathroom-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-bathroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tub"></div><div class="obj-bubble" style="bottom:60px; left:40px; width:20px; height:20px;"></div><div class="obj-bubble" style="bottom:90px; left:70px; width:15px; height:15px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Baño de burbujas</h2><p>Agua, espuma, burbujas y ambiente luminoso.</p></div>
    </article>

    <!-- 13. Dojo Ninja (Referencia de Calidad - Fuji y Torii Mejorados) -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old dojo-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-dojo"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid">
            <div class="obj-sun"></div>
            <div class="obj-fuji"></div>
            <div class="obj-torii">
              <div class="obj-torii-col left"></div><div class="obj-torii-col right"></div>
              <div class="obj-torii-beam1"></div><div class="obj-torii-beam2"></div>
            </div>
          </div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Dojo ninja</h2><p>Piso de madera, torii, Monte Fuji y sol rojo.</p></div>
    </article>

    <!-- 14. Pista de Baile -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old disco-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-disco"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-beam"></div><div class="obj-beam2"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Pista de baile</h2><p>Luz de color, reflejos y ambiente nocturno.</p></div>
    </article>

    <!-- 15. Castillo Real -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old castle-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-castle"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-throne"></div></div>
          <div class="env-fg"><div class="obj-carpet"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Castillo real</h2><p>Arquitectura, suelo ceremonial y luz violeta.</p></div>
    </article>

    <!-- 16. Mundo Submarino -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old underwater-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-underwater"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-ray"></div><div class="obj-coral"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Mundo submarino</h2><p>Agua profunda, corales y puntos de luz.</p></div>
    </article>

    <!-- 17. Guarida Volcánica -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old volcano-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-volcano"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-rock"></div></div>
          <div class="env-fg"><div class="obj-lava"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Guarida volcánica</h2><p>Lava, roca oscura y luz naranja de fondo.</p></div>
    </article>
  </main>
  
  <script>
    // JS del archivo original intacto.
    document.querySelectorAll('.scene.old').forEach(scene => {
      if (scene.querySelector('.pet')) return;
      scene.insertAdjacentHTML('beforeend', '<div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>');
    });
  </script>
</body>
</html>
```

### Archivo: `desarrollo/escenarios-demo_3.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Demo visual de escenarios</title>
  <style>
    /* ==========================================================
       ESTILOS ORIGINALES (NO TOCADOS)
       ========================================================== */
    :root {
      --ink: #3d2756;
      --muted: #756982;
      --panel: #fffdfd;
      --line: #eadff1;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 24px;
      background: #f1eaff;
      color: var(--ink);
      font-family: Georgia, 'Times New Roman', serif;
    }
    header { max-width: 1100px; margin: 0 auto 22px; }
    h1 { margin: 0 0 6px; font-size: clamp(1.6rem, 4vw, 2.5rem); }
    header p { margin: 0; color: var(--muted); font-family: Arial, sans-serif; }
    .gallery {
      max-width: 1100px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 18px;
    }
    .scene-card {
      overflow: hidden;
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: 14px;
      box-shadow: 0 12px 30px rgba(72, 42, 93, 0.13);
    }
    .scene {
      position: relative;
      min-height: 260px;
      overflow: hidden;
      isolation: isolate;
    }
    .scene::after {
      content: '';
      position: absolute;
      inset: auto 0 0;
      height: 34%;
      z-index: 3;
      background: linear-gradient(180deg, transparent, rgba(28, 21, 43, 0.18));
      pointer-events: none;
    }
    .scene-label {
      padding: 12px 14px 14px;
      font-family: Arial, sans-serif;
    }
    .scene-label h2 { margin: 0 0 3px; font-size: 1rem; }
    .scene-label p { margin: 0; color: var(--muted); font-size: .82rem; }
    
    /* Mascota Original */
    .pet {
      position: absolute; z-index: 4; left: 50%; bottom: 28px; width: 74px; height: 74px;
      transform: translateX(-50%); border-radius: 50%; background: #ff80ab;
      box-shadow: 0 14px 16px rgba(30, 18, 35, .25), inset 12px 9px 0 rgba(255,255,255,.25);
    }
    .pet::before, .pet::after { content: ''; position: absolute; top: 28px; width: 9px; height: 12px; border-radius: 50%; background: #38253c; }
    .pet::before { left: 22px; }
    .pet::after { right: 22px; }
    .face { position: absolute; inset: 0; }
    .mouth { position: absolute; left: 50%; top: 47px; transform: translateX(-50%); color: #38253c; font-size: 12px; }
    .pet-shadow { position: absolute; z-index: 3; left: 50%; bottom: 19px; width: 115px; height: 18px; transform: translateX(-50%); background: rgba(34, 19, 38, .22); border-radius: 50%; filter: blur(5px); }
    
    .comparison { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 1px; background: #eadff1; }
    .comparison .scene { min-height: 190px; }
    .version-tag { position: absolute; z-index: 20; top: 8px; left: 8px; padding: 3px 6px; border-radius: 6px; background: rgba(255,255,255,.95); color: #4a2b60; font: bold 10px Arial, sans-serif; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }

    @media (max-width: 680px) {
      .comparison { grid-template-columns: 1fr; }
    }
    
    /* Escenarios originales del ANTES (Intactos) */
    .scene.old::after { display: none; }
    .scene.old .pet { display: block; left: 72%; }
    .scene.old .pet-shadow { display: block; left: 72%; }
    .scene.old.living-old { background: linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%), linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%); }
    .scene.old.living-old::before { content: '🛋️'; position: absolute; left: 18px; bottom: 26px; font-size: 48px; }
    .scene.old.bedroom-old { background: linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%), linear-gradient(180deg, #311b92 0%, #512da8 72%); }
    .scene.old.bedroom-old::before { content: '🌙'; position: absolute; right: 28px; top: 28px; font-size: 42px; }
    .scene.old.beach-old { background: linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%), linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%); }
    .scene.old.beach-old::before { content: '🏖️'; position: absolute; left: 50%; top: 45%; transform: translate(-50%, -50%); font-size: 48px; }
    .scene.old.forest-old { background: linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%), linear-gradient(180deg, #aed581 0%, #558b2f 65%); }
    .scene.old.forest-old::before { content: '🌲'; position: absolute; left: 50%; bottom: 25px; transform: translateX(-50%); font-size: 62px; }
    .scene.old.kitchen-old { background: linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%), linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%); }
    .scene.old.kitchen-old::before { content: '🍳'; position: absolute; left: 50%; top: 42%; transform: translate(-50%, -50%); font-size: 52px; }
    .scene.old.space-old { background: radial-gradient(circle at 50% 30%, #37474f 0%, #000 100%); }
    .scene.old.space-old::before { content: '🚀'; position: absolute; left: 50%; top: 42%; transform: translate(-50%, -50%); font-size: 52px; }
    .scene.old.playroom-old { background: linear-gradient(180deg, #f3e5f5 0 70%, #80deea 70%); }
    .scene.old.playroom-old::before { content: '🧸'; }
    .scene.old.park-old { background: linear-gradient(180deg, #81d4fa 0 68%, #7cb342 68%); }
    .scene.old.park-old::before { content: '🌳'; }
    .scene.old.mountain-old { background: linear-gradient(180deg, #e0f7fa 0 70%, #90a4ae 70%); }
    .scene.old.mountain-old::before { content: '🏔️'; }
    .scene.old.camping-old { background: linear-gradient(180deg, #0d1b2a 0 72%, #1b5e20 72%); }
    .scene.old.camping-old::before { content: '⛺'; }
    .scene.old.desert-old { background: linear-gradient(180deg, #ffe0b2 0 68%, #e65100 68%); }
    .scene.old.desert-old::before { content: '🏜️'; }
    .scene.old.bathroom-old { background: linear-gradient(180deg, #e0f7fa 0 70%, #4dd0e1 70%); }
    .scene.old.bathroom-old::before { content: '🛁'; }
    .scene.old.dojo-old { background: linear-gradient(180deg, #efebe9 0 70%, #4e342e 70%); }
    .scene.old.dojo-old::before { content: '⛩️'; }
    .scene.old.disco-old { background: linear-gradient(135deg, #4a148c, #880e4f 50%, #311b92); }
    .scene.old.disco-old::before { content: '🪩'; }
    .scene.old.castle-old { background: linear-gradient(180deg, #f3e5f5 0 70%, #4a148c 70%); }
    .scene.old.castle-old::before { content: '🏰'; }
    .scene.old.underwater-old { background: linear-gradient(180deg, #00838f 0 70%, #006064 70%); }
    .scene.old.underwater-old::before { content: '🪸'; }
    .scene.old.volcano-old { background: linear-gradient(180deg, #bf360c 0 70%, #1b0000 70%); }
    .scene.old.volcano-old::before { content: '🌋'; }

    @media (max-width: 720px) {
      body { padding: 14px; }
      .gallery { grid-template-columns: 1fr; }
    }

    /* ==========================================================
       PANEL DE DECISIÓN (radios Propuesta/Mía + descarga de JSON)
       Autocontenido: no toca nada de la app real.
       ========================================================== */
    .approve-row {
      display: flex;
      gap: 16px;
      margin-top: 8px;
      padding-top: 8px;
      border-top: 1px dashed var(--line);
      font-family: Arial, sans-serif;
      font-size: 0.82rem;
      color: var(--ink);
    }
    .approve-row label {
      display: flex;
      align-items: center;
      gap: 5px;
      cursor: pointer;
      user-select: none;
    }
    .approve-row input[type="radio"] {
      width: 15px;
      height: 15px;
      cursor: pointer;
      accent-color: #8c52ff;
    }
    .scene-card.is-approved {
      outline: 3px solid #8c52ff;
      outline-offset: -1px;
    }

    #approve-bar {
      position: fixed;
      bottom: 16px;
      right: 16px;
      z-index: 100;
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: 14px;
      box-shadow: 0 12px 30px rgba(72, 42, 93, 0.25);
      padding: 12px 14px;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      gap: 8px;
      min-width: 200px;
    }
    #approve-count {
      font-weight: bold;
      font-size: 0.85rem;
      color: var(--ink);
      text-align: center;
    }
    #approve-bar button {
      border: none;
      border-radius: 10px;
      padding: 8px 10px;
      font-size: 0.8rem;
      font-weight: bold;
      cursor: pointer;
    }
    #btn-download { background: #8c52ff; color: #fff; }
    #btn-clear { background: #f1eaff; color: var(--ink); }

    @media (max-width: 720px) {
      #approve-bar { left: 14px; right: 14px; bottom: 14px; min-width: auto; }
    }

    /* ==========================================================
       NUEVO MOTOR DE PROPUESTAS (CSS Art Premium)
       ========================================================== */
    /* Capas base */
    .new-scene { box-shadow: inset 0 0 40px rgba(0,0,0,0.15); }
    .env-bg { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; }
    .env-mid { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 2; pointer-events: none; }
    .env-fg { position: absolute; bottom: 0; left: 0; width: 100%; height: 35%; z-index: 3; }
    
    /* Reposicionamiento de la mascota en propuestas para no tapar arte */
    .new-scene .pet, .new-scene .pet-shadow { z-index: 10; }
    .n-living .pet, .n-living .pet-shadow { left: 75%; }
    .n-bedroom .pet, .n-bedroom .pet-shadow { left: 75%; }
    .n-camping .pet, .n-camping .pet-shadow { left: 50%; }
    .n-dojo .pet, .n-dojo .pet-shadow { left: 80%; }
    .n-castle .pet, .n-castle .pet-shadow { left: 25%; }
    .n-desert .pet, .n-desert .pet-shadow { left: 70%; }
    .n-bathroom .pet, .n-bathroom .pet-shadow { left: 80%; }
    
    /* 1. Sala de Estar */
    .n-living .env-bg { background: linear-gradient(to bottom, #fdfbfb 0%, #ebedee 100%); border-bottom: 8px solid #dcdde1; }
    .n-living .env-fg { background: #d3b8ae; box-shadow: inset 0 15px 20px rgba(0,0,0,0.05); }
    .n-living .obj-window { position: absolute; top: 20px; left: 30px; width: 70px; height: 90px; background: linear-gradient(#81d4fa, #e1f5fe); border: 6px solid #fff; box-shadow: 0 10px 15px rgba(0,0,0,0.05), inset 0 0 15px rgba(255,255,255,0.8); }
    .n-living .obj-window::after { content:''; position:absolute; top: 40px; left:0; width: 100%; height: 6px; background: #fff; }
    .n-living .obj-sofa { position: absolute; bottom: 35px; left: 15px; width: 130px; height: 60px; background: #ff7675; border-radius: 15px 15px 5px 5px; box-shadow: inset -5px -10px 0 rgba(0,0,0,0.1), 0 15px 15px rgba(0,0,0,0.1); }
    .n-living .obj-sofa::before, .n-living .obj-sofa::after { content:''; position:absolute; bottom:0; width: 25px; height: 40px; background: #d63031; border-radius: 8px; }
    .n-living .obj-sofa::before { left: -10px; } .n-living .obj-sofa::after { right: -10px; }
    
    /* 2. Dormitorio */
    .n-bedroom .env-bg { background: linear-gradient(to bottom, #192a56 0%, #273c75 100%); }
    .n-bedroom .env-fg { background: #353b48; box-shadow: inset 0 20px 30px rgba(0,0,0,0.3); }
    .n-bedroom .obj-moon { position: absolute; top: 20px; right: 40px; width: 45px; height: 45px; background: #fbc531; border-radius: 50%; box-shadow: 0 0 30px #fbc531, inset -10px -5px 0 rgba(225, 177, 44, 0.5); }
    .n-bedroom .obj-bed { position: absolute; bottom: 35px; left: 20px; width: 150px; height: 50px; background: #8c7ae6; border-radius: 10px 10px 4px 4px; box-shadow: 0 15px 20px rgba(0,0,0,0.2); }
    .n-bedroom .obj-bed::after { content:''; position:absolute; bottom:0; left:0; width: 100%; height: 25px; background: #9c88ff; border-radius: 0 0 4px 4px;}
    .n-bedroom .obj-bed::before { content:''; position:absolute; top:-10px; left:10px; width: 50px; height: 25px; background: #f5f6fa; border-radius: 12px; box-shadow: 0 5px 10px rgba(0,0,0,0.1); }
    
    /* 3. Pista / Sala de Juegos */
    .n-playroom .env-bg { background: radial-gradient(circle at 50% 50%, #fff 0%, #f1f2f6 100%); border-bottom: 6px solid #ff4757; }
    .n-playroom .env-fg { background: repeating-conic-gradient(#7bed9f 0% 25%, #2ed573 0% 50%) 50% / 40px 40px; }
    .n-playroom .obj-block1 { position: absolute; bottom: 45px; left: 30px; width: 35px; height: 35px; background: #1e90ff; border: 3px solid #fff; box-shadow: 5px 5px 0 rgba(0,0,0,0.1); }
    .n-playroom .obj-block2 { position: absolute; bottom: 80px; left: 45px; width: 30px; height: 30px; background: #ffa502; border: 3px solid #fff; box-shadow: 5px 5px 0 rgba(0,0,0,0.1); }
    .n-playroom .obj-ball { position: absolute; bottom: 40px; left: 100px; width: 45px; height: 45px; background: radial-gradient(circle at 15px 15px, #ff4757, #ff6b81); border-radius: 50%; box-shadow: 5px 10px 15px rgba(0,0,0,0.15); }
    
    /* 4. Parque Mágico */
    .n-park .env-bg { background: linear-gradient(to bottom, #4b4b4b, #2d3436); }
    .n-park .env-fg { background: #00b894; box-shadow: inset 0 20px 30px rgba(0,0,0,0.2), 0 -10px 20px #55efc4; }
    .n-park .obj-shroom { position: absolute; bottom: 35px; left: 40px; width: 12px; height: 40px; background: #dfe6e9; border-radius: 6px; }
    .n-park .obj-shroom::before { content:''; position:absolute; top:-20px; left:-25px; width: 62px; height: 35px; background: #fd79a8; border-radius: 30px 30px 10px 10px; box-shadow: inset -5px -5px 0 rgba(0,0,0,0.1), 0 0 25px #fd79a8; }
    .n-park .obj-magic { position:absolute; width: 6px; height: 6px; background: #ffeaa7; border-radius: 50%; box-shadow: 0 0 15px 5px #ffeaa7; }
    
    /* 5. Playa Tropical */
    .n-beach .env-bg { background: linear-gradient(to bottom, #74b9ff, #0984e3 80%); }
    .n-beach .env-fg { background: #ffeaa7; box-shadow: inset 0 15px 20px rgba(0,0,0,0.05); }
    .n-beach .obj-sun { position: absolute; top: 20px; right: 30px; width: 60px; height: 60px; background: #fdcb6e; border-radius: 50%; box-shadow: 0 0 40px #fdcb6e; }
    .n-beach .obj-palm-trunk { position: absolute; bottom: 30px; left: 30px; width: 12px; height: 90px; background: #d63031; background: #b2bec3; background: #8d6e63; border-radius: 10px; transform: rotate(-10deg); box-shadow: -5px 5px 10px rgba(0,0,0,0.2); }
    .n-beach .obj-palm-leaves { position: absolute; bottom: 100px; left: 10px; width: 70px; height: 40px; background: #00b894; border-radius: 50%; box-shadow: 20px 10px 0 #00cec9, -15px 15px 0 #55efc4; }
    
    /* 6. Bosque Encantado */
    .n-forest .env-bg { background: linear-gradient(to bottom, #2d3436, #000); }
    .n-forest .env-fg { background: #013220; box-shadow: inset 0 25px 35px rgba(0,0,0,0.5); }
    .n-forest .obj-tree { position: absolute; bottom: 35%; width: 35px; height: 160px; background: #27ae60; border-radius: 15px 15px 0 0; box-shadow: inset -10px 0 20px rgba(0,0,0,0.3); }
    .n-forest .obj-tree.t1 { left: 10%; } .n-forest .obj-tree.t2 { right: 15%; background: #2ecc71; height: 120px; }
    .n-forest .obj-glow { position: absolute; width: 8px; height: 8px; background: #a29bfe; border-radius: 50%; box-shadow: 0 0 20px 8px #a29bfe; }
    
    /* 7. Montaña Nevada */
    .n-mountain .env-bg { background: linear-gradient(to bottom, #81ecec, #dfe6e9); }
    .n-mountain .env-fg { background: #fff; border-top: 5px solid #ecf0f1; box-shadow: inset 0 10px 20px rgba(116, 185, 255, 0.2); }
    .n-mountain .obj-mount { position: absolute; bottom: 35%; left: -20px; width: 0; height: 0; border-left: 90px solid transparent; border-right: 90px solid transparent; border-bottom: 140px solid #b2bec3; }
    .n-mountain .obj-mount::after { content:''; position:absolute; top: 20px; left: -30px; border-left: 30px solid transparent; border-right: 30px solid transparent; border-bottom: 40px solid #fff; }
    .n-mountain .obj-mount2 { position: absolute; bottom: 35%; right: 10px; border-left: 70px solid transparent; border-right: 70px solid transparent; border-bottom: 100px solid #636e72; z-index: -1; }
    
    /* 8. Campamento Nocturno */
    .n-camping .env-bg { background: linear-gradient(to bottom, #130f40, #30336b); }
    .n-camping .env-fg { background: #222f3e; box-shadow: inset 0 15px 30px rgba(0,0,0,0.4); }
    .n-camping .obj-tent { position: absolute; bottom: 35px; left: 20px; border-left: 50px solid transparent; border-right: 50px solid transparent; border-bottom: 80px solid #10ac84; filter: drop-shadow(10px 10px 10px rgba(0,0,0,0.3)); }
    .n-camping .obj-tent::after { content:''; position:absolute; top: 30px; left: -15px; border-left: 15px solid transparent; border-right: 15px solid transparent; border-bottom: 50px solid #01a3a4; }
    .n-camping .obj-fire { position: absolute; bottom: 35px; right: 30px; width: 35px; height: 35px; background: #ff9f43; border-radius: 50% 0 50% 50%; transform: rotate(-45deg); box-shadow: 0 0 30px 10px rgba(255, 159, 67, 0.4); }
    .n-camping .obj-fire::after { content:''; position:absolute; top: 8px; left: 8px; width: 15px; height: 15px; background: #feca57; border-radius: 50% 0 50% 50%; }
    
    /* 9. Oasis */
    .n-desert .env-bg { background: linear-gradient(to bottom, #ff9f43, #feca57); }
    .n-desert .env-fg { background: #f3a683; box-shadow: inset 0 20px 20px rgba(0,0,0,0.05); }
    .n-desert .obj-dune { position: absolute; bottom: 35%; right: -50px; width: 220px; height: 100px; background: #f19066; border-radius: 50% 50% 0 0; }
    .n-desert .obj-water { position: absolute; bottom: 20px; left: 20px; width: 130px; height: 35px; background: #54a0ff; border-radius: 50%; border: 3px solid #2e86de; box-shadow: inset 0 5px 10px rgba(0,0,0,0.2); }
    
    /* 10. Cocina */
    .n-kitchen .env-bg { background: repeating-linear-gradient(90deg, #f5f6fa, #f5f6fa 20px, #dcdde1 20px, #dcdde1 22px); }
    .n-kitchen .env-fg { background: repeating-conic-gradient(#7f8fa6 0% 25%, #f5f6fa 0% 50%) 50% / 40px 40px; }
    .n-kitchen .obj-counter { position: absolute; bottom: 35%; left: 0; width: 100%; height: 40px; background: #353b48; border-bottom: 6px solid #2f3640; box-shadow: 0 10px 15px rgba(0,0,0,0.2); }
    .n-kitchen .obj-fridge { position: absolute; bottom: 35%; left: 20px; width: 65px; height: 110px; background: #e1b12c; border-radius: 5px 5px 0 0; border: 3px solid #c23616; box-shadow: 10px 0 20px rgba(0,0,0,0.1); }
    
    /* 11. Baño */
    .n-bathroom .env-bg { background: linear-gradient(to bottom, #4bcffa, #0fbcf9); }
    .n-bathroom .env-fg { background: #d2dae2; }
    .n-bathroom .obj-tub { position: absolute; bottom: 30px; left: 20px; width: 150px; height: 60px; background: #fff; border-radius: 10px 10px 35px 35px; box-shadow: inset -10px -10px 15px rgba(0,0,0,0.05), 10px 15px 20px rgba(0,0,0,0.1); border-top: 6px solid #f5f6fa; }
    .n-bathroom .obj-bubble { position: absolute; border: 2px solid rgba(255,255,255,0.9); background: rgba(255,255,255,0.4); border-radius: 50%; box-shadow: inset 3px 3px 5px #fff; }
    
    /* 12. Dojo Ninja (Referencia de Calidad - Cero Emojis) */
    .n-dojo .env-bg { background: linear-gradient(to bottom, #ff9a9e 0%, #fecfef 100%); } /* Atardecer */
    .n-dojo .obj-fuji {
      position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%);
      width: 170px; height: 100px;
      background: #4a5568;
      clip-path: polygon(50% 0, 0 100%, 100% 100%);
    }
    .n-dojo .obj-fuji::after {
      content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 35px;
      background: #fff;
      clip-path: polygon(50% 0, 15% 100%, 30% 70%, 45% 100%, 65% 75%, 75% 100%, 85% 65%, 100% 100%);
    }
    .n-dojo .obj-sun { position: absolute; bottom: 45%; right: 20%; width: 55px; height: 55px; background: #e53e3e; border-radius: 50%; box-shadow: 0 0 25px #e53e3e; }
    .n-dojo .env-fg {
      background: #a0522d;
      background-image: repeating-linear-gradient(90deg, rgba(0,0,0,0.15) 0px, rgba(0,0,0,0.15) 2px, transparent 2px, transparent 25px);
      border-top: 4px solid #8b4513;
      box-shadow: inset 0 20px 20px rgba(0,0,0,0.2);
    }
    .n-dojo .obj-torii { position: absolute; bottom: 35%; left: 15px; width: 105px; height: 95px; }
    .n-dojo .obj-torii-col { position: absolute; bottom: 0; width: 12px; height: 85px; background: #c53030; box-shadow: inset -3px 0 5px rgba(0,0,0,0.2); }
    .n-dojo .obj-torii-col.left { left: 15px; } .n-dojo .obj-torii-col.right { right: 15px; }
    .n-dojo .obj-torii-beam1 { position: absolute; top: 10px; left: 0; width: 105px; height: 12px; background: #9b2c2c; border-radius: 6px 6px 0 0; box-shadow: 0 4px 6px rgba(0,0,0,0.2); }
    .n-dojo .obj-torii-beam2 { position: absolute; top: 30px; left: 10px; width: 85px; height: 8px; background: #9b2c2c; box-shadow: 0 4px 6px rgba(0,0,0,0.2); }
    
    /* 13. Pista de Baile */
    .n-disco .env-bg { background: #111; }
    .n-disco .env-fg {
      background: repeating-linear-gradient(90deg, #111 0, #111 20px, #e91e63 20px, #e91e63 22px),
                  repeating-linear-gradient(0deg, #111 0, #111 20px, #00bcd4 20px, #00bcd4 22px);
      transform: perspective(250px) rotateX(40deg); transform-origin: bottom;
      height: 50%; bottom: -10%; border-top: 2px solid #fff;
    }
    .n-disco .obj-beam { position: absolute; top: -20px; left: 20%; width: 100px; height: 200px; background: linear-gradient(to bottom, rgba(233,30,99,0.6), transparent); clip-path: polygon(50% 0, 0 100%, 100% 100%); mix-blend-mode: screen; }
    .n-disco .obj-beam2 { position: absolute; top: -20px; right: 20%; width: 100px; height: 200px; background: linear-gradient(to bottom, rgba(0,188,212,0.6), transparent); clip-path: polygon(50% 0, 0 100%, 100% 100%); mix-blend-mode: screen; }
    
    /* 14. Estación Espacial */
    .n-space .env-bg { background: #2f3640; border-bottom: 8px solid #192a56; }
    .n-space .env-fg { background: radial-gradient(circle, #718093 15%, transparent 16%), #353b48; background-size: 20px 20px; }
    .n-space .obj-window { position: absolute; top: 15px; left: 50%; transform: translateX(-50%); width: 140px; height: 90px; background: #000; border: 10px solid #dcdde1; border-radius: 40px; overflow: hidden; box-shadow: inset 0 0 20px rgba(255,255,255,0.2); }
    .n-space .obj-window::after { content:''; position:absolute; bottom: -15px; right: 10px; width: 45px; height: 45px; background: #e1b12c; border-radius: 50%; box-shadow: inset 10px 10px 15px rgba(0,0,0,0.5); }
    
    /* 15. Castillo Real */
    .n-castle .env-bg { background: #7f8fa6; background-image: radial-gradient(#dcdde1 10%, transparent 11%); background-size: 30px 30px; }
    .n-castle .env-fg { background: #353b48; }
    .n-castle .obj-carpet { position: absolute; bottom: 0; left: 50%; transform: translateX(-50%) perspective(150px) rotateX(40deg); width: 80px; height: 100%; background: #c23616; transform-origin: bottom; box-shadow: 0 0 15px rgba(0,0,0,0.5); }
    .n-castle .obj-throne { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); width: 55px; height: 80px; background: #fbc531; border-radius: 20px 20px 0 0; box-shadow: inset 0 -10px 20px rgba(0,0,0,0.3); }
    
    /* 16. Mundo Submarino */
    .n-underwater .env-bg { background: linear-gradient(to bottom, #0097e6, #273c75); }
    .n-underwater .env-fg { background: #e1b12c; box-shadow: inset 0 20px 30px rgba(0,0,0,0.2); }
    .n-underwater .obj-ray { position: absolute; top: 0; left: 25%; width: 50px; height: 100%; background: linear-gradient(to bottom, rgba(255,255,255,0.3), transparent); transform: rotate(20deg); }
    .n-underwater .obj-coral { position: absolute; bottom: 30%; left: 30px; width: 15px; height: 75px; background: #e84118; border-radius: 10px; box-shadow: 15px 15px 0 -2px #ff7f50; }
    
    /* 17. Guarida Volcánica */
    .n-volcano .env-bg { background: linear-gradient(to bottom, #2f3640, #192a56); }
    .n-volcano .env-fg { background: #111; }
    .n-volcano .obj-lava { position: absolute; bottom: 15px; left: 50%; transform: translateX(-50%); width: 150px; height: 35px; background: #e84118; border-radius: 50%; box-shadow: 0 0 25px #e84118, inset 0 0 15px #c23616; }
    .n-volcano .obj-rock { position: absolute; bottom: 35%; left: 20px; border-left: 40px solid transparent; border-right: 40px solid transparent; border-bottom: 70px solid #353b48; }

    /* ==========================================================
       MI PROPUESTA ALTERNATIVA ("Mía") — misma arquitectura de
       capas (env-bg / env-mid / env-fg), objetos y composición
       distintos a la columna "Propuesta" para cada escenario.
       ========================================================== */
    .my-scene { box-shadow: inset 0 0 40px rgba(0,0,0,0.15); }
    .my-scene .pet, .my-scene .pet-shadow { z-index: 10; }
    .m-living .pet, .m-living .pet-shadow { left: 30%; }
    .m-bedroom .pet, .m-bedroom .pet-shadow { left: 70%; }
    .m-playroom .pet, .m-playroom .pet-shadow { left: 75%; }
    .m-park .pet, .m-park .pet-shadow { left: 75%; }
    .m-beach .pet, .m-beach .pet-shadow { left: 75%; }
    .m-forest .pet, .m-forest .pet-shadow { left: 25%; }
    .m-mountain .pet, .m-mountain .pet-shadow { left: 70%; }
    .m-camping .pet, .m-camping .pet-shadow { left: 60%; }
    .m-desert .pet, .m-desert .pet-shadow { left: 70%; }
    .m-kitchen .pet, .m-kitchen .pet-shadow { left: 75%; }
    .m-bathroom .pet, .m-bathroom .pet-shadow { left: 65%; }
    .m-dojo .pet, .m-dojo .pet-shadow { left: 70%; }
    .m-disco .pet, .m-disco .pet-shadow { left: 30%; }
    .m-space .pet, .m-space .pet-shadow { left: 65%; }
    .m-castle .pet, .m-castle .pet-shadow { left: 30%; }
    .m-underwater .pet, .m-underwater .pet-shadow { left: 25%; }
    .m-volcano .pet, .m-volcano .pet-shadow { left: 25%; }

    /* M1. Sala de Estar — lámpara de pie + alfombra redonda, piso espigado */
    .m-living .env-bg { background: linear-gradient(to bottom, #fff5e6 0%, #ffe8cc 100%); }
    .m-living .env-fg { background: #c8946b; background-image: repeating-linear-gradient(45deg, rgba(0,0,0,0.07) 0 6px, transparent 6px 12px), repeating-linear-gradient(-45deg, rgba(0,0,0,0.07) 0 6px, transparent 6px 12px); box-shadow: inset 0 15px 20px rgba(0,0,0,0.08); }
    .m-living .obj-lamp { position: absolute; bottom: 35%; right: 25px; width: 6px; height: 90px; background: #6d4c41; box-shadow: inset -2px 0 0 rgba(0,0,0,0.15); }
    .m-living .obj-lamp::before { content:''; position:absolute; top:-35px; left:-17px; width: 40px; height: 32px; background: #ffd54f; border-radius: 4px 4px 20px 20px; box-shadow: 0 0 30px 10px rgba(255,213,79,0.5), inset -6px -6px 0 rgba(0,0,0,0.08); }
    .m-living .obj-lamp::after { content:''; position:absolute; bottom:-4px; left:-14px; width: 34px; height: 8px; background: #4e342e; border-radius: 50%; box-shadow: 0 3px 4px rgba(0,0,0,0.2); }
    .m-living .obj-rug { position: absolute; bottom: 8%; left: 50%; transform: translateX(-50%); width: 130px; height: 45px; background: #e57373; border-radius: 50%; box-shadow: 0 0 0 6px #fff, inset 0 -10px 14px rgba(0,0,0,0.12), inset 0 8px 10px rgba(255,255,255,0.15), 0 8px 10px rgba(0,0,0,0.15); }
    .m-living .obj-frame { position: absolute; top: 22px; left: 130px; width: 40px; height: 50px; background: #fff8e1; border: 5px solid #8d6e63; box-shadow: 0 6px 10px rgba(0,0,0,0.12); }
    .m-living .obj-frame::before { content:''; position:absolute; inset: 6px; background: linear-gradient(160deg, #a5d6a7, #81c784); }

    /* M2. Dormitorio — ventana circular con luna+estrellas, mesita con velador */
    .m-bedroom .env-bg { background: linear-gradient(to bottom, #0b1a3a 0%, #1e2f65 100%); }
    .m-bedroom .env-fg { background: #2b2140; background-image: radial-gradient(circle, rgba(255,255,255,0.06) 2px, transparent 3px); background-size: 18px 18px; box-shadow: inset 0 18px 24px rgba(0,0,0,0.35); }
    .m-bedroom .obj-window { position: absolute; top: 18px; left: 50%; transform: translateX(-50%); width: 90px; height: 90px; background: #16224a; border: 6px solid #3b2f6b; border-radius: 50%; overflow: hidden; box-shadow: inset 0 0 15px rgba(0,0,0,0.4); }
    .m-bedroom .obj-window::before { content:''; position:absolute; top: 18px; right: 18px; width: 26px; height: 26px; background: #fdfbe0; border-radius: 50%; box-shadow: -8px 0 0 0 #16224a; }
    .m-bedroom .obj-window::after { content:''; position:absolute; top: 55px; left: 20px; width: 4px; height: 4px; background: #fff; border-radius: 50%; box-shadow: 15px -20px 0 #fff, 30px 10px 0 1px #fff; }
    .m-bedroom .obj-table { position: absolute; bottom: 35%; left: 25px; width: 45px; height: 35px; background: #6c5ce7; border-radius: 4px 4px 0 0; box-shadow: inset -6px 0 0 rgba(0,0,0,0.15), 0 6px 8px rgba(0,0,0,0.25); }
    .m-bedroom .obj-table::before { content:''; position:absolute; top:-22px; left:12px; width: 20px; height: 20px; background: #ffeaa7; border-radius: 4px 4px 12px 12px; box-shadow: 0 0 20px 6px rgba(255,234,167,0.5); }

    /* M3. Sala de Juegos — locomotora de juguete + vías */
    .m-playroom .env-bg { background: linear-gradient(to bottom, #fff9e6, #ffe0f0); }
    .m-playroom .env-fg { background: #dfe6e9; background-image: repeating-linear-gradient(90deg, #b2bec3 0 4px, transparent 4px 30px); box-shadow: inset 0 15px 15px rgba(0,0,0,0.05); }
    .m-playroom .obj-train { position: absolute; bottom: 40px; left: 30px; width: 60px; height: 35px; background: #e17055; border-radius: 8px 8px 0 0; box-shadow: inset -8px 0 0 rgba(0,0,0,0.12), inset 5px 0 0 rgba(255,255,255,0.2), 0 8px 8px rgba(0,0,0,0.15); }
    .m-playroom .obj-train::before { content:''; position:absolute; top:-18px; left:5px; width: 22px; height: 18px; background: #d63031; border-radius: 4px 4px 0 0; box-shadow: inset -4px 0 0 rgba(0,0,0,0.15); }
    .m-playroom .obj-train::after { content:''; position:absolute; bottom:-8px; left:5px; width: 12px; height: 12px; background: #2d3436; border-radius: 50%; box-shadow: 30px 0 0 #2d3436; }
    .m-playroom .obj-smoke { position: absolute; bottom: 65px; left: 45px; width: 12px; height: 12px; background: rgba(255,255,255,0.8); border-radius: 50%; box-shadow: 12px -10px 0 -1px rgba(255,255,255,0.6), 20px -22px 0 -3px rgba(255,255,255,0.4); }

    /* M4. Parque Mágico — hamaca colgante */
    .m-park .env-bg { background: linear-gradient(to bottom, #74ebd5, #ACB6E5); }
    .m-park .env-fg { background: #6ab04c; background-image: repeating-linear-gradient(100deg, rgba(255,255,255,0.08) 0 2px, transparent 2px 8px); box-shadow: inset 0 15px 20px rgba(0,0,0,0.15); }
    .m-park .obj-swing { position: absolute; bottom: 35%; left: 45px; width: 90px; height: 10px; background: #5d4037; border-radius: 4px; }
    .m-park .obj-swing::before, .m-park .obj-swing::after { content:''; position:absolute; top:8px; width: 3px; height: 65px; background: #6d4c41; }
    .m-park .obj-swing::before { left: 15px; }
    .m-park .obj-swing::after { right: 15px; }
    .m-park .obj-swing-seat { position: absolute; bottom: calc(35% - 55px); left: 105px; width: 30px; height: 8px; background: #fdcb6e; border-radius: 3px; box-shadow: inset 0 -3px 0 #e1b12c, 0 3px 3px rgba(0,0,0,0.15); }

    /* M5. Playa Tropical — sombrilla a rayas */
    .m-beach .env-bg { background: linear-gradient(to bottom, #4fc3f7, #0288d1 75%); }
    .m-beach .env-fg { background: #ffe0b2; background-image: repeating-radial-gradient(circle at 20px 0, rgba(255,255,255,0.5) 0 2px, transparent 2px 20px); box-shadow: inset 0 12px 18px rgba(0,0,0,0.05); }
    .m-beach .obj-umbrella { position: absolute; bottom: 40%; left: 40px; width: 90px; height: 45px; }
    .m-beach .obj-umbrella::before { content:''; position:absolute; top:0; left:0; width: 90px; height: 45px; background: repeating-linear-gradient(90deg, #e53935 0 15px, #fff 15px 30px); clip-path: polygon(50% 0, 0 100%, 100% 100%); box-shadow: 0 6px 10px rgba(0,0,0,0.15); }
    .m-beach .obj-umbrella::after { content:''; position:absolute; top: 40px; left: 42px; width: 6px; height: 70px; background: #8d6e63; box-shadow: inset -2px 0 0 rgba(0,0,0,0.15); }
    .m-beach .obj-shell { position: absolute; bottom: 15%; right: 40px; width: 30px; height: 20px; background: #ffccbc; border-radius: 50% 50% 0 0; box-shadow: inset -4px -4px 0 rgba(0,0,0,0.08), 0 4px 5px rgba(0,0,0,0.12); }
    .m-beach .obj-beachball { position: absolute; bottom: 12%; left: 145px; width: 32px; height: 32px; border-radius: 50%; background: conic-gradient(#e53935 0 45deg, #fff 45deg 90deg, #1e88e5 90deg 135deg, #fff 135deg 180deg, #e53935 180deg 225deg, #fff 225deg 270deg, #1e88e5 270deg 315deg, #fff 315deg 360deg); box-shadow: inset -6px -6px 10px rgba(0,0,0,0.15), 0 5px 6px rgba(0,0,0,0.2); }

    /* M6. Bosque Encantado — casa hongo con ventana iluminada + luciérnagas */
    .m-forest .env-bg { background: linear-gradient(to bottom, #1b2a1b, #08140a); }
    .m-forest .env-fg { background: #2e1a0f; background-image: radial-gradient(circle, rgba(255,150,50,0.15) 2px, transparent 3px); background-size: 16px 16px; box-shadow: inset 0 20px 25px rgba(0,0,0,0.5); }
    .m-forest .obj-mushroom { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); width: 26px; height: 55px; background: #f5deb3; border-radius: 6px 6px 0 0; }
    .m-forest .obj-mushroom::before { content:''; position:absolute; top:-38px; left:-27px; width: 80px; height: 45px; background: #d63031; border-radius: 40px 40px 6px 6px; box-shadow: inset -8px -8px 0 rgba(0,0,0,0.1); }
    .m-forest .obj-mushroom::after { content:''; position:absolute; top:-22px; left:5px; width: 14px; height: 14px; background: #ffeaa7; border-radius: 50%; box-shadow: 0 0 15px 6px rgba(255,234,167,0.7); }
    .m-forest .obj-firefly { position:absolute; width: 5px; height: 5px; background: #d4ff5c; border-radius: 50%; box-shadow: 0 0 10px 4px rgba(212,255,92,0.8); }

    /* M7. Montaña Nevada — pino nevado */
    .m-mountain .env-bg { background: linear-gradient(to bottom, #a1c4fd, #e2e2e2); }
    .m-mountain .env-fg { background: #f4f6f8; background-image: radial-gradient(circle, rgba(180,200,220,0.6) 2px, transparent 3px); background-size: 14px 14px; box-shadow: inset 0 12px 18px rgba(120,150,190,0.2); }
    .m-mountain .obj-pine { position: absolute; bottom: 35%; left: 45px; width: 0; height: 0; border-left: 22px solid transparent; border-right: 22px solid transparent; border-bottom: 34px solid #2f6b3a; filter: drop-shadow(3px 4px 3px rgba(0,0,0,0.2)); }
    .m-mountain .obj-pine::before { content:''; position:absolute; top: 18px; left: -18px; border-left: 18px solid transparent; border-right: 18px solid transparent; border-bottom: 28px solid #386e42; }
    .m-mountain .obj-pine::after { content:''; position:absolute; bottom:-10px; left:-4px; width: 8px; height: 12px; background: #6d4c41; }
    .m-mountain .obj-snowcap { position: absolute; bottom: calc(35% + 30px); left: 45px; width: 0; height: 0; border-left: 8px solid transparent; border-right: 8px solid transparent; border-bottom: 10px solid #fff; }

    /* M8. Campamento Nocturno — farol colgante + leños cruzados */
    .m-camping .env-bg { background: linear-gradient(to bottom, #05070f, #131a2e); }
    .m-camping .env-fg { background: #3e2b23; background-image: radial-gradient(circle, rgba(0,0,0,0.25) 2px, transparent 3px); background-size: 14px 14px; box-shadow: inset 0 18px 25px rgba(0,0,0,0.4); }
    .m-camping .obj-lantern { position: absolute; top: 25px; left: 40px; width: 20px; height: 26px; background: #ffd54f; border-radius: 4px; box-shadow: 0 0 25px 8px rgba(255,213,79,0.5); }
    .m-camping .obj-lantern::before { content:''; position:absolute; top:-18px; left:8px; width: 2px; height: 18px; background: #555; }
    .m-camping .obj-logs { position: absolute; bottom: 35px; right: 40px; width: 55px; height: 14px; }
    .m-camping .obj-logs::before, .m-camping .obj-logs::after { content:''; position:absolute; width: 55px; height: 10px; background: #6d4c41; border-radius: 5px; box-shadow: inset 0 -3px 0 rgba(0,0,0,0.2), inset 0 2px 0 rgba(255,255,255,0.1); }
    .m-camping .obj-logs::before { transform: rotate(20deg); }
    .m-camping .obj-logs::after { transform: rotate(-20deg); }
    .m-camping .obj-star2 { position:absolute; width:3px; height:3px; background:#fff; border-radius:50%; box-shadow: 0 0 6px 2px rgba(255,255,255,0.7); }

    /* M9. Oasis del Desierto — cactus + sol */
    .m-desert .env-bg { background: linear-gradient(to bottom, #ffd97a, #ff9f5a); }
    .m-desert .env-fg { background: #d68c45; background-image: repeating-linear-gradient(95deg, rgba(255,255,255,0.08) 0 3px, transparent 3px 14px); box-shadow: inset 0 12px 18px rgba(0,0,0,0.1); }
    .m-desert .obj-cactus { position: absolute; bottom: 35%; left: 50px; width: 18px; height: 60px; background: #43a047; border-radius: 8px; box-shadow: inset -4px 0 0 rgba(0,0,0,0.12), inset 3px 0 0 rgba(255,255,255,0.15), 0 6px 6px rgba(0,0,0,0.15); }
    .m-desert .obj-cactus::before, .m-desert .obj-cactus::after { content:''; position:absolute; width: 10px; height: 24px; background: #43a047; border-radius: 6px; box-shadow: inset -3px 0 0 rgba(0,0,0,0.12); }
    .m-desert .obj-cactus::before { top: 10px; left: -14px; }
    .m-desert .obj-cactus::after { top: 22px; right: -14px; }
    .m-desert .obj-sun2 { position: absolute; top: 18px; right: 25px; width: 50px; height: 50px; background: #ffe082; border-radius: 50%; box-shadow: 0 0 35px #ffe082, inset -8px -8px 0 rgba(230,180,40,0.3); }
    .m-desert .obj-cactus2 { position: absolute; bottom: 35%; left: 100px; width: 10px; height: 32px; background: #4caf50; border-radius: 6px; box-shadow: inset -3px 0 0 rgba(0,0,0,0.12), 0 5px 5px rgba(0,0,0,0.15); }

    /* M10. Cocina del Chef — cocina con olla humeante */
    .m-kitchen .env-bg { background: linear-gradient(to bottom, #fff3e0, #ffe0b2); }
    .m-kitchen .env-fg { background: #efebe9; background-image: repeating-linear-gradient(0deg, rgba(0,0,0,0.06) 0 2px, transparent 2px 20px), repeating-linear-gradient(90deg, rgba(0,0,0,0.06) 0 2px, transparent 2px 20px); box-shadow: inset 0 12px 15px rgba(0,0,0,0.08); }
    .m-kitchen .obj-stove { position: absolute; bottom: 35%; left: 30px; width: 70px; height: 45px; background: #616161; border-radius: 4px; box-shadow: inset -8px 0 0 rgba(0,0,0,0.2), inset 5px 0 0 rgba(255,255,255,0.08), 0 6px 8px rgba(0,0,0,0.2); }
    .m-kitchen .obj-stove::before { content:''; position:absolute; top:-6px; left:10px; width: 20px; height: 6px; background: #424242; border-radius: 3px; box-shadow: 30px 0 0 #424242; }
    .m-kitchen .obj-pot { position: absolute; bottom: calc(35% + 39px); left: 45px; width: 40px; height: 24px; background: #90a4ae; border-radius: 4px 4px 10px 10px; }
    .m-kitchen .obj-pot::before { content:''; position:absolute; top:-6px; left:-4px; width: 48px; height: 6px; background: #78909c; border-radius: 3px; }
    .m-kitchen .obj-steam { position:absolute; width: 8px; height: 8px; background: rgba(255,255,255,0.7); border-radius: 50%; box-shadow: 10px -14px 0 -1px rgba(255,255,255,0.5), -8px -22px 0 -2px rgba(255,255,255,0.35); }

    /* M11. Baño de Burbujas — bacha con espejo + patito */
    .m-bathroom .env-bg { background: linear-gradient(to bottom, #e0f7fa, #b2ebf2); }
    .m-bathroom .env-fg { background: #eceff1; background-image: repeating-linear-gradient(0deg, rgba(0,0,0,0.05) 0 2px, transparent 2px 18px), repeating-linear-gradient(90deg, rgba(0,0,0,0.05) 0 2px, transparent 2px 18px); }
    .m-bathroom .obj-sink { position: absolute; bottom: 35%; left: 40px; width: 60px; height: 30px; background: #fff; border-radius: 30px 30px 6px 6px; box-shadow: inset 0 -6px 8px rgba(0,0,0,0.06), 0 8px 10px rgba(0,0,0,0.08); }
    .m-bathroom .obj-sink::before { content:''; position:absolute; top:-20px; left:26px; width: 8px; height: 20px; background: #b0bec5; }
    .m-bathroom .obj-mirror { position: absolute; bottom: calc(35% + 45px); left: 42px; width: 56px; height: 40px; background: linear-gradient(135deg, #e1f5fe, #b3e5fc); border: 4px solid #cfd8dc; border-radius: 6px; }
    .m-bathroom .obj-duck { position: absolute; bottom: 15%; right: 40px; width: 26px; height: 20px; background: #ffd600; border-radius: 50% 50% 45% 45%; box-shadow: inset -5px -3px 0 rgba(0,0,0,0.12), 0 3px 4px rgba(0,0,0,0.15); }
    .m-bathroom .obj-duck::before { content:''; position:absolute; top:-8px; right:2px; width: 12px; height: 12px; background: #ffd600; border-radius: 50%; }

    /* M12. Dojo Ninja — bonsai + farol de papel, piso tatami */
    .m-dojo .env-bg { background: linear-gradient(to bottom, #2d2340 0%, #4a3a63 100%); }
    .m-dojo .env-fg { background: #c9a876; background-image: repeating-linear-gradient(0deg, rgba(0,0,0,0.08) 0 2px, transparent 2px 30px), repeating-linear-gradient(90deg, rgba(0,0,0,0.06) 0 2px, transparent 2px 30px); border-top: 4px solid #a9824f; box-shadow: inset 0 15px 20px rgba(0,0,0,0.2); }
    .m-dojo .obj-bonsai { position: absolute; bottom: 35%; left: 45px; width: 30px; height: 20px; background: #8d6e63; border-radius: 4px 4px 10px 10px; box-shadow: inset -4px -3px 0 rgba(0,0,0,0.15), 0 5px 5px rgba(0,0,0,0.2); }
    .m-dojo .obj-bonsai::before { content:''; position:absolute; bottom: 18px; left: 10px; width: 6px; height: 22px; background: #5d4037; border-radius: 3px; }
    .m-dojo .obj-bonsai::after { content:''; position:absolute; bottom: 34px; left: -6px; width: 42px; height: 22px; background: #4caf50; border-radius: 50% 50% 40% 40%; box-shadow: 12px -8px 0 -4px #66bb6a, inset -6px -6px 0 rgba(0,0,0,0.1); }
    .m-dojo .obj-paper-lamp { position: absolute; top: 25px; right: 45px; width: 26px; height: 34px; background: #fff3d6; border-radius: 40%; box-shadow: 0 0 25px 8px rgba(255,243,214,0.5), inset -6px 0 0 rgba(0,0,0,0.06); }
    .m-dojo .obj-paper-lamp::before { content:''; position:absolute; top:-14px; left:12px; width: 2px; height: 14px; background: #555; }
    .m-dojo .obj-toro { position: absolute; bottom: 35%; right: 20px; width: 22px; height: 26px; background: #78716c; border-radius: 3px; box-shadow: inset -4px 0 0 rgba(0,0,0,0.2); }
    .m-dojo .obj-toro::before { content:''; position:absolute; top:-14px; left:1px; width: 20px; height: 14px; background: #78716c; border-radius: 40% 40% 10% 10%; box-shadow: inset -3px 0 0 rgba(0,0,0,0.2); }
    .m-dojo .obj-toro::after { content:''; position:absolute; top:-6px; left:6px; width: 10px; height: 8px; background: #ffd54f; border-radius: 2px; box-shadow: 0 0 12px 4px rgba(255,213,79,0.6); }

    /* M13. Pista de Baile — bola de espejos colgante + parlante */
    .m-disco .env-bg { background: radial-gradient(circle at 50% 30%, #3a1c5e, #100826); }
    .m-disco .env-fg { background: repeating-conic-gradient(#1a1128 0% 12.5%, #2a1a45 0% 25%) 50% / 40px 40px; box-shadow: inset 0 15px 20px rgba(0,0,0,0.4); }
    .m-disco .obj-ball { position: absolute; top: 20px; left: 50%; transform: translateX(-50%); width: 44px; height: 44px; background: radial-gradient(circle at 30% 30%, #eee, #9aa); border-radius: 50%; background-image: repeating-linear-gradient(45deg, rgba(0,0,0,0.15) 0 3px, transparent 3px 6px), repeating-linear-gradient(-45deg, rgba(0,0,0,0.15) 0 3px, transparent 3px 6px); box-shadow: 0 0 30px 10px rgba(255,255,255,0.3); }
    .m-disco .obj-ball::before { content:''; position:absolute; top:-16px; left:21px; width: 2px; height: 16px; background: #444; }
    .m-disco .obj-speaker { position: absolute; bottom: 35px; right: 25px; width: 34px; height: 55px; background: #1a1a1a; border-radius: 4px; }
    .m-disco .obj-speaker::before, .m-disco .obj-speaker::after { content:''; position:absolute; left:50%; transform:translateX(-50%); border-radius: 50%; background: #333; box-shadow: inset 0 0 0 3px #555; }
    .m-disco .obj-speaker::before { top: 6px; width: 22px; height: 22px; }
    .m-disco .obj-speaker::after { bottom: 6px; width: 16px; height: 16px; }

    /* M14. Estación Espacial — panel de control + planeta */
    .m-space .env-bg { background: radial-gradient(circle at 30% 20%, #1a2a4a 0%, #05070f 100%); }
    .m-space .env-fg { background: #232b3d; background-image: repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 2px, transparent 2px 24px); box-shadow: inset 0 15px 20px rgba(0,0,0,0.4); }
    .m-space .obj-star2 { position:absolute; width: 3px; height: 3px; background: #fff; border-radius: 50%; box-shadow: 0 0 6px 2px rgba(255,255,255,0.8); }
    .m-space .obj-panel { position: absolute; bottom: 35%; left: 30px; width: 70px; height: 34px; background: #37474f; border-radius: 6px; box-shadow: 0 6px 10px rgba(0,0,0,0.3); }
    .m-space .obj-panel::before { content:''; position:absolute; top: 8px; left: 8px; width: 10px; height: 10px; background: #ff5252; border-radius: 50%; box-shadow: 18px 0 0 #ffca28, 36px 0 0 #69f0ae; }
    .m-space .obj-planet2 { position: absolute; top: 20px; right: 30px; width: 40px; height: 40px; background: radial-gradient(circle at 30% 30%, #ff8a65, #bf360c); border-radius: 50%; box-shadow: 0 0 20px rgba(255,138,101,0.4); }

    /* M15. Castillo Real — vitral + armadura */
    .m-castle .env-bg { background: linear-gradient(to bottom, #2c1250, #4a1d7a); }
    .m-castle .env-fg { background: #d7ccc8; background-image: repeating-linear-gradient(45deg, rgba(0,0,0,0.06) 0 10px, transparent 10px 20px), repeating-linear-gradient(-45deg, rgba(0,0,0,0.06) 0 10px, transparent 10px 20px); box-shadow: inset 0 15px 20px rgba(0,0,0,0.15); }
    .m-castle .obj-glass { position: absolute; top: 15px; left: 50%; transform: translateX(-50%); width: 55px; height: 75px; background: conic-gradient(#e53935 0deg 90deg, #1e88e5 90deg 180deg, #fdd835 180deg 270deg, #43a047 270deg 360deg); border-radius: 50% 50% 0 0; border: 5px solid #4a2b60; box-shadow: 0 0 20px rgba(255,255,255,0.3); }
    .m-castle .obj-armor { position: absolute; bottom: 35%; right: 30px; width: 30px; height: 55px; background: #90a4ae; border-radius: 8px 8px 4px 4px; box-shadow: inset -6px 0 0 rgba(0,0,0,0.15), inset 4px 0 0 rgba(255,255,255,0.2), 0 6px 8px rgba(0,0,0,0.2); }
    .m-castle .obj-armor::before { content:''; position:absolute; top:-16px; left:5px; width: 20px; height: 20px; background: #78909c; border-radius: 50%; box-shadow: inset -4px -2px 0 rgba(0,0,0,0.2); }
    .m-castle .obj-banner { position: absolute; top: 10px; left: 30px; width: 32px; height: 70px; background: linear-gradient(#7b1fa2, #4a148c); clip-path: polygon(0 0, 100% 0, 100% 85%, 50% 100%, 0 85%); box-shadow: 4px 6px 10px rgba(0,0,0,0.25); }
    .m-castle .obj-banner::before { content:''; position:absolute; top:14px; left:8px; width: 16px; height: 16px; background: #ffd54f; border-radius: 50%; box-shadow: 0 0 8px rgba(255,213,79,0.5); }

    /* M16. Mundo Submarino — cofre del tesoro brillante */
    .m-underwater .env-bg { background: linear-gradient(to bottom, #01579b, #002136); }
    .m-underwater .env-fg { background: #e0c68a; background-image: radial-gradient(circle, rgba(255,255,255,0.18) 2px, transparent 3px); background-size: 14px 14px; box-shadow: inset 0 15px 20px rgba(0,0,0,0.2); }
    .m-underwater .obj-chest { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); width: 55px; height: 30px; background: #6d4c41; border-radius: 0 0 6px 6px; box-shadow: inset -8px 0 0 rgba(0,0,0,0.2), inset 5px 0 0 rgba(255,255,255,0.08), 0 8px 8px rgba(0,0,0,0.25); }
    .m-underwater .obj-chest::before { content:''; position:absolute; top:-16px; left:0; width: 55px; height: 20px; background: #8d6e63; border-radius: 10px 10px 0 0; box-shadow: inset -6px 0 0 rgba(0,0,0,0.15); }
    .m-underwater .obj-chest::after { content:''; position:absolute; top:-4px; left:24px; width: 8px; height: 8px; background: #ffd54f; border-radius: 2px; box-shadow: 0 0 20px 8px rgba(255,213,79,0.6); }
    .m-underwater .obj-bub { position:absolute; border: 2px solid rgba(255,255,255,0.6); border-radius: 50%; }

    /* M17. Guarida Volcánica — cono volcánico en erupción */
    .m-volcano .env-bg { background: linear-gradient(to bottom, #1a0a0a, #3d0c0c); }
    .m-volcano .env-fg { background: #1a1a1a; background-image: repeating-linear-gradient(60deg, rgba(255,80,0,0.06) 0 3px, transparent 3px 16px); box-shadow: inset 0 15px 20px rgba(0,0,0,0.4); }
    .m-volcano .obj-cone { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); width: 130px; height: 90px; background: #2b2b2b; clip-path: polygon(50% 0, 20% 100%, 80% 100%); }
    .m-volcano .obj-cone::after { content:''; position:absolute; top:-10px; left:50%; transform: translateX(-50%); width: 22px; height: 22px; background: #ff5722; border-radius: 50%; box-shadow: 0 0 30px 12px rgba(255,87,34,0.6); }
    .m-volcano .obj-ember { position:absolute; width: 4px; height: 4px; background: #ffab40; border-radius: 50%; box-shadow: 0 0 8px 3px rgba(255,171,64,0.7); }

  </style>
</head>
<body>
  <header>
    <h1>Demo visual de escenarios</h1>
    <p>Propuesta de profundidad para Mi Pet: fondo lejano, plano medio, primer plano, iluminación y mascota protagonista. Ahora con dos alternativas por escenario: "Propuesta" y "Mía".</p>
  </header>
  <main class="gallery">
    <!-- 1. Sala de Estar -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old living-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-living"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-window"></div><div class="obj-sofa"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-living"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-lamp"></div><div class="obj-rug"></div><div class="obj-frame"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Sala de estar</h2><p>Ventana, sofá, alfombra y luz cálida.</p></div>
    </article>

    <!-- 2. Dormitorio -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old bedroom-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-bedroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-moon"></div><div class="obj-bed"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-bedroom"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-window"></div><div class="obj-table"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Dormitorio nocturno</h2><p>Luna, estrellas y cama en capas.</p></div>
    </article>

    <!-- 3. Playa Tropical -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old beach-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-beach"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-sun"></div><div class="obj-palm-trunk"></div><div class="obj-palm-leaves"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-beach"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-umbrella"></div><div class="obj-shell"></div><div class="obj-beachball"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Playa tropical</h2><p>Horizonte, agua, arena, palmera y luz solar.</p></div>
    </article>

    <!-- 4. Bosque Encantado -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old forest-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-forest"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tree t1"></div><div class="obj-tree t2"></div><div class="obj-glow" style="top:40px; left:60px;"></div><div class="obj-glow" style="top:70px; right:80px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-forest"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-mushroom"></div><div class="obj-firefly" style="top:50px; left:70px;"></div><div class="obj-firefly" style="top:90px; right:60px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Bosque encantado</h2><p>Montaña lejana, árboles cercanos y suelo profundo.</p></div>
    </article>

    <!-- 5. Cocina del Chef -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old kitchen-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-kitchen"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-fridge"></div><div class="obj-counter"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-kitchen"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-stove"></div><div class="obj-pot"></div><div class="obj-steam" style="bottom:130px; left:130px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Cocina del chef</h2><p>Azulejos, muebles, mesada y objetos de cocina.</p></div>
    </article>

    <!-- 6. Estación Espacial -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old space-old"><span class="version-tag">Antes</span><div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div></div>
        <div class="scene new-scene n-space"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-window"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-space"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-star2" style="top:20px; left:30px;"></div><div class="obj-star2" style="top:60px; left:90px;"></div><div class="obj-planet2"></div><div class="obj-panel"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Estación espacial</h2><p>Planeta, estrellas, plataforma y luces de ambiente.</p></div>
    </article>

    <!-- 7. Sala de Juegos -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old playroom-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-playroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-block1"></div><div class="obj-block2"></div><div class="obj-ball"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-playroom"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-train"></div><div class="obj-smoke"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Sala de juegos</h2><p>Juguetes, burbujas de color y suelo acuático.</p></div>
    </article>

    <!-- 8. Parque Mágico -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old park-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-park"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-shroom"></div><div class="obj-magic" style="top:50px; left:80px;"></div><div class="obj-magic" style="top:30px; right:50px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-park"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-swing"></div><div class="obj-swing-seat"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Parque mágico</h2><p>Cielo abierto, árbol protagonista y césped en capas.</p></div>
    </article>

    <!-- 9. Montaña Nevada -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old mountain-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-mountain"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-mount2"></div><div class="obj-mount"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-mountain"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-pine"></div><div class="obj-snowcap"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Montaña nevada</h2><p>Montañas lejanas, nieve y suelo frío con profundidad.</p></div>
    </article>

    <!-- 10. Campamento Nocturno -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old camping-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-camping"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tent"></div><div class="obj-fire"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-camping"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-lantern"></div><div class="obj-logs"></div><div class="obj-star2" style="top:20px; left:100px;"></div><div class="obj-star2" style="top:40px; left:140px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Campamento nocturno</h2><p>Cielo oscuro, estrellas, pasto y tienda iluminada.</p></div>
    </article>

    <!-- 11. Oasis del Desierto -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old desert-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-desert"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-dune"></div></div>
          <div class="env-fg"><div class="obj-water"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-desert"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-cactus"></div><div class="obj-sun2"></div><div class="obj-cactus2"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Oasis del desierto</h2><p>Capas cálidas, oasis de agua y cactus en primer plano.</p></div>
    </article>

    <!-- 12. Baño de Burbujas -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old bathroom-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-bathroom"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-tub"></div><div class="obj-bubble" style="bottom:60px; left:40px; width:20px; height:20px;"></div><div class="obj-bubble" style="bottom:90px; left:70px; width:15px; height:15px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-bathroom"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-mirror"></div><div class="obj-sink"></div><div class="obj-duck"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Baño de burbujas</h2><p>Agua, espuma, burbujas y ambiente luminoso.</p></div>
    </article>

    <!-- 13. Dojo Ninja (Referencia de Calidad - Fuji y Torii Mejorados) -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old dojo-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-dojo"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid">
            <div class="obj-sun"></div>
            <div class="obj-fuji"></div>
            <div class="obj-torii">
              <div class="obj-torii-col left"></div><div class="obj-torii-col right"></div>
              <div class="obj-torii-beam1"></div><div class="obj-torii-beam2"></div>
            </div>
          </div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-dojo"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-bonsai"></div><div class="obj-paper-lamp"></div><div class="obj-toro"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Dojo ninja</h2><p>Piso de madera, torii, Monte Fuji y sol rojo.</p></div>
    </article>

    <!-- 14. Pista de Baile -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old disco-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-disco"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-beam"></div><div class="obj-beam2"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-disco"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-ball"></div><div class="obj-speaker"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Pista de baile</h2><p>Luz de color, reflejos y ambiente nocturno.</p></div>
    </article>

    <!-- 15. Castillo Real -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old castle-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-castle"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-throne"></div></div>
          <div class="env-fg"><div class="obj-carpet"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-castle"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-glass"></div><div class="obj-armor"></div><div class="obj-banner"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Castillo real</h2><p>Arquitectura, suelo ceremonial y luz violeta.</p></div>
    </article>

    <!-- 16. Mundo Submarino -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old underwater-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-underwater"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-ray"></div><div class="obj-coral"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-underwater"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-chest"></div><div class="obj-bub" style="bottom:80px; left:120px; width:14px; height:14px;"></div><div class="obj-bub" style="bottom:130px; left:140px; width:10px; height:10px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Mundo submarino</h2><p>Agua profunda, corales y puntos de luz.</p></div>
    </article>

    <!-- 17. Guarida Volcánica -->
    <article class="scene-card">
      <div class="comparison">
        <div class="scene old volcano-old"><span class="version-tag">Antes</span></div>
        <div class="scene new-scene n-volcano"><span class="version-tag">Propuesta</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-rock"></div></div>
          <div class="env-fg"><div class="obj-lava"></div></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
        <div class="scene my-scene m-volcano"><span class="version-tag">Mía</span>
          <div class="env-bg"></div>
          <div class="env-mid"><div class="obj-cone"></div><div class="obj-ember" style="top:60px; left:100px;"></div><div class="obj-ember" style="top:90px; left:150px;"></div></div>
          <div class="env-fg"></div>
          <div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>
        </div>
      </div>
      <div class="scene-label"><h2>Guarida volcánica</h2><p>Lava, roca oscura y luz naranja de fondo.</p></div>
    </article>
  </main>

  <div id="approve-bar">
    <span id="approve-count">0 de 17 decididos</span>
    <button id="btn-download">📥 Descargar elecciones (JSON)</button>
    <button id="btn-clear">🗑️ Limpiar selección</button>
  </div>

  <script>
    // JS del archivo original intacto.
    document.querySelectorAll('.scene.old').forEach(scene => {
      if (scene.querySelector('.pet')) return;
      scene.insertAdjacentHTML('beforeend', '<div class="pet-shadow"></div><div class="pet"><div class="face"><span class="mouth">⌣</span></div></div>');
    });
  </script>

  <script>
    // ==========================================================
    // PANEL DE DECISIÓN — agrega 2 radios (Propuesta / Mía) por
    // escenario y permite descargar un JSON con lo elegido.
    // El ID de cada escenario sale de la clase "n-XXX" de la
    // columna Propuesta (ej: n-dojo -> bg_dojo), igual que en los
    // IDs reales de la app.
    // ==========================================================
    const STORAGE_KEY = 'escenariosDemoElecciones';

    function initApprovalPanel() {
      document.querySelectorAll('.scene-card').forEach(card => {
        const newScene = card.querySelector('.new-scene');
        if (!newScene) return;
        const shapeClass = [...newScene.classList].find(c => c.startsWith('n-'));
        if (!shapeClass) return;

        const id = 'bg_' + shapeClass.slice(2);
        card.dataset.scenarioId = id;

        const label = card.querySelector('.scene-label');
        if (!label) return;

        const row = document.createElement('div');
        row.className = 'approve-row';
        row.innerHTML = `
          <label><input type="radio" name="choice-${id}" value="propuesta" data-id="${id}"> Propuesta</label>
          <label><input type="radio" name="choice-${id}" value="mia" data-id="${id}"> Mía</label>
        `;
        label.appendChild(row);
      });

      // Restaura selección guardada (si volvés a abrir el archivo)
      const saved = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
      document.querySelectorAll('.approve-row input[type="radio"]').forEach(radio => {
        if (saved[radio.dataset.id] === radio.value) {
          radio.checked = true;
          radio.closest('.scene-card').classList.add('is-approved');
        }
      });

      document.querySelectorAll('.approve-row input[type="radio"]').forEach(radio => {
        radio.addEventListener('change', onChoiceChange);
      });

      updateApprovalUI();
    }

    function onChoiceChange(e) {
      e.target.closest('.scene-card').classList.add('is-approved');

      const choices = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
      choices[e.target.dataset.id] = e.target.value;
      localStorage.setItem(STORAGE_KEY, JSON.stringify(choices));

      updateApprovalUI();
    }

    function updateApprovalUI() {
      const total = document.querySelectorAll('.scene-card').length;
      const decided = new Set();
      document.querySelectorAll('.approve-row input[type="radio"]:checked').forEach(r => decided.add(r.dataset.id));
      document.getElementById('approve-count').textContent = `${decided.size} de ${total} decididos`;
    }

    function downloadApproved() {
      const choices = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
      const data = {
        fecha: new Date().toISOString(),
        elecciones: choices
      };
      const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'escenarios-elegidos.json';
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    }

    function clearApproval() {
      document.querySelectorAll('.approve-row input[type="radio"]').forEach(r => r.checked = false);
      document.querySelectorAll('.scene-card').forEach(c => c.classList.remove('is-approved'));
      localStorage.removeItem(STORAGE_KEY);
      updateApprovalUI();
    }

    document.getElementById('btn-download').addEventListener('click', downloadApproved);
    document.getElementById('btn-clear').addEventListener('click', clearApproval);

    initApprovalPanel();
  </script>
</body>
</html>

```

### Archivo: `desarrollo/prueba-accesorios-visual.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Prueba visual de formas y accesorios</title>
  <style>
    @page { size: landscape; margin: 8mm; }
    * { box-sizing: border-box; }
    body { margin: 0; font-family: Arial, sans-serif; color: #222; font-size: 9px; }
    h1 { margin: 0 0 4px; color: #5d2a86; font-size: 18px; }
    p { margin: 2px 0 8px; }
    table { width: 100%; border-collapse: collapse; table-layout: fixed; }
    th, td { border: 1px solid #aaa; text-align: center; vertical-align: middle; padding: 3px; }
    th { background: #eee4f8; color: #44206a; }
    th.accessory-name { width: 70px; }
    th.accessory-head { height: 58px; font-size: 7px; }
    td { height: 48px; }
    .form-label { display: block; font-weight: bold; font-size: 8px; }
    .form-cell { display: flex; align-items: center; justify-content: center; gap: 3px; }
    .form-preview { position: relative; width: 30px; height: 26px; flex: 0 0 30px; }
    .mini-pet { position: absolute; left: 50%; top: 50%; width: 22px; height: 22px; transform: translate(-50%, -50%); background: #ff80ab; box-shadow: 0 1px 3px #999; }
    .circle { border-radius: 50%; }
    .mochi { width: 28px; height: 20px; border-radius: 50%; }
    .gotita { border-radius: 50% 50% 40% 40% / 62% 62% 30% 30%; }
    .squircle { border-radius: 28%; }
    .dumpling { width: 29px; height: 20px; border-radius: 50% 50% 35% 35% / 65% 65% 35% 35%; }
    .accessory-image { display: block; width: 30px; height: 30px; object-fit: contain; margin: 0 auto 2px; }
    .accessory-fallback { display: none; font-size: 24px; line-height: 30px; height: 30px; }
    .blank-cell { color: transparent; }
    .legend { margin-top: 8px; font-size: 9px; }
    .instructions { margin-top: 10px; border-top: 2px solid #5d2a86; padding-top: 6px; font-size: 9px; }
    .instructions h2 { margin: 4px 0 2px; color: #5d2a86; font-size: 12px; }
    .instructions ol { margin: 2px 0 6px 18px; padding: 0; }
    .instructions li { margin-bottom: 2px; }
    @media print { .legend { display: none; } }
  </style>
</head>
<body>
  <h1>Prueba visual: formas y accesorios</h1>
  <p>Las imágenes identifican cada accesorio y cada forma. Completa las celdas vacías con OK o el código del error. Imprimir en orientación horizontal.</p>
  <table id="test-table"></table>
  <section class="instructions">
    <h2>Cómo completar la hoja</h2>
    <ol>
      <li>Lee la forma y el tamaño de la primera columna, por ejemplo: <strong>Dumpling - Grande</strong>.</li>
      <li>En la aplicación, selecciona exactamente esa forma y ese tamaño.</li>
      <li>Prueba el accesorio que aparece en cada columna.</li>
      <li>Escribe <strong>OK</strong> si se ve bien en esa combinación.</li>
      <li>Si hay un problema, escribe el código y una nota corta. Ejemplo: <strong>CHICO - la corona no cubre la cabeza</strong>.</li>
    </ol>
    <h2>Qué enviarme después</h2>
    <p>Envíame solo las celdas con errores usando este formato:</p>
    <p><strong>Forma | Tamaño | Accesorio | Código | Explicación</strong></p>
    <p>Ejemplo: <strong>Dumpling | Grande | Corona Real | ARRIBA | La corona queda demasiado alta</strong>.</p>
    <p class="legend"><strong>Códigos:</strong> CHICO = demasiado pequeño; GRANDE = demasiado grande; ARRIBA = demasiado alto; ABAJO = demasiado bajo; IZQ = hacia la izquierda; DER = hacia la derecha; ROT = necesita rotación; SVG = tiene espacio vacío; FORMA = no se adapta a la silueta.</p>
  </section>
  <script>
    const forms = [
      ['Círculo', 'circle'], ['Mochi', 'mochi'], ['Gotita', 'gotita'],
      ['Squircle', 'squircle'], ['Dumpling', 'dumpling']
    ];
    const sizes = ['Chico', 'Mediano', 'Grande'];
    const fallbackIcons = {
      'Corona Real': '👑', 'Gorro Fiesta': '🥳', 'Lazo Rosado': '🎀',
      'Gorro de Chef': '👨‍🍳', 'Gorro de Mago': '🧙', 'Gorro Pirata': '🏴‍☠️',
      'Casco Espacial': '👨‍🚀', 'Gorro de Lana': '🧶', 'Gorro de Baño': '🛁',
      'Flor Primavera': '🌸', 'Gorro Ninja': '🥷', 'Lentes Corazón': '💖',
      'Lentes Estrella': '⭐', 'Visor de Buceo': '🤿', 'Auriculares Gato': '🎧',
      'Orejitas Violeta': '🐱', 'Orejas Conejo': '🐰', 'Moño Coquette': '🎀',
      'Varita Mágica': '🪄', 'Alas de Hada': '🧚', 'Bufanda Calientita': '🧣'
    };
    const accessories = [
      ['Corona Real', 'img/accessories/hat_crown.svg', 'head'],
      ['Gorro Fiesta', 'img/accessories/party_hat.svg', 'head'],
      ['Lazo Rosado', 'img/accessories/ribbon_pink.svg', 'head'],
      ['Gorro de Chef', 'img/accessories/chef_hat.svg', 'head'],
      ['Gorro de Mago', 'img/accessories/wizard_hat.svg', 'head'],
      ['Gorro Pirata', 'img/accessories/pirate_hat.svg', 'head'],
      ['Casco Espacial', 'img/accessories/space_helmet.svg', 'head'],
      ['Gorro de Lana', 'img/accessories/winter_beanie.svg', 'head'],
      ['Gorro de Baño', 'img/accessories/shower_cap.svg', 'head'],
      ['Flor Primavera', 'img/accessories/flower_pink.svg', 'head'],
      ['Gorro Ninja', 'img/accessories/ninja_band.svg', 'head'],
      ['Lentes Corazón', 'img/accessories/glasses_cool.svg', 'eyes'],
      ['Lentes Estrella', 'img/accessories/star_glasses.svg', 'eyes'],
      ['Visor de Buceo', 'img/accessories/snorkel.svg', 'eyes'],
      ['Auriculares Gato', 'img/accessories/headphones.svg', 'ears'],
      ['Orejitas Violeta', 'img/accessories/cat_ears.svg', 'ears'],
      ['Orejas Conejo', 'img/accessories/bunny_ears.svg', 'ears'],
      ['Moño Coquette', 'img/accessories/bow_tie.svg', 'neck'],
      ['Varita Mágica', 'img/accessories/magic_wand.svg', 'neck'],
      ['Alas de Hada', 'img/accessories/fairy_wings.svg', 'neck'],
      ['Bufanda Calientita', 'img/accessories/winter_scarf.svg', 'neck']
    ];

    const table = document.getElementById('test-table');
    const header = document.createElement('tr');
    header.innerHTML = '<th class="accessory-name">Forma<br>Tamaño</th>';
    accessories.forEach(([name, image]) => {
      const th = document.createElement('th');
      th.className = 'accessory-head';
      th.innerHTML = `<span class="accessory-fallback">${fallbackIcons[name] || '🎁'}</span><img class="accessory-image" src="../${image}" alt="${name}" onerror="this.style.display='none'; this.previousElementSibling.style.display='block';">${name}`;
      header.appendChild(th);
    });
    table.appendChild(header);

    forms.forEach(([formName, formClass]) => {
      sizes.forEach(size => {
      const row = document.createElement('tr');
      const label = document.createElement('th');
      label.className = 'accessory-name';
      label.innerHTML = `<div class="form-cell"><div class="form-preview"><div class="mini-pet ${formClass}"></div></div><span><span class="form-label">${formName}</span>${size}</span></div>`;
      row.appendChild(label);

      accessories.forEach(() => {
        const cell = document.createElement('td');
        cell.className = 'blank-cell';
        cell.innerHTML = '&nbsp;';
        row.appendChild(cell);
      });
      table.appendChild(row);
      });
    });
  </script>
</body>
</html>

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/minipet/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `img/accessories/bow_tie.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 60">
  <!-- Ala Izquierda -->
  <path d="M 50 30 L 10 10 C 5 25, 5 35, 10 50 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3" stroke-linejoin="round"/>
  <!-- Ala Derecha -->
  <path d="M 50 30 L 90 10 C 95 25, 95 35, 90 50 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3" stroke-linejoin="round"/>
  <!-- Pliegues internos -->
  <path d="M 25 22 Q 35 30 25 38" fill="none" stroke="#f50057" stroke-width="2" stroke-linecap="round"/>
  <path d="M 75 22 Q 65 30 75 38" fill="none" stroke="#f50057" stroke-width="2" stroke-linecap="round"/>
  <!-- Nudo central Corazón -->
  <path d="M 50 22 C 45 16, 38 24, 50 35 C 62 24, 55 16, 50 22 Z" fill="#ff4081" stroke="#c51162" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/bunny_ears.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 80">
  <!-- Diadema -->
  <path d="M 20 65 Q 50 55 80 65" fill="none" stroke="#ff80ab" stroke-width="4" stroke-linecap="round"/>
  <!-- Oreja Izquierda -->
  <path d="M 28 60 C 10 20 25 0 38 10 C 45 25 38 55 35 60 Z" fill="#ffffff" stroke="#f50057" stroke-width="2"/>
  <path d="M 28 50 C 18 25 28 12 34 18 C 38 28 34 48 32 50 Z" fill="#ff80ab"/>
  <!-- Oreja Derecha (Doblada) -->
  <path d="M 72 60 C 90 20 75 0 62 10 C 55 25 62 55 65 60 Z" fill="#ffffff" stroke="#f50057" stroke-width="2"/>
  <path d="M 72 50 C 82 25 72 12 66 18 C 62 28 66 48 68 50 Z" fill="#ff80ab"/>
</svg>

```

### Archivo: `img/accessories/cat_ears.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 60">
  <!-- Oreja Izquierda -->
  <polygon points="20,55 35,10 60,45" fill="#b388ff" stroke="#7c4dff" stroke-width="3" stroke-linejoin="round"/>
  <polygon points="28,50 37,20 52,43" fill="#ea80fc"/>
  <!-- Oreja Derecha -->
  <polygon points="100,55 85,10 60,45" fill="#b388ff" stroke="#7c4dff" stroke-width="3" stroke-linejoin="round"/>
  <polygon points="92,50 83,20 68,43" fill="#ea80fc"/>
</svg>

```

### Archivo: `img/accessories/chef_hat.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 80">
  <!-- Base/Banda -->
  <rect x="25" y="55" width="50" height="18" rx="3" fill="#ffffff" stroke="#e0e0e0" stroke-width="2"/>
  <line x1="25" y1="62" x2="75" y2="62" stroke="#eeeeee" stroke-width="2"/>
  <!-- Copete Esponjoso -->
  <path d="M 25 57 C 10 50 15 25 35 25 C 40 10 60 10 65 25 C 85 25 90 50 75 57 Z" fill="#ffffff" stroke="#e0e0e0" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/fairy_wings.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <path d="M 50 50 C 20 20 10 40 20 60 C 35 65 45 55 50 50 Z" fill="#f48fb1" opacity="0.75"/>
  <path d="M 50 50 C 80 20 90 40 80 60 C 65 65 55 55 50 50 Z" fill="#f48fb1" opacity="0.75"/>
  <path d="M 50 55 C 25 60 20 75 30 85 C 40 85 45 70 50 55 Z" fill="#ce93d8" opacity="0.75"/>
  <path d="M 50 55 C 75 60 80 75 70 85 C 60 85 55 70 50 55 Z" fill="#ce93d8" opacity="0.75"/>
</svg>

```

### Archivo: `img/accessories/flower_pink.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 80 80">
  <circle cx="40" cy="22" r="14" fill="#ff80ab"/>
  <circle cx="58" cy="40" r="14" fill="#ff80ab"/>
  <circle cx="40" cy="58" r="14" fill="#ff80ab"/>
  <circle cx="22" cy="40" r="14" fill="#ff80ab"/>
  <circle cx="40" cy="40" r="12" fill="#ffee58" stroke="#ffa000" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/glasses_cool.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 40">
  <!-- Puente -->
  <line x1="40" y1="20" x2="60" y2="20" stroke="#ff4081" stroke-width="4"/>
  <!-- Ojo Izquierdo Corazón -->
  <path d="M 25 10 C 15 0, 0 15, 25 32 C 50 15, 35 0, 25 10 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3"/>
  <!-- Ojo Derecho Corazón -->
  <path d="M 75 10 C 65 0, 50 15, 75 32 C 100 15, 85 0, 75 10 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3"/>
  <!-- Brillo Lentes -->
  <line x1="15" y1="12" x2="22" y2="22" stroke="white" stroke-width="3" stroke-linecap="round" opacity="0.7"/>
  <line x1="65" y1="12" x2="72" y2="22" stroke="white" stroke-width="3" stroke-linecap="round" opacity="0.7"/>
</svg>

```

### Archivo: `img/accessories/hat_crown.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 70">
  <!-- Sombra base -->
  <ellipse cx="50" cy="65" rx="35" ry="4" fill="rgba(0,0,0,0.1)"/>
  <!-- Base de la corona -->
  <path d="M 20 60 L 15 25 L 35 40 L 50 15 L 65 40 L 85 25 L 80 60 Z" fill="#ffd54f" stroke="#ffb300" stroke-width="3" stroke-linejoin="round"/>
  <!-- Borde inferior -->
  <rect x="20" y="54" width="60" height="8" rx="3" fill="#ffca28" stroke="#ffa000" stroke-width="2"/>
  <!-- Gemas -->
  <circle cx="50" cy="30" r="5" fill="#e91e63"/>
  <circle cx="30" cy="42" r="4" fill="#29b6f6"/>
  <circle cx="70" cy="42" r="4" fill="#29b6f6"/>
  <!-- Brillo -->
  <path d="M 50 8 L 52 14 L 58 16 L 52 18 L 50 24 L 48 18 L 42 16 L 48 14 Z" fill="#ffffff"/>
</svg>

```

### Archivo: `img/accessories/headphones.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 140 70">
  <!-- Diadema superior -->
  <path d="M 15 45 A 55 55 0 0 1 125 45" fill="none" stroke="#ba68c8" stroke-width="8" stroke-linecap="round"/>
  <!-- Orejitas de gato en la diadema -->
  <polygon points="30,18 42,3 48,22" fill="#e1bee7" stroke="#ba68c8" stroke-width="2"/>
  <polygon points="110,18 98,3 92,22" fill="#e1bee7" stroke="#ba68c8" stroke-width="2"/>
  <!-- Auricular Izquierdo -->
  <rect x="5" y="32" width="22" height="32" rx="10" fill="#ab47bc" stroke="#7b1fa2" stroke-width="2"/>
  <rect x="15" y="38" width="8" height="20" rx="4" fill="#ea80fc"/>
  <!-- Auricular Derecho -->
  <rect x="113" y="32" width="22" height="32" rx="10" fill="#ab47bc" stroke="#7b1fa2" stroke-width="2"/>
  <rect x="117" y="38" width="8" height="20" rx="4" fill="#ea80fc"/>
</svg>

```

### Archivo: `img/accessories/magic_wand.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <!-- Vara -->
  <line x1="25" y1="85" x2="60" y2="40" stroke="#b0bec5" stroke-width="6" stroke-linecap="round"/>
  <line x1="25" y1="85" x2="60" y2="40" stroke="#eceff1" stroke-width="3" stroke-linecap="round"/>
  <!-- Estrella -->
  <path d="M 68 12 L 72 26 L 86 28 L 75 38 L 78 52 L 68 44 L 58 52 L 61 38 L 50 28 L 64 26 Z" fill="#ffd54f" stroke="#ffb300" stroke-width="2.5" stroke-linejoin="round"/>
  <!-- Brillos dorados -->
  <circle cx="85" cy="15" r="2" fill="#ffe082"/>
  <circle cx="50" cy="18" r="3" fill="#ffe082"/>
  <circle cx="80" cy="55" r="2.5" fill="#ffe082"/>
</svg>

```

### Archivo: `img/accessories/ninja_band.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <rect x="10" y="40" width="80" height="16" rx="4" fill="#212121"/>
  <rect x="35" y="42" width="30" height="12" rx="2" fill="#b0bec5"/>
  <circle cx="42" cy="48" r="1.5" fill="#37474f"/>
  <circle cx="58" cy="48" r="1.5" fill="#37474f"/>
  <path d="M 85 48 L 95 42 L 92 52 Z" fill="#212121"/>
  <path d="M 85 50 L 98 52 L 90 58 Z" fill="#212121"/>
</svg>

```

### Archivo: `img/accessories/party_hat.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <!-- Cono del Gorro -->
  <path d="M 50 15 L 18 85 A 35 10 0 0 0 82 85 Z" fill="#4dd0e1" stroke="#00acc1" stroke-width="3" stroke-linejoin="round"/>
  <!-- Lunares de colores -->
  <circle cx="45" cy="45" r="5" fill="#ff4081"/>
  <circle cx="62" cy="60" r="6" fill="#ffee58"/>
  <circle cx="35" cy="70" r="5.5" fill="#b388ff"/>
  <circle cx="52" cy="78" r="4.5" fill="#ff9100"/>
  <!-- Pompón superior -->
  <circle cx="50" cy="12" r="9" fill="#ff80ab" stroke="#f50057" stroke-width="2"/>
  <circle cx="47" cy="10" r="3" fill="#ffffff" opacity="0.6"/>
</svg>

```

### Archivo: `img/accessories/pirate_hat.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 70">
  <!-- Ala del Sombrero -->
  <path d="M 10 50 Q 60 70 110 50 Q 95 30 60 30 Q 25 30 10 50 Z" fill="#212121" stroke="#424242" stroke-width="2"/>
  <!-- Copa Pirata -->
  <path d="M 25 45 Q 60 10 95 45 Z" fill="#212121"/>
  <!-- Borde Dorado -->
  <path d="M 10 50 Q 60 70 110 50" fill="none" stroke="#ffd54f" stroke-width="3"/>
  <!-- Calavera Kawaii / Cruz -->
  <circle cx="60" cy="35" r="6" fill="#ffffff"/>
  <circle cx="58" cy="34" r="1.5" fill="#000"/>
  <circle cx="62" cy="34" r="1.5" fill="#000"/>
  <path d="M 54 43 L 66 43 M 60 39 L 60 47" stroke="#ffffff" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/ribbon_pink.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 70">
  <path d="M 50 35 L 15 15 C 5 30, 5 40, 15 55 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3" stroke-linejoin="round"/>
  <path d="M 50 35 L 85 15 C 95 30, 95 40, 85 55 Z" fill="#ff80ab" stroke="#f50057" stroke-width="3" stroke-linejoin="round"/>
  <circle cx="50" cy="35" r="10" fill="#ff4081" stroke="#c51162" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/shower_cap.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <path d="M 20 50 A 30 25 0 0 1 80 50 A 30 15 0 0 1 20 50 Z" fill="#81d4fa"/>
  <path d="M 18 50 Q 50 58 82 50" fill="none" stroke="#ffffff" stroke-width="3" stroke-dasharray="4,3"/>
</svg>

```

### Archivo: `img/accessories/snorkel.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <rect x="20" y="38" width="28" height="20" rx="8" fill="#4dd0e1" stroke="#00838f" stroke-width="3" opacity="0.85"/>
  <rect x="52" y="38" width="28" height="20" rx="8" fill="#4dd0e1" stroke="#00838f" stroke-width="3" opacity="0.85"/>
  <rect x="44" y="45" width="12" height="6" fill="#00838f"/>
  <path d="M 76 48 L 85 48 L 85 20 A 4 4 0 0 1 89 16 L 89 16" fill="none" stroke="#ffeb3b" stroke-width="5" stroke-linecap="round"/>
</svg>

```

### Archivo: `img/accessories/space_helmet.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <path d="M 20 50 A 30 30 0 0 1 80 50 L 80 65 A 10 10 0 0 1 70 75 L 30 75 A 10 10 0 0 1 20 65 Z" fill="#e0e0e0" stroke="#9e9e9e" stroke-width="3"/>
  <ellipse cx="50" cy="50" rx="22" ry="16" fill="#29b6f6" opacity="0.8"/>
  <ellipse cx="44" cy="46" rx="8" ry="4" fill="#ffffff" opacity="0.6"/>
</svg>

```

### Archivo: `img/accessories/star_glasses.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 40">
  <line x1="38" y1="20" x2="62" y2="20" stroke="#ffd54f" stroke-width="4"/>
  <!-- Estrella Izquierda -->
  <path d="M 25 5 L 29 15 L 40 15 L 31 22 L 34 33 L 25 26 L 16 33 L 19 22 L 10 15 L 21 15 Z" fill="#ffee58" stroke="#ffa000" stroke-width="2"/>
  <!-- Estrella Derecha -->
  <path d="M 75 5 L 79 15 L 90 15 L 81 22 L 84 33 L 75 26 L 66 33 L 69 22 L 60 15 L 71 15 Z" fill="#ffee58" stroke="#ffa000" stroke-width="2"/>
</svg>

```

### Archivo: `img/accessories/winter_beanie.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <path d="M 25 55 A 25 25 0 0 1 75 55 Z" fill="#e53935"/>
  <rect x="20" y="52" width="60" height="12" rx="4" fill="#ffffff"/>
  <circle cx="50" cy="26" r="8" fill="#ffffff"/>
</svg>

```

### Archivo: `img/accessories/winter_scarf.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
  <path d="M 25 50 Q 50 65 75 50 Q 50 78 25 50 Z" fill="#43a047"/>
  <rect x="60" y="58" width="14" height="30" rx="3" fill="#388e3c" transform="rotate(-10 60 58)"/>
</svg>

```

### Archivo: `img/accessories/wizard_hat.svg`
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 90">
  <!-- Ala base -->
  <ellipse cx="50" cy="75" rx="42" ry="10" fill="#4a148c" stroke="#6a1b9a" stroke-width="2"/>
  <!-- Cono del sombrero -->
  <path d="M 18 73 Q 50 5 82 73 Z" fill="#6a1b9a"/>
  <!-- Cinta dorada -->
  <path d="M 24 65 Q 50 72 76 65" fill="none" stroke="#ffd54f" stroke-width="4"/>
  <!-- Estrellas mágicas -->
  <path d="M 50 30 L 52 36 L 58 36 L 53 40 L 55 46 L 50 42 L 45 46 L 47 40 L 42 36 L 48 36 Z" fill="#ffee58"/>
  <path d="M 62 48 L 63 52 L 67 52 L 64 55 L 65 59 L 62 56 L 59 59 L 60 55 L 57 52 L 61 52 Z" fill="#ffee58" transform="scale(0.7) translate(25, 10)"/>
</svg>

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <!-- Deshabilita el zoom táctil molesto en móviles -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Mi Mascota</title>

  <!-- PWA Meta Tags -->
  <link rel="manifest" href="manifest.json">
  <meta name="theme-color" content="#b388ff">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🐾</text></svg>">

  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/escenarios.css">
  <link rel="stylesheet" href="css/mascota.css">
  <link rel="stylesheet" href="css/juegos.css">
</head>
<body>
  <div id="app-container">
    <!-- Barra Superior -->
    <header class="top-bar">
      <div class="stat-box">🪙 <span id="coin-count">100</span></div>
      <div class="stat-box">❤️ <span id="happy-count">100</span>%</div>
      <div class="stat-box">🍕 <span id="hunger-count">100</span>%</div>
      <div class="stat-box">⚡ <span id="energy-count">100</span>%</div>
    </header>

    <!-- Globo de Pensamiento -->
    <div id="pet-thought" class="thought-bubble">¡Hola! ¿Jugamos?</div>

    <!-- Escenario Principal -->
    <main id="pet-room" class="room">
      <div id="bg-particles"></div>
      <div id="pet" class="pet-sprite" onclick="PetState.petPet()">
        <div id="pet-glitter" class="pet-glitter" aria-hidden="true"></div>
        <!-- Accesorio Equipado -->
        <div id="pet-accessory" class="accessory"></div>

        <!-- Cara Kawaii Modular -->
        <div class="kawaii-face" id="kawaii-face">
          <div class="eyebrows">
            <div class="eyebrow left"></div>
            <div class="eyebrow right"></div>
          </div>
          <div class="eyes">
            <div class="eye left"><div class="sparkle">★</div></div>
            <div class="eye right"><div class="sparkle">★</div></div>
          </div>
          <div class="cheeks">
            <div class="cheek left"></div>
            <div class="cheek right"></div>
          </div>
          <div class="mouth">:3</div>
          <div id="extra-prop" class="extra-prop"></div>
        </div>
      </div>
    </main>

    <!-- Menú Inferior en Rejilla 4x2 -->
    <nav class="bottom-nav">
      <button onclick="PetState.feed()">
        <span class="btn-icon">🍗</span>
        <span class="btn-text">Comer</span>
      </button>
      <button onclick="PetState.heal()">
        <span class="btn-icon">🏥</span>
        <span class="btn-text">Medicina</span>
      </button>
      <button onclick="PetState.sleep()">
        <span class="btn-icon">😴</span>
        <span class="btn-text">Dormir</span>
      </button>
      <button onclick="switchTab('minigames')">
        <span class="btn-icon">🎮</span>
        <span class="btn-text">Juegos</span>
      </button>
      <button onclick="switchTab('my-pet')">
        <span class="btn-icon">🐾</span>
        <span class="btn-text">Mi Pet</span>
      </button>
      <button onclick="switchTab('accessories')">
        <span class="btn-icon">🎀</span>
        <span class="btn-text">Accesorios</span>
      </button>
      <button onclick="switchTab('scenery')">
        <span class="btn-icon">🏡</span>
        <span class="btn-text">Escenario</span>
      </button>
      <button onclick="switchTab('store')">
        <span class="btn-icon">🛍️</span>
        <span class="btn-text">Tienda</span>
      </button>
    </nav>

    <!-- Modales -->
    <section id="modal-screen" class="modal hidden">
      <button class="close-btn" onclick="closeModal()">❌ Cerrar</button>
      <div id="modal-content"></div>
    </section>
  </div>

  <script src="js/mascota-estado.js"></script>
  <script src="js/mascota-visual.js"></script>
  <script src="js/particulas.js"></script>
  <script src="js/catalogo.js"></script>
  <script src="js/tienda.js"></script>
  <script src="js/juegos/juegos-base.js"></script>
  <script src="js/juegos/atrapa-dulces.js"></script>
  <script src="js/juegos/explota-globos.js"></script>
  <script src="js/juegos/torre-postres.js"></script>
  <script src="js/juegos/simon-memoria.js"></script>
  <script src="js/juegos/runner-magico.js"></script>
  <script src="js/juegos/burbujas-magicas.js"></script>
  <script src="js/app.js"></script>
</body>
</html>
```

### Archivo: `js/app.js`
```javascript
// Motor Sintetizador de Sonidos
const AudioEffects = {
  ctx: null,

  init() {
    if (!this.ctx) {
      this.ctx = new (window.AudioContext || window.webkitAudioContext)();
    }
  },

  playTone(freq, type, duration) {
    try {
      this.init();
      const osc = this.ctx.createOscillator();
      const gain = this.ctx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
      gain.gain.setValueAtTime(0.1, this.ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + duration);
      osc.connect(gain);
      gain.connect(this.ctx.destination);
      osc.start();
      osc.stop(this.ctx.currentTime + duration);
    } catch (e) {
      // Ignorar
    }
  },

  playEat() {
    this.playTone(300, 'sine', 0.1);
    setTimeout(() => this.playTone(450, 'sine', 0.15), 100);
  },

  playCoin() {
    this.playTone(987.77, 'sine', 0.08);
    setTimeout(() => this.playTone(1318.51, 'sine', 0.2), 80);
  },

  playHeal() {
    this.playTone(523.25, 'triangle', 0.15);
    setTimeout(() => this.playTone(659.25, 'triangle', 0.15), 120);
    setTimeout(() => this.playTone(783.99, 'triangle', 0.2), 240);
  }
};

// Inicialización del Juego y Registro del Service Worker PWA
document.addEventListener('DOMContentLoaded', () => {
  PetState.init();
  registerServiceWorker();
});

function registerServiceWorker() {
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('./sw.js').then((reg) => {
      // Escucha si hay una versión nueva esperando activarse
      reg.addEventListener('updatefound', () => {
        const newWorker = reg.installing;
        newWorker.addEventListener('statechange', () => {
          if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
            // Notificar al usuario que hay una actualización lista
            if (confirm('✨ ¡Hay una nueva versión del juego disponible! ¿Quieres actualizar ahora?')) {
              window.location.reload();
            }
          }
        });
      });
    }).catch((err) => console.log('Error al registrar PWA:', err));
  }
}

function switchTab(tab) {
  Minigames.forceExitIfActive();
  const modal = document.getElementById('modal-screen');
  modal.classList.remove('hidden');
  
  if (tab === 'store') {
    Store.renderStore();
  } else if (tab === 'accessories') {
    Store.renderAccessories();
  } else if (tab === 'scenery') {
    Store.renderScenery();
  } else if (tab === 'my-pet') {
    Store.renderMyPet();
  } else if (tab === 'minigames') {
    Minigames.renderMenu();
  }
}

function closeModal() {
  Minigames.forceExitIfActive();
  document.getElementById('modal-screen').classList.add('hidden');
}
```

### Archivo: `js/catalogo.js`
```javascript
// =========================================================
// CATALOGO.JS — Datos puros: accesorios, escenarios/fondos y
// formas de mascota disponibles, más los ajustes de tamaño y
// posición de cada accesorio sobre la mascota (accessoryFits).
// Sin lógica de UI ni acceso al DOM — solo datos.
// =========================================================
const Catalogo = {
  // Las formas de mascota disponibles (elegibles en Mi Pet)
  shapes: [
    { id: 'shape-circle',   name: 'Círculo',   emoji: '⚪' },
    { id: 'shape-mochi',    name: 'Mochi',      emoji: '🍡' },
    { id: 'shape-onigiri',  name: 'Gotita',     emoji: '💧' },
    { id: 'shape-squircle', name: 'Squircle',   emoji: '🔲' },
    { id: 'shape-dumpling', name: 'Dumpling',   emoji: '🥟' },
    //{ id: 'shape-capsule',  name: 'Cápsula',    emoji: '💊' },
    //{ id: 'shape-heart',    name: 'Corazón',    emoji: '❤️' },
    //{ id: 'shape-cathead',  name: 'Animalito',  emoji: '🐱' },
    //{ id: 'shape-cloud',    name: 'Nube',       emoji: '☁️' }
  ],

  items: [
    // -----------------------------------------------------------------
    // ACCESORIOS (Organizados para combinar con los escenarios)
    // -----------------------------------------------------------------
    // Básicos y Fiesta (Living, Dormitorio, Playroom)
    { id: 'hat_crown', name: 'Corona Real', price: 50, image: 'img/accessories/hat_crown.svg', type: 'head', category: 'acc' },
    { id: 'party_hat', name: 'Gorro Fiesta', price: 25, image: 'img/accessories/party_hat.svg', type: 'head', category: 'acc' },
    { id: 'ribbon_pink', name: 'Lazo Rosado', price: 15, image: 'img/accessories/ribbon_pink.svg', type: 'head', category: 'acc' },
    { id: 'bow_tie', name: 'Moño Coquette', price: 20, image: 'img/accessories/bow_tie.svg', type: 'neck', category: 'acc' },
    { id: 'headphones', name: 'Auriculares Gato', price: 40, image: 'img/accessories/headphones.svg', type: 'ears', category: 'acc' },
    { id: 'cat_ears', name: 'Orejitas Violeta', price: 45, image: 'img/accessories/cat_ears.svg', type: 'ears', category: 'acc' },
    { id: 'bunny_ears', name: 'Orejas Conejo', price: 40, image: 'img/accessories/bunny_ears.svg', type: 'ears', category: 'acc' },

    // Para "Playa Tropical" / "Oasis del Desierto"
    { id: 'glasses_cool', name: 'Lentes Corazón', price: 30, image: 'img/accessories/glasses_cool.svg', type: 'eyes', category: 'acc' },
    { id: 'star_glasses', name: 'Lentes Estrella', price: 35, image: 'img/accessories/star_glasses.svg', type: 'eyes', category: 'acc' },
    { id: 'pirate_hat', name: 'Gorro Pirata', price: 55, image: 'img/accessories/pirate_hat.svg', type: 'head', category: 'acc' },
    { id: 'snorkel_mask', name: 'Visor de Buceo', price: 45, image: 'img/accessories/snorkel.svg', type: 'eyes', category: 'acc' },

    // Para "Cocina del Chef"
    { id: 'chef_hat', name: 'Gorro de Chef', price: 35, image: 'img/accessories/chef_hat.svg', type: 'head', category: 'acc' },

    // Para "Parque Mágico" / "Bosque Encantado" / "Castillo Real"
    { id: 'flower_pink', name: 'Flor Primavera', price: 18, image: 'img/accessories/flower_pink.svg', type: 'head', category: 'acc' },
    { id: 'wizard_hat', name: 'Gorro de Mago', price: 60, image: 'img/accessories/wizard_hat.svg', type: 'head', category: 'acc' },
    { id: 'magic_wand', name: 'Varita Mágica', price: 60, image: 'img/accessories/magic_wand.svg', type: 'neck', category: 'acc' },
    { id: 'fairy_wings', name: 'Alas de Hada', price: 75, image: 'img/accessories/fairy_wings.svg', type: 'neck', category: 'acc' },

    // Para "Dojo Ninja"
    { id: 'ninja_band', name: 'Vanda Ninja', price: 50, image: 'img/accessories/ninja_band.svg', type: 'head', category: 'acc' },

    // Para "Estación Espacial"
    { id: 'space_helmet', name: 'Casco Espacial', price: 90, image: 'img/accessories/space_helmet.svg', type: 'head', category: 'acc' },

    // Para "Montaña Nevada" / "Campamento Nocturno"
    { id: 'winter_beanie', name: 'Gorro de Lana', price: 30, image: 'img/accessories/winter_beanie.svg', type: 'head', category: 'acc' },
    { id: 'winter_scarf', name: 'Bufanda Calientita', price: 25, image: 'img/accessories/winter_scarf.svg', type: 'neck', category: 'acc' },

    // Para "Baño de Burbujas"
    { id: 'shower_cap', name: 'Gorro de Baño', price: 20, image: 'img/accessories/shower_cap.svg', type: 'head', category: 'acc' },

    // -----------------------------------------------------------------
    // ESCENARIOS / FONDOS (Categoría 'bg')
    // El campo `preview` reproduce el gradiente real de css/escenarios.css
    // para mostrar una miniatura fiel en vez de un emoji genérico.
    // -----------------------------------------------------------------
    // Básicos
    { id: 'bg_living', name: 'Sala de Estar', price: 0, image: '🛋️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #d7ccc8 72%, #a1887f 100%), linear-gradient(180deg, #ffe0b2 0%, #fff3e0 72%);' },
    { id: 'bg_bedroom', name: 'Dormitorio', price: 40, image: '🛏️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #b388ff 72%, #7c4dff 100%), radial-gradient(circle at 80% 20%, #fff 2px, transparent 3px), radial-gradient(circle at 20% 35%, #fff 2px, transparent 3px), linear-gradient(180deg, #311b92 0%, #512da8 72%);' },
    { id: 'bg_playroom', name: 'Sala de Juegos', price: 60, image: '🧸', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #80deea 70%, #4dd0e1 100%), radial-gradient(#ff80ab 15%, transparent 16%), radial-gradient(#ffd54f 15%, transparent 16%), #f3e5f5; background-size: 100% 100%, 20px 20px, 20px 20px, 100% 100%; background-position: 0 0, 0 0, 10px 10px, 0 0;' },
    { id: 'bg_park', name: 'Parque Mágico', price: 80, image: '🌳', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #aed581 68%, #7cb342 100%), linear-gradient(180deg, #81d4fa 0%, #e1f5fe 68%);' },
    { id: 'bg_beach', name: 'Playa Tropical', price: 50, image: '🏖️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #ffe082 68%, #ffd54f 100%), linear-gradient(180deg, #80deea 0%, #26c6da 50%, #00acc1 68%);' },

    // Naturaleza y Exteriores
    { id: 'bg_forest', name: 'Bosque Encantado', price: 90, image: '🌲', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 65%, #33691e 65%, #1b5e20 100%), linear-gradient(180deg, #aed581 0%, #558b2f 65%);' },
    { id: 'bg_mountain', name: 'Montaña Nevada', price: 100, image: '🏔️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #cfd8dc 70%, #90a4ae 100%), linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);' },
    { id: 'bg_camping', name: 'Campamento Nocturno', price: 120, image: '⛺', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 72%, #2e7d32 72%, #1b5e20 100%), radial-gradient(circle at 75% 20%, #fff9c4 3px, transparent 4px), radial-gradient(circle at 25% 15%, #fff9c4 2px, transparent 3px), linear-gradient(180deg, #0d1b2a 0%, #1b263b 72%);' },
    { id: 'bg_desert', name: 'Oasis del Desierto', price: 110, image: '🏜️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 68%, #f57c00 68%, #e65100 100%), linear-gradient(180deg, #ffe0b2 0%, #ffcc80 68%);' },

    // Interiores y Temáticos
    { id: 'bg_kitchen', name: 'Cocina del Chef', price: 45, image: '🍳', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #d7ccc8 70%, #8d6e63 100%), linear-gradient(180deg, #fff3e0 0%, #ffe0b2 70%);' },
    { id: 'bg_bathroom', name: 'Baño de Burbujas', price: 35, image: '🛁', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #b2ebf2 70%, #4dd0e1 100%), linear-gradient(180deg, #e0f7fa 0%, #80deea 70%);' },
    { id: 'bg_dojo', name: 'Dojo Ninja', price: 130, image: '⛩️', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #8d6e63 70%, #4e342e 100%), linear-gradient(180deg, #efebe9 0%, #d7ccc8 70%);' },
    { id: 'bg_disco', name: 'Pista de Baile', price: 150, image: '🪩', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #311b92 70%, #1a237e 100%), linear-gradient(135deg, #4a148c 0%, #880e4f 50%, #311b92 100%);' },

    // Fantásticos y Especiales
    { id: 'bg_space', name: 'Estación Espacial', price: 200, image: '🚀', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 75%, #263238 75%, #102027 100%), radial-gradient(circle at 50% 30%, #37474f 0%, #000000 100%);' },
    { id: 'bg_castle', name: 'Castillo Real', price: 250, image: '🏰', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #9c27b0 70%, #4a148c 100%), linear-gradient(180deg, #f3e5f5 0%, #ce93d8 70%);' },
    { id: 'bg_underwater', name: 'Mundo Submarino', price: 180, image: '🪸', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #004d40 70%, #00251a 100%), linear-gradient(180deg, #00838f 0%, #006064 70%);' },
    { id: 'bg_volcano', name: 'Guarida Volcánica', price: 300, image: '🌋', type: 'bg', category: 'bg',
      preview: 'background: linear-gradient(to bottom, transparent 70%, #3e2723 70%, #1b0000 100%), linear-gradient(180deg, #bf360c 0%, #dd2c00 70%);' }
  ],

  // Escala/posición/rotación de cada accesorio equipado sobre la mascota
  accessoryFits: {
    hat_crown: { scale: 1.25, y: -4 },
    party_hat: { scale: 1.2, y: -4 },
    ribbon_pink: { scale: 1.15, y: -2, rotate: -4 },
    chef_hat: { scale: 1.25, y: -4 },
    wizard_hat: { scale: 1.25, y: -5 },
    pirate_hat: { scale: 1.2, y: -3 },
    space_helmet: { scale: 1.2, y: -2 },
    winter_beanie: { scale: 1.2, y: -3 },
    shower_cap: { scale: 1.2, y: -2 },
    flower_pink: { scale: 1.1, y: 0 },
    headphones: { scale: 1.15, y: 0 },
    cat_ears: { scale: 1.1, y: -2 },
    bunny_ears: { scale: 1.15, y: -3 },
    glasses_cool: { scale: 1.15, y: 1, x: 0 },
    star_glasses: { scale: 1.15, y: 1, x: 0 },
    snorkel_mask: { scale: 1.2, y: 1 },
    bow_tie: { scale: 1.25, y: -2 },
    magic_wand: { scale: 1.1, y: -4, x: 3, rotate: -8 },
    fairy_wings: { scale: 1.2, y: -3, x: 0 },
    ninja_band: { scale: 1.15, y: 0 },
    winter_scarf: { scale: 1.25, y: -3 }
  }
};

```

### Archivo: `js/juegos/atrapa-dulces.js`
```javascript
// =========================================================
// ATRAPA-DULCES.JS — Agarrar con una cesta los dulces que caen
// antes de que toquen el piso. Soporte de teclado y táctil.
// =========================================================
Object.assign(Minigames, {
  startCatchGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('catch');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🎈 Atrapa Dulces</h3>
        ${Minigames.renderHUD('🍬', best)}
        <canvas id="gameCanvas" width="300" height="300" style="background:linear-gradient(180deg, #e1f5fe 0%, #f3e5f5 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ce93d8; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Arrastra el dedo o usa ⬅️ ➡️ para moverte</p>
      `;

      const canvas = document.getElementById('gameCanvas');
      const ctx = canvas.getContext('2d');
      const CANDY_EMOJIS = ['🍬', '🍭', '🍡', '🍫'];
      let basketX = 120;
      let itemX = Math.random() * 260 + 10;
      let itemY = 0;
      let currentCandy = CANDY_EMOJIS[Math.floor(Math.random() * CANDY_EMOJIS.length)];
      let score = 0;
      let gameOver = false;

      // Teclado
      const keys = {};
      const handleKeyDown = (e) => keys[e.key] = true;
      const handleKeyUp = (e) => keys[e.key] = false;
      window.addEventListener('keydown', handleKeyDown);
      window.addEventListener('keyup', handleKeyUp);

      // Controles Táctiles (Móvil / Tablet)
      const handleTouch = (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const touchX = (e.touches[0].clientX - rect.left) * scale.x;
        basketX = Math.max(0, Math.min(240, touchX - 30));
      };
      canvas.addEventListener('touchstart', handleTouch, { passive: false });
      canvas.addEventListener('touchmove', handleTouch, { passive: false });

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        window.removeEventListener('keydown', handleKeyDown);
        window.removeEventListener('keyup', handleKeyUp);
        canvas.removeEventListener('touchstart', handleTouch);
        canvas.removeEventListener('touchmove', handleTouch);
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleKeyDown);
          window.removeEventListener('keyup', handleKeyUp);
          canvas.removeEventListener('touchstart', handleTouch);
          canvas.removeEventListener('touchmove', handleTouch);
          const isRecord = Minigames.saveBestScore('catch', score);
          Minigames.showResult('¡Juego terminado!', '🎈', score, () => Minigames.startCatchGame(), isRecord);
          return;
        }

        if (keys['ArrowLeft'] && basketX > 5) basketX -= 6;
        if (keys['ArrowRight'] && basketX < 235) basketX += 6;

        itemY += 3.8;

        if (itemY >= 260 && itemX >= basketX - 10 && itemX <= basketX + 60) {
          score += 5;
          Minigames.updateHudScore(score);
          AudioEffects.playTone(800, 'sine', 0.05);
          itemY = 0;
          itemX = Math.random() * 250 + 10;
          currentCandy = CANDY_EMOJIS[Math.floor(Math.random() * CANDY_EMOJIS.length)];
        }

        if (itemY > 300) gameOver = true;

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        // Cesta
        ctx.font = '38px sans-serif';
        ctx.fillText('🧺', basketX + 30, 285);

        // Dulce cayendo
        ctx.font = '26px sans-serif';
        ctx.fillText(currentCandy, itemX, itemY);

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  }
});

```

### Archivo: `js/juegos/burbujas-magicas.js`
```javascript
// =========================================================
// BURBUJAS-MAGICAS.JS — Tocar burbujas antes de que se vayan;
// las doradas valen más puntos. Juego a contrarreloj (15s).
// =========================================================
Object.assign(Minigames, {
  startBubbleGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('bubble');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">✨ Burbujas Mágicas</h3>
        ${Minigames.renderHUD('✨', best, 15)}
        <canvas id="bubbleCanvas" width="300" height="300" style="background:linear-gradient(180deg, #e0f2f1 0%, #e8eaf6 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #80cbc4; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca las burbujas para atraparlas</p>
      `;

      const canvas = document.getElementById('bubbleCanvas');
      const ctx = canvas.getContext('2d');
      let bubbles = [];
      let score = 0;
      let timeLeft = 15;
      let stopped = false;

      const timer = setInterval(() => {
        timeLeft--;
        Minigames.updateHudTimer(timeLeft);
        if (timeLeft <= 0) {
          clearInterval(timer);
          const isRecord = Minigames.saveBestScore('bubble', score);
          Minigames.showResult('¡Tiempo!', '✨', score, () => Minigames.startBubbleGame(), isRecord);
        }
      }, 1000);

      const handleTouchBubble = (e) => {
        if (e) e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const clientX = e.touches ? e.touches[0].clientX : e.clientX;
        const clientY = e.touches ? e.touches[0].clientY : e.clientY;
        const mx = (clientX - rect.left) * scale.x;
        const my = (clientY - rect.top) * scale.y;

        bubbles.forEach((b, i) => {
          if (Math.hypot(b.x - mx, b.y - my) < b.r + 8) { // Margen táctil
            score += b.isGolden ? 15 : 5;
            Minigames.updateHudScore(score);
            AudioEffects.playTone(b.isGolden ? 1000 : 750, 'sine', 0.1);
            bubbles.splice(i, 1);
          }
        });
      };

      canvas.addEventListener('touchstart', handleTouchBubble, { passive: false });
      canvas.onclick = handleTouchBubble;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        stopped = true;
        clearInterval(timer);
        canvas.removeEventListener('touchstart', handleTouchBubble);
        canvas.onclick = null;
        return score;
      };

      function loop() {
        if (stopped || timeLeft <= 0) return;

        if (Math.random() < 0.08 && bubbles.length < 7) {
          const isGolden = Math.random() < 0.3;
          bubbles.push({
            x: Math.random() * 250 + 25,
            y: Math.random() * 220 + 40,
            r: Math.random() * 8 + 16,
            isGolden: isGolden
          });
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        bubbles.forEach(b => {
          ctx.font = `${b.r * 2}px sans-serif`;
          ctx.fillText(b.isGolden ? '🌟' : '🫧', b.x, b.y);
        });

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  }
});

```

### Archivo: `js/juegos/explota-globos.js`
```javascript
// =========================================================
// EXPLOTA-GLOBOS.JS — Tocar los globos antes de que suban y
// se escapen de la pantalla. Juego a contrarreloj (20s).
// =========================================================
Object.assign(Minigames, {
  startPopGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('pop');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🌸 Explotar Globos</h3>
        ${Minigames.renderHUD('🎈', best, 20)}
        <canvas id="popCanvas" width="300" height="300" style="background:linear-gradient(180deg, #fff3e0 0%, #fce4ec 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ff80ab; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">¡Toca los globos antes de que suban!</p>
      `;

      const canvas = document.getElementById('popCanvas');
      const ctx = canvas.getContext('2d');
      let balloons = [];
      let score = 0;
      let timeLeft = 20;
      let stopped = false;

      const timerInterval = setInterval(() => {
        timeLeft--;
        Minigames.updateHudTimer(timeLeft);
        if (timeLeft <= 0) {
          clearInterval(timerInterval);
          const isRecord = Minigames.saveBestScore('pop', score);
          Minigames.showResult('¡Tiempo!', '🌸', score, () => Minigames.startPopGame(), isRecord);
        }
      }, 1000);

      const handlePop = (e) => {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scale = Minigames.getCanvasScale(canvas);
        const clientX = e.touches ? e.touches[0].clientX : e.clientX;
        const clientY = e.touches ? e.touches[0].clientY : e.clientY;
        const mouseX = (clientX - rect.left) * scale.x;
        const mouseY = (clientY - rect.top) * scale.y;

        balloons.forEach((b, index) => {
          const dist = Math.hypot(b.x - mouseX, b.y - mouseY);
          if (dist < b.radius + 10) { // Margen extra para dedos en pantalla táctil
            score += 5;
            Minigames.updateHudScore(score);
            AudioEffects.playTone(900, 'triangle', 0.08);
            balloons.splice(index, 1);
          }
        });
      };

      canvas.addEventListener('touchstart', handlePop, { passive: false });
      canvas.onclick = handlePop;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        stopped = true;
        clearInterval(timerInterval);
        canvas.removeEventListener('touchstart', handlePop);
        canvas.onclick = null;
        return score;
      };

      function loop() {
        if (stopped || timeLeft <= 0) return;

        if (Math.random() < 0.05 && balloons.length < 6) {
          balloons.push({
            x: Math.random() * 260 + 20,
            y: 320,
            radius: 18,
            speed: Math.random() * 1.5 + 1.2
          });
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        balloons.forEach((b, index) => {
          b.y -= b.speed;

          ctx.font = `${b.radius * 2}px sans-serif`;
          ctx.fillText('🎈', b.x, b.y);

          if (b.y < -20) balloons.splice(index, 1);
        });

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  }
});

```

### Archivo: `js/juegos/juegos-base.js`
```javascript
// =========================================================
// JUEGOS-BASE.JS — Estado y utilidades compartidas por los 6
// minijuegos: salida segura (forceExitIfActive), menú principal,
// conteo regresivo, pantalla de resultado, mejor puntaje por
// juego (localStorage), HUD reutilizable y escala de canvas.
// Los demás archivos de js/juegos/ agregan sus métodos a este
// mismo objeto Minigames vía Object.assign.
// =========================================================
const Minigames = {
  // Estado de un minijuego en curso. Si el usuario cierra el modal o
  // navega a otra pestaña mientras `inProgress` es true, `exitHandler()`
  // se ejecuta para limpiar listeners/timers y devolver las monedas
  // ganadas hasta ese momento, en vez de perderlas en silencio.
  inProgress: false,
  exitHandler: null,

  forceExitIfActive() {
    if (this.inProgress && this.exitHandler) {
      const earned = this.exitHandler();
      this.inProgress = false;
      this.exitHandler = null;
      if (earned > 0) {
        PetState.addCoins(earned);
      }
    }
  },

  // Menú Principal de Minijuegos (6 Opciones Kawaii)
 renderMenu() {
  const content = document.getElementById('modal-content');
  content.innerHTML = `
    <h2 style="text-align:center; color:#6a1b9a; margin-bottom:4px;">🎮 Minijuegos</h2>
    <p style="margin-bottom:14px; font-size:0.85rem; color:#7b1fa2; text-align:center; font-weight:600;">
      ¡Gana monedas y haz feliz a tu mascota!
    </p>
    <div class="grid-container">
      <div class="item-card" onclick="Minigames.startCatchGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🎈</span>
        <span class="card-title">Atrapa Dulces</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startPopGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🌸</span>
        <span class="card-title">Explotar Globos</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startCakeGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🍰</span>
        <span class="card-title">Torre de Postres</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startSimonGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🧠</span>
        <span class="card-title">Simón Memoria</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startRunnerGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">🦄</span>
        <span class="card-title">Runner Mágico</span>
        <button class="card-btn">Jugar</button>
      </div>
      <div class="item-card" onclick="Minigames.startBubbleGame()" style="cursor:pointer;">
        <span style="font-size:2rem; margin-bottom:4px;">✨</span>
        <span class="card-title">Burbujas Mágicas</span>
        <button class="card-btn">Jugar</button>
      </div>
    </div>
  `;
},

  // Conteo Regresivo Estilo Kawaii
  showCountdown(onComplete) {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <div style="display:flex; flex-direction:column; align-items:center; justify-content:center; height:260px;">
        <div id="countdown-num" class="kawaii-countdown">3</div>
        <p id="countdown-sub" style="font-weight:bold; color:#ab47bc; margin-top:10px;">¡Prepárate! ✨</p>
      </div>
    `;

    let count = 3;
    AudioEffects.playTone(523, 'sine', 0.15);

    const interval = setInterval(() => {
      count--;
      const numEl = document.getElementById('countdown-num');
      const subEl = document.getElementById('countdown-sub');

      if (!numEl) {
        clearInterval(interval);
        return;
      }

      if (count > 0) {
        numEl.innerText = count;
        numEl.style.animation = 'none';
        numEl.offsetHeight;
        numEl.style.animation = 'popNum 0.6s ease-out';
        AudioEffects.playTone(523 + (3 - count) * 100, 'sine', 0.15);
      } else if (count === 0) {
        numEl.innerText = '¡A JUGAR! 🌸';
        numEl.style.fontSize = '2rem';
        subEl.innerText = '✨ ¡Diviértete! ✨';
        AudioEffects.playTone(880, 'triangle', 0.3);
      } else {
        clearInterval(interval);
        onComplete();
      }
    }, 900);
  },

  // Pantalla de Resultado Kawaii (reemplaza los alert() nativos)
  showResult(title, emoji, score, restartFn, isRecord = false) {
    // El juego terminó de forma natural: ya no hay nada que cobrar al salir
    this.inProgress = false;
    this.exitHandler = null;

    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <div class="result-screen">
        <div class="result-emoji">${emoji}</div>
        <h2 class="result-title">${title}</h2>
        ${isRecord ? '<div class="result-record">🏆 ¡Nuevo récord!</div>' : ''}
        <div class="result-coins-box">
          <span>🪙</span>
          <span id="result-coin-num">0</span>
        </div>
        <div class="result-actions">
          <button class="card-btn result-btn-again" id="result-again">🔁 Jugar de nuevo</button>
          <button class="card-btn result-btn-menu" id="result-menu">🎮 Menú</button>
        </div>
      </div>
    `;

    PetState.addCoins(score);

    // Animación de conteo de monedas ganadas
    const numEl = document.getElementById('result-coin-num');
    let current = 0;
    const step = Math.max(1, Math.ceil(score / 25));
    const counter = setInterval(() => {
      current = Math.min(score, current + step);
      numEl.innerText = current;
      if (current >= score) clearInterval(counter);
    }, 25);

    document.getElementById('result-again').onclick = () => restartFn();
    document.getElementById('result-menu').onclick = () => Minigames.renderMenu();
  },

  // =========================================================
  // MEJOR PUNTAJE POR JUEGO (persistido en localStorage)
  // =========================================================
  BEST_SCORES_KEY: 'minipetBestScores',

  getBestScore(gameId) {
    try {
      const data = JSON.parse(localStorage.getItem(this.BEST_SCORES_KEY) || '{}');
      return data[gameId] || 0;
    } catch (e) {
      return 0;
    }
  },

  // Guarda el puntaje si es un nuevo récord. Devuelve true si lo fue.
  saveBestScore(gameId, score) {
    try {
      const data = JSON.parse(localStorage.getItem(this.BEST_SCORES_KEY) || '{}');
      if (score > (data[gameId] || 0)) {
        data[gameId] = score;
        localStorage.setItem(this.BEST_SCORES_KEY, JSON.stringify(data));
        return true;
      }
    } catch (e) {
      // Si localStorage falla, seguimos sin romper el juego
    }
    return false;
  },

  // =========================================================
  // HUD REUTILIZABLE (marcador + mejor puntaje + tiempo opcional)
  // =========================================================
  renderHUD(icon, best, timeLeft) {
    return `
      <div class="game-hud">
        <span class="hud-chip">${icon} <span id="hud-score">0</span></span>
        <span class="hud-chip hud-best">⭐ <span id="hud-best">${best}</span></span>
        ${timeLeft !== undefined ? `<span class="hud-chip hud-timer">⏰ <span id="hud-timer">${timeLeft}</span>s</span>` : ''}
      </div>
    `;
  },

  updateHudScore(score) {
    const el = document.getElementById('hud-score');
    if (el) el.innerText = score;
  },

  updateHudTimer(t) {
    const el = document.getElementById('hud-timer');
    if (el) el.innerText = t;
  },

  // Factor de escala entre la resolución lógica del canvas (width/height)
  // y su tamaño real en pantalla (que ahora es responsive vía CSS).
  // Necesario para que los toques/clics caigan en el punto correcto
  // sin importar qué tan grande o chico se vea el canvas.
  getCanvasScale(canvas) {
    const rect = canvas.getBoundingClientRect();
    return {
      x: canvas.width / rect.width,
      y: canvas.height / rect.height
    };
  }
};

```

### Archivo: `js/juegos/runner-magico.js`
```javascript
// =========================================================
// RUNNER-MAGICO.JS — Saltar obstáculos que se acercan; cada
// obstáculo esquivado suma monedas. Soporte de teclado y táctil.
// =========================================================
Object.assign(Minigames, {
  startRunnerGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('runner');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🦄 Runner Mágico</h3>
        ${Minigames.renderHUD('🦄', best)}
        <canvas id="runnerCanvas" width="300" height="280" style="background:linear-gradient(180deg, #fff9c4 0%, #f3e5f5 100%); border-radius:16px; margin:6px auto; display:block; border:3px solid #ffd54f; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:300/280;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca la pantalla o Espacio para saltar</p>
      `;

      const canvas = document.getElementById('runnerCanvas');
      const ctx = canvas.getContext('2d');

      let playerY = 210;
      let playerVY = 0;
      let isJumping = false;
      let obstacleX = 300;
      let score = 0;
      let gameOver = false;

      const jump = (e) => {
        if (e) e.preventDefault();
        if (!isJumping) {
          playerVY = -11;
          isJumping = true;
          AudioEffects.playTone(550, 'square', 0.08);
        }
      };

      const handleKeyDown = (e) => { if (e.code === 'Space') jump(); };
      window.addEventListener('keydown', handleKeyDown);
      canvas.addEventListener('touchstart', jump, { passive: false });
      canvas.onclick = jump;

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        window.removeEventListener('keydown', handleKeyDown);
        canvas.removeEventListener('touchstart', jump);
        canvas.onclick = null;
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleKeyDown);
          canvas.removeEventListener('touchstart', jump);
          const isRecord = Minigames.saveBestScore('runner', score);
          Minigames.showResult('¡Juego terminado!', '🦄', score, () => Minigames.startRunnerGame(), isRecord);
          return;
        }

        playerY += playerVY;
        playerVY += 0.65;

        if (playerY >= 210) {
          playerY = 210;
          isJumping = false;
        }

        obstacleX -= 4.2;
        if (obstacleX < -25) {
          obstacleX = 300;
          score += 10;
          Minigames.updateHudScore(score);
        }

        if (obstacleX < 45 && obstacleX > 15 && playerY > 180) gameOver = true;

        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Suelo
        ctx.fillStyle = '#e1bee7';
        ctx.fillRect(0, 240, 300, 40);

        // Personaje
        ctx.fillStyle = '#ff4081';
        ctx.beginPath();
        ctx.arc(35, playerY + 15, 15, 0, Math.PI * 2);
        ctx.fill();

        // Obstáculo
        ctx.fillStyle = '#ffb300';
        ctx.beginPath();
        ctx.arc(obstacleX + 10, 225, 12, 0, Math.PI * 2);
        ctx.fill();

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  },
});

```

### Archivo: `js/juegos/simon-memoria.js`
```javascript
// =========================================================
// SIMON-MEMORIA.JS — Memorizar y repetir la secuencia de
// colores que se enciende, una ronda más larga cada vez.
// =========================================================
Object.assign(Minigames, {
  startSimonGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('simon');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🎯 Simón Memoria</h3>
        ${Minigames.renderHUD('🧠', best)}
        <p id="simon-status" style="margin-top:5px; text-align:center; font-weight:bold; color:#7e57c2;">¡Memoriza la secuencia!</p>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px; margin:20px 0;">
          <button id="simon-0" onclick="Minigames.simonClick(0)" style="height:75px; background:#ff80ab; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #f50057; touch-action:manipulation;"></button>
          <button id="simon-1" onclick="Minigames.simonClick(1)" style="height:75px; background:#80deea; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #00acc1; touch-action:manipulation;"></button>
          <button id="simon-2" onclick="Minigames.simonClick(2)" style="height:75px; background:#b388ff; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #673ab7; touch-action:manipulation;"></button>
          <button id="simon-3" onclick="Minigames.simonClick(3)" style="height:75px; background:#ffe082; border:none; border-radius:18px; cursor:pointer; box-shadow:0 4px 0 #ffb300; touch-action:manipulation;"></button>
        </div>
      `;

      this.simonSequence = [];
      this.userSequence = [];
      this.simonScore = 0;
      this.nextSimonRound();

      Minigames.inProgress = true;
      Minigames.exitHandler = () => Minigames.simonScore * 10;
    });
  },

  nextSimonRound() {
    this.userSequence = [];
    this.simonSequence.push(Math.floor(Math.random() * 4));
    document.getElementById('simon-status').innerText = `Ronda ${this.simonSequence.length} ✨`;
    
    let i = 0;
    const interval = setInterval(() => {
      if (i >= this.simonSequence.length) {
        clearInterval(interval);
        return;
      }
      this.flashSimonButton(this.simonSequence[i]);
      i++;
    }, 700);
  },

  flashSimonButton(index) {
    const btn = document.getElementById(`simon-${index}`);
    if (!btn) return;
    const freqs = [350, 450, 550, 650];
    AudioEffects.playTone(freqs[index], 'sine', 0.15);
    btn.style.opacity = '0.3';
    setTimeout(() => btn.style.opacity = '1', 350);
  },

  simonClick(index) {
    this.flashSimonButton(index);
    this.userSequence.push(index);

    const currentIndex = this.userSequence.length - 1;
    if (this.userSequence[currentIndex] !== this.simonSequence[currentIndex]) {
      const reward = this.simonScore * 10;
      const isRecord = Minigames.saveBestScore('simon', this.simonScore);
      Minigames.showResult('¡Te equivocaste!', '🧠', reward, () => Minigames.startSimonGame(), isRecord);
      return;
    }

    if (this.userSequence.length === this.simonSequence.length) {
      this.simonScore++;
      Minigames.updateHudScore(this.simonScore);
      setTimeout(() => this.nextSimonRound(), 1000);
    }
  },
});

```

### Archivo: `js/juegos/torre-postres.js`
```javascript
// =========================================================
// TORRE-POSTRES.JS — Apilar capas de pastel tocando en el
// momento justo; si la capa cae mal apilada, se acaba el juego.
// =========================================================
Object.assign(Minigames, {
  startCakeGame() {
    this.showCountdown(() => {
      const content = document.getElementById('modal-content');
      const best = Minigames.getBestScore('cake');
      content.innerHTML = `
        <h3 style="color:#ab47bc; margin-bottom:2px;">🍰 Torre de Postres</h3>
        ${Minigames.renderHUD('🍰', best)}
        <canvas id="cakeCanvas" width="300" height="300" style="background:#f3e5f5; border-radius:16px; margin:6px auto; display:block; border:3px solid #b388ff; touch-action:none; width:100%; max-width:300px; height:auto; aspect-ratio:1/1;"></canvas>
        <p style="font-size:0.8rem; text-align:center; color:#666;">Toca la pantalla para apilar el pastel</p>
      `;

      const canvas = document.getElementById('cakeCanvas');
      const ctx = canvas.getContext('2d');
      let layers = [{ x: 100, width: 100, y: 270 }];
      let currentX = 0;
      let currentSpeed = 3;
      let score = 0;
      let gameOver = false;

      const dropLayer = (e) => {
        if (e) e.preventDefault();
        if (gameOver) return;
        const prev = layers[layers.length - 1];
        const diff = currentX - prev.x;

        if (Math.abs(diff) > prev.width) {
          gameOver = true;
          const isRecord = Minigames.saveBestScore('cake', score);
          Minigames.showResult('¡Se cayó la torre!', '🍰', score, () => Minigames.startCakeGame(), isRecord);
          return;
        }

        score += 10;
        Minigames.updateHudScore(score);
        AudioEffects.playTone(700, 'sine', 0.1);
        const newWidth = prev.width - Math.abs(diff);
        const newX = diff > 0 ? currentX : prev.x;

        layers.push({ x: newX, width: newWidth, y: prev.y - 25 });
        currentX = 0;

        if (layers.length > 8) {
          layers.forEach(l => l.y += 25);
        }
      };

      canvas.addEventListener('touchstart', dropLayer, { passive: false });
      canvas.onclick = dropLayer;

      const handleSpace = (e) => { if (e.code === 'Space') dropLayer(); };
      window.addEventListener('keydown', handleSpace);

      Minigames.inProgress = true;
      Minigames.exitHandler = () => {
        canvas.removeEventListener('touchstart', dropLayer);
        canvas.onclick = null;
        window.removeEventListener('keydown', handleSpace);
        gameOver = true;
        return score;
      };

      function loop() {
        if (gameOver) {
          window.removeEventListener('keydown', handleSpace);
          return;
        }

        currentX += currentSpeed;
        const prevWidth = layers[layers.length - 1].width;
        if (currentX > 300 - prevWidth || currentX < 0) currentSpeed *= -1;

        ctx.clearRect(0, 0, canvas.width, canvas.height);

        layers.forEach((l, i) => {
          ctx.fillStyle = i % 2 === 0 ? '#ff80ab' : '#80cbc4';
          ctx.beginPath();
          ctx.roundRect(l.x, l.y, l.width, 22, 6);
          ctx.fill();
        });

        const currentY = layers[layers.length - 1].y - 25;
        ctx.fillStyle = '#ffd54f';
        ctx.beginPath();
        ctx.roundRect(currentX, currentY, prevWidth, 22, 6);
        ctx.fill();

        requestAnimationFrame(loop);
      }

      requestAnimationFrame(loop);
    });
  }
});

```

### Archivo: `js/mascota-estado.js`
```javascript
// =========================================================
// MASCOTA-ESTADO.JS — Datos de la mascota (stats, inventario,
// looks, diálogos) y todo lo que los modifica: acciones (comer,
// dormir, medicina), setters de personalización, persistencia
// en localStorage y el decaimiento periódico (tick).
// mascota-visual.js agrega a este mismo objeto PetState los
// métodos de renderizado y movimiento vía Object.assign.
// =========================================================
const PetState = {
  coins: 500,
  happiness: 100,
  hunger: 100,
  energy: 100,
  petColor: '#ff80ab',
  petShape: 'shape-circle',
  petSize: 'medium',
  glitterEnabled: false,
  glitterIntensity: 0,
  isSick: false,
  isSleeping: false,
  inventory: ['bow_tie', 'bg_living'],
  equippedAccessory: 'bow_tie',
  equippedBackground: 'bg_living',
  looks: [],
  dialogues: {},

  async init() {
    this.loadData();
    await this.loadDialogues();
    this.updateUI();
    setInterval(() => this.tick(), 8000);
    this.startWandering();
  },

  async loadDialogues() {
    try {
      const response = await fetch('data/dialogues.json');
      this.dialogues = await response.json();
    } catch (e) {
      this.dialogues = {
        happy: ['❤️ ¡Me encanta estar contigo!', '✨ ¡Hoy es un día genial!', '🌸 ¡Eres mi persona favorita!'],
        hungry: ['🍕 ¡Tengo hambre! ¡Comida rica!', '🍗 Mmm... ¿Hay algo para picar?'],
        sick: ['🥺 Me siento mal... ¿Me das medicina?', '🤒 Me dolió la pancita...'],
        tired: ['😴 Tengo sueño... ¡A dormir!', '💤 Mis ojitos se cierran...'],
        bored: ['🎮 ¿Jugamos un minijuego?', '🎈 ¡Estoy aburrida! Vamos a jugar.'],
        sleeping: ['💤 Zzz... Descansando...', '🌙 Zzz... Soñando con caramelos...']
      };
    }
  },

  feed() {
    if (this.isSleeping) return;
    if (this.hunger >= 100) {
      this.showDialogue(this.getDialogue('full', 'No preciso comer ahora.'));
      return;
    }
    this.hunger = Math.min(100, this.hunger + 30);
    AudioEffects.playEat();
    this.saveData();
    this.updateUI();
  },

  heal() {
    if (!this.isSick) {
      this.showDialogue(this.getDialogue('healthy', 'No preciso un doctor ahora.'));
      return;
    }
    this.isSick = false;
    this.happiness = Math.min(100, this.happiness + 20);
    AudioEffects.playHeal();
    this.saveData();
    this.updateUI();
  },

  sleep() {
    this.isSleeping = !this.isSleeping;
    
    // Si se duerme, quita la animación de caminar de inmediato
    const petEl = document.getElementById('pet');
    if (petEl) petEl.classList.remove('walking');

    if (this.isSleeping) {
      this.energy = 100;
      AudioEffects.playTone(300, 'sine', 0.3);
    }
    this.saveData();
    this.updateUI();
  },

  setColor(color) {
    this.petColor = color;
    this.saveData();
    this.updateUI();
  },

  setShape(shapeId) {
    this.petShape = shapeId;
    AudioEffects.playTone(700, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  },

  setSize(size) {
    if (!['small', 'medium', 'large'].includes(size)) return;
    this.petSize = size;
    AudioEffects.playTone(700, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  },

  setGlitterIntensity(intensity) {
    this.glitterIntensity = Math.max(0, Math.min(100, Number(intensity) || 0));
    this.glitterEnabled = this.glitterIntensity > 0;
    this.saveData();
    this.updateUI();
  },

  setGlitterEnabled(enabled) {
    this.glitterEnabled = Boolean(enabled);
    this.saveData();
    this.updateUI();
  },

  equipItem(itemId) {
    const item = Catalogo.items.find(i => i.id === itemId);
    if (!item) return;

    if (item.type === 'bg') {
      this.equippedBackground = (this.equippedBackground === itemId) ? 'bg_living' : itemId;
    } else {
      this.equippedAccessory = (this.equippedAccessory === itemId) ? null : itemId;
    }

    AudioEffects.playTone(800, 'sine', 0.08);
    this.saveData();
    this.updateUI();
  },

  // Guarda la combinación actual (accesorio + fondo + color) como un "look".
  // Máximo 6 guardados: al superar el límite se descarta el más viejo.
  saveLook(name) {
    if (!this.looks) this.looks = [];
    const look = {
      id: 'look_' + Date.now(),
      name: (name && name.length > 0) ? name : `Look ${this.looks.length + 1}`,
      accessory: this.equippedAccessory,
      background: this.equippedBackground,
      color: this.petColor
    };
    this.looks.push(look);
    if (this.looks.length > 6) this.looks.shift();
    AudioEffects.playCoin();
    this.saveData();
  },

  applyLook(lookId) {
    const look = (this.looks || []).find(l => l.id === lookId);
    if (!look) return;
    this.equippedAccessory = look.accessory;
    this.equippedBackground = look.background;
    this.petColor = look.color;
    AudioEffects.playTone(800, 'sine', 0.08);
    this.saveData();
    this.updateUI();
  },

  deleteLook(lookId) {
    this.looks = (this.looks || []).filter(l => l.id !== lookId);
    this.saveData();
  },

  addCoins(amount) {
    this.coins += amount;
    if (amount > 0) {
      this.happiness = Math.min(100, this.happiness + 15);
      AudioEffects.playCoin();
    }
    this.saveData();
    this.updateUI();
  },

  tick() {
    if (this.isSleeping) {
      this.energy = Math.min(100, this.energy + 10);
      if (this.energy === 100) this.isSleeping = false;
    } else {
      this.hunger = Math.max(0, this.hunger - 3);
      this.happiness = Math.max(0, this.happiness - 3);
      this.energy = Math.max(0, this.energy - 2);

      if (!this.isSick && (this.hunger < 40 || this.happiness < 40) && Math.random() < 0.2) {
        this.isSick = true;
      }
    }
    this.saveData();
    this.updateUI();
  },

  saveData() {
    localStorage.setItem('virtualPetData', JSON.stringify({
      coins: this.coins,
      happiness: this.happiness,
      hunger: this.hunger,
      energy: this.energy,
      petColor: this.petColor,
      petShape: this.petShape,
      petSize: this.petSize,
      glitterEnabled: this.glitterEnabled,
      glitterIntensity: this.glitterIntensity,
      isSick: this.isSick,
      inventory: this.inventory,
      equippedAccessory: this.equippedAccessory,
      equippedBackground: this.equippedBackground,
      looks: this.looks
    }));
  },

  loadData() {
    const saved = localStorage.getItem('virtualPetData');
    if (saved) {
      const data = JSON.parse(saved);
      this.coins = data.coins ?? 100;
      this.happiness = data.happiness ?? 100;
      this.hunger = data.hunger ?? 100;
      this.energy = data.energy ?? 100;
      this.petColor = data.petColor || '#ff80ab';
      this.petShape = data.petShape || 'shape-circle';
      this.petSize = ['small', 'medium', 'large'].includes(data.petSize) ? data.petSize : 'medium';
      const savedGlitterIntensity = data.glitterIntensity ?? (data.glitterEnabled ? 50 : 0);
      this.glitterIntensity = Math.max(0, Math.min(100, Number(savedGlitterIntensity)));
      this.glitterEnabled = this.glitterIntensity > 0;
      this.isSick = data.isSick ?? false;
      this.inventory = data.inventory || ['bow_tie', 'bg_living'];
      this.equippedAccessory = data.equippedAccessory || null;
      this.equippedBackground = data.equippedBackground || 'bg_living';
      this.looks = data.looks || [];
    }
  }
};

```

### Archivo: `js/mascota-visual.js`
```javascript
// =========================================================
// MASCOTA-VISUAL.JS — Todo lo que dibuja o mueve a la mascota
// en pantalla: pintar stats/forma/color/accesorio (updateUI),
// rostro y globo de diálogo, la animación de acariciar (petPet)
// y el paseo autónomo por la habitación. Se agrega al mismo
// objeto PetState (definido en mascota-estado.js) vía
// Object.assign, así que debe cargarse después de ese archivo.
// =========================================================
Object.assign(PetState, {
  getRandomPhrase(category) {
    const list = this.dialogues[category] || ['❤️ ¡Hola!'];
    const index = Math.floor(Math.random() * list.length);
    return list[index];
  },

  getDialogue(key, fallback) {
    return this.dialogues.alerts?.[key] || fallback;
  },

  showDialogue(message) {
    const thought = document.getElementById('pet-thought');
    if (!thought) return;
    thought.innerText = message;
    clearTimeout(this.dialogueTimeout);
    this.dialogueTimeout = setTimeout(() => this.updateFaceAndThought(), 3000);
  },

  startWandering() {
    clearInterval(this.wanderingTimer);
    this.moveRandomly();
    this.wanderingTimer = setInterval(() => {
      // Solo se mueve si NO está durmiendo ni enferma
      if (!this.isSleeping && !this.isSick) {
        this.moveRandomly();
      }
    }, 5000);
  },

  moveRandomly() {
    const petEl = document.getElementById('pet');
    const roomEl = document.getElementById('pet-room');
    if (!petEl || !roomEl) return;

    const roomWidth = roomEl.clientWidth;
    const petWidth = petEl.offsetWidth;
    const minX = 20;
    const maxX = Math.max(minX, roomWidth - petWidth - 20);

    const targetX = Math.floor(Math.random() * (maxX - minX + 1)) + minX;

    petEl.classList.add('walking');
    petEl.style.left = `${targetX}px`;

    setTimeout(() => {
      petEl.classList.remove('walking');
    }, 2500);
  },

  updateUI() {
    document.getElementById('coin-count').innerText = this.coins;
    document.getElementById('happy-count').innerText = this.happiness;
    document.getElementById('hunger-count').innerText = this.hunger;
    document.getElementById('energy-count').innerText = this.energy;

    const petEl = document.getElementById('pet');
    if (petEl) {
      petEl.style.backgroundColor = this.petColor;
      // currentColor: lo usan las formas decorativas (orejitas, nube, corazón)
      petEl.style.color = this.petColor;
      // No pisa 'walking'/'happy-jump' u otras clases temporales que ya tenga el elemento
      petEl.className = petEl.className.replace(/\bshape-\S+/g, '').trim();
      petEl.classList.add(this.petShape || 'shape-circle');
      petEl.className = petEl.className.replace(/\bsize-\S+/g, '').trim();
      petEl.classList.add(`size-${this.petSize || 'medium'}`);
      const glitterEl = document.getElementById('pet-glitter');
      if (glitterEl) {
        glitterEl.classList.toggle('enabled', this.glitterIntensity > 0);
        glitterEl.style.setProperty('--glitter-opacity', (0.1 + this.glitterIntensity / 100 * 0.9).toFixed(2));
      }
    }

    const roomEl = document.getElementById('pet-room');
    if (roomEl) {
      roomEl.className = `room ${this.equippedBackground}`;
    }

    if (typeof Particles !== 'undefined') {
      Particles.render(this.equippedBackground);
    }

    const accessoryEl = document.getElementById('pet-accessory');
    accessoryEl.className = 'accessory';
    accessoryEl.style.cssText = '';

    if (this.equippedAccessory) {
      const item = Catalogo.items.find(i => i.id === this.equippedAccessory);
      if (item) {
        accessoryEl.innerHTML = `<img src="${item.image}" alt="${item.name}">`;
        accessoryEl.classList.add(`pos-${item.type}`);
        accessoryEl.classList.add(`item-${item.id}`);
        const fit = Catalogo.accessoryFits?.[item.id];
        if (fit) {
          const accessoryImg = accessoryEl.querySelector('img');
          accessoryImg.style.setProperty('--accessory-scale', fit.scale);
          accessoryImg.style.setProperty('--accessory-y', `${fit.y}px`);
          accessoryImg.style.setProperty('--accessory-x', `${fit.x || 0}px`);
          accessoryImg.style.setProperty('--accessory-rotate', `${fit.rotate || 0}deg`);
        }
      }
    } else {
      accessoryEl.innerHTML = '';
    }

    this.updateFaceAndThought();
  },

  updateFaceAndThought() {
    const face = document.getElementById('kawaii-face');
    const thought = document.getElementById('pet-thought');
    const extraProp = document.getElementById('extra-prop');
    
    if (!face || !thought) return;

    face.className = 'kawaii-face';
    extraProp.innerText = '';

    if (this.isSleeping) {
      face.classList.add('sleeping');
      thought.innerText = this.getRandomPhrase('sleeping');
      extraProp.innerText = '💤';
    } else if (this.isSick) {
      face.classList.add('sick');
      thought.innerText = this.getRandomPhrase('sick');
      extraProp.innerText = '🌡️';
    } else if (this.hunger < 30) {
      face.classList.add('hungry');
      thought.innerText = this.getRandomPhrase('hungry');
      extraProp.innerText = '💧';
    } else if (this.energy < 30) {
      face.classList.add('tired');
      thought.innerText = this.getRandomPhrase('tired');
    } else if (this.happiness < 40) {
      face.classList.add('bored');
      thought.innerText = this.getRandomPhrase('bored');
    } else {
      face.classList.add('happy');
      thought.innerText = this.getRandomPhrase('happy');
    }
  },

  petPet() {
    if (this.isSleeping) return;
    this.happiness = Math.min(100, this.happiness + 10);
    const petEl = document.getElementById('pet');
    petEl.classList.add('happy-jump');
    setTimeout(() => petEl.classList.remove('happy-jump'), 500);

    for (let i = 0; i < 3; i++) {
      const heart = document.createElement('div');
      heart.className = 'heart-particle';
      heart.innerText = '❤️';
      const offsetX = (Math.random() - 0.5) * 60;
      heart.style.setProperty('--dx', `${offsetX}px`);
      heart.style.left = `calc(50% + ${offsetX / 2}px)`;
      heart.style.top = '10px';
      petEl.appendChild(heart);
      setTimeout(() => heart.remove(), 800);
    }

    AudioEffects.playTone(600, 'sine', 0.1);
    this.saveData();
    this.updateUI();
  }
});

```

### Archivo: `js/particulas.js`
```javascript
// =========================================================
// PARTÍCULAS AMBIENTALES POR ESCENARIO (CSS puro + spawn en JS)
// Usa el contenedor #bg-particles y las animaciones .fall-slow,
// .float-up y .twinkle ya definidas en css/escenarios.css.
//
// Para sumar partículas a un fondo nuevo, alcanza con agregar
// una entrada en CONFIG con su modo y sus emojis.
// =========================================================
const Particles = {
  CONFIG: {
    // --- Bloque 4 parte 1 ---
    bg_space:      { mode: 'twinkle',   emojis: ['⭐', '☄️'], count: 12 },
    bg_underwater: { mode: 'float-up',  emojis: ['🫧', '💧', '🐟'], count: 9, interval: 1800 }, // 🐟 Pequeños peces subiendo
    bg_beach:      { mode: 'twinkle',   emojis: ['☀️', '🐚'], count: 8 },
    bg_forest:     { mode: 'fall-slow', emojis: ['🍃', '🍂', '🦋'], count: 8, interval: 2200 }, // 🦋 Mariposas cayendo/volando suavemente

    // --- Bloque 4 parte 2 ---
    bg_living:     { mode: 'twinkle',   emojis: ['✨'],       count: 9 },
    bg_bedroom:    { mode: 'twinkle',   emojis: ['⭐', '💤'], count: 9 },
    bg_playroom:   { mode: 'float-up',  emojis: ['🎈'],       count: 8, interval: 2000 },
    bg_park:       { mode: 'fall-slow', emojis: ['🍃', '🌸'], count: 8, interval: 2200 },
    bg_mountain:   { mode: 'fall-slow', emojis: ['❄️'],       count: 12, interval: 1400 },
    bg_camping:    { mode: 'twinkle',   emojis: ['⭐', '🔥', '🦉'], count: 10 }, // 🦉 Búhos y brasas de la fogata
    bg_desert:     { mode: 'twinkle',   emojis: ['☀️', '💨'], count: 8 },
    bg_kitchen:    { mode: 'float-up',  emojis: ['♨️', '🧂', '🧁'], count: 6, interval: 2200 }, // 🧂 Especias y vapor subiendo
    bg_bathroom:   { mode: 'float-up',  emojis: ['🫧', '💧', '🦆'], count: 9, interval: 1600 }, // 🦆 Patitos de hule flotando entre burbujas
    bg_dojo:       { mode: 'fall-slow', emojis: ['🌸', '🍃', '☯️'], count: 8, interval: 2200 }, // ☯️ Equilibrio y pétalos de cerezo
    bg_disco:      { mode: 'twinkle',   emojis: ['✨', '🎶', '🪩'], count: 12 }, // 🎶 Música y destellos de la bola de espejos
    bg_castle:     { mode: 'float-up',  emojis: ['✨', '👑', '🧚', '🪄'], count: 8, interval: 2400 },
    bg_volcano:    { mode: 'float-up',  emojis: ['🔥', '🌋', '💥'],       count: 8, interval: 2000 }
  },

  _current: null,
  _spawnTimer: null,

  // Cambia las partículas activas según el fondo equipado.
  // No hace nada si el fondo no cambió (evita reiniciar la animación
  // cada vez que se llama updateUI()).
  render(backgroundId) {
    if (this._current === backgroundId) return;
    this._current = backgroundId;

    const container = document.getElementById('bg-particles');
    if (!container) return;

    container.innerHTML = '';
    if (this._spawnTimer) {
      clearInterval(this._spawnTimer);
      this._spawnTimer = null;
    }

    const config = this.CONFIG[backgroundId];
    if (!config) return; // Este fondo todavía no tiene partículas asignadas

    if (config.mode === 'twinkle') {
      // Partículas fijas que titilan en su lugar (estrellas, brillos)
      for (let i = 0; i < config.count; i++) {
        this.spawnTwinkle(container, config.emojis);
      }
    } else {
      // Ráfaga inicial para no esperar al primer intervalo
      for (let i = 0; i < config.count; i++) {
        setTimeout(() => this.spawnStream(container, config), i * (config.interval / config.count));
      }
      // Cuántas partículas nacen por tick para que la densidad sostenida
      // en pantalla siga reflejando el `count` configurado (antes solo
      // nacía 1 por tick sin importar cuánto se subiera `count`).
      const perTick = Math.max(1, Math.round(config.count / 5));
      this._spawnTimer = setInterval(() => {
        for (let i = 0; i < perTick; i++) {
          this.spawnStream(container, config);
        }
      }, config.interval);
    }
  },

  spawnTwinkle(container, emojis) {
    const el = document.createElement('span');
    el.className = 'particle twinkle';
    el.innerText = emojis[Math.floor(Math.random() * emojis.length)];
    el.style.left = `${Math.random() * 88 + 4}%`;
    el.style.top = `${Math.random() * 65 + 5}%`;
    el.style.fontSize = `${(Math.random() * 0.6 + 0.7).toFixed(2)}rem`;
    el.style.animationDuration = `${(Math.random() * 1.6 + 1.4).toFixed(2)}s`;
    el.style.animationDelay = `${(Math.random() * 2).toFixed(2)}s`;
    container.appendChild(el);
  },

  spawnStream(container, config) {
    const el = document.createElement('span');
    const emoji = config.emojis[Math.floor(Math.random() * config.emojis.length)];
    el.className = `particle ${config.mode}`;
    el.innerText = emoji;
    el.style.left = `${Math.random() * 86 + 4}%`;
    el.style.fontSize = `${(Math.random() * 0.5 + 0.9).toFixed(2)}rem`;
    const duration = Math.random() * 2.5 + 4.5;
    el.style.animationDuration = `${duration.toFixed(2)}s`;
    container.appendChild(el);

    setTimeout(() => el.remove(), (duration + 0.3) * 1000);
  }
};

```

### Archivo: `js/tienda.js`
```javascript
// =========================================================
// TIENDA.JS — Renderizado de la tienda, el armario (Accesorios/
// Escenario) y el panel "Mi Pet" (color, tamaño, forma,
// brillantina). Usa los datos de Catalogo (catalogo.js) y el
// estado de PetState — no define datos propios.
// =========================================================
const Store = {
  storeCategory: 'all',

  // Ícono de la card: mini-preview del gradiente real para fondos,
  // ícono SVG para accesorios.
  renderIcon(item) {
    if (item.category === 'bg') {
      return `<div class="bg-preview" style="${item.preview}"><span class="bg-preview-badge">${item.image}</span></div>`;
    }
    return `<img src="${item.image}" class="card-icon" alt="${item.name}">`;
  },

  renderStore(category = this.storeCategory) {
    this.storeCategory = category;
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">🛍️ Tienda de Objetos</h2>
      <div class="grid-container"></div>
      <div class="store-tabs" role="tablist" aria-label="Categorías de la tienda">
        <button class="store-tab ${category === 'all' ? 'active' : ''}" data-category="all">Todos</button>
        <button class="store-tab ${category === 'acc' ? 'active' : ''}" data-category="acc">Accesorios</button>
        <button class="store-tab ${category === 'bg' ? 'active' : ''}" data-category="bg">Escenarios</button>
      </div>
    `;

    content.querySelectorAll('.store-tab').forEach(tab => {
      tab.onclick = () => this.renderStore(tab.dataset.category);
    });
    
    const container = content.querySelector('.grid-container');
    
    Catalogo.items.filter(item => category === 'all' || item.category === category).forEach(item => {
      const isOwned = PetState.inventory.includes(item.id);
      const card = document.createElement('div');
      card.className = 'item-card';

      card.innerHTML = `
        ${this.renderIcon(item)}
        <span class="card-title">${item.name}</span>
        <span class="card-price">${item.price === 0 ? 'Gratis' : '🪙 ' + item.price}</span>
        <button onclick="Store.buyItem('${item.id}')" ${isOwned ? 'disabled' : ''} class="card-btn ${isOwned ? 'owned' : ''}">
          ${isOwned ? 'Comprado' : 'Comprar'}
        </button>
      `;
      container.appendChild(card);
    });
  },

  renderPetCustomization() {
    return `
      <section class="pet-customization-panel" aria-label="Personalizar Mi Pet">
        <h3>Mi Pet</h3>
        <div class="wardrobe-top-row">
          <div style="margin: 8px 0; text-align:center;">
            <p class="wardrobe-label">Color de Piel:</p>
            <div class="color-picker">
              <button class="color-option ${PetState.petColor === '#ff80ab' ? 'selected' : ''}" onclick="PetState.setColor('#ff80ab'); Store.renderMyPet();" aria-label="Color rosa" style="background:#ff80ab;"></button>
              <button class="color-option ${PetState.petColor === '#b388ff' ? 'selected' : ''}" onclick="PetState.setColor('#b388ff'); Store.renderMyPet();" aria-label="Color violeta" style="background:#b388ff;"></button>
              <button class="color-option ${PetState.petColor === '#80cbc4' ? 'selected' : ''}" onclick="PetState.setColor('#80cbc4'); Store.renderMyPet();" aria-label="Color turquesa" style="background:#80cbc4;"></button>
            </div>
          </div>
          <div style="margin: 8px 0; text-align:center;">
            <p class="wardrobe-label">Tamaño:</p>
            <div id="size-picker" class="size-picker">
              <button class="card-btn size-option ${PetState.petSize === 'small' ? 'equipped' : ''}" data-size="small" aria-label="Tamaño chico">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(0.6);"></span></span>
              </button>
              <button class="card-btn size-option ${PetState.petSize === 'medium' ? 'equipped' : ''}" data-size="medium" aria-label="Tamaño mediano">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(0.8);"></span></span>
              </button>
              <button class="card-btn size-option ${PetState.petSize === 'large' ? 'equipped' : ''}" data-size="large" aria-label="Tamaño grande">
                <span class="size-preview"><span class="pet-sprite shape-squircle" style="transform:scale(1);"></span></span>
              </button>
            </div>
          </div>
        </div>
        <div style="margin: 4px 0 8px; text-align:center;">
          <p class="wardrobe-label">Forma:</p>
          <div id="shape-picker" class="shape-picker"></div>
        </div>
        <div class="glitter-intensity">
          <label for="glitter-intensity">✨ Brillantina: <output id="glitter-intensity-value">${PetState.glitterIntensity}%</output></label>
          <input id="glitter-intensity" type="range" min="0" max="100" value="${PetState.glitterIntensity}">
        </div>
      </section>
    `;
  },

  bindPetCustomization(content) {
    const shapePicker = content.querySelector('#shape-picker');
    Catalogo.shapes.forEach(shape => {
      const isSelected = PetState.petShape === shape.id;
      const btn = document.createElement('button');
      btn.className = `card-btn shape-option ${isSelected ? 'selected' : ''}`;
      btn.setAttribute('aria-label', `Forma ${shape.name}`);
      btn.innerHTML = `${shape.emoji}<br>${shape.name}`;
      btn.onclick = () => { PetState.setShape(shape.id); Store.renderMyPet(); };
      shapePicker.appendChild(btn);
    });

    content.querySelectorAll('.size-option').forEach(btn => {
      btn.onclick = () => { PetState.setSize(btn.dataset.size); Store.renderMyPet(); };
    });

    const glitterIntensity = content.querySelector('#glitter-intensity');
    const glitterIntensityValue = content.querySelector('#glitter-intensity-value');
    glitterIntensity.oninput = event => {
      glitterIntensityValue.value = `${event.target.value}%`;
      glitterIntensityValue.textContent = `${event.target.value}%`;
      PetState.setGlitterIntensity(event.target.value);
    };
  },

  renderMyPet() {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">🐾 Mi Pet</h2>
      ${this.renderPetCustomization()}
    `;
    this.bindPetCustomization(content);
  },

  renderCollection(title, category, includeLooks, rerender) {
    const content = document.getElementById('modal-content');
    content.innerHTML = `
      <h2 style="text-align:center; color:#6a1b9a;">${title}</h2>
      ${includeLooks ? `<div style="margin: 4px 0 8px; text-align:center;">
        <p style="font-size:0.8rem; color:#666; margin-bottom:4px; font-weight:bold;">⭐ Mis Looks:</p>
        <div id="looks-list" style="display:flex; flex-wrap:wrap; gap:6px; justify-content:center; margin-bottom:8px;"></div>
        <button onclick="Store.saveCurrentLook()" class="card-btn" style="width:auto; padding:6px 16px; background:#7e57c2;">💾 Guardar look actual</button>
      </div>` : ''}
      <hr style="border:none; border-top:1px solid #eee; margin:8px 0;">
      <div class="grid-container"></div>
    `;

    if (includeLooks) {
      const looksList = content.querySelector('#looks-list');
      if (!PetState.looks || PetState.looks.length === 0) {
        looksList.innerHTML = `<span style="font-size:0.72rem; color:#999;">Todavía no guardaste ningún look</span>`;
      } else {
        PetState.looks.forEach(look => {
          const chip = document.createElement('div');
          chip.className = 'look-chip';
          chip.innerHTML = `
            <span class="look-chip-name">${look.name}</span>
            <div class="look-chip-actions">
              <button onclick="PetState.applyLook('${look.id}'); Store.renderAccessories();" title="Aplicar look">✨</button>
              <button onclick="PetState.deleteLook('${look.id}'); Store.renderAccessories();" title="Borrar look">🗑️</button>
            </div>
          `;
          looksList.appendChild(chip);
        });
      }
    }

    const container = content.querySelector('.grid-container');

    PetState.inventory.forEach(itemId => {
      const item = Catalogo.items.find(i => i.id === itemId);
      if (!item || item.category !== category) return;

      const isEquipped = item.type === 'bg' 
        ? PetState.equippedBackground === item.id 
        : PetState.equippedAccessory === item.id;

      const card = document.createElement('div');
      card.className = 'item-card';

      card.innerHTML = `
        ${this.renderIcon(item)}
        <span class="card-title">${item.name}</span>
        <button class="card-btn ${isEquipped ? 'equipped' : ''}">
          ${isEquipped ? '✨ Usando' : 'Poner'}
        </button>
      `;
      card.querySelector('.card-btn').onclick = () => {
        PetState.equipItem(item.id);
        rerender();
      };
      container.appendChild(card);
    });
  },

  renderAccessories() {
    this.renderCollection('🎒 Accesorios', 'acc', true, () => this.renderAccessories());
  },

  renderScenery() {
    this.renderCollection('🏡 Escenario', 'bg', false, () => this.renderScenery());
  },

  saveCurrentLook() {
    const name = prompt('Nombre para este look:', `Look ${(PetState.looks ? PetState.looks.length : 0) + 1}`);
    if (name === null) return; // El usuario canceló
    PetState.saveLook(name.trim());
    this.renderAccessories();
  },

  buyItem(itemId) {
    const item = Catalogo.items.find(i => i.id === itemId);
    if (item && (PetState.coins >= item.price || item.price === 0) && !PetState.inventory.includes(itemId)) {
      PetState.coins -= item.price;
      PetState.inventory.push(itemId);
      AudioEffects.playCoin();
      PetState.saveData();
      PetState.updateUI();
      this.renderStore(this.storeCategory);
    } else if (item && PetState.coins < item.price) {
      alert('¡No tienes suficientes monedas!');
    }
  }
};

```

### Archivo: `manifest.json`
```json
{
  "name": "Mi Mascota",
  "short_name": "MiniPet",
  "description": "Tu mascota virtual con minijuegos y tienda de accesorios.",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#f0e6ff",
  "theme_color": "#b388ff",
  "orientation": "portrait",
  "icons": [
    {
      "src": "data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🐾</text></svg>",
      "sizes": "192x192 512x512",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    }
  ]
}
```

### Archivo: `readme.md`
```md
# 🐾 Mi Mascota  (MiniPet)

Mascota virtual estilo Tamagotchi, 100% web, instalable como **PWA**. Cuidá a tu mascota, vestila con accesorios, decorá su escenario y ganá monedas jugando minijuegos.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/minipet/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/minipet/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## ✨ Funcionalidades

- **Estado en vivo**: monedas 🪙, felicidad ❤️, hambre 🍕 y energía ⚡, que decaen con el tiempo (`tick()` cada 8s).
- **Mascota animada**: rostro kawaii modular (feliz, con hambre, enferma, cansada, aburrida, durmiendo) y frases aleatorias cargadas desde `data/dialogues.json`.
- **La mascota se mueve sola** por la habitación cada 6s (si no está durmiendo ni enferma).
- **Acciones**: Comer 🍗, Medicina 🏥 (cura si está enferma) y Dormir 😴 (recupera energía al 100%).
- **Armario**: 22 accesorios equipables (sombreros, orejas, lentes, alas, etc.) + selector de color de piel.
- **Tienda**: 17 escenarios/fondos temáticos (living, playa, espacio, castillo, volcán, dojo, disco, etc.), cada uno comprable con monedas.
- **6 minijuegos** para ganar monedas: Atrapa Dulces, Explotar Globos, Torre de Postres, Simón Memoria, Runner Mágico y Burbujas Mágicas — todos con soporte táctil y de teclado.
- **Efectos de sonido** generados con Web Audio API (sin archivos de audio externos).
- **PWA instalable**: `manifest.json` + `sw.js` con estrategia *network-first* y caché offline; avisa al usuario cuando hay una versión nueva disponible.
- **Persistencia local**: todo el progreso se guarda en `localStorage`.

## 📁 Estructura del proyecto

```
minipet/
├── index.html              # Estructura principal de la app
├── manifest.json            # Metadatos PWA (ícono, colores, modo standalone)
├── sw.js                    # Service Worker (caché offline, versión de caché)
├── data/
│   └── dialogues.json       # Frases de la mascota por estado de ánimo
├── css/
│   └── styles.css           # Estilos, animaciones y fondos de escenarios
├── js/
│   ├── pet.js                # Estado del juego (PetState): stats, guardado, UI
│   ├── store.js               # Catálogo de accesorios/fondos, tienda y armario
│   ├── minigames.js           # Los 6 minijuegos
│   └── app.js                 # Sonidos (AudioEffects), init y registro del SW
└── img/
    └── accessories/          # Iconos SVG de los accesorios equipables
```

## ▶️ Cómo correrlo

Es una app estática: basta con servir la carpeta con cualquier servidor HTTP (necesario para que `fetch('data/dialogues.json')` y el Service Worker funcionen; no se puede abrir `index.html` directo con `file://`).

```bash
npx serve .
# o
python3 -m http.server
```

Luego abrir la URL en el navegador. En móvil, se puede "Agregar a pantalla de inicio" para instalarla como PWA.

## 🔧 Notas técnicas

- Al agregar un accesorio nuevo hay que sumarlo en tres lugares: el `.svg` en `img/accessories/`, la entrada en `Store.items` (`js/store.js`) y la ruta en `ASSETS_TO_CACHE` (`sw.js`) para que quede disponible offline.
- Al agregar un fondo/escenario nuevo: la entrada en `Store.items` (categoría `bg`) y el estilo `.room.bg_<id>` en `css/styles.css`.
- Cada vez que se publica una versión nueva, conviene subir `CACHE_VERSION` en `sw.js` para forzar la actualización en los dispositivos de los usuarios.

```

### Archivo: `sw.js`
```javascript
const CACHE_VERSION = 'v2.4.0'; // Change version here to force reload on users' devices
const CACHE_NAME = `minipet-cache-${CACHE_VERSION}`;

const ASSETS_TO_CACHE = [
  './',
  './index.html',
  './css/base.css',
  './css/escenarios.css',
  './css/mascota.css',
  './css/juegos.css',
  './js/app.js',
  './js/mascota-estado.js',
  './js/mascota-visual.js',
  './js/particulas.js',
  './js/catalogo.js',
  './js/tienda.js',
  './js/juegos/juegos-base.js',
  './js/juegos/atrapa-dulces.js',
  './js/juegos/explota-globos.js',
  './js/juegos/torre-postres.js',
  './js/juegos/simon-memoria.js',
  './js/juegos/runner-magico.js',
  './js/juegos/burbujas-magicas.js',
  './data/dialogues.json',
  './manifest.json',
  './img/accessories/hat_crown.svg',
  './img/accessories/glasses_cool.svg',
  './img/accessories/bow_tie.svg',
  './img/accessories/party_hat.svg',
  './img/accessories/headphones.svg',
  './img/accessories/magic_wand.svg',
  './img/accessories/ribbon_pink.svg',
  './img/accessories/star_glasses.svg',
  './img/accessories/flower_pink.svg',
  './img/accessories/cat_ears.svg',
  './img/accessories/pirate_hat.svg',
  './img/accessories/wizard_hat.svg',
  './img/accessories/chef_hat.svg',
  './img/accessories/bunny_ears.svg',
  './img/accessories/fairy_wings.svg',
  './img/accessories/ninja_band.svg',
  './img/accessories/snorkel.svg',
  './img/accessories/space_helmet.svg',
  './img/accessories/winter_beanie.svg',
  './img/accessories/winter_scarf.svg',
  './img/accessories/shower_cap.svg'
];

// Instalación del Service Worker
self.addEventListener('install', (event) => {
  self.skipWaiting(); // Forzar activación inmediata de la nueva versión
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.addAll(ASSETS_TO_CACHE);
    })
  );
});

// Activación y limpieza de caché de versiones anteriores
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cache) => {
          if (cache !== CACHE_NAME) {
            return caches.delete(cache); // Borra caché vieja
          }
        })
      );
    }).then(() => self.clients.claim()) // Toma control de los clientes abiertos
  );
});

// Estrategia de búsqueda: Network First (prioriza descargar lo más nuevo si hay conexión)
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then((networkResponse) => {
        // Si hay red, actualiza la copia en caché y devuelve la versión más reciente
        if (networkResponse && networkResponse.status === 200 && event.request.method === 'GET') {
          const responseClone = networkResponse.clone();
          caches.open(CACHE_NAME).then((cache) => cache.put(event.request, responseClone));
        }
        return networkResponse;
      })
      .catch(() => {
        // Si no hay conexión (offline), devuelve desde la caché
        return caches.match(event.request);
      })
  );
});
```

