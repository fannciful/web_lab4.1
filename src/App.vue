<template>
  <div id="hero">
    <div class="navbar">
      <div class="logo">Kolizey.</div>
      <div class="user">
        <button type="button" @click="redirectToForm">Sign in</button>
        <input id="menu-toggle" type="checkbox" />
        <label class="menu-button-container" for="menu-toggle">
          <div class="menu-button"></div>
        </label>
        <ul class="menu">
          <li><a href="#">ABOUT</a></li>
          <li><a href="#">OFFERS</a></li>
          <li><a href="#">TRAINERS</a></li>
          <li><a href="#">PRICE</a></li>
          <li><a href="#">CONTACTS</a></li>
        </ul>
      </div>
    </div>
    <h1>Опитування</h1>
    <form @submit.prevent="submitForm">
      <label for="question1">Оберіть локацію:</label><br />
      <select v-model="formData.question1" id="question1" name="question1">
        <option value="PanasaMyrnogo">вул. Панаса Мирного,24</option>
        <option value="Shevchenka">вул. Шевченка,21</option>
        <option value="Franka">вул. Івана Франка,4</option>
        <option value="Zelena">вул. Зелена,55</option>
        <option value="Sykhivska">вул. Сихівська,8</option>
      </select><br /><br />

      <label for="question2">Виберіть вид тренування:</label>
      <select v-model="formData.question2" id="question2" name="question2">
        <option value="Yoga">Yoga</option>
        <option value="Spain">Spain Class</option>
        <option value="Hiit">HIIT</option>
        <option value="Pilates">Pilates</option>
        <option value="Pump">Body Pump</option>
        <option value="Cardio">Cardio Kickboxing</option>
        <option value="Intensive">Intensive Training</option>
        <option value="Zumba">Zumba</option>
        <option value="Boot">Boot Camp</option>
      </select><br /><br />

      <label for="question3">Оберіть тип залу:</label><br />
      <input type="radio" v-model="formData.question3" id="question3-type1" name="question3" value="Cardio Zone" required />Кардіо-зона
      <input type="radio" v-model="formData.question3" id="question3-type2" name="question3" value="Force Zone" />Силова зона
      <input type="radio" v-model="formData.question3" id="question3-type3" name="question3" value="Functional Zone" />Функціональна зона<br /><br />

      <label for="question4">Ваш рівень фізичної підготовки:</label><br />
      <input type="radio" v-model="formData.question4" id="question4-low" name="question4" value="low" required />Слабкий
      <input type="radio" v-model="formData.question4" id="question4-medium" name="question4" value="medium" />Середній
      <input type="radio" v-model="formData.question4" id="question4-high" name="question4" value="high" />Високий<br /><br />

      <label for="question5">Найбільш зручний час для відвідування тренувань:</label><br />
      <input type="datetime-local" v-model="formData.question5" id="question5" name="eventDateTime" /><br /><br />

      <label for="question6">Вкажіть оптимальну для вас кількість тренувань в тиждень</label><br />
      <input type="number" v-model="formData.question6" id="question6" name="averageAmount" min="0" max="7" /><br /><br />

      <label for="question7">Ваша стать:</label><br />
      <input type="radio" v-model="formData.question7" id="question7-male" name="question7" value="male" required />Чоловіча
      <input type="radio" v-model="formData.question7" id="question7-female" name="question7" value="female" />Жіноча<br /><br />

      <label for="question8">Ваш вік:</label><br />
      <input type="number" v-model="formData.question8" id="question8" name="age" min="10" max="99" /><br /><br />

      <label for="question9">Як ви оцінюєте чистоту в залі?</label><br />
      <input type="range" v-model="formData.question9" id="question9" name="clearness" min="1" max="10" value="5" /><br /><br />

      <label for="question10">З якою метою ви відвідуєте зал?</label><br />
      <textarea v-model="formData.question10" id="question10" name="reason" rows="4" cols="50"></textarea><br /><br />

      <label for="question11">Залиште ваш відгук:</label><br />
      <textarea v-model="formData.question11" id="question11" name="feedback" rows="4" cols="50"></textarea><br /><br />

      <div class="button-container">
        <input type="submit" value="Відправити" />
        <button type="button" @click="displayResults">Показати результати</button>
      </div>
    </form>
    <ul>
      <li v-for="(value, key) in criteria" :key="key">{{ key }}: {{ value }}</li>
      <li>Задоволені чистотою залу: {{ criteria.satisfiedCleanliness }} людей</li>
      <li>Не задоволені чистотою залу: {{ criteria.notSatisfiedCleanliness }} людей</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        question1: '',
        question2: '',
        question3: '',
        question4: '',
        question5: '',
        question6: null,
        question7: '',
        question8: null,
        question9: 5,
        question10: '',
        question11: ''
      },
      surveys: JSON.parse(localStorage.getItem('surveys1')) || [],
      criteria: {
        yogaTraining: 0,
        cardyoType: 0,
        mediumPhysical: 0,
        averageAmount: 0,
        femaleHuman: 0,
        satisfiedCleanliness: 0,
        notSatisfiedCleanliness: 0
      }
    };
  },
  methods: {
    redirectToForm() {
      window.location.href = 'form.html';
    },
    displayResults() {
      this.criteria = {
        yogaTraining: 0,
        cardyoType: 0,
        mediumPhysical: 0,
        averageAmount: 0,
        femaleHuman: 0,
        satisfiedCleanliness: 0,
        notSatisfiedCleanliness: 0
      };

      this.surveys.forEach((survey) => {
        const { question2, question3, question4, question6, question7, question9 } = survey;
        if (question2 === 'Yoga') {
          this.criteria.yogaTraining++;
        }
        if (question3 === 'Cardio Zone') {
          this.criteria.cardyoType++;
        }
        if (question4 === 'medium') {
          this.criteria.mediumPhysical++;
        }
        if (question6 >= 3 && question6 <= 5) {
          this.criteria.averageAmount++;
        }
        if (question7 === 'female') {
          this.criteria.femaleHuman++;
        }
        if (question9 >= 8 && question9 <= 10) {
          this.criteria.satisfiedCleanliness++;
        }
        if (question9 >= 1 && question9 <= 4) {
          this.criteria.notSatisfiedCleanliness++;
        }
      });
    },
    submitForm() {
      this.surveys.push({ ...this.formData });
      localStorage.setItem('surveys1', JSON.stringify(this.surveys));
      this.formData = {
        question1: '',
        question2: '',
        question3: '',
        question4: '',
        question5: '',
        question6: null,
        question7: '',
        question8: null,
        question9: 5,
        question10: '',
        question11: ''
      };
    }
  }
};
</script>

<style>
@import url('/public/form.css'); 
</style>
