<template>
  <el-tooltip :disabled="tipDisabled" :placement="placement">
    <div slot="content" :style="tipStyle">
      {{title}}
    </div>
    <clamp ref="clamp" :ellipsis="ellipsis" :expanded.sync="localExpanded" v-bind="$attrs" v-on="$listeners">
      <slot></slot>
      <template v-if="isExpand" slot-scope="{ expand, collapse, toggle, clamped, expanded }" slot="after">
        <slot :expand="expand" :collapse="collapse" :toggle="toggle" :clamped="clamped", :expanded="expanded" name="after">
          <tc-button v-if="expanded" type="text" @click="doClamp">{{clampText}}</tc-button>
          <tc-button v-if="!expanded && clamped" type="text" @click="doExpand">{{expandText}}</tc-button>
        </slot>
      </template>
    </clamp>
  </el-tooltip>
</template>

<script>
import clamp from './Clamp'
export default {
  name: 'TcClamp',
  componentName: 'TcClamp',
  components: {clamp},
  props: {
    useMode: {type: String, default: 'tip', validator: function(value) {
      return ['tip', 'expand'].indexOf(value) !== -1
    }},
    ellipsis: {type: String, default: '…'},
    placement: {type: String, default: 'top'},
    autoTip: {type: Boolean, default: false},
    tipWidth: {type: Number, default: 200},
    expanded: {type: Boolean, default: false},
    expandText: {type: String, default: '展开'},
    clampText: {type: String, default: '收起'}
  },
  data() {
    return {
      localExpanded: !!this.expanded,
      isClamped: false
    }
  },
  watch: {
    expanded(val) {
      this.localExpanded = val
    },
    localExpanded(val) {
      if (this.expanded !== val) {
        this.$emit('update:expanded', val)
      }
    }
  },
  computed: {
    isExpand() {
      return this.useMode === 'expand'
    },
    isTipAndTitle() {
      return this.useMode === 'tip' && this.autoTip
    },
    tipDisabled() {
      // 如果设置不显示tip， 直接禁用
      if (!this.isTipAndTitle) {
        return true
      }
      // 需要判断是否clamp, 没有截断的时候，也禁用tip
      return !this.isClamped
    },
    title() {
      return this.isTipAndTitle ? this.getText() : ''
    },
    tipStyle() {
      const style = {}
      style.width = this.tipWidth + 'px'
      return style
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.isClamped = this.$refs.clamp.isClamped
      console.log(this.isClamped, 'isClamped')
    })
  },
  methods: {
    getText() {
      // Look for the first non-empty text node
      let [content] = (this.$slots.default || []).filter(
        node => !node.tag && !node.isComment
      )
      return content ? content.text : ''
    },
    doExpand() {
      this.localExpanded = true
    },
    doClamp() {
      this.localExpanded = false
    }
  }
}
</script>

<style lang="scss">
.my-pp{
  width: 300px !important;
}
</style>