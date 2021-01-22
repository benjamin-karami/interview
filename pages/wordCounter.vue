<template>
<div>
    <div>
      <b-nav class="mt-4 d-flex justify-content-center">
        <NuxtLink class="mx-3" to="/">Home</NuxtLink>
      </b-nav>
    </div>

    <h2>Word Count: <span id="count">{{wordCount}}</span></h2>

    <textarea v-model="textarea" @keyup="wordCounter(textarea)" id="input" rows="8" cols="60"></textarea>

    <table style="width: 500px; border: 1px solid #a5a5a5;border-collapse: collapse;">
      <thead>
        <th style="padding: 5px; text-align: center;border: 1px solid #a5a5a5;border-collapse: collapse;">word</th>
        <th style="padding: 5px; text-align: center;border: 1px solid #a5a5a5;border-collapse: collapse;">count</th>
      </thead>
      <tbody>
        <tr v-for="(value, name, index) in counts" :key="index">
          <td style="padding: 5px; text-align: center;border: 1px solid #a5a5a5;border-collapse: collapse;">{{name}}</td>
          <td style="padding: 5px; text-align: center;border: 1px solid #a5a5a5;border-collapse: collapse;">{{value}}</td>
        </tr>
      </tbody>
    </table>

</div>
</template>

<script>
export default {

    data() {
      return {
        wordCount: 0,
        textarea: "",
        //wordSplit:[],
        counts: []
      }
    },

    methods: {
     isWord(str) {
        var alphaNumericFound = false;
        for (var i = 0; i < str.length; i++) {
            var code = str.charCodeAt(i);
            if ((code > 47 && code < 58) ||
                (code > 64 && code < 91) ||
                (code > 96 && code < 123)) {
                alphaNumericFound = true;
                return alphaNumericFound;
                }
        }
        return alphaNumericFound;
     },

     wordCounter(text) {
        var text = this.textarea.split(' ');
        //this.wordSplit = text;
        var wordCount = 0;
        for (var i = 0; i < text.length; i++) {
            if (text[i] !== ' ') {
              wordCount++;
            }
        }
        this.wordCount = wordCount;
        this.counts = this.find_duplicate_in_array(text);     },

      find_duplicate_in_array(array){
        const count = {}

        array.forEach(item => {
            if (count[item]) {
              count[item] +=1
              return
            }
            if (item == '' || item == ' ') {
              return
            } 
            count[item] = 1
        })
        return count;

      }

    },

    // compute:{
    //   distribution = uniqueCount.reduce((acum,cur) => Object.assign(acum,{[cur]: (acum[cur] || 0)+1}),{})
    // }
}
</script>