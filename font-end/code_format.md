### ESlint和prettier
1. prettier是直接根据规则重写整个代码,所以不会出现语法错误
2. prettier不具备lint的代码检查能力,检查不出类似no-unused-vars(声明但没使用)这种JS逻辑错误
3. prettier提供的配置项较少,比较粗暴
4. prettier可以集成到eslint中,使用eslint的规则
```
// eslintrc.json
"plugins": ["prettier"],
```
5. vscode安装prettier插件后,右键格式化会自动使用prettier(-eslint)的格式化替代原本vscode的格式化
6. 安装husky lint-staged并设置了lint-staged后,每次提交代码,prettier都会自动格式化,或者手动执行
```
./node_modules/.bin/prettier --single-quote --write "src/**/*.{js,jsx,json,css}"
```

### EditorConfig
EditorConfig是VScode编辑器的一个插件,安装插件后再在根目录下新建一个.editorconfig文件
```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

```
每次在VScode下保存时就会格式化代码