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
              <h5 class="modal-title" id="staticBackdropLabel">정보 입력</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div>
                직업:
                <select>
                  <option :key="i" :value="job.name" v-for="(job,i) in jobs">{{job.name}}</option>
                </select>
              </div>
              <div>
                쿨타임 감소 % (메르 유니온) [2~6%]:
                <input type="number" class="numberInput" min="0" max="6" v-model="cdrP">
              </div>
              <div>
                쿨타임 감소 초 (쿨감뚝):
                <input type="number" class="numberInput" min="0" v-model="cdrV">
              </div>
              <div>
                버프 지속시간 증가 %:
                <input type="number" class="numberInput" min="0" v-model="bdiP">
              </div>

              <div v-for="(skill, i) in skills" :key="i" class="inputSkill">
                <button @click="removeSkill(i)" class="remove">-</button>
                <span>스킬{{i + 1}}</span>
                <img src="./assets/logo.png" class="skillIcon" alt="">
                <input type="text" class="textInput" v-model="skills[i].name">
                <input type="number" class="numberInput" v-model="skills[i].cooldown">
                <input type="number" class="numberInput" v-model="skills[i].duration">
                <br>
                <input type="checkbox" id="cdrPCheck" value="checked" v-model="cdrPCheck[i]">
                <label for="cdrPCheck">쿨%</label>
                <input type="checkbox" id="cdrVCheck" value="checked" v-model="cdrVCheck[i]">
                <label for="cdrVCheck">쿨뚝</label>
                <input type="checkbox" id="bdiPCheck" value="checked" v-model="bdiPCheck[i]">
                <label for="bdiPCheck">벞지</label>
                | 쿨타임: {{cooldownCal(skills[i].cooldown, i)}} | 지속시간: |
              </div>
              <button @click="addSkill()">추가</button>
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
import jobs from './data/jobs.json'

export default {
  name : 'App',

  data(){
    return{
      jobs,
      skills : [{}],
      bdiPCheck : [false],
      cdrPCheck : [false],
      cdrVCheck : [false],
      bdiP : 0,
      cdrP : 0,
      cdrV : 0,
    }
  },

  methods:{
    addSkill : function(){
      this.skills.push({})
      this.bdiPCheck.push(false)
      this.cdrPCheck.push(false)
      this.cdrVCheck.push(false)
    },
    
    removeSkill : function(i){
      this.skills.splice(i, 1)
      this.bdiPCheck.splice(i, 1)
      this.cdrPCheck.splice(i, 1)
      this.cdrVCheck.splice(i, 1)
    },

    cooldownCal : function(cooldown, i){
      let cdrP = this.cdrP
      let cdrV = this.cdrV
      if(this.cdrPCheck[i] == false){
        cdrP = 0
      }
      if(this.cdrVCheck[i] == false){
        cdrV = 0
      }

      if(!cooldown) return 0
      let cd = cooldown * (100 - cdrP) / 100
      cd = this.decimalRound(cd, 2)
      if(cooldown<=5) return cd
      if(cd<=10){
        cd = cd * (100 - (cdrV * 5)) / 100
        cd = this.decimalRound(cd, 2)
        if(cd<5) return 5
        return cd
      }
      if(cd-cdrV<=10){
        cd = 10 * (100 - (cdrV - (cd - 10))*5) / 100
        cd = this.decimalRound(cd, 2)
        return cd
      }
      else{
        cd = cd - cdrV
        cd = this.decimalRound(cd, 2)
        return cd
      }
    },

    decimalRound : function(value, n){
      value = Math.round(value * Math.pow(10, n)) / Math.pow(10, n)
      value = value.toFixed(n)
      if(value.toString().endsWith('.00')){
        value = value - '.00'
      }
      return value
    }

    
  },

  computed:{

  },

  components:{

  }
}
</script>

<style>
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
  
  .inputSkill{
    border-style: solid;
    border-width: 1px;
    padding: 10px;
  }

  .minContainer{
    white-space: nowrap;
  }

  .minute{
    width: 120px;
    text-align: center;
    padding: 0px;
  }

  .minute:nth-child(even){
    background-color: burlywood;
  }
  .minute:nth-child(odd){
    background-color: cornflowerblue;
  }

  .numberInput{
    width: 40px;
    height: 20px;
  }

  .remove{
    position: absolute;
    right: 30px;
  }

  .skillIcon{
    width: 20px;
    height: 20px;
    border-width: 1px;
    border-style: solid;
    border-radius: 6px;
    padding: 2px;
    vertical-align: center;
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

  .tenSecContainer{
    white-space: nowrap;
  }

  .tenSeconds{
    width: 20px;
    text-align: center;
    padding: 0px;
  }

  .tenSeconds:nth-child(even){
    background-color: burlywood;
  }

  .tenSeconds:nth-child(odd){
    background-color: cornflowerblue;
  }

  .textInput{
    width: 100px;
    height: 20px;
  }

  span{
    display: inline-block;
  }

  #timeline{
    overflow: scroll;
  }


</style>