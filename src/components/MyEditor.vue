<template>
    <div class="myEditor">
        <div id="container" ref="container" style="height:800px"></div>
    </div>
</template>
<script>
import * as monaco from 'monaco-editor';
export default {
    props:{
        codes:{
            type: String,
            default: ''
        },
        language:{
            type: String,
            default: 'html'
        },
        theme:{
            type: String,
            default: 'vs'
        },
        readOnly: {
            type: String,
            default: 'false'
        },
        wordWrap: {
            type: String,
            default: 'false'
        },
        fontSize: {
            type: Number,
            default: 28
        },
        cursorStyle: {
            type: Number,
            default: 1
        }
    },
    data(){
        return{
            codesCopy:null,//内容备份
            wordWrapStatue: true,
            editorOptions:{
                selectOnLineNumbers: true,
                roundedSelection: false,
                readOnly: false,        // 只读
                cursorStyle: 'line',        //光标样式
                automaticLayout: true, //自动布局
                glyphMargin: true,  //字形边缘
                useTabStops: false,
                fontSize: 28,       //字体大小
                autoIndent:true,//自动布局
                accessibilitySupport: 'auto',
                wordWrap: 'on'
                //quickSuggestionsDelay: 500,   //代码提示延时
            }
        }
    },
    watch: {
        language() {
            this.initEditor()
        },
        theme() {
            this.initEditor()
        },
        fontSize() {
            monaco.editor.EditorOptions.fontSize.defaultValue = this.fontSize
            this.initEditor();
        },
        cursorStyle() {
            monaco.editor.EditorOptions.cursorStyle.defaultValue = this.cursorStyle
            this.initEditor();
        },
        wordWrap() {
            if(this.wordWrap === 'true') {
                monaco.editor.EditorOptions.wordWrap.defaultValue = 'on'
                this.initEditor();
            }else {
                monaco.editor.EditorOptions.wordWrap.defaultValue = 'off'
                this.initEditor();
            }
        },
        readOnly() {
            if(this.readOnly === 'true') {
                monaco.editor.EditorOptions.readOnly.defaultValue = true
            } else {
                monaco.editor.EditorOptions.readOnly.defaultValue = false
            }
            this.initEditor();
        }
    },
    mounted(){
        this.initEditor()
    },
    methods:{
        initEditor(){
            let self = this;
            self.$refs.container.innerHTML = '';
            self.monacoEditor = monaco.editor.create(self.$refs.container, {
                value: self.codesCopy || self.codes,
                language: self.language,
                theme: self.theme,//vs, hc-black, or vs-dark
                editorOptions: self.editorOptions,
            });
            self.$emit('onMounted',self.monacoEditor);//编辑器创建完成回调
            self.monacoEditor.onDidChangeModelContent(function(event){//编辑器内容changge事件
                self.codesCopy = self.monacoEditor.getValue();
                self.$emit('onCodeChange',self.monacoEditor.getValue(),event);
            });
            //编辑器随窗口自适应
            window.addEventListener('resize',function(){
                this.initEditor();
            })
        },
        RunResult(){
            console.log(this.monacoEditor.getValue());
        },
        themeChange(){
            this.initEditor();
        }
    }
}
</script>
<style scoped>
    #container{
        height:100%;
        text-align: left;
    }
</style>