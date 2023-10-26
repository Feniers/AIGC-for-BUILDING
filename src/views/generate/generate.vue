<template>
    <div>
      <!-- 多行输入框 -->
      <el-row >
        <el-col :span="24">
          <el-input
            type="textarea"
            placeholder="请输入内容"
            autosize
            clearable
            v-model="inputText"
          ></el-input>
        </el-col>
      </el-row>

      <br>
      <br>
      <br>
  
      <!-- 可选标签 -->
      <el-row>
        <el-col :span="24">
          <el-checkbox-group v-model="selectedTags">
            <el-checkbox label="标签1">标签1</el-checkbox>
            <el-checkbox label="标签2">标签2</el-checkbox>
            <el-checkbox label="标签3">标签3</el-checkbox>
          </el-checkbox-group>
        </el-col>
      </el-row>

      <br><br>
  
      <!-- 可供点击的图片 -->
      <el-row>
        <el-col :span="8" v-for="(image, index) in imageOptions" :key="index">
          <el-image
            :src="image.url"
            @click="selectStyle(image.style)"
            :class="{'selected': selectedStyle === image.style}"
          ></el-image>
        </el-col>
      </el-row>

      <br><br>
  
      <!-- 放大的上传图片展示 -->
      <el-row>
        <el-col :span="20">
          <el-upload
            class="upload-demo"
            action="/your-upload-api"
            list-type="picture-card"
            :on-success="handleUploadSuccess"
            :on-remove="handleRemove"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
        </el-col>
        <el-col :span="20   ">
          <el-image
            v-for="(image, index) in uploadedImages"
            :key="index"
            :src="image.url"
            :preview-src-list="previewImageList"
            style="max-width: 100%; max-height: 400px; margin-bottom: 10px;"
          ></el-image>
        </el-col>
      </el-row>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        inputText: '',
        selectedTags: [],
        selectedStyle: null,
        uploadedImages: [],
        previewImageList: [],
        imageOptions: [
          { url: 'src/views/generate/017ead5d8a3af0a801211d53b4e54a.jpg@3000w_1l_0o_100sh.jpg', style: '风格1' },
          { url: 'url_to_image2', style: '风格2' },
          { url: 'url_to_image3', style: '风格3' },
        ],
      };
    },
    methods: {
      selectStyle(style) {
        this.selectedStyle = style;
      },
      handleUploadSuccess(response, file, fileList) {
        // 处理上传成功的图片
        this.uploadedImages.push({ url: response.url, name: file.name });
        this.previewImageList.push(response.url);
      },
      handleRemove(file, fileList) {
        // 处理移除图片
        const index = this.uploadedImages.findIndex((image) => image.name === file.name);
        if (index !== -1) {
          this.uploadedImages.splice(index, 1);
          this.previewImageList.splice(index, 1);
        }
      },
    },
  };
  </script>
  
  
  <style scoped>
  .selected {
    border: 2px solid #007BFF;
  }
  .upload-demo {
    border: 1px dashed #D9D9D9;
    border-radius: 6px;
    cursor: pointer;
    width: 500px;
    height: 500px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  </style>
  