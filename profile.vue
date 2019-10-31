<template>
  <div>
    <div class="relative">
      <img id="absolute" v-show="uploadedImage" :src="uploadedImage" v-bind:style="{opacity:opa}" />
      <input
        v-if="isEditable"
        id="upHeaderImg"
        type="file"
        v-on:change="onFileChange"
        style="opacity:0"
      />
      <p v-else id="headerImg"></p>
    </div>
    <div class="name_button">
      <h2 id="userName">名前</h2>
      <button id="edit" @click="edit">編集</button>
      <button v-show="isEditable" id="save" @click="save">保存</button>
    </div>

    <textarea v-if="isEditable" class="intro" v-model="introMsg"></textarea>
    <textarea v-else class="intro" v-model="introMsg" disabled></textarea>

    <ul>
      <p>なんかリストを続ける</p>
      <li v-for="list in lists" v-bind:key="list">{{list}}</li>
    </ul>

  </div>
</template>

<script>
export default {
  data() {
    return {
      lists: ["a", "b", "c", "d"],
      isEditable: false,
      opa: 1.0,
			uploadedImage: "https://placehold.jp/2000x250.png",
			introMsg:'',
    };
  },
  methods: {
    //編集可能にする
    edit: function() {
      this.isEditable = true;
      this.opa = 0.3;
    },
    save: function() {
      this.isEditable = false;
			this.opa = 1.0;

			makeNewMsg()
    },
    onFileChange(e) {
      let files = e.target.files;
      // console.log(files[0]);

      this.createImage(files[0]);
    },
    // アップロードした画像を表示
    createImage(file) {
      let reader = new FileReader();
      reader.onload = e => {
        this.uploadedImage = e.target.result;
      };
      this.opa = 1.0;
      reader.readAsDataURL(file);
		},
		makeNewMsg(){
			this.introMsg = this.introMsg.str.replace(/\n/g, '\n')
		}
	}
};
</script>

<style>
#headerImg {
  width: 100%;
  height: 200px;
}
.intro {
  width: 100%;
  height: 400px;
  border: solid;
}
.name_button #userName {
  display: inline-block;
}
.name_button #edit,
#save {
  display: inline-block;
  margin-left: 100px;
}
#upHeaderImg {
  width: 100%;
  height: 200px;
}
.relative {
  position: relative;
}
#absolute {
  width: 100%;
  height: 200px;
  position: absolute;
  right: 0px;
  bottom: 0px;
}
</style>