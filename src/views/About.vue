<template>
  <div class="home">
    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <cropper
          ref="cropper"
          class="preview-result-example__cropper"
          :src="img"
          :debounce="false"
          :stencil-props="{
            aspectRatio: 1,
          }"
          @change="onChange"
        />
        </b-col>
        <b-col>
        <div class="preview-result-example__previews">
          <preview class="preview-result-example__preview" :image="result.image" :coordinates="result.coordinates" />
          <preview
            class="preview-result-example__preview preview-result-example__preview--small"
            :image="result.image"
            :coordinates="result.coordinates"
          />
        </div>
        <strong @click="onCrop()"> Download<br> <b-icon-cloud-download-fill class="h1" title="Download" /></strong>
      </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { Cropper, Preview } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css';

export default {
  name: 'About',
  components: {
    Cropper,
    Preview
  },
  data() {
    return{
			img:
				'https://images.unsplash.com/photo-1590291409749-452efbe0d76c?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=634&q=80',
      result: {
        coordinates: null,
        image: null,
      },
    }	
	},
	methods: {
		flip(x, y) {
			if (this.$refs.cropper.customImageTransforms.rotate % 180 !== 0) {
				this.$refs.cropper.flip(!x, !y);
			} else {
				this.$refs.cropper.flip(x, y);
			}
		},
		rotate(angle) {
			this.$refs.cropper.rotate(angle);
		},
		onChange({ coordinates, image }) {
			this.result = {
				coordinates,
				image,
			};
		},
		onCrop() {
			const result = this.$refs.cropper.getResult().canvas.toDataURL();
			const newTab = window.open();
			newTab.document.body.innerHTML = `<img src="${result}"></img>`;
		},
	},
}
</script>
<style lang="scss">
.preview-result-example {
	display: flex;
	&__previews {
		margin-left: 32px;
	}
	&__preview {
		border-radius: 50%;
		overflow: hidden;
		margin-top: 24px;
		margin-bottom: 24px;
		width: 200px;
		height: 200px;
		&--small {
			width: 120px;
			height: 120px;
		}
	}
	&__preview-image {
		width: 100%;
	}
	&__button {
		position: absolute;
		left: 16px;
		bottom: 0;
	}
}
</style>
