<template>

        
<Dialog header="Sign Up or Login" v-model:visible="displayModal" :style="{width: '50vw'}" :modal="true">

<SocialLogin @auth="populateHeader" />
        </Dialog>

<Fieldset  style=" border-radius: 0.5rem" @mouseenter="vName()" @mouseleave="ivName()">
  <template #legend>

    <div class="field-card" @click="openModal" >
       <transition name="slide-fade">
      <p class="log" v-if="displayName==true">{{user}}</p>
       </transition>
    </div>

  </template>
  <h1  >What's Next?</h1>
  
  
  <form @submit.prevent="newItem()">
    <div @mouseenter="showTool()" @mouseleave="hideTool()">
    <label class="newToDo">New ToDo </label>
    <Editor v-model="newTodo" placeholder="What's on your mind bub?"
       
     >
      <template #toolbar  >



        <span class="ql-formats" >
          <transition name="slide-fade">
          <button class="ql-bold" v-show="toolBar" ></button>
          </transition>
          <transition name="slide-fade"> 
             <button class="ql-italic" v-show="toolBar" ></button>   
          </transition>     
          <transition name="slide-fade">   
            <button class="ql-underline" v-show="toolBar" ></button> 
          </transition>     
                    <transition name="slide-fade">    
                       <button class="ql-image" v-show="toolBar" ></button>
          </transition>     
                    <transition name="slide-fade"> 
                       <button class="ql-code" v-show="toolBar" ></button>   
          </transition>     
                   
         
         
          
         
         
        </span>

  
      </template>
    </Editor>
    </div>
    <BigButton title="Add ToDo"/>
  </form>
  <h2 >ToDo List</h2>
  <ProgressBar :value="count" style="margin-bottom:1rem; text-color:white">
    {{message}} ({{count}}%)
</ProgressBar>


<SlickList  class="long-list"  axis="y" lockAxis="y" v-model:list="todos" :useDragHandle="true">

    <SlickItem v-for="(todo, index) in todos" :key="index" :index="index">
      <div :class="{todoDone:todo.done, todoPend:!todo.done}" style="margin-bottom:0.5rem;">




       
                        
                    
      <Editor v-model="todo.content" id="inputtext" >
        <template #toolbar>
       
          <div class='todo___item'>

           
                    <div  v-handle>
                       <i class="pi pi-bars" style="fontSize: 1rem"></i>
                       
                    </div>
                    <span class="p-buttonset">
                       
                        <Button class="save-btn" label="Save" icon="pi pi-check" v-if="!todo.done"  @click="strikeItem(index)" style="margin-right:2px;"/>
                        
                         
                         <Button class="save-btn" label="Save" icon="pi pi-unlock" v-if="todo.done"  @click="strikeItem(index)" style="margin-right:2px;background-color:red;"/>
                        
                        <Button class="del-btn" label="Delete" icon="pi pi-trash"   @click="removeItem(index)" />
                       
                    </span>

      
                    
          </div>
         
          
        </template>

      </Editor>

      </div>
    </SlickItem>
  </SlickList>



  <h4 v-if="todos.length === 0">Empty list.</h4>
 
</Fieldset>

</template>

<script>
import { ref } from "vue";
import { SlickList, SlickItem,HandleDirective } from 'vue-slicksort';
import BigButton from './components/BigButton.vue'
import SocialLogin from './components/SocialLogin.vue'
import ProgressBar from 'primevue/progressbar';
import Dialog from 'primevue/dialog';
import Fieldset from 'primevue/fieldset';


export default {
  name: "App",
  components:{SlickList,SlickItem, BigButton,ProgressBar,SocialLogin,Dialog,Fieldset},
   directives: { handle: HandleDirective },
   
  setup() {
    const newTodo = ref("");
    const toolBar = ref(false);
    const displayModal= ref(false);
    const displayName = ref(false);
    
    const todoList = JSON.parse(localStorage.getItem("todos")) || [];
    const todos = ref(todoList);

    const count = ref(0)
    const message = ref('Big things have small beginnings')
    const user = ref("Login")
    
    



    function newItem() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = "";
      }
      
      storeItem();
    }
    function  GA() {
      
      this.$gtag.event('login-test', {
        'event_category': 'documentation',
        'event_label': 'Just a placeholder for testing',
        'value': 1
      })
    }
    function populateHeader(e)
    {
      
      if(e['loginType'] == 'fb')
      {
        user.value = e['fb']['user']['name']


      }
      else if(e['loginType'] == 'google')
      {
        user.value = e['google']['user']['name']

      }
      closeModal()
      GA()
    }
    function crossOut(todo) {
      todo.done = !todo.done;
      storeItem();
    }
    function removeItem(index) {
      todos.value.splice(index, 1);
      storeItem();
    }
    function strikeItem(index) {
      todos.value[index].done = !todos.value[index].done;
      storeItem();

    }
    function storeItem() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
      
      updateBar()
    }
     function updateBar()
    {
      const done = todos.value.filter((el)=>
      {
        return el.done;
      })
      count.value = 100*(done.length/todos.value.length)
      if(count.value<=20)
      {
        message.value="Big things have small beginnings"
      }
      else if(count.value <50)
      {
        message.value="Almost halfway there"
      }
       else if(count.value <70)
      {
        message.value="Keep at it!"
      }
       else if(count.value <90)
      {
        message.value="Home stretch lets go!!"
      }
       else if(count.value ==100)
      {
        message.value="Yay! You're done 🤗"
      }
     
      



    }
     function showTool()
      {
        toolBar.value = true;

      }
      function hideTool()
      {
        toolBar.value = false;

      }
      function openModal() {
            displayModal.value = true;
        }
      function closeModal() {
            displayModal.value = false;
        }
      function vName() {
            displayName.value = true;
        }
      function ivName() {
            displayName.value = false;
        }


    return {
      todos,
      newTodo,
      newItem,
      crossOut,
      removeItem,
      storeItem,
      strikeItem,
      count,
      updateBar,
      message,
      showTool,
      hideTool,
      toolBar,
      displayModal,
      openModal,
      closeModal,
      populateHeader,
      user,
      displayName,
      vName,
      ivName
      
    
    };
  },
};
</script>

<style lang="scss">
@import "./assets/mytemplate.css";
$border: 2px solid
  rgba(
    $color: white,
    $alpha: 0.35,
  );
$customsize1: 6px;
$customsize2: 12px;
$customsize3: 18px;
$customsize4: 24px;
$customsize5: 48px;
$bgCol: #eeeeee;
$textColor: rgb(47, 47, 47);
$Col1: #86ec56;
$otherTextCol: rgb(32, 32, 32);
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: $bgCol;
  color: $textColor;
  .todo___item
  {
    display: flex;
    justify-content: space-between;
  }
  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    h1 {
      font-weight: bold;
      font-size: 28px;
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      width: 100%;
      label {
        font-size: 14px;
        font-weight: bold;
      }
      input {
        background-color: transparent;
        border: $border;
        color: inherit;
      }
    }
    .btn {
      cursor: pointer;
      background-color: $Col1;
      border: 1px solid $Col1;
      color: $otherTextCol;
      font-weight: bold;
      outline: none;
      border-radius: $customsize1;
    }
    
    h2 {
      font-size: 22px;
      border-bottom: $border;
      padding-bottom: $customsize1;
    }
    ul {
      padding: 10px;
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $customsize2 $customsize4;
        border-radius: $customsize1;
        margin-bottom: $customsize2;
        span {
          cursor: pointer;
        }
        .done {
          text-decoration: line-through;
        }
        button {
          font-size: $customsize2;
          padding: $customsize1;
        }
      }
    }
    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
.save-btn
{
  background-color: #eeeeee !important;
  color: #444444 !important;
}
.save-btn:hover
{
  background-color: #dddddd !important;
  color: $Col1 !important;
}
.del-btn
{
  background-color: #eeeeee !important;
  color: #444444 !important;
  
 
}
.del-btn:hover
{
  background-color: #dddddd !important;
 
  color: rgb(150, 0, 0) !important;
  
 
}
.todoDone
{
  border-top: 6px solid green;
  border-radius: 8px;
}
.todoPend
{
  border-top: 4px solid grey;
    border-radius: 8px;
}
.p-progressbar
{
  background-color: #86ec56;
}
.long-list {
  max-height: 400px;
  overflow: auto;
}

.slide-fade-enter-active {
  transition: all .8s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(50px);
  opacity: 0;
}
.field-card
{
  width: max-content;
  height: inherit;
  
  cursor: pointer;
  

}
.log{
  padding: 0;
  margin: 0;
 
  font-size: 16px;
}
.log:hover
{
  color:green;
}

</style>
