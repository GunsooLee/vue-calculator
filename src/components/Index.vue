<template>
        <div>
             <div class="calculator">
                <div class="display">{{result || '0'}}</div>
                <div @click="clear" class="btn etc">AC</div>
                <div @click="sign" class="btn etc">+/-</div>
                <div @click="del" class="btn etc">DEL</div>
                <div @click="oper('/')" class="btn operator">/</div>
                <div @click="append('7')" class="btn">7</div>
                <div @click="append('8')" class="btn">8</div>
                <div @click="append('9')" class="btn">9</div>
                <div @click="oper('x')" class="btn operator">x</div>
                <div @click="append('4')" class="btn">4</div>
                <div @click="append('5')" class="btn">5</div>
                <div @click="append('6')" class="btn">6</div>
                <div @click="oper('-')" class="btn operator">-</div>
                <div @click="append('1')" class="btn">1</div>
                <div @click="append('2')" class="btn">2</div>
                <div @click="append('3')" class="btn">3</div>
                <div @click="oper('+')" class="btn operator">+</div>
                <div @click="append('0')" class="btn zero">0</div>
                <div @click="dot" class="btn">.</div>
                <div @click="equal" class="btn operator">=</div>
            </div>
        </div>
</template>

<script>
export default {
    name: 'Index',
    data() {
        return {
            result: '0',
        }
    },
    methods: {
        clear() {
            this.result = '0'; 
        },
        sign() {
            this.result = this.result.charAt(0) === '-' ? 
            this.result.slice(1) : `-${this.result}`;
        },
        del() {
            if(this.result.charAt(this.result.length-1) === ' '){
                this.result = this.result.slice(0,-3);
            } else {
                this.result = this.result.slice(0,-1); 
            } 
        },
        append(number) {
            if(this.result === '0'){
                this.result = number;
            } else if (this.result === '-0'){
                this.result = `-${number}`;
            } else {
                this.result = `${this.result}${number}`;
            }
        },
        oper(op){
            if(this.result.charAt(this.result.length-1) === '.'){
                this.result = this.result.slice(0,-1); 
                this.result = `${this.result} ${op} `;
            } else if('0'<=this.result.charAt(this.result.length-1) && this.result.charAt(this.result.length-1)<='9'){
                this.result = `${this.result} ${op} `;
            } else {
                this.result = this.result.slice(0,-3);
                this.result = `${this.result} ${op} `;
            }
        },
        dot() {
            if(this.result.lastIndexOf('.') <= this.result.lastIndexOf('x') ||
            this.result.lastIndexOf('.') <= this.result.lastIndexOf('/') ||
            this.result.lastIndexOf('.') <= this.result.lastIndexOf('+') ||
            this.result.lastIndexOf('.') <= this.result.lastIndexOf('-')
            ){
                if (this.result.length == 0 || this.result.charAt(this.result.length-1) === ' ') {
                    this.append('0.');
                } else if (this.result.charAt(this.result.length-1) !== '.') {
                    console.log("Vvv");
                    this.result = `${this.result}.`;
                }
            }
        },
        calc(_op, _b, _a){
            var result = 0;
            if(_op == "x"){
                result = parseFloat(_a) * parseFloat(_b);
            } else if (_op == "/"){
                result = parseFloat(_a) / parseFloat(_b);
            } else if (_op == "+"){
                result = parseFloat(_a) + parseFloat(_b);
            } else if (_op == "-"){
                result = parseFloat(_a) - parseFloat(_b);
            } 
            return result;
        },
        equal() {
            if(this.result.length == 1 && this.result.charAt(this.result.length-1) === '-'){
                this.result = '0';
            }
            if(this.result.charAt(this.result.length-1) === ' '){
                this.result = this.result.slice(0,-3);
            }
            var _op = [];
            var _num = [];
            var inputArray = this.result.split(' ');
            var step;
            for (step = 0; step < inputArray.length; step++) {
                var tok = inputArray[step];
                if(tok == "x" || tok == "/" ){
                    _num.push(this.calc(tok, inputArray[++step], _num.pop()));
                } else if (tok == "+" || tok == "-") {
                    _op.push(tok);
                } else {
                    _num.push(tok);
                }
            }

            while(_op.length > 0){
                _num.push(this.calc(_op.pop(), _num.pop(), _num.pop()));
            }

            this.result = `${_num.pop()}`;

            if(this.result === "NaN"){
                this.result = "0";
            } else if(this.result === "Infinity"){
                this.result = "0";
            }
        },
    }
}
</script>

<style scoped>
.calculator {
    margin: 0 auto;
    width: 400px;
    font-size: 40px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
}
.display {
    grid-column: 1 / 5;
    text-align: right;
    padding: 5px;
    background-color: #333;
    color: white;
    border: 2px solid #333;
}
.zero {
  grid-column: 1 / 3;
}
.btn {
    border: 2px solid black;
    border-radius: 0;
    background-color: #7f7f7f;
    color: white;
}
 .etc {
    background-color: #d9d9d9;
    color: black;
}
.operator {
    background-color: #c45911;
}
</style>