<template>
  <div class="quiz loading-block"
       :class="{'is-fetch': isFetch}"
  >

    <preloader v-if="isFetch"></preloader>

    <h2 class="quiz__title">Please answer a few questions</h2>

    <progress-component
        class="quiz__progress"
        :count="quizTotal"
        :current="showStep"
    ></progress-component>

    <div class="quiz__form">
      <h3 class="quiz__form-question-title">
        {{ currentQuiz.question }} ({{ showStep }}/{{ quizTotal }})
      </h3>

      <!-- Блок вопросов с radiobutton -->
      <div
          class="quiz__form-questions"
          v-if="currentQuiz.answer_type === 'radiobutton'"
      >
        <radio-component
            v-for="item in currentQuiz.answers.items"
            :key="item"
            name="radio"
            :value="item"
            :label="item"
            v-model="formData[showStep - 1].answer"
        ></radio-component>
      </div>

      <!-- Блок вопросов с textarea -->
      <div
          class="quiz__form-questions"
          v-if="currentQuiz.answer_type === 'textarea-with-disable'"
      >
        <textarea
            v-model="formData[showStep - 1].answer"
            :disabled="formData[showStep - 1].disable"
        ></textarea>

        <checkbox-component
            :label="currentQuiz.answers.disable_text"
            :inputValue="false"
            v-model="formData[showStep - 1].disable"
        ></checkbox-component>
      </div>

      <!-- Блок вопросов с checkbox -->
      <div
          class="quiz__form-questions"
          v-if="currentQuiz.answer_type === 'checkbox-with-my'"
      >
        <checkbox-component
            v-for="item in currentQuiz.answers.items"
            :key="item"
            :label="item"
            :inputValue="item"
            v-model="formData[showStep - 1].answer"
        ></checkbox-component>

        <checkbox-component
            :label="currentQuiz.answers.my_variant_text"
            :inputValue="false"
            v-model="formData[showStep - 1].my_variant_enable"
        ></checkbox-component>

        <textarea
            v-model="formData[showStep - 1].my_variant_answer"
            v-if="formData[showStep - 1].my_variant_enable"
        ></textarea>
      </div>

      <button
          class="btn quiz__form-btn"
          @click="onNextStep"
      >{{ showStep < quizTotal ? 'Next' : 'Submit' }}
      </button>
    </div>
  </div>
</template>

<script>
import CheckboxComponent from "../ui/CheckboxComponent.vue";
import RadioComponent from "../ui/RadioComponent.vue";
import ProgressComponent from "@/ui/ProgressComponent";
import Preloader from "@/ui/Preloader";

export default {
  components: {Preloader, ProgressComponent, RadioComponent, CheckboxComponent},

  name: "Quiz",

  data() {
    return {
      isFetch: false,
      quizStep: 0,
      showStep: 1,
      quizList: [],
      quizTotal: 0,
      formData: [],
      conditionForShowing: [],
    };
  },

  computed: {
    // Возвращает текущий вопрос
    currentQuiz() {
      let currentQuiz = this.quizList[this.quizStep];

      if (!currentQuiz) return false

      let data = {
        question: currentQuiz.question,
        answer: "",
      };

      if (currentQuiz.answer_type === "textarea-with-disable") {
        data = {
          ...data,
          disable: false,
        };
      }

      if (currentQuiz.answer_type === "checkbox-with-my") {
        data = {
          ...data,
          answer: [],
          my_variant_enable: false,
          my_variant_answer: ''
        };
      }

      this.formData.push(data);

      return currentQuiz;
    },

    // Валидация текущего ответа
    validCurrentQuestion() {
      let validCurrentQuestion = false

      let formDataLastData = this.formData[this.formData.length - 1]

      validCurrentQuestion =
          formDataLastData.answer.length > 0 ||
          formDataLastData.disable ||
          (!!formDataLastData.my_variant_answer && formDataLastData.my_variant_enable)

      return validCurrentQuestion
    }
  },

  methods: {
    // Переход к следующему шагу
    onNextStep() {
      if (this.validCurrentQuestion) {

        if (this.showStep >= this.quizTotal) { // Если последний шаг, то отправить данные
          this.onSubmit()
        }

        if ("set_condition" in this.quizList[this.quizStep]) { // Если есть условия для отображения следующих вопросов
          let answer = this.formData[this.formData.length - 1].answer;
          let pos = this.quizList[this.quizStep].answers.items.indexOf(answer);

          this.conditionForShowing.push(
              this.quizList[this.quizStep].set_condition[pos]
          );
        }

        let nextStep = this.quizStep + 1;
        let condition = nextStep < this.quizList.length;

        while (condition) { // Цикл проверки условий для отображения следующих вопросов
          if ("condition_for_showing" in this.quizList[nextStep]) {
            let inCondition = false;
            this.quizList[nextStep].condition_for_showing.forEach((item) => {
              if (this.conditionForShowing.indexOf(item) !== -1) {
                inCondition = true;
              }
            });

            if (inCondition) {
              this.quizStep = nextStep;
              condition = false;
            } else {
              nextStep = nextStep + 1;
            }
          } else {
            this.quizStep = nextStep;
            condition = false;
          }
        }

        if (nextStep < this.quizList.length) {
          this.showStep++;
        }
      } else {
        console.log("Не ответили на вопрос!");
      }
    },

    // Отправка данных
    async onSubmit() {
      const data = {
        method: "post",
        headers: {
          "Content-Type": "application/json",
          "x-access-token": "token-value",
        },
        body: JSON.stringify(this.formData)
      }

      try {
        this.isFetch = true

        // Имитация отправки.
        await new Promise(resolve => setTimeout(resolve, 2000));

        // const response = await await fetch("/some-server-address", data)
        // if (!response.ok) {
        //   const message = `An error has occured: ${response.status} - ${response.statusText}`;
        //   throw new Error(message);
        // }

        this.$emit('quizFinish')
        this.isFetch = false
      } catch (error) {
        this.isFetch = false
        console.log('ERROR: ', error.message)
      }
    }
  },

  async created() {
    // Получение списка вопросов
    try {
      this.isFetch = true
      const response = await fetch("quizlist.json")
      if (!response.ok) {
        const message = `An error has occured: ${response.status} - ${response.statusText}`;
        throw new Error(message);
      }
      const json = await response.json()

      this.quizList = [...json['questions']]
      this.quizTotal = json['total'];
      this.isFetch = false
    } catch (e) {
      this.isFetch = false
      console.log('Error loading json:', e)
    }
  },
};
</script>

<style lang="scss">
.quiz {
  width: 600px;
  margin: 0 auto;
  padding: 32px 38px 40px 31px;
  background: #f5f5f5;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
  transition: .8s;

  &__title {
    font-weight: 700;
    font-size: 18px;
    line-height: 30px;
    text-align: center;
  }

  &__progress {
    margin-top: 50px;
  }

  &__form {
    margin-top: 22px;
  }

  &__form-question-title {
    font-weight: 500;
    font-size: 15px;
    line-height: 30px;
    margin: 0;
  }

  &__form-questions {
    margin-top: 30px;
    margin-bottom: 30px;
  }
}
</style>
