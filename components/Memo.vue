<template>
    <div>
        メモ
        <!-- {{allData}} -->

        <ul v-for="data in allData" :key="data.id" class="menu-list" >
            <li>
                {{data.name}} / {{data.age}}
            </li>
        </ul>

        <p>
            <input v-model="name" placeholder="名前">
            <input v-model="age" placeholder="年齢">
            <button v-on:click='post'>送信</button>
        </p>
    </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";

export default {
    components: {},

    data(){
        return{
            db: {},
            allData: [],
            name: '',
            age: 0
        }
    },

    methods: {
        //初期化、設定
        init: () => {
            const config = {
                apiKey: "AIzaSyDErAYqDCowJQgbTYd7qx5hRfOCPcaRkbQ",
                authDomain: "class20200523.firebaseapp.com",
                databaseURL: "https://class20200523.firebaseio.com",
                projectId: "class20200523",
                storageBucket: "class20200523.appspot.com",
                messagingSenderId: "8916758614",
                appId: "1:8916758614:web:5576fbb74c08ca144acbf7"
            };

            // Initialize Firebase
            firebase.initializeApp(config);
        },

        //データ追加
        post: function(){
            const testId = firebase.firestore().collection('memos').doc().id; //ユニークなIDを生成
            const docRef = firebase.firestore().collection('memos').doc(testId);
            const setAda = docRef.set({
                name: this.name,
                age: this.age
            });

            this.get();
        },

        //データ取得
        get: function(){
            this.allData = [];
            firebase.firestore().collection('memos').get().then(snapshot => {
                snapshot.forEach(doc => {
                    // console.log(doc);
                    this.allData.push(doc.data());
                })
            });
        }

    },

    mounted(){
        this.init();
        this.get();
    },
}

</script> 

<style>
</style>
