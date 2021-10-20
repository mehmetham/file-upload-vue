<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" width="100">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <b-container class="bv-example-row">
      <b-row style="height:400px;">
        <cropper class="cropper" :src="img"
          ref="cropper"
          :stencil-props="{
           aspectRatio: 2/2
          }"
          @change="change"
        />
      </b-row>
      <b-row>
         <b-button @click="crop" variant="success">Crop Image</b-button>
      </b-row>
      <b-container v-if="resultImage"  fluid class="p-4 bg-light">
        <b-row>
          <b-col>
            <b-img thumbnail fluid :src="resultImage" alt="Image 1"></b-img>
            <p>{{coordinates}}</p>
          </b-col>
        </b-row>
      </b-container>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import { Cropper } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css';

export default {
  name: 'Home',
  components: {
    HelloWorld,
    Cropper
  },
  data() {
    return{
      img: 'https://images.pexels.com/photos/1988684/pexels-photo-1988684.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=450&w=640',
      resultImage: null
    }	
	},
	methods: {
		change({ coordinates, canvas }) {
			console.log(coordinates, canvas)
    },
    crop() {
			const { coordinates, canvas } = this.$refs.cropper.getResult();
			this.coordinates = coordinates;
			// You able to do different manipulations at a canvas
			// but there we just get a cropped image
			this.resultImage = canvas.toDataURL();
		},
	},
}
</script>
