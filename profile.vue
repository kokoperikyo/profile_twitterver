<template>
  <div>
		<!-- 箱的なイメージ　relativeにしておくことで中のabsolute要素の場所をいい感じに動かせる -->
    <div class="relative">
      <!-- 初期画面で見えるサンプル画像がセットされている -->
			<!-- 画像をアップした後もここにその画像を差し込むのでabsoluteで場所をいい感じにしてあげる -->
      <img class="absolute" :src="uploadedImage" v-bind:style="{opacity:opa}" />
			<!-- 画像アップフォーム
			編集ボタンを押すとv-showによって表示される
			上の画像セット場所に完全にかぶる
			opacity:0で見えないけど押せる状態を実装 -->
      <input
        v-show="isEditable"
        class="headerImg"
        type="file"
        v-on:change="onFileChange"
        style="opacity:0"
      />
    </div>
    <button id="edit" @click="edit">編集</button>
    <button v-show="isEditable" id="save" @click="save">保存</button>
    <!-- <div class="name_button">
      <h2 id="userName">名前</h2>
      <button id="edit" @click="edit">編集</button>
      <button v-show="isEditable" id="save" @click="save">保存</button>
    </div>-->

    <!-- <textarea v-if="isEditable" class="intro" v-model="introMsg"></textarea>
    <textarea v-else class="intro" v-model="introMsg" disabled></textarea>

    <ul>
      <p>なんかリストを続ける</p>
      <li v-for="list in lists" v-bind:key="list">{{list}}</li>
    </ul>-->
  </div>
</template>

<script>
export default {
  data() {
    return {
      // lists: ["a", "b", "c", "d"],
      isEditable: false,
      opa: 1.0,
      uploadedImage: "https://placehold.jp/2000x250.png"
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
		},
		// 画像を押すと呼ばれる
    onFileChange(e) {
      let files = e.target.files;
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
    }
  }
};
</script>

<style>
.headerImg {
  width: 100%;
  height: 200px;
}
#edit,
#save {
  display: inline-block;
  margin-left: 100px;
}
.relative {
  position: relative;
  width: 100%;
  height: 200px;
}
/* relative要素に対しての場所を指定できる
なのでrelative要素の中に要素を入れ込むこともできる */
.absolute {
  position: absolute;
  width: 100%;
  height: 200px;
  right: 0px;
  bottom: 0px;
}
/* .intro {
  width: 100%;
  height: 400px;
  border: solid;
} */
/* .name_button #userName {
  display: inline-block;
} */
</style>