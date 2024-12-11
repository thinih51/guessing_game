<script>
    let userInput = $state("");
    let secretNumber;
    let output = $state("");
    let deaktiviert = $state(false);
    let attempts = $state(1);
    let min = 1;
    let max = 10;
    let buttons = $state([]);

    function initGame() {
        secretNumber = Math.floor(Math.random() * (max - min + 1)) + min;
        userInput = "";
        output = "";
        deaktiviert = true;
        attempts = 1;

        buttons = Array.from({ length: max - min + 1 }, (_, i) => ({
            value: min + i,
            disabled: false,
        }));
    }

    function guess(value) {
        userInput = value;

        if (!userInput) {
            output = "Bitte eine Zahl eingeben.";
            return;
        }

        const guessedNumber = parseInt(userInput, 10);

        if (guessedNumber > secretNumber) {
            output = `Die gesuchte Zahl ist kleiner als ${guessedNumber}.`;
        } else if (guessedNumber < secretNumber) {
            output = `Die gesuchte Zahl ist größer als ${guessedNumber}.`;
        } else {
            output = `🎉 Getroffen! Du hast die Zahl ${guessedNumber} erraten! Es hat ${attempts} Versuche gebraucht.`;
            deaktiviert = false;
            return;
        }

        attempts++;

        buttons = buttons.map((button) =>
            button.value === guessedNumber ? { ...button, disabled: true } : button
        );
    }

    initGame();
</script>

<style>
    .container {
        max-width: 600px;
        margin: 50px auto;
        text-align: center;
        background: rgba(255, 255, 255, 0.9);
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        padding: 20px;
    }

    h1 {
        color: #007acc;
        font-size: 2.5rem;
        margin-bottom: 10px;
    }

    p {
        color: #444;
        font-size: 1.2rem;
    }

    .buttons-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(50px, 1fr));
        gap: 5px;
        margin-top: 20px;
    }

    button {
        margin: 5px;
        padding: 10px;
        font-size: 1rem;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: 0.3s;
        background-color: #007acc;
        color: white;
    }

    button:hover {
        background-color: #005f99;
    }

    button[disabled] {
        background-color: #cccccc;
        cursor: not-allowed;
    }

    .output {
        font-size: 1.5rem;
        color: #333;
        margin-top: 20px;
        font-weight: bold;
    }
</style>

<div class="container">
    <h1>Guessing Game</h1>
    <p>Errate die Geheimzahl zwischen {min} und {max}:</p>

    <div class="buttons-grid">
        {#each buttons as button}
            <button
                onclick={() => guess(button.value)}
                disabled={button.disabled || !deaktiviert}
            >
                {button.value}
            </button>
        {/each}
    </div>

    <p class="output">{output}</p>

    <button onclick={initGame} style="margin-top: 20px;">Spiel neu starten</button>
</div>