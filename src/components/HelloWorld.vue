<template>
  <div class="hello">
    <div :class="['btn-section']">
      <div :class="['btn']" @click="btnClick('undo')">
        <img src="../assets/Vector.png" alt="" />
      </div>
      <div :class="['btn']" @click="btnClick('redo')">
        <img src="../assets/redo.png" alt="" />
      </div>
      <div :class="['btn']" @click="btnClick('heading')">
        <img src="../assets/bold.png" alt="" />
      </div>
      <div :class="['btn']" @click="btnClick('insertParagraph')">
        <div :class="['icon']">
          <img src="../assets/par.png" alt="" />
          <img src="../assets/par.png" alt="" />
        </div>
      </div>
      <div :class="['btn']" @click="btnClick('insertImage')">
        <img src="../assets/imgic.png" alt="" />
      </div>
      <div :class="['copyHTML']" @click="btnClick('copy')">
        <span>Скопировать HTML</span>
      </div>
      <div v-if="iscopied">скопировано</div>
    </div>
    <iframe
      id="output"
      :class="['border']"
      ref="textField"
    ></iframe>
       <iframe id="display" frameborder="0" name="display"    ref="output"></iframe>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      iscopied: false,
      isTitle:false
    };
  },
  watch:{
isTitle:function(){
  return this.isTitle ? this.$refs.textField.contentDocument.execCommand('formatBlock', false, '<h1>'):this.$refs.textField.contentDocument.execCommand('formatBlock', false, '<div>')
}
  },
  methods: {
    fitImage: function (contentDocument) {
      let imgs = contentDocument.querySelectorAll("img");
      imgs.forEach((img) => {
        img.style.maxWidth = "100%";
        img.style.height = "70%";
      });
    },
    insertImg: function (contentDocument, cmd) {
      let url = prompt("Введите ссылку", "");
      if (!url) {
        return;
      }
      contentDocument.execCommand(cmd, false, url);
      this.fitImage(contentDocument);
    },
    copyHTML: function (contentDocument) {
      if (contentDocument.body.innerHTML === "") {
        return;
      }
      contentDocument.execCommand("selectAll", false, null);
      contentDocument.execCommand("copy", false, null);
      contentDocument.getSelection().removeAllRanges();
      contentDocument.designMode = "off";
      this.iscopied = true;
      setTimeout(() => {
        this.iscopied = false;
        contentDocument.designMode = "on";
      }, 500);
    },
    btnClick: function (cmd) {
      let { contentDocument } = this.$refs.textField;
      if (cmd === "insertImage") {
        this.insertImg(contentDocument, cmd);
      } else if (cmd === "copy") {
        this.copyHTML(contentDocument);
      }
      else if(cmd==='heading'){
let prevTitle=this.isTitle
this.isTitle=!prevTitle
      }
       else {
        contentDocument.execCommand(cmd, false);
      }
      this.fitImage(contentDocument);
    },
  },
  mounted: function () {
    this.$refs.textField.contentDocument.body.style.color = "#EAEAEA";
       this.$refs.textField.contentDocument.body.style.wordBreak = "break-word";
    this.$refs.textField.contentDocument.designMode = "On";
  },
};
</script>
<style scoped>
.hello {
  height: 100%;
}
.btn-section {
  display: flex;
  align-items: center;
}
.btn {
  width: 2em;
  height: 2em;
  background: #282828;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 0.3em;
  box-sizing: border-box;
  border-radius: 0.5em;
}
.icon {
  display: table;
  margin: auto;
}
.copyHTML {
  color: #639eff;
  margin-left: 0.9em;
}
iframe {
  margin-top: 1.5em;
  width: 70%;
  height: 70%;
}
.copyHTML,
.btn {
  cursor: pointer;
}
.no {
  border: initial;
}
.border {
  border: 1px solid wheat;
}
</style>
