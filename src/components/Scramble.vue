<template>
  <div class="scramble">
    <div class="body" v-if="!close">    
      <h1>Welcome to Scrabble words Game!</h1>
      <div class="scrambled_word">
        <h2>The Srambled workd:</h2>
        <!-- <input type="text" v-model="scrambled_word"> -->
        <h3>{{scrambled_word}}</h3>
        <button @click="pass">Pass</button>
      </div>
      <div class="guessing_word">
        <input type="text" v-model="guess_word">
        <button @click="check">Submit</button>
        <h2 v-if="correct">correct</h2>
        <h2 v-if="wrong">wrong</h2>
      </div>
      <div class="points">
        <h3>Points:</h3>
        <h3>{{point}}</h3>
      </div>
      <Modal
        v-show="isModalVisible"
        @close="closeModal"
        @restart="restart"
        @end="end"
      />
    </div>
    <div class="over" v-if="close">
      <h1>Game Over</h1>
    </div>
  </div>
</template>

<script>
import Modal from './Modal.vue'
export default {
  name: 'Scramble',
  components: {
    Modal
  },
  data() {
    return{
      words:["penguin","magazine","gun","paper","picture","friend","work","money","call","ball","family"
      ],
      scrambled_word:"",
      guess_word:"",
      random_word:"",
      point:0,
      number:2,
      isModalVisible: false,
      correct:false,
      wrong:false,
      close:false
      }
  },
  created(){
    this.scrambled();
  },
  methods:{
    shuffle: function (word){
        const copy = [...word]

          const length = copy.length
          for (let i = 0; i < length; i++) {
            const x = copy[i]
            const y = Math.floor(Math.random() * length)
            const z = copy[y]
            copy[i] = z
            copy[y] = x
          }
          if (typeof word === 'string') {
            return copy.join('')
          }
          return copy
      },
      scrambled(){
        this.random_word =this.words[Math.floor(Math.random() * this.words.length)];
        this.scrambled_word = this.shuffle(this.random_word);
        // this.number=this.words.length;
        console.log(this.number);
      },
      pass(){
        this.scrambled();
      },
      restart(){
        this.$router.go();
      },
      check(){
        console.log(this.guess_word);
        console.log(this.random_word);
      if (this.guess_word == this.random_word)
      {
        // alert('You\'re right!');
        this.point++;
        console.log(this.point);
        this.guess_word="";
        this.scrambled();
        this.number--;
        if(this.number==0){
          this.showModal();
          localStorage.name=this.point;
        }
        this.correct=true;
        this.wrong=false;
      }
      else {
        // alert('Oops You\'re Wrong!');
        this.point--;
        this.guess_word="";
        this.wrong=true;
        this.correct=false;
      }
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    end(){
      this.close=true;
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .scrambled_word,.points{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .scrambled_word h2{
    margin-right: 10px;
  }
  .scrambled_word h3{
    color: green;
  }
  button{
    margin: 10px;
  }
  .guessing_word h2{
    color: red;
  }
</style>
