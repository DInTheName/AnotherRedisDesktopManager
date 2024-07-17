<template>
  <div>
    <JsonEditor ref="editor" :content="newContent" :readOnly="false"></JsonEditor>
  </div>
</template>

<script>
import JsonEditor from '@/components/JsonEditor';
const JSONbig = require('@qii404/json-bigint')({ useNativeBigInt: false });
const lz4 = require('lz4');

export default {
  components: { JsonEditor },
  props: ['content'],
  computed: {
    newContent() {
      const { formatStr } = this;

     if (typeof formatStr === 'string') {
        if (this.$util.isJson(formatStr)) {
          return JSONbig.parse(formatStr);
        }
        return formatStr;
      }
      return 'Zlib LZ4 Parse Failed!';
    },
    formatStr() {
      return this.$util.zippedToString(this.content, 'lz4');
    },
  },
  methods: {
    getContent() {
      const content = this.$refs.editor.getRawContent(true);
      const decompressed = lz4.decode(content);
      return decompressed;
   },
    copyContent() {
      return this.formatStr;
    },
  },
};
</script>
 
