# Type_Without_Looking_at_keyboard

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Typing Trainer - Enhanced</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f4f4f4; }
    #prompt { font-size: 24px; margin: 20px 0; }
    #inputBox { font-size: 20px; width: 100%; padding: 10px; }
    #status { margin-top: 10px; }
    #nextBtn { margin-top: 20px; display: none; }
    #mistakes, #tipBox { margin-top: 20px; background: #fff; padding: 10px; border-radius: 8px; }
  </style>
</head>
<body>
  <h1>Typing Trainer - Progressive Levels</h1>
  <h2 id="levelTitle">Level 1: Home Row</h2>
  <div id="tipBox"></div>
  <div id="prompt"></div>
  <input type="text" id="inputBox" autocomplete="off" />
  <div id="status"></div>
  <button id="nextBtn">Next Prompt</button>
  <div id="mistakes"><strong>Mistyped Keys:</strong> <span id="mistypedKeys">None</span></div>

  <script>
    const levels = [
      {
        title: "Level 1: Home Row",
        tips: "💡 Home row: Place left-hand fingers on A-S-D-F and right-hand on J-K-L-;",
        prompts: ["sad lad", "ask dad", "fall as sad", "a salad", "add flask"],
        allowedKeys: "asdfjkl;"
      },
      {
        title: "Level 2: Top Row",
        tips: "💡 Top row: Left hand on Q-W-E-R, right hand on U-I-O-P.",
        prompts: ["try to type", "read it out", "tire out", "dare to read", "you look up"],
        allowedKeys: "asdfjkl;ertyuio"
      },
      {
        title: "Level 3: Bottom Row",
        tips: "💡 Bottom row: Left hand on Z-X-C-V, right hand on M-N-,-.",
        prompts: ["zoom in quick", "pack my bag", "vivid job", "jump back", "kick the box"],
        allowedKeys: "asdfjkl;zxcvbnm"
      }
    ];

    let currentLevel = 0;
    let currentPromptIndex = 0;
    const mistyped = {};

    const promptEl = document.getElementById("prompt");
    const inputBox = document.getElementById("inputBox");
    const status = document.getElementById("status");
    const nextBtn = document.getElementById("nextBtn");
    const levelTitle = document.getElementById("levelTitle");
    const tipBox = document.getElementById("tipBox");
    const mistypedKeysEl = document.getElementById("mistypedKeys");

    function updateMistypedDisplay() {
      const keys = Object.entries(mistyped)
        .sort((a, b) => b[1] - a[1])
        .map(([k, v]) => `${k} (${v})`)
        .join(', ') || "None";
      mistypedKeysEl.textContent = keys;
    }

    function loadPrompt() {
      const level = levels[currentLevel];
      levelTitle.textContent = level.title;
      tipBox.textContent = level.tips;
      promptEl.textContent = level.prompts[currentPromptIndex];
      inputBox.value = "";
      status.textContent = "";
      nextBtn.style.display = "none";
      inputBox.focus();
    }

    inputBox.addEventListener("input", () => {
      const typed = inputBox.value;
      const prompt = levels[currentLevel].prompts[currentPromptIndex];
      for (let i = 0; i < typed.length; i++) {
        if (typed[i] !== prompt[i] && prompt[i]) {
          mistyped[typed[i]] = (mistyped[typed[i]] || 0) + 1;
          updateMistypedDisplay();
        }
      }
      if (typed === prompt) {
        status.textContent = "✅ Correct! Press Enter or click Next.";
        nextBtn.style.display = "inline-block";
      } else {
        status.textContent = "";
        nextBtn.style.display = "none";
      }
    });

    inputBox.addEventListener("keydown", (e) => {
      if (e.key === "Enter" && nextBtn.style.display === "inline-block") {
        nextBtn.click();
      }
    });

    nextBtn.addEventListener("click", () => {
      const level = levels[currentLevel];
      if (currentPromptIndex < level.prompts.length - 1) {
        currentPromptIndex++;
      } else {
        if (currentLevel < levels.length - 1) {
          currentLevel++;
          currentPromptIndex = 0;
          alert("🎉 Level up!");
        } else {
          alert("🏁 You've completed all levels!");
        }
      }
      loadPrompt();
    });

    loadPrompt();
  </script>
</body>
</html>
