<template>
  <gls-panel title="组件设置">
    <b-tabs ref="tabs" card-body>
      <b-tab title="样式" active>
        <b-card>
          <div class="label-content">
            <label>
              复选框
            </label>
            <div>
              <b-form-checkbox id="checkbox1"
                               v-model="selected"
                               value="accepted"
                               unchecked-value="not_accepted">
                复选框测试
              </b-form-checkbox>
            </div>
          </div>

          <div class="label-content">
            <label>
              单选框
            </label>
            <div>
              <b-form-radio-group id="radios1" v-model="selected" :options="options" name="radioOpenions">
              </b-form-radio-group>
            </div>
          </div>

          <div class="label-content">
            <label>
              单行文本
            </label>
            <div>
              <b-form-input type="text"
                            placeholder="单行文本测试"></b-form-input>
            </div>
          </div>

          <div class="label-content">
            <label>
              多行文本
            </label>
            <div>
              <b-form-textarea id="textarea1"
                               placeholder="多行文本测试"
                               :rows="3"
                               :not-resize="true"
              >
              </b-form-textarea>
            </div>
          </div>

          <div class="label-content">
            <label>
              数字
            </label>
            <div>
              <b-form-input type="number"
                            placeholder="单行文本测试"></b-form-input>
            </div>
          </div>

          <div class="label-content">
            <label>
              颜色选择器
            </label>
            <div>
              <gls-color-picker v-model="colors"></gls-color-picker>
            </div>
          </div>

          <div class="label-content">
            <label>
              下拉框
            </label>
            <div>
              <b-form-select v-model="selected" :options="options" class="mb-3">
              </b-form-select>
            </div>
          </div>
          <div class="label-content">
            <label>
              滑动条
            </label>
            <div>
              <gls-slider v-model="sliderValue" :max="200" :min="0"></gls-slider>
            </div>
          </div>
          <!--{{sliderValue}}-->
          <!--<div role="tablist">-->
            <!--<b-card no-body class="mb-1">-->
              <!--<b-card-header header-tag="header" class="p-1" role="tab">-->
                <!--<b-btn block href="#" v-b-toggle.accordion1 variant="info">外观</b-btn>-->
              <!--</b-card-header>-->
              <!--<b-collapse id="accordion1" visible accordion="my-accordion" role="tabpanel">-->
                <!--<b-card-body>-->
                  <!--通用-->
                <!--</b-card-body>-->
              <!--</b-collapse>-->
            <!--</b-card>-->
            <!--<b-card no-body class="mb-1">-->
              <!--<b-card-header header-tag="header" class="p-1" role="tab">-->
                <!--<b-btn block href="#" v-b-toggle.accordion2 variant="info">通用</b-btn>-->
              <!--</b-card-header>-->
              <!--<b-collapse id="accordion2" accordion="my-accordion" role="tabpanel">-->
                <!--<b-card-body>-->
                  <!--外观-->
                <!--</b-card-body>-->
              <!--</b-collapse>-->
            <!--</b-card>-->
          <!--</div>-->
        </b-card>
      </b-tab>
      <b-tab title="动画">
        <b-card></b-card>
      </b-tab>
      <b-tab title="触发">
        <b-card></b-card>
      </b-tab>
    </b-tabs>
  </gls-panel>
</template>

<script lang="ts">
  /**
   * create by 给力叔 2017/9/19
   * 单选的时候的组件编辑器,也就是组件的属性编辑器了
   */
  import Vue from 'vue'
  import {Component, Inject, Model, Prop, Watch} from 'vue-property-decorator'
  import {Scene} from "../../core/scene/Scene";
  import glsPanel from "../../common/panel/Panel";
  import GlsOverlayPanel from "../../common/overlayPanel/OverlayPanel.vue"
  import GlsColorPicker from '../../common/colorPicker/ColorPicker.vue'
  import GlsSlider from '../../common/slider/Slider.vue'
  @Component({
    name: "GlsSingleSelectEditor",
    components: {
      glsPanel: glsPanel,
      GlsOverlayPanel: GlsOverlayPanel,
      GlsColorPicker:GlsColorPicker,
      GlsSlider:GlsSlider
    }
  })
  export default class GlsSingleSelectEditor extends Vue {
    @Prop({required: true}) scene: Scene;

//    todo 测试数据，记得删
    checkboxValue = true;
    radioValue = "yes";
    selected = null;
    sliderValue = 500;
    colors = {
      hex: '#194d33',
      hsl: {
        h: 150,
        s: 0.5,
        l: 0.2,
        a: 1
      },
      hsv: {
        h: 150,
        s: 0.66,
        v: 0.30,
        a: 1
      },
      rgba: {
        r: 25,
        g: 77,
        b: 51,
        a: 1
      },
      a: 1
    };
    options = [
      {value: null, text: '1'},
      {value: 'a', text: '1'},
      {value: 'b', text: '3'},
      {value: {'C': '3PO'}, text: '4'},
      {value: 'd', text: '5', disabled: true}
    ];

    show() {
      console.log(this.$refs.colorPanel);
    }

    stop(){

    }
  }
</script>

<style scoped lang="scss">
  .label-content {
    padding: 5px 0;
    position: relative;
    > label {
      display: inline-block;
      width: 70px;
      position: absolute;
      left: 0;
      top: 5px;
      height: 18px;
      line-height: 18px;
    }
    > div {
      margin-left: 70px;
      > input {
        width: 100%;
      }
      textarea {
        width: 100%;
      }
    }
  }
</style>
