<template>
    <div class="ctr">
        <transition name="fade" mode="out-in">
            <question v-if="questionsAnswered < questions.length"
                      :questions="questions"
                      :questionsAnswered="questionsAnswered"
                      @question-answered="updateTotalResult"
            ></question>

            <result v-else
                    :results="results"
                    :totalCorrect="totalResult"
            ></result>
        </transition>

        <button type="button" class="reset-btn" v-if="questionsAnswered === questions.length" @click="reset">Reset
        </button>
    </div>
</template>

<script>
import Question from "./components/Question.vue";
import Result from "./components/Result.vue";

export default {
    name: "App",
    components: {
        Question,
        Result
    },
    data() {
        return {
            totalResult: 0,
            questionsAnswered: 0,
            questions: [
                {
                    q: 'What is 2 + 2?',
                    answers: [
                        {
                            text: '4',
                            is_correct: true
                        },
                        {
                            text: '3',
                            is_correct: false
                        },
                        {
                            text: 'Fish',
                            is_correct: false
                        },
                        {
                            text: '5',
                            is_correct: false
                        }
                    ]
                },
                {
                    q: 'How many letters are in the word "Banana"?',
                    answers: [
                        {
                            text: '5',
                            is_correct: false
                        },
                        {
                            text: '7',
                            is_correct: false
                        },
                        {
                            text: '6',
                            is_correct: true
                        },
                        {
                            text: '12',
                            is_correct: false
                        }
                    ]
                },
                {
                    q: 'Find the missing letter: C_ke',
                    answers: [
                        {
                            text: 'e',
                            is_correct: false
                        },
                        {
                            text: 'a',
                            is_correct: true
                        },
                        {
                            text: 'i',
                            is_correct: false
                        }
                    ]
                },
            ],
            results: [
                {
                    min: 0,
                    max: 2,
                    title: "Try again!",
                    desc: "Do a little more studying and you may succeed!"
                },
                {
                    min: 3,
                    max: 3,
                    title: "Wow, you're a genius!",
                    desc: "Studying has definitely paid off for you!"
                }
            ]
        }
    },
    methods: {
        updateTotalResult(is_correct) {
            if (is_correct) {
                this.totalResult++;
            }
            this.questionsAnswered++
        },
        reset() {
            this.questionsAnswered = 0
            this.totalResult = 0
        }
    },

}
</script>
