<template>
    <div>
        <div class="some ce">

            <button  @click = "onClick()" class="addNote styled-box-green p-ripple" v-ripple>{{title}}</button>
           
        </div>
    </div>
</template>

<script>

export default {
    name:'BigButton',
    props:{
        title:String,

       
    },
    methods:
    {
        onClick()
        {

    
        this.$gtag.event('Task-Added', {
            'event_category': 'documentation',
            'event_label': 'Just a placeholder for testing',
            'value': 1
        })
    
           

        }
    }
    
}
</script>


<style lang="scss" scoped>
$bgCol: inherit;
$textColor: rgb(47, 47, 47);
$Col1: #2f2f2f;
$otherTextCol: rgb(32, 32, 32);
::v-deep(.some) {
    .addNote {
      cursor: pointer;
      background-color: $bgCol;
      border: 1px solid $Col1;
      color: $otherTextCol;
      font-weight: bold;
      outline: none;
      margin: 1em 0em 0em 0em;
      padding: 0;
      
      

        &.styled-box-green {
            .p-ink {
                background: rgba(#4baf50, 0.3);
            }
        }

        &.styled-box-orange {
            .p-ink {
                background: rgba(#ffc106, 0.3);
            }
        }

        &.styled-box-purple {
            .p-ink {
                background: rgba(#9c27b0, 0.3);
            }
        }

        &:last-child {
            margin-right: 0;
        }
    }
}
.ce{
  display: flex;
  justify-content: center;
}
</style>
