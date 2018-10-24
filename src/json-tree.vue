<template>
  <span class="json-tree" :class="{'json-tree-root': parsed.depth === 0}">
    <span class="json-tree-row" v-if="parsed.primitive">
      <span class="json-tree-indent" v-for="n in (parsed.depth * 2 + 3)" :key="n">&nbsp;</span>
      <span class="json-tree-key" v-if="parsed.key">{{ parsed.key }}</span>
      <span class="json-tree-colon" v-if="parsed.key">:&nbsp;</span>
      <span class="json-tree-value" :class="'json-tree-value-' + parsed.type" :title="`${parsed.value}`">{{ `${parsed.value}` }}</span>
      <span class="json-tree-comma" v-if="!parsed.last">,</span>
      <span class="json-tree-indent">&nbsp;</span>
    </span>
    <span class="json-tree-deep" v-if="!parsed.primitive">
      <span class="json-tree-row json-tree-expando" @click="expanded = !expanded" @mouseover="hovered = true" @mouseout="hovered = false">
        <span class="json-tree-indent">&nbsp;</span>
        <span class="json-tree-sign">{{ expanded ? '-' : '+' }}</span>
        <span class="json-tree-indent" v-for="n in (parsed.depth * 2 + 1)" :key="n">&nbsp;</span>
        <span class="json-tree-key" v-if="parsed.key">{{ parsed.key }}</span>
        <span class="json-tree-colon" v-if="parsed.key">:&nbsp;</span>
        <span class="json-tree-open">{{ parsed.type === 'array' ? '[' : '{' }}</span>
        <span class="json-tree-collapsed" v-show="!expanded">&nbsp;/*&nbsp;{{ format(parsed.value.length) }}&nbsp;*/&nbsp;</span>
        <span class="json-tree-close" v-show="!expanded">{{ parsed.type === 'array' ? ']' : '}' }}</span>
        <span class="json-tree-comma" v-show="!expanded && !parsed.last">,</span>
        <span class="json-tree-indent">&nbsp;</span>
      </span>
      <span class="json-tree-deeper" v-show="expanded">
        <json-tree v-for="(item, index) in parsed.value" :key="index" :kv="item" :level="level"></json-tree>
      </span>
      <span class="json-tree-row" v-show="expanded">
        <span class="json-tree-ending" :class="{'json-tree-paired': hovered}">
          <span class="json-tree-indent" v-for="n in (parsed.depth * 2 + 3)" :key="n">&nbsp;</span>
          <span class="json-tree-close">{{ parsed.type === 'array' ? ']' : '}' }}</span>
          <span class="json-tree-comma" v-if="!parsed.last">,</span>
          <span class="json-tree-indent">&nbsp;</span>
        </span>
      </span>
    </span>
  </span>
</template>

<script src="./json-tree.js"></script>

<style scoped src="./json-tree.css"></style>
