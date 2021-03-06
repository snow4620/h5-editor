<template>
  <div class="gls-slider">
    <!--滑块-->
    <div class="gls-slider-background" ref="background">
      <div class="gls-slider-progress" ref="progress"></div>
      <div class="gls-slider-trigger" ref="trigger"></div>
    </div>
    <!--数字输入框-->
    <div class="gls-slider-number">
      <gls-stepper v-model="inputValue" @input="renderValue" :max="max" :min="min"></gls-stepper>
    </div>
  </div>
</template>

<script lang="ts">
  /**
   * create by 给力叔 2017/9/27
   * 滑动条
   */
  import Vue from 'vue'
  import {Component, Inject, Model, Prop, Watch} from 'vue-property-decorator'
  import {Renderer} from "../../../../common/render/Renderer";
  import {IDraggableResult} from "../dragable/IDraggableResult";
  import {Draggable} from "../interact/Draggable";
  import GlsStepper from '../stepper/Stepper.vue'

  @Component({
    name: "GlsSlider",
    components: {
      GlsStepper:GlsStepper
    }
  })
  export default class GlsSlider extends Vue {
    _width: number;
    _draggable: Draggable;

    inputValue: number = 0;

    @Prop({
      default: function () {
        return 100;
      }
    }) max: number;

    @Prop({
      default: function () {
        return 0;
      }
    }) value: number;

    @Prop({
      default: function () {
        return 0;
      }
    }) min: number;

    @Watch("inputValue")
    onValueChange() {
      this.$emit("input", parseFloat(this.inputValue.toString()));
    }

    beforeMount(){
      this.inputValue = this.value;
    }

    mounted() {
      let backgroundWidth = Renderer.getBoundingClientRect(this.$refs.background).width
      this._width = backgroundWidth;
      this._draggable = new Draggable({
        target: this.$refs.trigger,
        elementRect: {top: 0, right: 0, bottom: 1, left: 0},
        onMove: (result: IDraggableResult) => {
          this.onMoveHandler(result);
        }
      });

      this.renderValue();
    }

    /**
     * 设置滑块的位置和更新进度
     */
    renderValue() {
      let x = (this.inputValue - this.min) / this.length * this._width;
      //更新进度
      Renderer.setStyle(this.$refs.progress, "width", x + 'px');
      //拖拽的时候,滑块的位置由拖拽决定,就不用重新设置位置了
      if (!this._draggable || this._draggable.isDragging) {
        return;
      }
      //设置滑块的位置
      this._draggable.setPosition(x, 0);
    }

    onMoveHandler(result: IDraggableResult) {
      this.updateValueByDrag(result);
      this.renderValue();
    }

    //根据拖拽更新值比较特殊,到左右零界点的时候
    updateValueByDrag(result: IDraggableResult) {
      let x = result.position.x + 1;
      let value;
      if (x == 0) {
        value = this.min;
      } else if (x == this._width) {
        value = this.max;
      } else {
        value = Math.floor(x / this._width * this.length) + this.min;
      }
      this.inputValue = value;
    }

    get length(){
      return this.max - this.min;
    }

    beforeDestroy() {
      this._draggable.destroy();
    }
  }
</script>

<style scoped lang="scss">
  .gls-slider {
    position: relative;
  }

  .gls-slider-background {
    width: 98px;
    height: 20px;
    background-color: #CCD5DB;
    border: solid 1px #CCCCCC;
    position: absolute;
  }

  .gls-slider-progress {
    position: absolute;
    height: 18px;
    background-color: #08A2EF;
    left: 0;
    top: 0;
  }

  .gls-slider-number {
    margin-left: 103px;
    > input {
      padding: 0;
      width: 100%;
    }
  }

  .gls-slider-trigger {
    width: 5px;
    height: 20px;
    cursor: e-resize;
    display: inline-block;
  }
</style>
