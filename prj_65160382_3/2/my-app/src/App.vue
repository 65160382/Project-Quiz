<template>
  <div>
    <div class="start-screen" v-if="showStartScreen">
      <h1>Quiz Game</h1>
      <button id="start-button" @click="startQuiz">Start</button>
    </div>
    <div v-else-if="!showScore">
      <div id="container">
        <div class="header">
          <span class="number-of-question">{{ questionCount }} of {{ quizArray.length }} questions</span>
          <div class="time-left">{{ timeLeft }}</div>
        </div>
        <div class="container-mid">
          <p class="question">{{ currentQuestion.question }}</p>
          <button
            v-for="(option, index) in currentQuestion.options"
            :key="index"
            class="option-div"
            @click="checker(option)"
          >
            {{ option }}
          </button>
        </div>
        <button id="next-button" @click="displayNext">Next</button>
      </div>
    </div>
    <div class="score-container" v-else>
      <p>Your score is {{ scoreCount }} out of {{ quizArray.length }}</p>
      <button id="restart" @click="restartQuiz">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeLeft: 0,
      quizArray: [
        {
          id: "1",
          question: "หมูอะไรกินไม่อิ่ม?",
          options: ["หมูน้อย", "หมูหาย", "หมูหมดเอาไก่แทนได้ไหม", "หมูทอด"],
          correct: "หมูน้อย",
        },
        {
          id: "2",
          question: "ผึ้งอะไรเจ็บที่สุด?",
          options: [
            "ผึ้งต่อย",
            "ผึ้งรถล้ม",
            "ผึ้งรู้ว่าเธอไม่รัก",
            "ผึ้งรู้ตัวว่าติด F",
          ],
          correct: "ผึ้งรู้ว่าเธอไม่รัก",
        },
        {
          id: "3",
          question: "นกอะไรเอ่ยปากอยู่ใต้เท้า ปีกก็อยู่ใต้เท้า?",
          options: ["นกบินไม่ได้", "นกแก้ว", "นกนางนวล", "นกถูกเหยียบ"],
          correct: "นกถูกเหยียบ",
        },
        {
          id: "4",
          question: "ปลาอะไร ขี้เกียจ?",
          options: ["ปลาวาน", "ปลาตาย", "ปลาโลมา", "ปลาฉลาม"],
          correct: "ปลาวาน",
        },
        {
          id: "5",
          question: "หมาอะไรทำไมผอมโซ?",
          options: ["หมาไม่กิน", "หมาหมักน้ำมันหอย", "หมาวัด", "หมาอิ่ม"],
          correct: "หมาไม่กิน",
        },
      ],
      questionCount: 0,
      scoreCount: 0,
      countdown: null,
      showStartScreen: true,
      showScore: false,
    };
  },
  displayNext() {
    this.questionCount += 1;
    if (this.questionCount === this.quizArray.length) {
      this.showScore = true;
      return;
    }
    this.quizDisplay(this.questionCount); // เรียกใช้ quizDisplay() เพื่อแสดงคำถามถัดไป
    this.timerDisplay();
    
  },
  computed: {
    currentQuestion() {
      return this.quizArray[this.questionCount];
    },
  },
  methods: {
    startQuiz() {
      this.showStartScreen = false;
      this.initial();
    },
    restartQuiz() {
      this.initial();
      this.showScore = false;
      this.showStartScreen = false;
      this.showQuiz = true;
      this.changeBackgroundColor();
    },

    displayNext() {
      this.questionCount += 1;
      if (this.questionCount === this.quizArray.length) {
        this.showScore = true;
        return;
      }
      this.timerDisplay();
      this.changeBackgroundColor();
    },
    timerDisplay() {
      this.timeLeft = 11;
      clearInterval(this.countdown);
      this.countdown = setInterval(() => {
        this.timeLeft--;
        if (this.timeLeft === 0) {
          clearInterval(this.countdown);
          this.displayNext();
        }
      }, 1000);
    },
    checker(userOption) {
      if (userOption === this.currentQuestion.correct) {
        this.scoreCount++;
      }
      this.displayNext();
    },
    isCorrect(option) {
      return option === this.currentQuestion.correct;
    },
    initial() {
      this.questionCount = 0;
      this.scoreCount = 0;
      this.displayNext();
    },
    changeBackgroundColor() {
      var randomColor = this.getRandomColor(); // สร้างสีสุ่ม
      document.body.style.backgroundColor = randomColor; // กำหนดสีพื้นหลัง
    },
    getRandomColor() {
      var letters = "0123456789ABCDEF"; // สร้างตัวอักษรที่ใช้ในการสร้างสี
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)]; // เลือกสุ่มตัวอักษร
      }
      return color;
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}
body {
  background-color: #bc7fcd;
  transition: background-color 0.5s ease;
}
h1 {
  color: white;
}
.start-screen,
.score-container {
  color: white;
  font-size: 18px;
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
button {
  margin-top: 20px;
  border: none;
  outline: none;
  cursor: pointer;
}
#start-button,
#restart {
  font-size: 1.3em;
  padding: 0.5em 1.8em;
  border-radius: 0.2em;
}
#restart {
  margin-top: 0.9em;
}
#container {
  background-color: #ffffff;
  padding: 3.1em 1.8em;
  width: 80%;
  max-width: 37.5em;
  margin: 0 auto;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  border-radius: 0.6em;
}
.header {
  margin-bottom: 1.8em;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 0.6em;
  border-bottom: 0.1em solid #c0bfd2;
}
.timer-left {
  background-color: #e1f5fe;
  width: 7.5em;
  border-radius: 1.8em;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.7em 1.8em;
}
.timer-div img {
  width: 25px;
  height: 25px;
  margin-left: -5px;
}
.question {
  margin-bottom: 1.25em;
  font-weight: 600;
}
.option-div {
  font-size: 0.9em;
  width: 100%;
  padding: 1em;
  margin: 0.3em 0;
  text-align: left;
  outline: none;
  background: transparent;
  border: 1px solid #c0bfd2;
  border-radius: 0.3em;
}
.option-div:disabled {
  color: #000000;
  cursor: not-allowed;
}
#next-button {
  font-size: 1em;
  margin-top: 1.5em;
  background-color: #86469c;
  color: #ffffff;
  padding: 0.7em 1.8em;
  border-radius: 0.3em;
  float: right;
}
.hide {
  display: none;
}
.incorrect {
  background-color: #ffdde0;
  color: #d32f2f;
  border-color: #d32f2f;
}
.correct {
  background-color: #e7f6d5;
  color: #689f38;
  border-color: #689f38;
}
#user-score {
  font-size: 1.5em;
  color: #ffffff;
}
</style>


