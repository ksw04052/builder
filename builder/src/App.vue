<template>
  <div>
    <h2>무릉 빌드 도우미</h2>
    <div id="inputArea">
      <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
        정보 입력
      </button>
      <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="staticBackdropLabel">Modal title</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div v-for="(skill, i) in skills" :key="i">
                <span>스킬{{i + 1}}</span>
                <img src="./assets/logo.png" class="skillIcon" alt="">
                <input type="text" class="textInput" v-model="skills[i].name">
                <input type="number" class="numberInput" v-model="skills[i].cooldown">
                <input type="number" class="numberInput" v-model="skills[i].duration">
                <button @click="removeSkill(i)">삭제</button>
              </div>
              <button @click="addSkill()">추가</button>
              {{skills}}
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-primary" data-bs-dismiss="modal">Close</button>
            </div>
          </div>
        </div>
      </div>      
    </div>

    <div id="timeline">
      <div id="timestamp">
        <div class="minContainer">
          <span class="headerColumn1">분</span>
          <span v-for="index in 25" :key="index" class="minute">{{index}}</span>
        </div>
        <div class="tenSecContainer">
          <span class="headerColumn1">초</span>
          <span v-for="index in 150" :key="index" class="tenSeconds">{{((index - 1) % 6 * 10) + 10}}</span>
        </div>
      </div>
      <div id="floor"></div>
      <div class="" v-for="(skill, i) in skills" :key="i">
        <span class="headerColumn2">
          <span>스킬{{i + 1}}</span>
          <img src="./assets/logo.png" class="skillIconLarge" alt="">
        </span>
        
      </div>
    </div>
      
    
  </div>
</template>

<script>
export default {
  name : 'App',

  data(){
    return{
      skills : [{}]
    }
  },

  methods:{
    removeSkill : function(i){
      this.skills.splice(i, 1)
    },

    addSkill : function(){
      this.skills.push({})
    }
  },

  computed:{

  },

  components:{

  }
}
</script>

<style>
  .numberInput{
    width: 40px;
    height: 20px;
  }

  .textInput{
    width: 100px;
    height: 20px;
  }

  .skillIcon{
    width: 20px;
    height: 20px;
    border-width: 1px;
    border-style: solid;
    border-radius: 6px;
    padding: 2px;
    vertical-align: bottom;
  }

  .skillIconLarge{
    width: 40px;
    height: 40px;
    border-width: 1px;
    border-style: solid;
    border-radius: 12px;
    padding: 2px;
    vertical-align: bottom;
  }

  .headerColumn1{
    width: 200px;
    text-align: center;
    padding: 0px;
    background-color: dimgray;
  }

  .headerColumn2{
    display: inline-flex;
    align-items: center;
    width: 200px;
    height: 60px;
    padding: 0px;
    background-color: rgb(197, 197, 197);
  }

  .minute{
    width: 120px;
    text-align: center;
    padding: 0px;
  }

  .tenSeconds{
    width: 20px;
    text-align: center;
    padding: 0px;
  }

  .minute:nth-child(even){
    background-color: burlywood;
  }
  .minute:nth-child(odd){
    background-color: cornflowerblue;
  }

  .tenSeconds:nth-child(even){
    background-color: burlywood;
  }

  .tenSeconds:nth-child(odd){
    background-color: cornflowerblue;
  }

  .minContainer{
    white-space: nowrap;
  }

  .tenSecContainer{
    white-space: nowrap;
  }

  span{
    display: inline-block;
  }

  #timeline{
    overflow: scroll;
  }


</style>