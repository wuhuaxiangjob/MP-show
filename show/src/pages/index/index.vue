<template>
  <view class="content">
    <view class="btn-list">
      <button
        class="btn-item"
        :class="pickType === 'one' ? 'btn-active' : ''"
        data-type="one"
        @click="pickMethod"
      >
        样式一
      </button>

      <!-- <button
        class="btn-item"
        data-type="landscape"
        :class="pickType === 'landscape' ? 'btn-active' : ''"
        @click="pickMethod"
      >
        横屏
      </button> -->

      <!-- <button class="btn-item btn-active">默认</button> -->
    </view>

    <view class="input-box">
      <input
        class=""
        type="text"
        :placeholder="placeholder"
				:value="inputVal"
        @input="onKeyInput"
        @blur="onKeyInput"
      />
      <button class="input-btn" @click="handleClick">确定</button>
    </view>
    <veiw class="tip">
      <text>输入你想说的话(大于4个字)，点击确定按钮</text>
    </veiw>
    <veiw v-if="showOne" class="modal one-modal" @tap="closeModal">
      <!-- <text class="input-val">{{ inputVal }}</text> -->
      <text class="render-text">{{ renderText }}</text>
    </veiw>
  </view>
</template>

<script>
export default {
  data() {
    return {
      inputVal: '',
      pickType: 'one',
      showOne: false,
      placeholder: '例如：小哥哥，加个微信吗！',
      renderArr: [],
      renderText: ''
    };
  },
  onLoad() {},
  methods: {
    /**
     * @description 处理点击确定
     */
    handleClick() {
      if (!this.inputVal) {
        uni.showModal({
          title: '提示',
          content: '请输入你想说的话',
          showCancel: false,
          success: function(res) {}
        });
        return false;
      }
      if (this.inputVal.length < 5) {
        uni.showModal({
          title: '提示',
          content: '请输入多于4个字',
          showCancel: false,
          success: function(res) {}
        });
        return false;
      }
      this.renderModal();
    },
    /**
     * @description 渲染modal
     */
    renderModal() {
      if (this.pickType === 'one') {
        this.showOne = true;
        this.renderOne();
      }
    },
    renderOne() {
      let i = 0;
      this.renderArr = this.inputVal.split('');
      this.renderText = this.renderArr[0];
      setInterval(() => {
        i++;
        if (i < this.renderArr.length) {
          this.renderText = this.renderArr[i];
        } else {
          i = -1;
        }
      }, 1000);
    },
    /**
     * @description 获取输入框中的值
     */
    onKeyInput(e) {
      this.inputVal = e.target.value;
    },
    /**
     * @description 选择展示方式
     */
    pickMethod(e) {
      this.pickType = e.currentTarget.dataset.type;
    },
    /**
     * @description 关闭modal
     */
    closeModal() {
      this.showOne = false;
      this.inputVal = '';
    }
  }
};
</script>

<style scoped>
@import url('index.css');
</style>
