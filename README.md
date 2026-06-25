<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Do You Love Me? 💖</title>
  <style>
    :root {
      --bg: #ffe6e6;
      --pink: #d63384;
      --yes: #28a745;
      --no: #dc3545;
      --btn-radius: 5px;
      --transition: 150ms ease;
    }

    html,body {
      height: 100%;
      margin: 0;
    }

    body {
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg);
      text-align: center;
      padding: 1rem;
    }

    .container {
      max-width: 520px;
      width: 100%;
      padding: 1.25rem;
      box-sizing: border-box;
    }

    img.media {
      width: 200px;
      height: 200px;
      object-fit: cover;
      border-radius: 10px;
      display: block;
      margin: 0 auto;
    }

    h1 {
      color: var(--pink);
      font-size: 2rem;
      margin: 1rem 0;
    }

    .buttons {
      display: flex;
      gap: 1rem;
      align-items: center;
      justify-content: center;
      margin-top: 1rem;
      flex-wrap: wrap;
    }

    button {
      -webkit-font-smoothing: antialiased;
      appearance: none;
      border: none;
      padding: 0.75rem 1.25rem;
      font-size: 1.05rem;
      font-weight: 700;
      border-radius: var(--btn-radius);
      cursor: pointer;
      transition: transform var(--transition), box-shadow var(--transition);
    }

    /* Use transform scale for growth (avoids layout shift) */
    #yesBtn {
      background: var(--yes);
      color: white;
      transform: scale(1);
      box-shadow: 0 4px 0 rgba(0,0,0,0.06);
    }
    #noBtn {
      background: var(--no);
      color: white;
    }

    /* Visually-hidden utility for accessibility */
    .visually-hidden {
      position: absolute !important;
      height: 1px; width: 1px;
      overflow: hidden;
      clip: rect(1px, 1px, 1px, 1px);
      white-space: nowrap;
    }

    .hidden { display: none !important; }

    /* Respect user preference for reduced motion */
    @media (prefers-reduced-motion: reduce) {
      button { transition: none; }
    }
  </style>
</head>
<body>
  <main class="container" id="app">
    <!-- Live region for updates for assistive tech -->
    <div id="announcer" class="visually-hidden" aria-live="polite" aria-atomic="true"></div>

    <!-- Question screen -->
    <section id="questionScreen" aria-labelledby="questionText">
      <!-- Replace the src with a real GIF/Image permalink; provided as placeholder -->
      <img class="media" id="gifContainer" src="https://media.giphy.com/media/3o7TKtnuHOHHUjR38Y/giphy.gif" alt="Cute dancing bear" width="200" height="200" loading="lazy" />
      <h1 id="questionText">Do you love me? 🥺</h1>

      <div class="buttons">
        <button id="yesBtn" type="button" aria-label="Yes — I love you">Yes</button>
        <button id="noBtn" type="button" aria-label="No — I don't" >No</button>
      </div>
    </section>

    <!-- Success screen -->
    <section id="successScreen" class="hidden" aria-labelledby="successTitle" role="region">
      <img class="media" src="https://media.giphy.com/media/l0MYt5jPR6QX5pnqM/giphy.gif" alt="Happy dance" width="200" height="200" loading="lazy" />
      <h1 id="successTitle" tabindex="-1">Yay! I love you too! ❤️✨</h1>
    </section>
  </main>

  <script>
    (function () {
      const yesBtn = document.getElementById('yesBtn');
      const noBtn = document.getElementById('noBtn');
      const questionScreen = document.getElementById('questionScreen');
      const successScreen = document.getElementById('successScreen');
      const questionText = document.getElementById('questionText');
      const announcer = document.getElementById('announcer');

      // Use transform scale to avoid layout reflow
      let yesScale = 1;
      let noClicks = 0;

      // playful phrases (no redundant "No" because the button already starts as "No")
      const phrases = [
        "Are you sure? 💔",
        "Really sure?? 🥺",
        "Think again! 😭",
        "Last chance! 🐷",
        "Surely not? ⛔",
        "You're breaking my heart... ❤️‍🩹",
        "Just click Yes already! 😂"
      ];

      function announce(message) {
        // small ARIA helper: put message into the live region
        announcer.textContent = message;
      }

      noBtn.addEventListener('click', () => {
        noClicks++;

        // 1) enlarge the yes button smoothly using transform
        yesScale = Math.min(2.2, yesScale + 0.12); // cap scale to avoid ridiculous sizes
        yesBtn.style.transform = `scale(${yesScale})`;
        yesBtn.style.boxShadow = '0 6px 10px rgba(0,0,0,0.08)';

        // 2) update the No button text from the phrases array (one per click)
        if (noClicks <= phrases.length) {
          noBtn.innerText = phrases[noClicks - 1];
          announce(phrases[noClicks - 1]);
        } else {
          // hide the No button after they've exhausted phrases
          noBtn.classList.add('hidden');
          announce('No button hidden — only Yes remains.');
          // move focus to the Yes button so keyboard users aren't stuck
          yesBtn.focus();
        }
      });

      // Accept Yes via click or keyboard activation (button already supports keyboard)
      yesBtn.addEventListener('click', () => {
        questionScreen.classList.add('hidden');
        successScreen.classList.remove('hidden');
        // move focus to success heading for screen readers
        const successTitle = document.getElementById('successTitle');
        successTitle.focus();
        announce('Yay! I love you too!');
      });

      // Keyboard nicety: press Y to accept (optional)
      document.addEventListener('keydown', (e) => {
        if (e.key.toLowerCase() === 'y') {
          yesBtn.click();
        }
        if (e.key.toLowerCase() === 'n') {
          // simulate a no click but don't steal focus
          noBtn.click();
        }
      });
    })();
  </script>
</body>
</html>
