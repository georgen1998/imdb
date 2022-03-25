<template>
  <div>
    <div class="base-card shadow my-4">
      <div class="row align-items-center">
        <div class="col-md-4" >
          <span >ID: {{obj.id}} </span>
          <h1 class="title">{{obj.title}}</h1>
          <p>{{obj.description}}</p>
          <div class="icon text-center">
            <div v-html="icons.heartIcon" />
            <span class='bi bi-heart-fill' v-bind:style="{'color':colorH}" @click="like"></span>
          </div>
        </div>
        <div class="col-md-8">
          <img :src="obj.image" width="200" height="200">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import icons from "@/helpers/icons";
export default {
  name: "Card",
  props:{
      obj:Object,
  },
  data() {
    return {
      icons: icons,
      colorH:'white'
    }
  },
  methods:{
    like(){
      if (this.user.name){
        if (this.colorH=='white') {
          this.colorH = 'red';
          console.log(this.obj.title);
          console.log(this.obj.id);
          this.$likes.push({'description':this.obj.description,'image':this.obj.image,'id':this.obj.id,'title':this.obj.title});
          console.log(this.$likes);
          console.log(this.user.name);
        }else{
        this.colorH='white';
        }
      }else{
        console.log('ERROR! you need to login first');
      }
    }
  }
}

</script>

<style scoped>
.icon{
  width:16px;
  margin:0 auto;
}
.base-card {
  padding: 1em;
  margin-bottom: 24px;
  transition: all 0.2s linear;
  cursor: pointer;
  border-radius:7px;
  /*background-color:  black  ;*/
  /*color: white;*/
}
.base-card:hover {
  transform: scale(1.03);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2), 0 1px 15px 0 rgba(0, 0, 0, 0.19);
}
</style>