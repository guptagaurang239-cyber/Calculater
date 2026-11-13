
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #1e1e2f, #3a3a5f);
        }

        .calculator {
            width: 320px;
            background: #0f0e0e;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(141, 108, 108, 0.4);
        }

        .display {
            width: 100%;
            height: 70px;
            margin-bottom: 20px;
            border: none;
            outline: none;
            border-radius: 10px;
            background: #111;
            color: white;
            text-align: right;
            padding: 15px;
            font-size: 32px;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            height: 65px;
            border: none;
            border-radius: 12px;
            font-size: 22px;
            cursor: pointer;
            transition: 0.2s;
        }

        button:hover {
            transform: scale(1.05);
        }

        .number {
            background: #444;
            color: white;
        }

        .operator {
            background: #ff9500;
            color: white;
        }

        .clear {
            background: #ff3b30;
            color: white;
        }

        .delete {
            background: #007aff;
            color: white;
        }

        .equal {
            background: #34c759;
            color: white;
            grid-column: span 2;
        }
    </style>
</head>

<body>

    <div class="calculator">

        <input type="text" class="display" id="display" readonly>

        <div class="buttons">

            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="delete" onclick="deleteValue()">DEL</button>
            <button class="operator" onclick="appendValue('/')">÷</button>
            <button class="operator" onclick="appendValue('*')">×</button>

            <button class="number" onclick="appendValue('7')">7</button>
            <button class="number" onclick="appendValue('8')">8</button>
            <button class="number" onclick="appendValue('9')">9</button>
            <button class="operator" onclick="appendValue('-')">−</button>

            <button class="number" onclick="appendValue('4')">4</button>
            <button class="number" onclick="appendValue('5')">5</button>
            <button class="number" onclick="appendValue('6')">6</button>
            <button class="operator" onclick="appendValue('+')">+</button>

            <button class="number" onclick="appendValue('1')">1</button>
            <button class="number" onclick="appendValue('2')">2</button>
            <button class="number" onclick="appendValue('3')">3</button>
            <button class="operator" onclick="appendValue('%')">%</button>

            <button class="number" onclick="appendValue('0')">0</button>
            <button class="number" onclick="appendValue('00')">00</button>
            <button class="number" onclick="appendValue('.')">.</button>
            <button class="operator" onclick="appendValue('**')">^</button>

            <button class="operator" onclick="appendValue('(')">(</button>
            <button class="operator" onclick="appendValue(')')">)</button>
            <button class="equal" onclick="calculate()">=</button>

        </div>
    </div>

    <script>

        let display = document.getElementById("display");
        let calculated = false;

        // Add value
        function appendValue(value) {

            if (display.value === "Error") {
                display.value = "";
            }

            // Start fresh after calculation when entering a number
            if (calculated && /^[0-9.]$/.test(value)) {
                display.value = "";
            }

            // Continue with operator after calculation
            calculated = false;

            display.value += value;
        }

        // Clear all
        function clearDisplay() {
            display.value = "";
            calculated = false;
        }

        // Delete last character
        function deleteValue() {
            display.value = display.value.slice(0, -1);
            calculated = false;
        }

        // Calculate result
        function calculate() {

            try {

                let expression = display.value;

                if (expression === "") {
                    return;
                }

                // Allow only calculator characters
                if (!/^[0-9+\-*/%.() ]+$/.test(expression)) {
                    throw new Error("Invalid input");
                }

                // Calculate expression
                let result = Function(
                    '"use strict"; return (' + expression + ')'
                )();

                // Check invalid result
                if (!Number.isFinite(result)) {
                    throw new Error("Invalid calculation");
                }

                // Remove floating point precision errors
                result = Number(result.toPrecision(12));

                display.value = result;
                calculated = true;

            } catch (error) {

                display.value = "Error";
                calculated = false;

            }
        }

        // Keyboard support
        document.addEventListener("keydown", function(event) {

            let key = event.key;

            if (key >= "0" && key <= "9") {
                appendValue(key);
            }

            else if (["+", "-", "*", "/", "%", "(", ")"].includes(key)) {
                appendValue(key);
            }

            else if (key === ".") {
                appendValue(".");
            }

            else if (key === "^") {
                appendValue("**");
            }

            else if (key === "Enter" || key === "=") {
                calculate();
            }

            else if (key === "Backspace") {
                deleteValue();
            }

            else if (key === "Escape") {
                clearDisplay();
            }

        });

    </script>

</body>
</html>



