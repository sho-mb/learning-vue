<script>
import BlogPost from './components/BlogPost.vue'
import Aleart from './components/ErrorHandle.vue'
import TabA from './components/TabA.vue'
import TabB from './components/TabB.vue'
import CustomInput from './components/CustomInput.vue'
import MyComponent from './components/MyComponent.vue'
import MyMouseTracker from './components/MyMouseTracker.vue'

export default {
  components: {
    BlogPost,
    Aleart,
    TabA,
    TabB,
    CustomInput,
    MyComponent,
    MyMouseTracker
  },
  data() {
    return {
      msg : "hello mango",
      seen : true,
      seek : this.seen ? 'hide' : 'find',
      count : 0,
      isActive : true,
      numbers : [1, 2, 3, 4, 5, 6, 7],
      sets : [[1,2,3,4,5],[6,7,8,9,10]],
      message : "",
      question : '',
      answer: 'Questions usually contain a question mark',
      loading: false,
      posts: [
        {id: 1, title: 'My journey whit Vue'},
        {id: 2, title: 'Bologins'},
        {id: 3, title: 'Cambodia wow'},
      ],
      postFontSize : 1,
      currentTab : 'TabA',
      tabs:['TabA', 'TabB'],
      searchText:'hello',
      myText:''
    }
  },
  watch: {
    question(newQuestion, oldQuestion) {
      if (newQuestion.includes('?')) {
        this.getAnswer()
      }
    }
  },
  methods : {
    async getAnswer() {
      this.loading = true
      this.answer = 'Thinking'
      try {
        const res = await fetch('https://yesno.wtf/api')
        this.answer = (await res.json()).answer
      } catch (error) {
        this.answer = 'Error! Could not reach the API. ' + error 
      } finally {
        this.loading = false
      }
    },
    increment() {
      this.count ++
    },
    activate() {
      this.isActive = true
    },
    even(numbers) {
      return numbers.filter(number => number % 2 === 0)
    }
  },
  mounted() {
    this.increment();
    this.isActive = false;
  },
  computed: {
    classObject() {
      return {
        active: this.isActive
      }
    },
    evenNumber() {
      return this.numbers.filter(n => n % 2 === 0)
    }
  }
}

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <div class="demo">
      <button 
        v-for="tab in tabs"
        :key="tab"
        :class="['tab-button', { active: currentTab === tab }]"
        @click="currentTab = tab"
      >
        {{ tab }}
      </button>
      <component :is="currentTab" class="tab"></component>
    </div>

    <div>{{ msg.split('').reverse().join('') }}</div>

    <p v-if="seen">Now you see me ?</p>
    <button @click="this.seen = !this.seen">{{ seek }}</button>
    <br>
    <button @click="increment">{{ count }}</button>
    <br>
    <button @click="activate">Activate</button>
    <div :class="classObject">is Active then red</div>

    <ul>
      <li v-for="n in evenNumber" :key="n">{{ n }}</li>
    </ul>
    <ul v-for="numbers in sets" :key="numbers">
      <li v-for="n in even(numbers)" :key="n">
        {{ n }}
      </li>
    </ul>
    <p>
      v-model to bind text and reaturn to specific varable.
    </p>
    <p>{{ message }}</p>
    <input v-model="message" type="text">

    <p>
      watcher will compare input changes and this gives condition to check it contain "?" or not.
    </p>
    <p>
      <input v-model="question" :disabled="loading" />
    </p>
    <p>{{ answer }}</p>

    <div :style="{ fontSize: postFontSize + 'em' }">
      <BlogPost 
      v-for="post in posts"
      :key="post.id"
      :title="post.title"  
      @enlarge-text="postFontSize += 0.1" 
      />
    </div>

    <Aleart>
      Message will change if you change here
      It's using slot to place-holding your text to child compornet
    </Aleart>
    
    <CustomInput v-model="searchText"/> {{ searchText }}
    
    <p>Change first letter to capital</p>
    <MyComponent v-model.capitalize="myText" />
    
    <p>Mouse tracking</p>
    <MyMouseTracker v-slot="{x, y}">
      Mouse is at: {{ x }} , {{ y }}
    </MyMouseTracker>

  </main>
</template>

<style scoped>
.active {
  color:red;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

.demo {
  font-family: sans-serif;
  border: 1px solid #eee;
  border-radius: 2px;
  padding: 20px 30px;
  margin-top: 1em;
  margin-bottom: 40px;
  user-select: none;
  overflow-x: auto;
}

.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>
