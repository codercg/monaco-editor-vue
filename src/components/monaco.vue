<template>
    <div>
        <div class="title">
            <div class="language">
                <span>  Language：</span>
                <a-select :default-value="selectLanguage" style="width: 150px" @change="handleChangeLanguage">
                    <a-select-option
                        v-for="item in language"
                        :key="item"
                        :value="item"
                    >
                        {{ item }}
                    </a-select-option>
                </a-select>
            </div>
            <div class="theme">
                <span>  Theme：</span>
                <a-select :default-value="selectTheme" style="width: 150px" @change="handleChangeTheme">
                    <a-select-option
                        v-for="item in theme"
                        :key="item"
                        :value="item"
                    >
                        {{ item }}
                    </a-select-option>
                </a-select>
            </div>
            <div class="font-size">
                <span> FontSize: </span>
                <a-input-number id="inputNumber" v-model="fontSize" :min="6" :max="100" @change="onChange" />
            </div>
            <div class="cursor-style">
                <span> cursorStyle: </span>
                <a-select :default-value="selectCursorStyle" style="width: 150px" @change="handleChangeCursorStyle">
                    <a-select-option
                        v-for="item in cursorStyle"
                        :key="item.key"
                        :value="item.key"
                    >
                        {{ item.value }}
                    </a-select-option>
                </a-select>
            </div>
            <div class="read-only">
                <span> readOnly: </span>
                <a-select :default-value="selectReadOnly" style="width: 150px" @change="handleChangeReadOnly">
                    <a-select-option
                        v-for="item in readOnly"
                        :key="item"
                        :value="item"
                    >
                        {{ item }}
                    </a-select-option>
                </a-select>
            </div>
            <div class="word-warp">
                <span> wordWrap: </span>
                <a-select :default-value="selectWordWrap" style="width: 150px" @change="handleChangeWordWrap">
                    <a-select-option
                        v-for="item in wordWrap"
                        :key="item"
                        :value="item"
                    >
                        {{ item }}
                    </a-select-option>
                </a-select>
            </div>
        </div>
        <MyEditor
            :language="selectLanguage"
            :codes="codes"
            :theme="selectTheme"
            @onMounted="onMounted"
            @onCodeChange="onCodeChange"
            class="my-editor"
            :fontSize="fontSize"
            :cursorStyle="selectCursorStyle"
            :readOnly="selectReadOnly"
            :wordWrap="selectWordWrap"
        />
    </div>
</template>
 
<script>
import MyEditor from './MyEditor'
export default {
    name: "monaco",
    components:{
        MyEditor
    },
    data () {
        return {
            codes:'<div>This is html</div>',
            editor:null,
            selectLanguage: 'shell',
            selectTheme: 'vs',
            selectCursorStyle: 0,
            selectReadOnly: 'false',
            language: ['html', 'typescript', 'yaml', 'json', 'c', 'java', 'javascript', 'python', 'php', 'mysql', 'sql', 'shell'],  // 需要更多language：https://microsoft.github.io/monaco-editor/
            theme: ['vs', 'hc-black', 'vs-dark'],
            selectWordWrap: 'false',
            editorOptions: {
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
            },
            fontSize: 14,
            cursorStyle: [
                {
                    key: 0,
                    value: 'line' 
                },
                {
                    key: 1,
                    value: 'block' 
                },
                {
                    key: 2,
                    value: 'underline' 
                },
                {
                    key: 3,
                    value: 'line-thin' 
                },
                {
                    key: 4,
                    value: 'block-outline' 
                },
                {
                    key: 5,
                    value: 'underline-thin' 
                },
            ],
            readOnly: ['false', 'true'],
            wordWrap: ['false', 'true']
        }
    },
    methods:{
        onMounted(edit){
            this.editor = edit;
        },
        onCodeChange(value,event){
            console.log(value, event)
        },
        onChange(value) {
            this.fontSize = value
        },
        handleChangeLanguage(value) {
            this.selectLanguage = value
        },
        handleChangeTheme(value) {
            this.selectTheme = value
        },
        handleChangeCursorStyle(value) {
            this.selectCursorStyle = value
        },
        handleChangeReadOnly(value) {
            this.selectReadOnly = value
        },
        handleChangeWordWrap(value) {
            this.selectWordWrap = value
        },
    }
}
</script>
<style scoped>
.title {
    width: 100%;
    line-height: 100px;
    display: flex; 
}
.language {
    text-align: center;
    flex: 1;
    line-height: 100px;
}

.theme {
    flex: 1;
    line-height: 100px;
    text-align: center;
}

.my-editor {
    /* border: 1px #4e4e4e solid; */
}

.font-size {
    flex: 1;
    line-height: 100px;
    text-align: center;
}

.cursor-style {
    flex: 1;
    line-height: 100px;
    text-align: center;
}

.read-only {
    flex: 1;
    line-height: 100px;
    text-align: center;
}

.word-warp {
    flex: 1;
    line-height: 100px;
    text-align: center;
}
</style>