<script lang="ts">
    const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
    const operations = ["/", "*", "-", "+", "="];

    let firstNumber = "";
    let secondNumber = "";
    let selectedOperation = "";
    let display = "";
    let displayingResult = false;
    const handleClear = () => {
        firstNumber = "";
        secondNumber = "";
        selectedOperation = "";
        display = "";
        displayingResult = false;
    };
    const handleOperationClick = (operation: string) => {
        if(displayingResult) handleClear();
        if (operation === "=") {
            console.log(firstNumber, secondNumber);
            if (!firstNumber) return;
            if (firstNumber && !secondNumber) {
                selectedOperation = "";
                return (display = firstNumber);
            }
            const f = parseInt(firstNumber);
            const s = parseInt(secondNumber);
            displayingResult = true;
            switch (selectedOperation) {
                case "+":
                    display = (f + s).toString();
                    break;
                case "-":
                    display = (f - s).toString();
                    break;
                case "*":
                    display = (f * s).toString();
                    break;
                case "/":
                    display = (f / s).toString();
                    break;
            }
        }
        selectedOperation = operation;
        if (firstNumber && !secondNumber) {
            display = "";
        }
    };
    const handleNumberClick = (number: string) => {
        if(displayingResult) handleClear();
        if (number === "0" && display === "") return;
        if (number === "." && display.includes(".")) return;

        if (selectedOperation === "") {
            if (display === "" && number === ".") {
                firstNumber = "0.";
                return (display = firstNumber);
            }
            firstNumber += number;
            return (display = firstNumber);
        } else {
            if (display === "" && number === ".") {
                secondNumber = "0.";
                return (display = secondNumber);
            }
            secondNumber += number;
            return (display = secondNumber);
        }
    };
</script>

<main>
    <div class="calculator">
        <div class="results">
            {display}
        </div>
        <div class="digits">
            <div class="numbers">
                <button class="btn btn-xlg" on:click={()=> handleClear()}>C</button>
                {#each numbers as number (number)}
                    <button
                        class={`btn ${number === "0" ? "btn-lg" : ""}`}
                        on:click={() => handleNumberClick(number)}
                        >{number}</button
                    >
                {/each}
            </div>
            <div class="operations">
                {#each operations as operation (operation)}
                    <button
                        class={`btn btn-orange ${
                            selectedOperation === operation ? "btn-silver" : ""
                        }`}
                        on:click={() => handleOperationClick(operation)}
                        >{operation}</button
                    >
                {/each}
            </div>
        </div>
    </div>
</main>

<style>
    main {
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .calculator {
        background-color: rgb(28, 28, 28);
        width: 240px;
        padding: 15px;
        border-radius: 7px;
    }
    .digits {
        display: flex;
    }
    .operations {
        display: flex;
        flex-direction: column;
    }
    .numbers {
        display: flex;
        flex-wrap: wrap;
        width: 200px;
    }
    .btn {
        width: 50px;
        height: 50px;
        border-radius: 100px;
        background-color: rgb(114, 113, 113);
        font-size: 20px;
        font-weight: bold;
        color: white;
        margin: 5px;
        border: none;
    }
    .btn-lg {
        width: 110px;
    }
    .btn-orange {
        background-color: orange;
    }
    .btn-silver {
        background-color: silver;
    }
    .btn-xlg {
        width: 170px;
    }
    .results {
        height: 60px;
        color: white;
        font-size: 50px;
        display: flex;
        flex-direction: row-reverse;
        margin-right: 10px;
    }
</style>
