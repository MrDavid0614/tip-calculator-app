<template>
  <form class="container" @submit.prevent="onSubmit">
        <div>
            <div class="bill-container">
                <label>Bill</label>
                <div class="bill-input">
                    <img src="../assets/images/icon-dollar.svg" class="dollar-icon">
                    <input 
                        type="text"
                        placeholder="0.00"
                        v-model.number="bill">
                </div>
            </div>

            <div class="tips-container">
                <label>Select Tip %</label>
                <div class="tips-list">
                    <div 
                        v-for="(tip, index) in tipList"
                        :key="index"
                        class="tip"
                        @click="onClickTip($event.target)"
                    >
                        {{ tip }}
                    </div>
                    <div>
                        <input 
                            type="text" 
                            class="custom-tip" 
                            placeholder="Custom" 
                            v-model="customTip"
                            @keyup="onKeyUpCustom($event.target.value)">
                    </div>
                </div>
            </div>

            <div class="people">
                <label>Number of People</label>
                <img src="../assets/images/icon-person.svg" class="person-icon">
                <input 
                    type="number"
                    placeholder="0"
                    v-model.number="numberOfPeople">
            </div>
        </div>

        <div class="total-container">
            <div class="prices">
                <div class="total-amount">
                    <div class="total-amount-content">
                        <p>Tip Amount</p>
                        <p class="text-muted">/ person</p>
                    </div>
                    <div class="total-amount-price">
                        <p class="price">${{ totalTip }}</p>
                    </div>
                </div>
                <div class="total-amount">
                    <div class="total-amount-content">
                        <p>Total</p>
                        <p class="text-muted">/ person</p>
                    </div>
                    <div class="total-amount-price">
                        <p class="price">${{ total }}</p>
                    </div>
                </div>
            </div>
            <div class="buttons-container">
                <button 
                    type="submit"
                    class="btn btn-sumbit"
                    >
                    Calculate</button>

                <button 
                    type="reset" 
                    class="btn"
                    @click="onClickReset()">Reset</button>
            </div>
        </div>
    </form>
</template>

<script>
const tipList = [
                "5%",
                "10%",
                "15%",
                "25%",
                "50%"
            ]

export default {
    data() {
        return {
            bill: "",
            tip: 0,
            total: "0.00",
            totalTip: "0.00",
            numberOfPeople: "",
            tipList,
            customTip: "",
        }
    },
    methods: {
        onClickTip(tipElement) {
            if(tipElement.classList.contains("tip-active")){
                tipElement.classList.remove("tip-active")
                this.tip = 0
                this.totalTip = "0.00"
                return
            }

            this.removeSelectedTip()
            this.addTip(tipElement.textContent)

            tipElement.classList.add("tip-active")
        },
        onSubmit(){
            this.calculateTotal()
        },
        onClickReset(){
            this.bill = 0
            this.tip = 0
            this.total = "0.00"
            this.totalTip = "0.00"
            this.numberOfPeople = ""
            this.customTip = ""
        },
        onKeyUpCustom(elementValue){
            this.removeSelectedTip()
            if(!elementValue){
                this.totalTip = "0.00"
                return
            }
            this.addTip(elementValue)
        },
        addTip(tip) {
            const parsedTip = parseInt(tip)
            if(Number.isInteger(parsedTip)) {
                const tipPercentage = parsedTip / 100
                this.tip = tipPercentage
                this.calculateTotalTip()
                return
            }

            this.tip = 0
            return
        },
        removeSelectedTip(){
            document.querySelectorAll(".tip").forEach(tip => {
                if(tip.classList.contains("tip-active")) {
                    tip.classList.remove("tip-active");
                }
            });
        },
        calculateTotal(){
            if(!this.bill || !this.numberOfPeople){
                this.total = "0.00"
                this.calculateTotalTip()
                return
            }
            this.total = (+this.bill / +this.numberOfPeople || 1.0).toFixed(2)
            this.calculateTotalTip()
        },
        calculateTotalTip(){
            this.totalTip = (this.total * this.tip).toFixed(2) || 1.0
        },
        formatPrice(value) {
            let val = (value/1).toFixed(2)
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
        }
    }
 }
</script>

<style scoped>

.container {
    display: flex;
    flex-direction: column;
    gap: 20px;
    background-color: var(--white);
    border-radius: 15px;
    padding: 10px 20px 30px 20px;
}

input {
    width: 100%;
    text-align: right;
    padding: 10px;
    border: 2px solid transparent;
    background-color: var(--lightest-grayish-cyan);
    color: var(--very-dark-cyan);
    border-radius: 6px;
    font-size: var(--main-font-size);
    font-weight: bold;
    outline: none;
}

input:focus {
    border-color: var(--strong-cyan);
}

.bill-input {
    position: relative;
}

.dollar-icon {
    position: absolute;
    top: 18px;
    left: 20px;
    width: 15px;
}

.bill-container {
    width: 100%;
    margin: 10px 0;
}

.tips-container {
    margin-top: 20px;
}

.tips-container .tips-list {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
}

.tip {
    padding: 10px;
    text-align: center;
    background-color: var(--very-dark-cyan);
    border: 2px solid transparent;
    border-radius: 5px;
    color: var(--white);
    font-size: var(--main-font-size);
    transition: all 100ms ease;
    font-weight: bold;
    cursor: pointer;
}

.tip:hover {
    border-color: var(--strong-cyan);
    background-color: var(--light-grayish-cyan);
    color: var(--very-dark-cyan);
}

.tip-active {
    background-color: var(--strong-cyan);
    color: var(--very-dark-cyan);
}

.custom-tip {
    width: 100%;
    font-size: calc(var(--main-font-size) - 4px);
}

.people {
    position: relative;
    margin-top: 20px;
}

.person-icon {
    position: absolute;
    top: 40px;
    left: 20px;
    width: 20px;
}

.total-container {
    display: flex;
    flex-direction: column;
    padding: 30px;
    background-color: var(--very-dark-cyan);
    color: var(--white);
    border-radius: 6px;
}

.total-amount {
    display: flex;
    justify-content: space-between;
    color: var(--white);
    margin-bottom: 10px;
}

.text-muted {
    color: grey;
    font-size: 14px;
}

.price {
    color: var(--strong-cyan);
    font-size: calc(var(--main-font-size) + 10px);
}

.buttons-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.btn {
    padding: 10px;
    margin-bottom: 5px;
    width: 100%;
    color: var(--very-dark-cyan);
    background-color: var(--strong-cyan);
    border-radius: 5px;
    font-size: calc(var(--main-font-size) - 4px);
    transition: all 100ms ease;
    cursor: pointer;
    text-transform: uppercase;
    font-weight: bold;
    border: 1px solid var(--strong-cyan);
    outline: none;
}

.btn:hover {
    background-color: var(--light-grayish-cyan);
    color: var(--very-dark-cyan);
}

.btn:active {
    --very-dark-cyan: hsl(183, 100%, 15%);
    --strong-cyan: hsl(172, 67%, 50%);
    background-color: var(--very-dark-cyan);
    color: var(--strong-cyan);
}

@media(min-width: 770px) {
    .container {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        padding: 30px;
    }

    .prices {
        display: grid;
        gap: 20px;
    }

    .buttons-container {
        margin-top: 120px;
    }
}

@media(min-width: 960) {
    .buttons-container {
        padding: 0 50px;
    }
}
</style>