<template>
    <div class="monaco-container">
        <div class="ChosseLanguage">
            语言：
            <el-select v-model="language" clearable placeholder="请选择" size="mini" @change="changeLanguage">
                <el-option v-for="(item, index) in sets.language" :key="index" :label="item.key" :value="item">
                </el-option>
            </el-select>

        </div>
        <!--调用子组件-->
        <div class="monaco-editor">
            <monaco ref="monaco" :opts="opts" @change="changeValue"></monaco>
            <div class="monaco-editor-result">{{ myresult }}</div>
        </div>
        <el-button class="submit-button" type="primary" size="mini" @click="postQues">提交代码</el-button>
    </div>
</template>
<script>
import monaco from "./monaco";
export default {
    name: 'ToEditor',
    components: { monaco },
    data() {
        return {
            sets: {
                language: [],
                theme: {
                    vs: "vs",
                    "vs-dark": "vs-dark",
                    "hc-black": "hc-black",
                },
            },
            opts: {
                value: "",
                readOnly: false, // 是否可编辑
                language: "rust", // 语言类型
                theme: "hc-black", // 编辑器主题
            },
            language: {},
            mycode: "",
            mylanguage: "",
            myresult: "结果展示"
        };
    },
    methods: {
        changeLanguage(val) {
            console.log(val)
            this.mylanguage = val.key
            this.opts.language = val.value;
            console.log(this.mylanguage, this.opts.language)
        },
        changeTheme(val) {
            this.opts.theme = val;
        },
        // 手动获取值
        getValue() {

            console.log(this.mycode);
        },
        // 内容改变自动获取值
        changeValue(val) {
            this.mycode = val;
        },
        async postQues() {
            let param = {
                code: this.mycode,
                input: "",
                lang: this.mylanguage
            }
            let res = await this.$axios.post('http://www.tangtangnas.online:10000/api/run', param)
            console.log(res)
            if (res.data.code == 0) {
                this.myresult = res.data.data.stdout
            } else {
                this.$message.error(res.data.msg || "运行失败")
            }
        }
    },
    mounted() {
        let language = {
            "rust": "rust",
            "c": "c",
            "c89": "c",
            "c99": "c",
            "c11": "c",
            "c17": "c",
            "cpp": "cpp",
            "cpp98": "cpp",
            "cpp11": "cpp",
            "cpp14": "cpp",
            "cpp17": "cpp",
            "cpp20": "cpp",
            "cpp23": "cpp",
            "java": "java",
            "golang": "go",
            "python2": "python",
            "python3": "python",
            "php5": "php",
            "php7": "php",
            "php8": "php",
            "nodejs": "javascript",
            "shell": "shell",
            "swift": "swift",
            "ruby": "ruby",
            "perl": "perl"
        }
        for (const key in language) {
            if (Object.hasOwnProperty.call(language, key)) {
                const element = language[key];
                this.sets.language.push({
                    key: key,
                    value: element
                })
            }
        }
    },
};
</script>
<style scoped>
.monaco-container {
    position: relative;
    height: 75%;
    width: 80%;
    margin-bottom: 10px;
    left: 50%;
    transform: translate(-50%, 0%);
}

.monaco-editor {
    height: 100%;
    border: 1px solid rgb(91, 93, 93);
}

.submit-button {
    position: relative;
    right: 0;
}

.ChosseLanguage {
    margin-bottom: 10px;
}

.monaco-editor-result {
    height: calc(100% - 500px);
}
</style>