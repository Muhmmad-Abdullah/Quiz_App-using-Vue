<template>
  <form @submit.prevent="registerAnswer">
    <h1>Checkboxes in Vue</h1>
<header>
  Made BY Abdullah Nadeem
</header>
    <div v-if="currentForm <= quizForms.length" class="form-container">
      <h2>{{ quizForms[currentForm - 1].question }}</h2>
      <p v-for="(answer, index) in quizForms[currentForm - 1].answers" :key="index">
        <label
          :class="{
            'selected-answer': selectedAnswers.includes(answer) && showResult,
            'incorrect-answer': selectedAnswers.includes(answer) && showResult && !isAnswerCorrect(answer)
          }"
          @click="onAnswerSelect(answer)"
        >
          <input type="checkbox" :disabled="showResult || (isAnyAnswerSelected && !selectedAnswers.includes(answer))" :checked="selectedAnswers.includes(answer)">
          {{ answer }}
        </label>
      </p>
    </div>

    <div class="navigation-buttons">
      <button v-if="currentForm > 1" @click="goToPreviousForm" class="navigation-button">Previous</button>
      <button v-if="currentForm < quizForms.length" @click="goToNextForm" class="navigation-button">Next</button>
    </div>

    <div v-if="showResult" class="result-message">
      {{ isAnyAnswerSelected && isAnswerCorrect() ? 'Correct Answer' : 'Incorrect Answer' }}
    </div>

    <button class="Btn" type="submit" :disabled="!isAnyAnswerSelected">Submit</button>
  </form>
</template>
<script>
export default {
  data() {
    return {
      currentForm: 1,
      selectedAnswers: [],
      showResult: false,
      quizForms: [
       {
          question: 'What is the Capital of Pakistan?',
          answers: ['Lahore', 'Karachi', 'Islamabad', 'Punjab'],
          correctAnswer: ['Islamabad'] 
        },
        {
          question: 'What is the National game of Pakistan?',
          answers: ['Cricket', 'Squash', 'Football', 'Hockey'],
          correctAnswer: ['Hockey']
        },

        {
          question: 'What is the National  Fruit of Pakistan?',
          answers: ['Banana', 'Mango', 'Apple', 'Watermelon'],
          correctAnswer: ['Mango']
        },

        {
          question: 'What is the National Tree of Pakistan?',
          answers: ['Palm', 'Oak', 'Birch', 'DeodarTree'],
          correctAnswer: ['Deodar']
        },

        {
          question: 'What is the National Food of Pakistan?',
          answers: ['Bngpaya', 'Nihari', 'Nanchana', 'Qorma'],
          correctAnswer: ['Nihari']
        },

        {
          question: 'What is the National Poet of Pakistan?',
          answers: ['Quaid-e-Azam', 'ZAfaralikhan', 'SirsaeedAhmedkhan', 'AllamaIqbal'],
          correctAnswer: ['AllamaIqbal']
        },

        {
          question: 'What is the National Vegetable of Pakistan?',
          answers: ['Onion', 'Tomato', 'Ladyfinger', 'Garlic'],
          correctAnswer: ['Ladyfinger']
        },

        {
          question: 'What is the National Bird of Pakistan?',
          answers: ['Chakor', 'Sparrow', 'Cocktail', 'Lovebird'],
          correctAnswer: ['Chakor']
        },

        {
          question: 'What is the National Animal of Pakistan?',
          answers: ['Elephant', 'Markhor', 'Tiger', 'Lion'],
          correctAnswer: ['Markhor']
        },
        {
          question: 'What is the National Language of Pakistan?',
          answers: ['English', 'Hindi', 'Urdu', 'Bangali'],
          correctAnswer: ['Urdu']
        },
      ],
      totalScore: 0,
      showScore: false,
    };
  },
  computed: {
    isAnyAnswerSelected() {
      return this.selectedAnswers.length > 0;
    },
  },
  methods: {
    registerAnswer() {
      this.showResult = true;
      this.calculateScore();
    },
    calculateScore() {
      const currentForm = this.quizForms[this.currentForm - 1];
      const correctAnswers = currentForm.correctAnswer;
      const userAnswers = this.selectedAnswers;

      let score = 0;
      correctAnswers.forEach((answer) => {
        if (userAnswers.includes(answer)) {
          score++;
        }
      });

      this.totalScore += score;
      this.showScore = true;
    },
    restartQuiz() {
      this.currentForm = 1;
      this.selectedAnswers = [];
      this.showResult = false;
      this.totalScore = 0;
      this.showScore = false;
    },
    goToPreviousForm() {
      if (this.currentForm > 1) {
        this.currentForm--;
        this.selectedAnswers = [];
        this.showResult = false;
      }
    },
    goToNextForm() {
      if (this.currentForm < this.quizForms.length) {
        this.currentForm++;
        this.selectedAnswers = [];
        this.showResult = false;
      }
    },
    onAnswerSelect(answer) {
      if (this.showResult) {
        // Reset the result message when a new answer is selected
        this.showResult = false;
      }
      if (this.selectedAnswers.includes(answer)) {
        // Deselect the answer if it was already selected
        this.selectedAnswers = this.selectedAnswers.filter(selected => selected !== answer);
      } else {
        // Add the answer to the selected answers
        this.selectedAnswers.push(answer);
      }
    },
    isAnswerCorrect() {
      const correctAnswers = this.quizForms[this.currentForm - 1].correctAnswer;
      return (
        this.selectedAnswers.length === correctAnswers.length &&
        this.selectedAnswers.every(answer => correctAnswers.includes(answer))
      );
    },
    getTotalPossibleScore() {
      const currentForm = this.quizForms[this.currentForm - 1];
      return currentForm.correctAnswer.length;
    },
  },
};

</script>

<style>
/* Custom CSS Styles */

body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  background-color: #f0f0f0;
}

form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

h2 {
  margin-bottom: 10px;
}

.form-container {
  margin-bottom: 30px;
}

label {
  display: block;
  cursor: pointer;
  margin-bottom: 5px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

label:hover {
  background-color: #f9f9f9;
}

input[type="checkbox"] {
  margin-right: 10px;
}

.selected-answer {
  background-color: #d6f5d6;
  border-color: #5cb85c;
}

.incorrect-answer {
  background-color: #f5d6d6;
  border-color: #d9534f;
}

.navigation-buttons {
  display: flex;
  justify-content: space-between;
}

.navigation-button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  color: #fff;
  background-color: #007bff;
  cursor: pointer;
}

.result-message {
  text-align: center;
  margin-bottom: 20px;
  font-weight: bold;
}

.Btn {
  display: block;
  margin: 0 auto;
  padding: 10px 30px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
}

.Btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
