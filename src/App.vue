<template>
 <div class="container-login">
    <div class="wrap-login">
    <div class="form-login">
        <span class="title-login title-bulk">VUE.JS EXADS TEST</span>
        <div class="wrap-input">
            <input class="input" type="text" placeholder="Enter String" v-model="objString" @keydown="clearErrorMessage">
            <span class="error-message" v-if="nullStringError">{{nullStringError}}</span>
        </div>

        <div class="container-btn">
            <button class="login-btn" @click="normalize()">
            Normalize String
            </button>
        </div>
        <label class="input" v-if="firstAnswer"><span class="answer-title">First Answer: <br></span>{{firstAnswer}}</label>
        <br>
        <label class="input" v-if="secondAnswer">
          <span class="answer-title">Second Answer: <br></span>
          <span v-for="string in secondAnswer">
            {{string}}
            <br>
          </span>
        </label>
        <br>
        <label class="input" v-if="thirdAnswer" style="margin-top:12rem"><span class="answer-title">Third Answer: <br></span>{{thirdAnswer}}</label>
        <!-- I dont hace time to fix the design so i hardcode this style (style="margin-top:12rem")-->
        <br> 
        <label class="input" v-if="fourthAnswer">
          <span class="answer-title">Fourth Answer: <br></span>
          <span v-for="string in fourthAnswer">
            {{string}}
            <br>
          </span>
        </label>
    </div>
    </div>
</div>
</template>


<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'App',
  data(){
    return {
      objString: '',
      nullStringError: '',
      firstAnswer: '',
      secondAnswer: '',
      thirdAnswer: '',
      thirdAnswer: '',
      fourthAnswer: ''
    }
  },
  methods: {
    clearErrorMessage(){
      this.nullStringError = ""
    },
    normalize(){
      if(!this.objString){
        this.nullStringError = "The string cannot be null"
        return;
      }

      //First Answer: I remove all remove upper case characters, punctuation, spaces, 
      var punctRE = /[\u2000-\u206F\u2E00-\u2E7F\\'!"#$%&()*+,\-.\/:;<=>?@\[\]^_`{|}~]/g;
      var spaceRE = /\s+/g;
      this.firstAnswer = this.objString.replace(punctRE, '').replace(spaceRE, '').toLowerCase();

      this.firstAnswer = this.firstAnswer.substr(0,63) //Max 64 characters long

      //Second Answer: I call to an external function to keep this one clean. 
      this.secondAnswer = this.rectangle(this.firstAnswer)


      //Third Answer: We have to make our string linear again (sort of backwards this time)
      this.thirdAnswer = this.linear(this.secondAnswer)

      //Fourth Answer: 
      this.fourthAnswer = this.chunk(this.thirdAnswer)
      //In order to be able to visually decode the text by stacking te chunks i would have to redefine the rectangle(s) method ...
      //changing the way it iterates the string and having another criterion to split it

    },
    rectangle(string){
      let i: number =0
      let squareString:string = "";
      const x = Math.sqrt(string.length) //I get the square and take the integer
      const integer = Math.round(x);
      for(i=0; i<= string.length; i+=integer){
        squareString = `${squareString}${string.slice(i, i+integer)}\n`; //
      }
      let stringArray: string = squareString.split('\n') //I Get an array of string for my v-for
      return stringArray;
    },
    linear(string){
      //I use Object Values to create an array of string
      let str: string = Object.values(string)
      let i,j: number = 0 //I define variables to use matrix iteration
      let output: string = ''
      //rows and columns of my rectangle to iterate
      let rows: number = string[0].length
      let columns: number = string.length - 1

      for(i=0;i<rows;i++){
        for(j=0;j<columns;j++){
          if(string[j][i])output += string[j][i]
        }
      }
      return output
    },
    chunk(string){
      const x: number = Math.sqrt(string.length) 
      const int: number = Math.round(x)
      return this.rectangle(string)
    }
  }
})
</script>


<style scoped>

@font-face {
  font-family: anononymous;
  src: url(../../public/fonts/Anonymous.ttf);
}

.container-login{
    width: 100%;
    min-height: 100vh;
    display:flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    padding: 0.5rem;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
     font-family: anononymous;
}

.wrap-login{
    width:50rem;
    background: #fff;
    border-radius:0.3rem;
    position:relative;
    padding-bottom: 3rem;
    padding-top:1.5rem;
}

.form-login{
    width:100%;
    justify-content: space-between;
    display:flex;
    flex-direction: column;
}

.title-login{
    font-size: 4rem;
    color: #403866;
    line-height: 1.2;
    text-transform: uppercase;
    text-align: center;
    width: 100%;
    display: block;
}

.title-bulk{
    padding-bottom: 1.7rem;
}

.wrap-input{
    width: 100%;
    background-color: #e6e6e6;
    border: 1px solid transparent;
    border-radius: 10px;
    margin-bottom: 0.6rem;
    display:flex;
    flex-direction:column;

}

.input{
    color: #403866;
    line-height: 1.2;
    font-size: 1.5rem;
    text-align:center;
    display: block;
    width: 100%;
    background: 0 0;
    height: 62px;
    padding: 0 20px 0 38px;
    border: none;
    outline:none
}

.second-input{
    color: #403866;
    line-height: 1.2;
    font-size: 1.5rem;
    display: block;
    width: 100%;
    background: 0 0;
    height: 62px;
    padding: 0 20px 0 38px;
    border: none;
    outline:none
}


.container-btn{
    margin-top:0.5rem;
    margin-bottom:1rem;
}

.login-btn{
    font-size: 2rem;
    color: #fff;
    line-height: 1.2;
    text-transform: uppercase;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0 20px;
    width: 100%;
    height: 62px;
    background-color: #827FFE;
    border-radius: 10px;
    transition: all .4s;
    outline: none!important;
    border: none;
    cursor: pointer
}

.login-btn:hover{
    background-color: #3c138f
}
.error-message{
    color: red;
    text-align: center;
}

.answer-title{
  text-decoration: underline;
}

</style>
