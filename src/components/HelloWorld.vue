<template>
  <section>
    <form class="form" autocomplete="off" method="get" name="Loginform" enctype ref="Loginform">
      <div class="logo_box">
        <label for="logo" class="logo_label label">企业logo</label>
        <input
          id="logo"
          v-show="false"
          type="file"
          accept="image/*"
          @change="getFile($event)"
          ref="input_one"
          name="logo"
        />
        <div class="logo_one" @click="openImg()">
          <i class="fs-across"></i>
          <i class="fs-upright"></i>
          <img v-if="imgUrl!=''" :src="imgUrl" class="img_one" />
        </div>
        <input
          id="logo"
          v-show="false"
          type="file"
          accept="image/*"
          @change="getFiletwo($event)"
          ref="input_two"
          name="logo-big"
        />
        <div class="logo_two" @click="openImgtwo()">
          <i class="fs-across_two"></i>
          <i class="fs-upright_two"></i>
          <img v-if="imgUrltwo!=''" :src="imgUrltwo" class="img_one" />
        </div>
        <div class="logo_msg">
          <p>请上传两种尺寸的企业logo</p>
          <p>
            <span class="num">1)</span>
            <span class="num">方形logo</span>
            <span>400px*400px</span>
          </p>
          <p>
            <span class="num">2)</span>
            <span class="num">矩形logo</span>
            <span>800px*400px</span>
          </p>
          <p>
            <span class="num">3)</span>
            <span>文件大小在2M以内</span>
          </p>
          <p>
            <span class="num">4)</span>
            <span>支持 jpg、png 两种格式</span>
          </p>
        </div>
      </div>

      <div>
        <label for="name" class="label">企业名称</label>
        <input
          type="text"
          name="name"
          id="name"
          class="name input"
          placeholder="请输入企业名称"
          @blur="checkout($event,namereg)"
          @input="changeValue($event)"
        />
        <span class="err">*企业名称不能为空</span>
        <span class="err">*请输入正确的企业名称为中文</span>
      </div>
      <div class="select_box">
        <label for="nature" class="label">企业性质</label>
        <select
          name="property"
          id="nature"
          class="nature select"
          @blur="notNaN($event)"
          @change="valueChange($event)"
        >
          <option value="-1" disabled selected>请选择企业性质</option>
          <option value="0">国有企业</option>
          <option value="1">私有企业</option>
          <option value="2">外资企业</option>
          <option value="3">合资企业</option>
        </select>
        <span class="err">*请选择企业性质</span>
        <i class="fa fa-angle-down right"></i>
      </div>
      <div class="select_box">
        <label for="scale" class="label">企业规模</label>
        <select
          name="scale"
          id="scale"
          class="scale select"
          @blur="notNaN($event)"
          @change="valueChange($event)"
        >
          <option value="-1" disabled selected>请选择企业规模</option>
          <option value="0">0-1百人</option>
          <option value="1">1百-5百人</option>
          <option value="2">5百-1千人</option>
          <option value="3">1千-2千人</option>
          <option value="2">2千-5千人</option>
          <option value="3">5千人++</option>
        </select>
        <span class="err">*请选择企业规模</span>
        <i class="fa fa-angle-down right"></i>
      </div>
      <div class="select_box">
        <label for="industry" class="label">所属行业</label>
        <select
          name="industry"
          id="industry"
          class="industry select"
          placeholder="请选择行业"
          ref="industry"
          @blur="notNaN($event)"
          @change="valueChange($event)"
        >
          <option value="-1" disabled selected>请选择行业</option>
          <option value="0">制造业</option>
          <option value="1">加工业</option>
          <option value="2">服务业</option>
          <option value="3">工商业</option>
          <option value="4">金融业</option>
          <option value="5">其他</option>
        </select>
        <span class="err">*请选择行业</span>
        <i class="fa fa-angle-down right"></i>
      </div>
      <div>
        <label for="url" class="label">企业网址</label>
        <input
          type="text"
          name="url"
          id="url"
          class="url input"
          placeholder="https://"
          @blur="checkout($event,urlreg)"
          @input="changeValue($event)"
        />
        <span class="err">*企业网址不能为空</span>
        <span class="err">*请输入正确的企业网址</span>
      </div>
      <div>
        <label for="address" class="label">企业地址</label>
        <input
          type="text"
          name="address"
          id="address"
          class="adress input"
          placeholder="请输入企业地址"
          @blur="checkout($event,addressreg)"
          @input="changeValue($event)"
        />
        <span class="err">*企业地址不能为空</span>
        <span class="err">*请输入正确的企业地址</span>
      </div>

      <div class="plus_tag_box">
        <label for="lightspot" class="label">企业亮点</label>
        <div class="tag_box" ref="tag_input_box">
          <ul class="taglist" ref="taglist">
            <li v-for="(tag,index) in taglist" :key="index" ref="li">
              {{tag.tag}}
              <span class="del_btn" v-on:click="delTags(index)">x</span>
            </li>
          </ul>
          <input
            type="text"
            name="feature"
            id="lightspot"
            class="lightspot"
            v-model="tag"
            @input="notErr"
            @blur="err"
            ref="tag_input"
          />
        </div>

        <span class="err" ref="light">*请至少输入一条企业亮点</span>
        <span class="err" ref="lights">*最多输入五条企业亮点</span>
        <span class="plus" @click="addTags">
          <i class="fa fa-plus"></i>
        </span>
      </div>
      <span class="sub" @click="sumbit">提交</span>
    </form>
  </section>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      namereg: /^[\u2E80-\u9FFF]+$/,
      addressreg: /^[\u4e00-\u9fa50-9a-zA-Z #]+$/,
      urlreg: /^((https|http|ftp|rtsp|mms)?:\/\/)[^\s]+/,
      imgUrl: "",
      imgUrltwo: "",
      tag: "",
      taglist: [],
      label_span: ""
    };
  },
  methods: {
    // 获取logo
    getFile: function(event) {
      let file = event.target.files[0];
      let url = "";
      console.log(file);
      let size = Math.floor(file.size / 1024);
      console.log(size);
      if (size > 2048) {
        alert("图片的大小不能超过2M");
        return false;
      }
      var reader = new FileReader();
      reader.readAsDataURL(file);
      let that = this;
      reader.onload = function() {
        url = this.result.substring(this.result.indexOf(",") + 1);
        that.imgUrl = "data:image/png;base64," + url;
      };
    },

    // 获取logo-big
    getFiletwo: function(event) {
      let file = event.target.files[0];
      let url = "";
      let size = Math.floor(file.size / 1024);
      console.log(size);
      if (size > 2048) {
        alert("图片的大小不能超过2M");
        return false;
      }
      var reader = new FileReader();
      reader.readAsDataURL(file);
      let that = this;
      reader.onload = function() {
        url = this.result.substring(this.result.indexOf(",") + 1);
        that.imgUrltwo = "data:image/png;base64," + url;
      };
    },
    openImg() {
      this.$refs.input_one.click();
    },
    openImgtwo() {
      this.$refs.input_two.click();
    },

    // 验证企业名称，企业地址，企业网址
    checkout(event, reg) {
      console.log(event.currentTarget);
      var value = event.currentTarget.value;
      console.log(value);
      if (value === "") {
        console.log(event.currentTarget.nextElementSibling);
        event.currentTarget.style.border = "1px solid red";
        event.currentTarget.nextElementSibling.style.display = "inline-block";
      } else if (!reg.test(value)) {
        event.currentTarget.style.border = "1px solid red";
        event.currentTarget.nextElementSibling.nextElementSibling.style.display =
          "inline-block";
      }
    },
    // 监听企业名称，企业地址，企业网址的值改变，取消提示错误
    changeValue(event) {
      event.currentTarget.style.border = "1px solid #bbbbbb";
      event.currentTarget.nextElementSibling.style.display = "none";
      event.currentTarget.nextElementSibling.nextElementSibling.style.display =
        "none";
    },
    // 监听select值的变化
    notNaN(event) {
      console.log(event);
      console.log(event.currentTarget.value);
      let val = event.currentTarget.value;
      console.log(event.currentTarget.nextElementSibling);
      if (val === "-1") {
        event.currentTarget.style.border = "1px solid red";
        event.currentTarget.nextElementSibling.style.display = "inline-block";
      }
    },
    // 监听select值的变化，取消提示错误

    valueChange(event) {
      let val = event.currentTarget.value;
      if (val !== "-1") {
        event.currentTarget.style.border = "1px solid #bbbbbb";
        event.currentTarget.nextElementSibling.style.display = "none";
      }
    },
    // 添加企业亮点Tags
    addTags() {
      // console.log( this.$refs.tag_input)
      if (!this.tag || this.tag.trim() == "") {
        return false;
      }
      if (this.taglist.length < 5) {
        this.taglist.push({
          tag: this.tag
        });
        this.tag = "";
      }
    },
    // 删除企业亮点Tags
    delTags(index) {
      this.taglist.splice(index, 1);
      this.$refs.lights.style.display = "none";
      this.$refs.tag_input_box.style.border = "1px solid #bbbbbb";
    },
    // 判断企业亮点的数量，至少有一条且最多有五条
    err(event) {
      console.log(event);
      // console.log(this.taglist.length);
      console.log(event.currentTarget.value);
      if (this.taglist.length === 5) {
        this.$refs.lights.style.display = "inline-block";
        this.$refs.tag_input_box.style.border = "1px solid red";
      } else if (
        event.currentTarget.value === "" &&
        this.taglist.length === 0
      ) {
        this.$refs.light.style.display = "inline-block";
        this.$refs.tag_input_box.style.border = "1px solid red";
      }
    },

    notErr(event) {
      console.log(event);
      // console.log(this.taglist.length);
      // console.log(event.currentTarget);
      this.$refs.light.style.display = "none";
      this.$refs.tag_input_box.style.border = "1px solid #bbbbbb";
      this.$refs.lights.style.display = "none";
      this.$refs.tag_input_box.style.border = "1px solid #bbbbbb";
      if (this.taglist.length < 5) {
        this.$refs.lights.style.display = "none";
        this.$refs.tag_input_box.style.border = "1px solid #bbbbbb";
      } else if (this.taglist.length !== 0) {
        this.$refs.light.style.display = "none";
        this.$refs.tag_input_box.style.border = "1px solid #bbbbbb";
      }
    },

    // 提交form表单
    sumbit() {
      let data = new FormData(this.$refs.Loginform);
      console.log(this.$refs.Loginform);
      console.log(data);

      if (data !== "") {
        this.axios({
          url: "http://118.24.210.103:8888/api/enterprise",
          method: "post",
          data: data,
          headers: {
            "Content-Type": "multipart/form-data"
          }
        }).then(res => {
          console.log(res);
        });
      }
    }
  }
};
</script>

<style scoped lang="less">
// select 清除默认样式
select {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none; /*兼容苹果手机*/
  -ms-appearance: none;
  -webkit-tap-highlight-color: #fff;
  background-color: transparent;
}
section {
  // height: 100vh;
  background-color: #f4f4f4;
}
.form {
  width: 1200px;
  margin: 0px auto;
  padding: 65px;
  font-size: 18px;
  background-color: #ffffff;
  // border: 1px solid black;
}
.label {
  margin-right: 20px;
  color: #bbbbbb;
}
.logo_label {
  color: #000000;
}
.logo_box {
  line-height: 160px;
  margin-bottom: 25px;
  color: #bbbbbb;
}
.logo_one {
  display: inline-block;
  vertical-align: middle;
  width: 160px;
  height: 160px;
  margin: 0px 10px 0px 0px;
  background-color: #f8f8f8;
  border: 1px dashed #ededed;
  position: relative;
}
.img_one {
  width: 100%;
  height: 100%;
  position: absolute;
}
.fs-upright {
  display: block;
  width: 5px;
  height: 75px;
  background-color: #000000;
  translate: 90deg;
  position: absolute;
  top: 47px;
  left: 77px;
  border: 1px solid #000000;
  border-radius: 5px;
}
.fs-across {
  display: block;
  width: 75px;
  height: 5px;
  background-color: #000000;
  position: absolute;
  top: 77px;
  left: 42px;
  border: 1px solid #000000;
  border-radius: 5px;
}
.logo_two {
  display: inline-block;
  vertical-align: middle;
  width: 320px;
  height: 160px;
  background-color: #f8f8f8;
  border: 1px dashed #ededed;
  position: relative;
}

.fs-upright_two {
  display: block;
  width: 5px;
  height: 75px;
  background-color: #000000;
  translate: 90deg;
  position: absolute;
  top: 47px;
  left: 155px;
  border: 1px solid #000000;
  border-radius: 5px;
}
.fs-across_two {
  display: block;
  width: 75px;
  height: 5px;
  background-color: #000000;
  position: absolute;
  top: 77px;
  left: 122px;
  border: 1px solid #000000;
  border-radius: 5px;
}
.logo_msg {
  display: inline-block;
  line-height: 30px;
  font-size: 14px;
  vertical-align: middle;
  margin-left: 25px;
}
.num {
  margin-right: 5px;
}
.err {
  display: none;
  margin-left: 15px;
  color: red;
  font-size: 18px;
}
.input,
.select {
  width: 535px;
  height: 60px;
  font-size: 18px;
  outline: none;
  color: #bbbbbb;
  border: 1px solid #bbbbbb;
  box-sizing: border-box;
  text-indent: 0.5rem;
  margin-bottom: 30px;
}
.select {
  color: #000000;
}
.select_box {
  position: relative;
}
.right {
  position: absolute;
  font-size: 18px;
  top: 25px;
  left: 600px;
}
.plus_tag_box {
  position: relative;
  display: flex;
  align-items: center;
  overflow: hidden;
}
.plus {
  width: 58px;
  height: 59px;
  font-size: 16px;
  text-align: center;
  line-height: 60px;
  background-color: #f8f8f8;
  cursor: pointer;
  position: absolute;
  top: 2px;
  left: 570px;
  box-sizing: border-box;
  border-left: 1px solid #bbbbbb;
}
.lightspot {
  margin-top: 1px;
  height: 58px;
  border: none;
  font-size: 18px;
  outline: none;
  color: #bbbbbb;
  box-sizing: border-box;
}
.lightspot:focus {
  max-width: 100%;
  width: auto;
  margin-top: 1px;
  height: 58px;
  border: none;
  font-size: 18px;
  outline: none;
  color: #bbbbbb;
  box-sizing: border-box;
}
.tag_box {
  width: 535px;
  height: 60px;
  display: flex;
  border: 1px solid #bbbbbb;
}
.taglist {
  margin-top: 5px;
  margin-left: 15px;
}
.taglist li {
  float: left;
  height: 40px;
  line-height: 40px;
  padding: 5px 15px;
  background-color: #f1f1f1;
  border-radius: 5px;
  color: #000000;
  font-size: 20px;
  margin: 0 10px 0 0;
  cursor: pointer;
  position: relative;
}
.del_btn {
  display: inline-block;
  font-size: 16px;
  color: gray;
  font-weight: bold;
  position: absolute;
  right: 4px;
  top: -11px;
}
.sub {
  display: inline-block;
  width: 120px;
  height: 60px;
  color: #ffffff;
  text-align: center;
  line-height: 60px;
  border-radius: 12px;
  margin-left: 120px;
  margin-top: 40px;
  background-color: green;
}
</style>
