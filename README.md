            <span class="prefix">$</span>
            <input id="bill" type="number" min="0" step="0.01" inputmode="decimal" placeholder="45.00" required>
          </span>
        </label>

        <fieldset>
          <legend>Tip</legend>
          <div class="tip-options">
            <label>
              <input type="radio" name="tip" value="10">
              <span>10%</span>
            </label>
            <label>
              <input type="radio" name="tip" value="15" checked>
              <span>15%</span>
            </label>
            <label>
              <input type="radio" name="tip" value="20">
              <span>20%</span>
            </label>
          </div>
        </fieldset>

        <label>
          People
          <input id="people" type="number" min="1" step="1" inputmode="numeric" value="2" required>
        </label>

        <div class="result" aria-live="polite">
          <span>Each person should pay</span>
          <output class="amount" id="amount">$0.00</output>
          <div class="details" id="details"></div>
        </div>

        <div class="message" id="message"></div>

        <div class="actions">
          <button class="primary" type="submit">Calculate</button>
          <button class="secondary" type="button" id="reset">Reset</button>
        </div>
      </form>
    </section>
  </main>

  <script>
    const form = document.querySelector("#tip-form");
    const billInput = document.querySelector("#bill");
    const peopleInput = document.querySelector("#people");
    const amountOutput = document.querySelector("#amount");
    const detailsOutput = document.querySelector("#details");
    const messageOutput = document.querySelector("#message");
    const resetButton = document.querySelector("#reset");

    function money(value) {
      return value.toLocaleString("en-US", {
        style: "currency",
        currency: "USD"
      });
    }

    function getTip() {
      return Number(document.querySelector("input[name='tip']:checked").value);
    }

    function calculate() {
      const bill = Number(billInput.value);
      const tip = getTip();
      const people = Number(peopleInput.value);

      messageOutput.textContent = "";

      if (!bill || bill <= 0) {
        amountOutput.textContent = "$0.00";
        detailsOutput.textContent = "";
        messageOutput.textContent = "Enter a bill amount greater than zero.";
        return;
      }

      if (!Number.isInteger(people) || people < 1) {
        amountOutput.textContent = "$0.00";
        detailsOutput.textContent = "";
        messageOutput.textContent = "Enter at least 1 person.";
        return;
      }

      const tipAsPercent = tip / 100;
      const totalTipAmount = bill * tipAsPercent;
      const totalBill = bill + totalTipAmount;
      const billPerPerson = totalBill / people;

      amountOutput.textContent = money(billPerPerson);
      detailsOutput.textContent = `${money(totalBill)} total with ${tip}% tip, split ${people} ways.`;
    }

    form.addEventListener("submit", function (event) {
      event.preventDefault();
      calculate();
    });

    form.addEventListener("input", calculate);

    resetButton.addEventListener("click", function () {
      form.reset();
      amountOutput.textContent = "$0.00";
      detailsOutput.textContent = "";
      messageOutput.textContent = "";
      billInput.focus();
    });
  </script>
</body>
</html>
