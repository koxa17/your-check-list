<template>
  <q-list bordered class="rounded-borders q-pt-sm q-pb-sm question__list">
    <template v-for="(question, idx) in questions" :key="question.id">
      <q-item class="question__list__item">

        <q-item-section center class="col-8">
          <q-item-label>{{ idx + 1 }}. {{ question.title }}</q-item-label>
        </q-item-section>

        <q-item-section center side class="col-4">
          <div class="flex no-wrap">
            <q-btn v-if="!question.more" outline  icon="done" color="positive" class="question__list__item__btn-done"/>
            <q-input class="q-ml-md question-result__block" dense outlined v-model="question.result"
                     input-class="question-result__input"/>
          </div>
        </q-item-section>
      </q-item>

      <q-list v-if="question.more" class="question__list question__list__subquestions">
        <template v-for="(subQuestion, i) in question.more" :key="subQuestion.id">

          <q-separator v-if="i === 0" spaced inset/>
          <q-item class="question__list__item question__list__subquestions__item">
            <q-item-section center class="col-8 q-pl-md">
              <q-item-label><q-icon :name="fasMinus"/> {{ subQuestion.title }}</q-item-label>
            </q-item-section>

            <q-item-section side center class="col-4">
              <div class="flex no-wrap">
                <q-btn outline icon="done" color="positive" class="question__list__item__btn-done"/>
                <q-input class="q-ml-md question-result__block" dense outlined v-model="subQuestion.result"
                         input-class="question-result__input"/>
              </div>
            </q-item-section>

          </q-item>
          <q-separator class="q-m-none" v-if="i !== question.more.length - 1" spaced inset/>

        </template>
      </q-list>

      <q-separator class="q-m-none" v-if="idx !== questions.length - 1" spaced/>
    </template>
  </q-list>
</template>

<script>
import {defineComponent, onMounted, ref} from 'vue';
import {getDeepCopyOfQuestionsDefault} from "src/data/script";
import {getDataFromLocalStorage, saveDataToLocalStorage} from "src/assets/tools/script"
import {fasMinus} from "@quasar/extras/fontawesome-v6";

export default defineComponent({
  name: "ListQuestion",
  setup() {
    onMounted(() => {
      let questionsLS = getDataFromLocalStorage("questions")
      if (questionsLS) {
        questions.value = questionsLS
      } else {
        saveDataToLocalStorage("questions", questions.value)
      }
    })


    const questions = ref(getDeepCopyOfQuestionsDefault())

    function questionIncrement(question, more, operation = true) {
      let indexElement = questions.value.findIndex(item => {
        return item === question
      })

      if (more) {
        let moreIndexElement = questions[indexElement].more.findIndex(moreItem => {
          return moreItem === more
        })
        const result = parseInt(questions.value[indexElement].more[moreIndexElement].result)
        questions.value[indexElement].more[moreIndexElement].result = operation ? result + 1 : result - 1
      } else {
        const result = parseInt(questions[indexElement].result)
        questions.value[indexElement].result = operation ? result + 1 : result - 1
      }
    }

    return {
      questions,
      fasMinus,
      questionIncrement
    }
  }
})
</script>

<style lang="scss">
.question__list {
  max-width: 840px;
  margin: 0 auto;
  .q-separator .q-separator--horizontal {
    margin: 0;
  }
}

.question-result__block {
  .q-field__control {
    padding: 0;
  }
}

.question-result__input {
  text-align: center;
  width: 56px;
  padding: 0;
}

</style>
