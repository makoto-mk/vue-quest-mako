<template>
    <v-card
        class="my-10"
        :elevation="formElevation"
        v-bind:rounded="formRounded"
        width="100%"
    >
        <v-container
            class="mx-10 text-center"
        >
            <v-form
                ref="movieForm"
            >
                <v-text-field
                    label="YouTube動画ID"
                    v-model="movieUrl"
                    :error-messages="movieErrorMessages"
                    placeholder="動画URLの【V=】のあとに続き英数字"
                    style="width: 300px"
                />
                <v-text-field
                    label="コメント"
                    v-model="comment"
                    :rules="commentRules"
                    placeholder="動画紹介のための任意のコメント"
                    class="mb-5"
                    style="width: 500px"
                />
                <v-btn
                    @click="formClick"
                    color="blue"
                    type="button"
                    x-large
                >
                    <span style="color: white">CLICK!</span>
                </v-btn>
            </v-form>
            </v-container>
    </v-card>
</template>

<script>


export default {
    name: 'Inputform',
    data () {
        return {
            movieUrl: "",
            movieErrorMessages: [], // 初期化
            comment: "",
            commentRules: [
                value => !!value || '入力してください',
                value => value.length <= 16 || '16文字以内で入力してください',
            ],
            headUrl: 'https://www.youtube.com/watch?',
        }
    },
    props: {
        formRounded: {
            type: String,
            required: true,
            default: 'xl',


        },
        formElevation: {
            type: String,
            required: false,
            default: '10',
        }

    },
    watch: { // データの中のプロパティを監視
        movieUrl: function (value) { // value : movieUrlが変化した後の値
            this.validateMovieUrl(value)
        }
    },
    methods: {
        formClick () {
            this.validateMovieUrl(this.movieUrl);
            const validateComment = this.$refs.movieForm.validate()

            // エラーがなかったら親クラスのstoreMovieを実行する
            if (this.movieErrorMessages.length === 0 && validateComment){
                this.$emit ('storeMovie', this.movieUrl, this.comment)
            }
        },
        validateMovieUrl (value) {
                    ///正規表現を変更する必要あり
            const stringValidation = /^[a-zA-Z0-9!-/:-@\\[-`{-~]+$/.test(value)   // test(value)でバリデーションチェックを実行
            if(!value){ // valueを必須にする
                this.movieErrorMessages = [
                    '入力してください',
                ]
            } else if (value.length !== 11) { // 文字数を制限する
                this.movieErrorMessages = [
                    '11文字で入力してください',
                ]
            } else if (!stringValidation) { // 入力できる値を制限する
                this.movieErrorMessages = [
                    '半角英数記号で入力してください',
                ]
            } else {
                this.movieErrorMessages = [] // 
            }
        }
    }
}
</script>
